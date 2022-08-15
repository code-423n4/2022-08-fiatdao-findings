1. Missing Indexed

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IERC20.sol#L29-L30

indexed can be use for uint256 value 

2. Incorrect named function

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IVotingEscrow.sol#L14

Since this function was used to be    `/// @notice Locks more tokens in an existing lock` it can be changed into fn `increaseLockAmount()`, i know this was intendeed but sure this was good implementation for this case.

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L440

3. Need to be checked `locked_.end` on fn `increaseUnlockTime()`

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L501-L504

this implementation below can be set for validate input, since it was needed to be checked if `locked` was ended

```
require(locked_.end > block.timestamp, "Lock expired");
```

4. Incorrect Reason String 

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L776

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L877

rather than using "Only past block number", you can used "Must pass block number" instead.

5. Avoid Floatin Pragma's

Since it was used ^0.8.3. As the compiler can be use for example 0.8.x and consider locking at this version the same as another. It can be consider using  locking the pragma version whenever possible and avoid using a floating pragma in the final deployment. Since it can be problematic, if there are publicly disclosed bugs and issues that affect the current compiler version used.

6. Natspec Incomplete

File :

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L30-L35

Missing @return