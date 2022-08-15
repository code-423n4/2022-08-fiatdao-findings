# [L-01] Lack of Checks-Effects-Interactions pattern in withdraw(), quitLock() and collectPenalty()

In the file contracts/VotingEscrow.sol, the `withdraw()` and `quitLock()` functions are updating the `locked` state variable before calling `token.transfer`. Also, the `penaltyAccumulated` state variable is being updated before the external call in `collectPenalty()`.

Even if `withdraw()` and `quitLock()` are already using the `nonReentrant` modifier, I would still recommend following the [checks-effects-interactions pattern](https://docs.soliditylang.org/en/v0.8.16/security-considerations.html#re-entrancy). 

## Recommended Mitigation Steps

Only update the state after the external call.

```
$ git diff --no-index VotingEscrow.sol.orig VotingEscrow.sol
diff --git a/VotingEscrow.sol.orig b/VotingEscrow.sol
index f15781a..d3eb126 100644
--- a/VotingEscrow.sol.orig
+++ b/VotingEscrow.sol
@@ -536,7 +536,6 @@ contract VotingEscrow is IVotingEscrow, ReentrancyGuard {
         newLocked.end = 0;
         newLocked.delegated -= int128(int256(value));
         newLocked.delegatee = address(0);
-        locked[msg.sender] = newLocked;
         newLocked.delegated = 0;
         // oldLocked can have either expired <= timestamp or zero end
         // currentLock has only 0 end
@@ -544,6 +543,7 @@ contract VotingEscrow is IVotingEscrow, ReentrancyGuard {
         _checkpoint(msg.sender, locked_, newLocked);
         // Send back deposited tokens
         require(token.transfer(msg.sender, value), "Transfer failed");
+        locked[msg.sender] = newLocked;
         emit Withdraw(msg.sender, value, LockAction.WITHDRAW, block.timestamp);
     }

@@ -641,7 +641,6 @@ contract VotingEscrow is IVotingEscrow, ReentrancyGuard {
         newLocked.amount = 0;
         newLocked.delegated -= int128(int256(value));
         newLocked.delegatee = address(0);
-        locked[msg.sender] = newLocked;
         newLocked.end = 0;
         newLocked.delegated = 0;
         // oldLocked can have either expired <= timestamp or zero end
@@ -655,6 +654,7 @@ contract VotingEscrow is IVotingEscrow, ReentrancyGuard {
         uint256 remainingAmount = value - penaltyAmount;
         // Send back remaining tokens
         require(token.transfer(msg.sender, remainingAmount), "Transfer failed");
+        locked[msg.sender] = newLocked;
         emit Withdraw(msg.sender, value, LockAction.QUIT, block.timestamp);
     }

@@ -672,8 +672,8 @@ contract VotingEscrow is IVotingEscrow, ReentrancyGuard {
     /// @dev Everyone can collect but penalty is sent to `penaltyRecipient`
     function collectPenalty() external {
         uint256 amount = penaltyAccumulated;
-        penaltyAccumulated = 0;
         require(token.transfer(penaltyRecipient, amount), "Transfer failed");
+        penaltyAccumulated = 0;
         emit CollectPenalty(amount, penaltyRecipient);
     }
```

#  [L-02] Add nonReentrant modifier into collectPenalty()

The lack of a reentrancy check exposes the function to recursive calls while previous calls are still being executed.

## Recommended Mitigation Steps

Add the `nonReentrant` modifier to prevent reentrancy attacks.

```
$ git diff --no-index VotingEscrow.sol.orig VotingEscrow.sol
diff --git a/VotingEscrow.sol.orig b/VotingEscrow.sol
index f15781a..3af7f13 100644
--- a/VotingEscrow.sol.orig
+++ b/VotingEscrow.sol
@@ -670,7 +670,7 @@ contract VotingEscrow is IVotingEscrow, ReentrancyGuard {

     /// @notice Collect accumulated penalty from quitters
     /// @dev Everyone can collect but penalty is sent to `penaltyRecipient`
-    function collectPenalty() external {
+    function collectPenalty() external nonReentrant {
         uint256 amount = penaltyAccumulated;
         penaltyAccumulated = 0;
         require(token.transfer(penaltyRecipient, amount), "Transfer failed");
```

## [L-03] Lack of balance checks for fee-on-transfer tokens

Some tokens will collect a fee for a transfer that could affet token accounting in the contract.

### Recommended Mitigation Steps

Ensure the balance of the token transfer is checked before and after the transfer call. E.g.

```
uint256 balanceBefore = IERC20(token).balanceOf(address(this));
require(IERC20(token).transfer(recipient, amount), "transfer failed")
uint256 balanceAfter = IERC20(token).balanceOf(address(this));
require(balanceAfter - amount == balanceBefore, "fee-on-transfer tokens not supported");
```

## [NC-01] Lock the pragma version

[Locking the pragma](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/) will ensure that the contracts do not get deployed using outdated compiler versions.

All the contract in scope are rounding the pragma version

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L2
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/features/Blocklist.sol#L2
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IBlocklist.sol#L2
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IERC20.sol#L2
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IVotingEscrow.sol#L2
```

## [NC-02] Use external instead of public for functions not called by the contract

```
File: contracts/VotingEscrow.sol
864: function totalSupply() public view override returns (uint256) {

871: function totalSupplyAt(uint256 _blockNumber)
```

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol
