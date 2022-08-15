[S]: Suggested optimation, save a decent amount of gas without compromising readability;

[M]: Minor optimation, the amount of gas saved is minor, change when you see fit;

[N]: Non-preferred, the amount of gas saved is at cost of readability, only apply when gas saving is a top priority.



# ISSUE LIST

#### C4-001 : Adding unchecked directive can save gas [S]
#### C4-002 : Check if amount > 0 before token transfer can save gas [S]
#### C4-003 : There is no need to assign default values to variables [S]
#### C4-004 : Using operator && used more gas [S]
#### C4-005 : Non-strict inequalities are cheaper than strict ones [M]
#### C4-006 : Cache array length in for loops can save gas [S]
#### C4-007 : Use calldata instead of memory for function parameters [M]
#### C4-008 : ++i is more gas efficient than i++ in loops forwarding
#### C4-009 : `> 0` can be replaced with `!= 0` for gas optimization
#### C4-010 : Free gas savings for using solidity 0.8.10+ [S]
#### C4-011 : Use Custom Errors instead of Revert Strings to save Gas [S]
#### C4-012 : Function Ordering via Method ID [M]
#### C4-013 : State Variables that can be changed to immutable [S]
#### C4-015 : Exponential is more costly [S]



# C4-001 : Adding unchecked directive can save gas

## Impact

For the arithmetic operations that will never over/underflow, using the unchecked directive (Solidity v0.8 has default overflow/underflow checks) can save some gas from the unnecessary internal over/underflow checks.

## Proof of Concept

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834
```

## Tools Used

None

## Recommended Mitigation Steps

Consider applying unchecked arithmetic where overflow/underflow is not possible. Example can be seen from below.

```
Unchecked{i++};
```

# C4-002 : Check if amount > 0 before token transfer can save gas

## Impact

Since _amount can be 0. Checking if (_amount != 0) before the transfer can potentially save an external call and the unnecessary gas cost of a 0 token transfer.

## Proof of Concept

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L426
```

All Contracts

## Tools Used

None

## Recommended Mitigation Steps

Consider checking amount != 0.

# C4-003 : There is no need to assign default values to variables

## Impact -  Gas Optimization

Uint is default initialized to 0. There is no need assign false to variable.

## Proof of Concept

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834
```

## Tools Used

Code Review

## Recommended Mitigation Steps

uint x = 0 costs more gas than uint x without having any different functionality.



# C4-004 : Using operator && used more gas

## Impact

Using double require instead of operator && can save more gas.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L236

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L244

```

## Tools Used

Code Review

## Recommended Mitigation Steps

Example

```

using &&:

function check(uint x)public view{
require(x == 0 && x < 1 );
}
// gas cost 21630

using double require:

require(x == 0 );
require( x < 1);
}
}
// gas cost 21622
```


# C4-005 : Non-strict inequalities are cheaper than strict ones

## Impact

Strict inequalities add a check of non equality which costs around 3 gas.

## Proof of Concept

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Use >= or <= instead of > and < when possible.



# C4-006 : Cache array length in for loops can save gas

## Impact

Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

Caching the array length in the stack saves around 3 gas per iteration.

## Proof of Concept

1. Navigate to the following smart contract line.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834
```

## Tools Used

None

## Recommended Mitigation Steps

Consider to cache array length.



# C4-008 : Use calldata instead of memory for function parameters

## Code Location

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L597

## Impact


In some cases, having function arguments in calldata instead of
memory is more optimal.

Consider the following generic example:

```
contract C {
function add(uint[] memory arr) external returns (uint sum) {
uint length = arr.length;
for (uint i = 0; i < arr.length; i++) {
  sum += arr[i];
}
}
}
```

In the above example, the dynamic array arr has the storage location
memory. When the function gets called externally, the array values are
kept in calldata and copied to memory during ABI decoding (using the
opcode calldataload and mstore). And during the for loop, arr[i]
accesses the value in memory using a mload. However, for the above
example this is inefficient. Consider the following snippet instead:

```
contract C {
function add(uint[] calldata arr) external returns (uint sum) {
uint length = arr.length;
for (uint i = 0; i < arr.length; i++) {
  sum += arr[i];
}
}
}
```

In the above snippet, instead of going via memory, the value is directly
read from calldata using calldataload. That is, there are no
intermediate memory operations that carries this value.

Gas savings: In the former example, the ABI decoding begins with
copying value from calldata to memory in a for loop. Each iteration
would cost at least 60 gas. In the latter example, this can be
completely avoided. This will also reduce the number of instructions and
therefore reduces the deploy time cost of the contract.

In short, use calldata instead of memory if the function argument
is only read.

Note that in older Solidity versions, changing some function arguments
from memory to calldata may cause "unimplemented feature error".
This can be avoided by using a newer (0.8.*) Solidity compiler.


## Proof of Concept

1. Navigate to the following smart contract line.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L597

```

