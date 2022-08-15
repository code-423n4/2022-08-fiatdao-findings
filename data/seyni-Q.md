# QA

## Non-critical

### [N-01] Duplicated require() checks should be refactored to a modifier.

Consider replacing every `require(msg.sender == owner, "Only owner")` by a modifier `OnlyOwner` to improve readability.

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L140

```solidity
140: require(msg.sender == owner, "Only owner");
```

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L147

```solidity
147: require(msg.sender == owner, "Only owner");
```

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L154

```solidity
154: require(msg.sender == owner, "Only owner");
```

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L162

```solidity
162: require(msg.sender == owner, "Only owner");
```

### [N-02] Use of floating pragma for implementation contract.

Implementation contracts `VotingEscrow.sol` and `BlockList.sol` should not have a floating pragma to ensure code has been tested and deployed with the same version.

## Low severity

### [L-01] `decimal` does not follow ERC20 standard and is unnecessarily harcoded to 18.

In `VotingEscrow.sol`, `decimal` type is `uint256` when it should be `uint8` as set in the interface IERC20.sol.

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L66

```solidity
66: uint256 public decimals = 18;
```

`decimal` is also hardcoded to 18 while its value will change in the constructor anyway using decimals().

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L66

```solidity
66: uint256 public decimals = 18;
```

File: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L115

```solidity
115: decimals = IERC20(_token).decimals();
```