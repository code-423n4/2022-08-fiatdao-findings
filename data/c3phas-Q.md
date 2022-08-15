## QA
## Missing checks for address(0x0) when assigning values to address state variables
File: Blocklist.sol [Line 15](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L15)
```solidity
        manager = _manager;
```

File: Blocklist.sol [Line 16](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L16)
```solidity
        ve = _ve;
```

File: VotingEscrow.sol [Line 120](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L120)
```solidity
        owner = _owner;
```
File: VotingEscrow.sol [Line 121](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L121)
```solidity
        penaltyRecipient = _penaltyRecipient;
```

## constants should be defined rather than using magic numbers
There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. 

File: VotingEscrow.sol [Line 653](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L653)
`10**18`

```solidity
        uint256 penaltyAmount = (value * penaltyRate) / 10**18; // quitlock_penalty is in 18 decimals precision
```

File: VotingEscrow.sol [Line 309](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309)
**255**
```solidity
        for (uint256 i = 0; i < 255; i++) {
```

File: VotingEscrow.sol [Line 834](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L834)
**255**
```solidity
        for (uint256 i = 0; i < 255; i++) {
```

## Lock pragmas to specific compiler version
Contracts should be deployed with the same compiler version and flags that they have been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors.

File: Blocklist.sol [Line 2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L2)
```solidity
pragma solidity ^0.8.3;
```

File: VotingEscrow.sol [Line 2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L2)
```solidity
pragma solidity ^0.8.3;
```

## Natspec is incomplete
File: VotingEscrow.sol [Line 152-157](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L152-L157)
Missing @param \_addr
```solidity
    /// @notice Updates the recipient of the accumulated penalty paid by quitters
    function updatePenaltyRecipient(address _addr) external {
        require(msg.sender == owner, "Only owner");
        penaltyRecipient = _addr;
        emit UpdatePenaltyRecipient(_addr);
    }
```

File: VotingEscrow.sol [Line 167-170](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L167-L170)
Missing @param \_addr
```solidity
    /// @notice Forces an undelegation of virtual balance for a blocked locker
    /// @dev Can only be called by the Blocklist contract (as part of a block)
    /// @dev This is an irreversible action
    function forceUndelegate(address _addr) external override {
```

File: VotingEscrow.sol [Line 145-146](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L145-L146)
Missing @param \_addr
```solidity
    /// @notice Updates the blocklist contract
    function updateBlocklist(address _addr) external {
```


## public functions not called by the contract should be declared external instead
Contracts are allowed to override their parents' functions and change the visibility from external to public.

File:Blocklist.sol [Line 33-35](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L33-L35)
```solidity
    function isBlocked(address addr) public view returns (bool) {
        return _blocklist[addr];
    }
```

File:VotingEscrow.sol [Line 754-767](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L754-L767)
```solidity
    function balanceOf(address _owner) public view override returns (uint256) {
        uint256 epoch = userPointEpoch[_owner];
        if (epoch == 0) {
            return 0;
        }
        Point memory lastPoint = userPointHistory[_owner][epoch];
        lastPoint.bias =
            lastPoint.bias -
            (lastPoint.slope * int128(int256(block.timestamp - lastPoint.ts)));
        if (lastPoint.bias < 0) {
            lastPoint.bias = 0;
        }
        return uint256(uint128(lastPoint.bias));
    }
```

File:VotingEscrow.sol [Line 770](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L770)
```solidity
    function balanceOfAt(address _owner, uint256 _blockNumber)
```
