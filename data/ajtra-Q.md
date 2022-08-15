# Summary
## Low
1. L-1. Use of Floating Pragma
2. L-2. Prefer safetransfer and safetransferfrom for erc20 token transfers

## Non-Critical
1. NC-1. Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)


# Low
## L-1. Use of Floating Pragma
### Description
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. 
Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version 
that might introduce bugs that affect the contract system negatively.

### Lines in the code
[VotingEscrow.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L2)
[Blocklist.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L2)
[IBlocklist.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IBlocklist.sol#L2)
[IVotingEscrow.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IVotingEscrow.sol#L2)
[IERC20.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IERC20.sol#L2)

## L-2. Prefer safetransfer and safetransferfrom for erc20 token transfers
### Description
Consider using OpenZeppelin’s SafeERC20 library to handle edge cases in ERC20 token transfers. 
This prevents accidentally forgetting to check the return value.

### Lines in the code
[VotingEscrow.sol#L426](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L426)
[VotingEscrow.sol#L486](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L486)
[VotingEscrow.sol#L546](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L546)
[VotingEscrow.sol#L657](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L657)
[VotingEscrow.sol#L676](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L676)


# Non-Critical
## NC-1. Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)
### Lines in the code
[VotingEscrow.sol#L48](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L48)
[VotingEscrow.sol#L51](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L51)
[VotingEscrow.sol#L653](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L653)
