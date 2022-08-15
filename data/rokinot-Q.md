## Low

### Missing zero checks for important operations

Both contract's constructors are missing zero address checks for their deployment.

[#L100-L122](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L100-L122)
[#L14-L17](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/features/Blocklist.sol#L14-L17)

## Non-critical

### Missing dev parameters

[#L170](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L170)
