# [2022-08-fiatdao] QA report
### Wrong comment
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L414
This comment makes reader misunderstood because from using `quitLock()`, users must create new lock with function `createLock()`. If users use `increaseAmount`, it will be reverted: https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L449

### Should use safeCast
should use `SafeCast.toInt128(int256())` instead of `int128(int256())`