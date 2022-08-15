# **[L-01] ZERO-ADDRESS CHECKS ARE MISSING**

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.

Impact: Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

```solidity
owner = _owner;
penaltyRecipient = _penaltyRecipient;
```

[https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L120](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L120)

[https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L121](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L121)

## **L002 - Use Two-Step Transfer Pattern for Access Controls**

### **Description**

Contracts implementing access control's, e.g. `owner`, should consider implementing a Two-Step Transfer pattern.

Otherwise it's possible that the role mistakenly transfers ownership to the wrong address, resulting in a loss of the role.

```solidity
function transferOwnership(address _addr) external {
        require(msg.sender == owner, "Only owner");
        owner = _addr;
        emit TransferOwnership(_addr);
    }
```

[https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L139](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L139)