## Tools Used

None

## Recommended Mitigation Steps

Some parameters in examples given above are later hashed. It may be beneficial for those parameters to be in memory rather than calldata.


# C4-009 : ++i is more gas efficient than i++ in loops forwarding

## Impact

++i is more gas efficient than i++ in loops forwarding.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834
```

## Tools Used

Code Review

## Recommended Mitigation Steps

It is  recommend to use unchecked{++i} and change i declaration to uint256.


# C4-010 : `> 0` can be replaced with `!= 0` for gas optimization

## Impact

`!= 0` is a cheaper operation compared to `> 0`, when dealing with uint.


## Proof of Concept

1. Navigate to the following contract sections.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L621

```

## Tools Used

None

## Recommended Mitigation Steps

Consider to  replace `> 0`  with `!= 0` for gas optimization.


# C4-011 : Free gas savings for using solidity 0.8.10+

## Impact

Using newer compiler versions and the optimizer gives gas optimizations and additional safety checks are available for free.

## Proof of Concept

```
All Contracts
```


Solidity 0.8.13 has a useful change which reduced gas costs of external calls which expect a return value: https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/

Solidity 0.8.15 has some improvements too but not well tested.

Code Generator: Skip existence check for external contract if return data is expected. In this case, the ABI decoder will revert if the contract does not exist

All Contracts

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L2

## Tools Used

None

## Recommended Mitigation Steps

Consider to upgrade pragma to at least 0.8.15.

# C4-012 : Use Custom Errors instead of Revert Strings to save Gas


Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

Source Custom Errors in Solidity:

Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

Instances include:

All require Statements

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L637

## Tools Used

Code Review

## Recommended Mitigation Steps

Recommended to replace revert strings with custom errors.


**13. Function Ordering via Method ID**

**Context**: [All Contracts](https://github.com/code-423n4/2022-06-nested)

**Description**:

Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. One could use [This tool](https://emn178.github.io/solidity-optimize-name/) to help find alternative function names with lower Method IDs while keeping the original name intact.

**Recommendation**:

Find a lower method ID name for the most called functions for example `mostCalled()` vs. `mostCalled_41q()` is cheaper by 44 gas.


# C4-013 : State Variables that can be changed to immutable

## Code Location

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L64

## Impact

Solidity 0.6.5
introduced immutable as a major feature. It allows setting
contract-level variables at construction time which gets stored in code
rather than storage.

Consider the following generic example:

```
contract C {
/// The owner is set during contruction time, and never changed afterwards.
address public owner = msg.sender;
}
```

In the above example, each call to the function owner() reads from
storage, using a sload. After
EIP-2929, this costs 2100 gas
cold or 100 gas warm. However, the following snippet is more gas
efficient:

```
contract C {
/// The owner is set during contruction time, and never changed afterwards.
address public immutable owner = msg.sender;
}
```

In the above example, each storage read of the owner state variable is
replaced by the instruction push32 value, where value is set during
contract construction time. Unlike the last example, this costs only 3
gas.


## Tools Used

None

## Recommended Mitigation Steps

Consider using immutable variable.


# C4-014 : Less than 256 uints are not gas efficient

## Impact -  Gas Optimization

Lower than uint256 size storage instance variables are actually less gas efficient. E.g. using uint128 does not give any efficiency, actually, it is the opposite as EVM operates on default of 256-bit values so uint16 is more expensive in this case as it needs a conversion. It only gives improvements in cases where you can pack variables together, e.g. structs.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L639
```

## Tools Used

None

## Recommended Mitigation Steps

Consider to review all uint types. Change them with uint256 If the integer is not necessary to present with uint128.`


# C4-015 : Exponential is more costly

## Impact

In the solidity exponential is more costly than 1e18 definition.

## Proof of Concept

1. Navigate to the following contract.

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L48


## Tools Used

None

## Recommended Mitigation Steps

Consider changing 10**18 definition with 1e18.