## 1. One step ownership transfer
In function [`transferOwnership`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L139) `owner` is changed in a single step. Consider using a two step procedure to avoid the risk of losing contract ownership.

## 2. Critical consequences of `unlock()` function not explicitly stated 
While the team confirmed that the [`unlock()`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L161) function is intended to be used for migration, so it won't cause any issues, it would be better to state explicitly in natspec comment that it will break the escrow functionality and must only be used for migration.
  
## 3. Unused/redundant code:
- Unused initial value of state variable `decimals`: 
State variable `decimals` is assigned in the constructor ([L115](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L115)). 
- If block at [`VotingEscrow.sol#L257-L259`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L257-L259) is redundant since `userPointHistory[_addr][uEpoch + 1]` is always assigned afterwards at [line 264](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L264).

## 4. Missing or incomplete natspec comments
- In [`VotigEscrow.sol#L45-L66`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L45-L66) public state variables lack naspec comment.
- In external function [`updateBlocklist`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L146) `@param _addr` is missing
- In external function [`updatePenaltyRecipient`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L153) `@param _addr` is missing
- In external function [`forceUndelegate`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L170) `@param _addr` is missing

## 5. Shadowed built in special variable `block`
In [`Blocklist.sol#L23`](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L23) function `block()` shadows solidity built in special variable `block`.