# Executive Summary

The codebase is pretty well thought out and would benefit by using `immutable` as much as possible

Upgrading to a new `nonReentrant` modifier will also save a lot of gas

Beside that, minor gas savings are available.

All suggestions are sorted by impact and ease of application.

At the bottom I list a set of "usual suspects" which I assume most other wardens will have sent.

Unless otherwise noted, all gas savings are estimates.

## Make unchanged storage variable immutable - 2.1k per operation per variable

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L107-L108

```solidity
        token = IERC20(_token);

```

Since `token` is never changed, you can make it immutable, this will save a SLOAD each time `token` is used, saving at least 2.1k per function using that variable.

This is most likely the easiest most effective gas saving you can apply


https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L63-L67


### Also applies to:

```solidity
    // Voting token
    string public name;
    string public symbol;
    uint256 public decimals = 18;

```

And 
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L11-L17

```solidity
    address public manager;
    address public ve;

    constructor(address _manager, address _ve) {
        manager = _manager;
        ve = _ve;
    }
```

## Massive 15k per tx gas savings - use 1 and 2 for Reentrancy guard

I understand the contract `ReentrancyBlock` is out of scope, however it's extensively used in `VotingEscrow` and the change would net 15k gas savings on most transactions.

Using `true` and `false` will trigger gas-refunds, which after London are 1/5 of what they used to be, meaning using `1` and `2` (keeping the slot non-zero), will cost 5k per change (5k + 5k) vs 20k + 5k, saving you 15k gas per function which uses the modifier

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/libraries/ReentrancyBlock.sol#L12-L13

```solidity
        _entered = true;

```

See Solmate's implementation:
https://github.com/transmissions11/solmate/blob/main/src/utils/ReentrancyGuard.sol

## No need to copy old lock if you're setting to 0 - 300+ saved per `quitLock` call
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L646-L652

```solidity
        LockedBalance memory newLocked = _copyLock(locked_);
        newLocked.amount = 0;
        newLocked.delegated -= int128(int256(value));
        newLocked.delegatee = address(0);
        locked[msg.sender] = newLocked;
        newLocked.end = 0;
        newLocked.delegated = 0;
```

Can instead just declare a new lock will all zeros and save the extra cost of Reading from Storage (300 gas min) and the rest of the operations (probably another hundred or 200 gas)

If you need to keep `delegated`, just add it back.

## Use Unchecked if you've already established no overflow can happen - 20 / 80 gas per unchecked operation

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L668

```solidity
    function _calculatePenaltyRate(uint256 end)

```


## Minor Loop Gas Savings via Unchecked and avoiding default assignment - 25 / 80 gas per loop per instance

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L719-L720

```solidity
        for (uint256 i = 0; i < 128; i++) {

```

Change to

```solidity
        for (uint256 i /* Remove default assignment */; i < 128; /* i++ is commented */) {
          // Code here


          // End of code
          unchecked { ++i; } // Riskless gas savings
        }

```
