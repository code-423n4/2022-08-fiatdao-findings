# Executive Summary

Overall the code is elegantly written and seems to be at a very late stage of development with pretty solid testing.

Below are minor observations found

## Legend:
- U -> Impact is unclear and potentially higher than Low Severity
- L -> Low Severity -> Could cause issues however impact is limited
- R -> Refactoring -> Suggested Code Change to improve readability and maintainability or to offer better User Experience
- NC -> Non-Critical / Informational -> No risk of loss, pertains to events or has no impact

## QA - U - No check for Blocklist on Withdraw 
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L528-L529

```solidity
    function withdraw() external override nonReentrant {

```

It may be preferrable to allow anyone to withdraw when their lock expires.

However I believe it's important to flag it up.


## QA - L - Lack of Zero Checks on Setters

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L145-L157

```solidity
    /// @notice Updates the blocklist contract
    function updateBlocklist(address _addr) external {
        require(msg.sender == owner, "Only owner");
        blocklist = _addr;
        emit UpdateBlocklist(_addr);
    }

    /// @notice Updates the recipient of the accumulated penalty paid by quitters
    function updatePenaltyRecipient(address _addr) external {
        require(msg.sender == owner, "Only owner");
        penaltyRecipient = _addr;
        emit UpdatePenaltyRecipient(_addr);
    }
```

### Also see:

- [Blocklist.constructor](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L14-L17)
- [VotingEscrow.constructor](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L107)

## QA - R - Add comment to explain why setting to zero on Memory is Correct

The code in [`quitlock`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L644-L648) elegantly updates the storage and then changes the memory variable to pass to checkpoint to ensure voting power is set to 0.


```solidity
        locked[msg.sender] = newLocked;
        newLocked.end = 0;
        newLocked.delegated = 0;
```

However, to the untrained eye, this can look like a mistake as the memory variable is changed, leaving the storage untouched.

I'd recommend adding a comment such as:

```solidity
        locked[msg.sender] = newLocked;

        // Set to zero to keep internal `delegated` math working
        //  But make sure `checkpoint` captures the lock being broken
        newLocked.end = 0;
        newLocked.delegated = 0;
```

## QA - R - Style guide - Move struct declaration above Storage Usage - TODO

Per the Solidity Style guide, struct declarations should be moved above the Storage Layout Declaration

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L56-L74

See: https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-layout

## QA - R - Prefer Financial Notation to exponents

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L48-L49

```solidity
    uint256 public constant MULTIPLIER = 10**18;

```

Can be rewritten to

```solidity
    uint256 public constant MULTIPLIER = 1e18;
```

Which is more commonly used in financial applications


## QA - NC - Infinite "free" locks as long as each lock is below divisor

The function [`quitlock`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L632)

Will allow anyone to remove tokens from their lock, by paying an early breaking penalty.

Because of integer division, the penalty is always rounding down, this is fine and may be reported as a QA issue.

However, due to the rounding down, a clever attacker will now be able to setup as many locks as they want, with minor amounts of Token, with the ability of unlocking at any time.

At `MAXTIME` (lock and unlock in same block), this rounding would only apply to 1 wei, and it then grows as the amount of time that passes increases, however impact seems extremely minor.

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L652-L653

```solidity
        uint256 penaltyRate = _calculatePenaltyRate(locked_.end);
        uint256 penaltyAmount = (value * penaltyRate) / 10**18; // quitlock_penalty is in 18 decimals precision
```


## POC
Create a lock for X with X < 10**18 * penaltyRate (99%)
Repeat

All locks can be broken, for free, at any time

Obvious downside is the gas cost for each lock, however if the token is expected to have a volatile price, this allows lockers to effectively vote without any skin in the game.



# Ultra Low Prio Findings

Adding these reports because I don't want to be penalized, however I believe for your use case the contract is fine.

Consider downgrading any Med submission to QA or even invalid.

## NC - Unchecked Castings

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L468-L469

```solidity
            locked_.amount += int128(int256(_value));

```

Casting are unchecked, in that an overflow will not be detected, however, for the FDT token, the totalSupply is: 1,000,000,000 meaning `1e27`. `uint128` covers up to `3.4e38` meaning it will never reasonably overflow if used with your DAO token.


## NC - Contract will not work for non-standard ERC-20
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L659-L660

```solidity
        require(token.transfer(msg.sender, remainingAmount), "Transfer failed");

```

If the ERC20 won't return (e.g. USDT) the contract will simply never work as the interface is expecting a non-zero return value.

Knowing you will use `FDT` we know this won't be an issue.

## NC - Contract won't work with feeOnTransfer tokens

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L428-L429

```solidity
            token.transferFrom(msg.sender, address(this), _value),

```

Because there's no check for the actual value received, any token that takes a fee (e.g USDT) will cause accounting errors for the contract

Knowing you will use `FDT` we know this won't be an issue.
