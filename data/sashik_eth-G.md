## G01 - Variables that could be immutable
Next variables never change after contract creation:
```solidity
contracts/VotingEscrow.sol:45   IERC20 public token;

contracts/VotingEscrow.sol:64   string public name;
contracts/VotingEscrow.sol:65   string public symbol;
contracts/VotingEscrow.sol:66   uint256 public decimals = 18;

contracts/features/Blocklist.sol:11 address public manager; 
contracts/features/Blocklist.sol:12 address public ve;
```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L45
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L64-L66
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/features/Blocklist.sol#L11-12

## G02 - Struct packing
Struct *LockedBalance* 
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L75-80
could be packed in more efficient way: 
```solidity
    struct LockedBalance {
        int128 amount;
        int128 delegated;
        uint256 end;
        address delegatee;
    }
```

## G03 - ```unchecked``` block can be used for gas efficiency of the expression that can't overflow/underflow

Next lines could be unchecked since these operations with constant values could not overflow/underflow and constant values are greater than 0:
```solidity
contracts/VotingEscrow.sol:237    userOldPoint.slope = _oldLocked.delegated / int128(int256(MAXTIME));

contracts/VotingEscrow.sol:245    userNewPoint.slope = _newLocked.delegated / int128(int256(MAXTIME));

contracts/VotingEscrow.sol:416    require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");

contracts/VotingEscrow.sol:702    return (_t / WEEK) * WEEK;
```

Next lines could ```unchecked``` since ```mid``` always > 0:
```solidity
contracts/VotingEscrow.sol:723  max = mid - 1; 
contracts/VotingEscrow.sol:747  max = mid - 1;
```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L723
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L747

Line 891 could unchecked due to check on line 890 - ```targetEpoch``` has less then max value, so adding 1 to it would not cause overflow:
```solidity
contracts/VotingEscrow.sol:890  if (targetEpoch < epoch) {
contracts/VotingEscrow.sol:891  Point memory pointNext = pointHistory[targetEpoch + 1];
```

Counter increment in for loop could be ```unchecked``` and prefix view ```++i```:
```solidity
contracts/VotingEscrow.sol:309  for (uint256 i = 0; i < 255; i++) { 
contracts/VotingEscrow.sol:717  for (uint256 i = 0; i < 128; i++) { 
contracts/VotingEscrow.sol:739  for (uint256 i = 0; i < 128; i++) {
contracts/VotingEscrow.sol:834  for (uint256 i = 0; i < 255; i++) {
```

## G04 - Cache storage variables in memory

For the storage variables that will be accessed multiple times, cache and read from the stack can save gas from each read if variable accessed more than once.```penaltyRecipient``` accessed 2 times:

```solidity 
contracts/VotingEscrow.sol:676  require(token.transfer(penaltyRecipient, amount), "Transfer failed");
contracts/VotingEscrow.sol:677  emit CollectPenalty(amount, penaltyRecipient);  
```

## G05 - No needed computation
No need to add 1 to ```uEpoch``` since it has zero value due to previous check:
```solidity
contracts/VotingEscrow.sol:257    if (uEpoch == 0) {
contracts/VotingEscrow.sol:258		userPointHistory[_addr][uEpoch + 1] = userOldPoint; 
contracts/VotingEscrow.sol:259    }
```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L258

