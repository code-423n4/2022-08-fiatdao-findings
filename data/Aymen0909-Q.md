# Low Risk & QA

## Summary

|               | Issue         | Risk     |
| :-------------: |:-------------|:-------------:|
| 1       | Missing Zero address checks    | Low      |
| 2       | Contract Blocking lack decentralization    | Low      |
| 3      | Public functions not called by the contract should be declared external instead     | QA |
| 4      | Large multiples of 10 should use scientific notation (eq 1e6) rather than decimal literals or exponentiation (e.g. 1000000, 10**18) | QA    |
| 5      | Duplicated require() checks should be refactored to a modifier for saving deployment costs     |  QA  |
| 6      | Named return variables not used anywhere in the function    |  QA  |
| 7      | Use more recent solidity versions   |  QA  |

## Findings

### 1- Missing Zero address checks :

*Impact - LOW RISK

There is no ZERO address checks for transferOwnership function inside VoteEscrow.sol contract, so if zero address is passed by error the contract control will be lost.

The same issue appear in the constructor of the Blocklist.sol contract, there is no ZERO address check for the manager or ve contract addresses,  if zero address is passed for the manager by error there no way anymore to block a contract from the VoteEscrow

There are 3 instances of this issue:

```
File: contracts/VotingEscrow.sol

139      owner = _addr;

File: contracts/features/Blocklist.sol

139      manager = _manager;
140      ve = _ve;
```

### 2- Contract Blocking lack decentralization :

*Impact - LOW RISK

If the manager of Blocklist.sol contract is an EOA then he has all the power to block any contract and force it to undelegate from VoteEscrow, which is very centralized. 

The manager should be another smart contract or a multisig contract where a group of people elected  by DAO voting are responsible for voting if a contract must be blocked or not , and then the manager contract calls the block function.

### 3- Public functions not called by the contract should be declared external instead :

*Impact - NON CRITICAL

There is 1 instance of this issue:

```
File: contracts/features/Blocklist.sol

33     function isBlocked(address addr) public view returns (bool)
```


### 4- Large multiples of 10 should use scientific notation (eq 1e6) rather than decimal literals or exponentiation (e.g. 1000000, 10**18) :

*Impact - NON CRITICAL

When using multiples of 10 you shouldn't use decimal literals or exponentiation but use scientific notation for better readability.

There are 4 instances of this issue:

```
File: contracts/VotingEscrow.sol

46      uint256 public constant MULTIPLIER = 10**18;
49      uint256 public maxPenalty = 10**18;
55      Point[1000000000000000000] public pointHistory;
56      mapping(address => Point[1000000000]) public userPointHistory;
```

### 5- Duplicated require() checks should be refactored to a modifier for saving deployment costs :

*Impact - NON CRITICAL

require() checks repeated in multiple functions should be refactored into a modifier for better readability and also to save deployment gas.

There are 4 instances of this issue:

```
File: contracts/VotingEscrow.sol

140     require(msg.sender == owner, "Only owner");
147     require(msg.sender == owner, "Only owner");
154     require(msg.sender == owner, "Only owner");
162     require(msg.sender == owner, "Only owner");
```
Those checks should be replaced by a **onlyOwner** modifier.


### 6- Named return variables not used anywhere in the function :

*Impact - NON CRITICAL

Named return variable should be used inside the function or if not they should be removed to avoid confusion.

There is 1 instance of this issue in the **getLastUserPoint** function :

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L201-L216

### 7- Use more recent solidity versions :

*Impact - NON CRITICAL

Use a solidity version of at least 0.8.4 to get Custom errors which are used in place of require()/revert() strings to save deployment cost.

```
File: contracts/VotingEscrow.sol

2      pragma solidity ^0.8.3;

File: contracts/features/Blocklist.sol

2      pragma solidity ^0.8.3;
```