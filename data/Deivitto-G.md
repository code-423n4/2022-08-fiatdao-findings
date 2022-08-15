
# GAS

## Public function visibility can be made external
### summary
 Functions should have the strictest visibility possible. Public functions may lead to more gas usage by forcing the copy of their parameters to memory from calldata.
### details
 If a function is never called from the contract it should be marked as external. This will save gas.
### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L871-L904
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L770-L819
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L754-L767
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L864-L868
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L33-L35

### mitigation
 Consider changing visibility from public to external.


## duplicated require() check should be refactored
### summary
duplicated require() / revert() checks should be
refactored to a modifier or function to save gas
### details
Event appears twice and can be reduced

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L125-L128
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L563
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L140
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L147
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L154
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L162
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L412
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L448
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L414
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L503
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L416
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L504
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L427
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L487
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L546
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L657
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L676
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L449
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L502
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L529
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L564
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L635
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L450
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L511
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L636
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L469
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L587
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L470
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L588
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L531
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L637
- - - -
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L776
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L877

### mitigation
refactor this checks to different functions to save gas

## use != rather than >0 for unsigned integers in require() statements
### Summary
When the optimizer is enabled, gas is wasted by doing a greater-than operation, rather than a not-equals operation inside require() statements. When Using != , the optimizer is able to avoid the EQ, ISZERO, and associated operations, by relying on the JUMPI that comes afterwards, which itself checks for zero.
### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L412
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L448

### mitigation
Use != 0 rather than > 0 for unsigned integers in require()  statements.


## Using bools for storage incurs overhead { 
### summary
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

### details
Here is one example of OpenZeppelin about this optimization
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L10
### mitigation
Consider using uint256 with values 0 and 1 rather than bool


## Pack structs tightly
### summary
Gas efficiency can be achieved by tightly packing the struct. Struct variables are stored in 32 bytes each so you can group smaller types to occupy less storage. 
### details
You can read more here: https://fravoll.github.io/solidity-patterns/tight_variable_packing.html or in the official documentation: https://docs.soliditylang.org/en/v0.4.21/miscellaneous.html
### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L75-L80

### mitigation
Search for an optimal size and order of structs to reduce gas usage.

## Pack state variables tightly
### summary
State variables are expected to be ordered by data type, this helps readability
and also gas optimization by tightly packing the variables.
### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L44-L66
### mitigation
Order in a proper way the state variables to improve readability and to reduce gas usage.

## Store using Struct over multiple mappings
### summary
All these variables could be combine in a Struct in order to reduce the gas cost. 
### details
As noticed in: 
https://gist.github.com/alexon1234/b101e3ac51bea3cbd9cf06f80eaa5bc2
When multiple mappings that access the same addresses, uints, etc, all of them can be mixed into an struct and then that data accessed like:
mapping(datatype => newStructCreated) newStructMap;
Also, you have this post where it explains the benefits of using Structs over mappings 
https://medium.com/@novablitz/storing-structs-is-costing-you-gas-774da988895e

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L58-L59


### mitigation
Consider mixing different mappings into an struct when able in order to save gas.


## Make immutable state variables that do not change but assigned in the constructor
### summary
State variables which value isn't changed by any function in the contract but constructor, can be declared as a immutable state variable to save some gas during deployment.

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L12
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L11
### mitigation
- Add immutable to state variables that do not change but which value is assigned in constructor

## Using private rather than public for constants saves gas
### summary
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L46-L48
### mitigation
Consider replacing public for private in constants for gas saving.

## Explicit initialization
### summary
It is not needed to initialize variables to the default value. Explicitly initializing it with its default value is an anti-pattern and wastes gas.
### details
If a variable is not set/initialized, it is assumed to have the default value ( 0 for uint, false for bool, address(0) for address…). 

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L229-L230
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L298
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L313
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L714
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L737
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L793-L794
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L836
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L889


### mitigation
Don't initialize variables to default value

## Index initialized in for loop
### summary
In for loops is not needed to initialize indexes to 0 as it is the default uint/int value. This saves gas.

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L309
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L834
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L717
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L739

### mitigation
Don't initialize variables to default value


## use of i++ in loop rather than ++i
### summary
++i costs less gas than i++, especially when it's used in for loops
### details
using ++i doesn't affect the flow of regular for loops and improves gas cost

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L309
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L834
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L717
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L739

### mitigation
Substitute to ++i 


## increments can be unchecked in loops
### summary
Unchecked operations as the ++i on for loops are cheaper than checked one.

### details
In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline..

    The code would go from:

    for (uint256 i; i < numIterations; i++) {
    // ...
    }

    to

    for (uint256 i; i < numIterations;) {
      // ...
      unchecked { ++i; }
    }
    The risk of overflow is inexistent for a uint256 here.

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L309
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L834
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L717
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L739
### mitigation
Add unchecked ++i at the end of all the for loop where it's not expected to overflow and remove them from the for header








## Shift right instead of dividing by 2
### Summary
Shifting is cheaper than dividing by 2

### Details
A division by 2 can be calculated by shifting one to the right.
While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity’s division operation also includes a division-by-0 prevention which is bypassed using shifting.

### Github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L719
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L743

### mitigation
Consider replacing / 2 with >> 1 here


## Internal functions only called once can be inlined to save gas
### Summary
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.
### Github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L662
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L732
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L37

### mitigation
Consider changing internal function only called once to inline code for gas savings



## >= cheaper than >
### Summary
Strict inequalities ( > ) are more expensive than non-strict ones ( >= ). This is due to some supplementary checks (ISZERO, 3 gas)

### Github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L176
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L236
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L244
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L288
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L412
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L448
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L449
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L469
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L502
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L529
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L564
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L587
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L621
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L635
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L42

### mitigation
Consider using >= 1 instead of > 0 to avoid some opcodes


## <X> += <Y> costs more gas than <X> = <X> + <Y> for state variables
### Summary
x+=y costs more gas than x=x+y for state variables

### Github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L654
### Mitigation
Don't use += for state variables as it cost more gas.


## Unused named returns
### summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 
### details
Also as returns variable is ignored, it wastes extra gas

### github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L204-L208

### mitigation
Remove return or returns when both used



## Functions guaranteed to revert when called by normal users can be marked payable
### Summary
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function.

Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Details
The extra opcodes avoided are:
CALLVALUE (2), DUP1 (3), ISZERO (3), PUSH2 (3), JUMPI (10), PUSH1 (3), DUP1 (3), REVERT(0), JUMPDEST (1), POP (2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

### Github permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L139-L165
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L23-L28
### Mitigation
It's suggested to add payable to functions guaranteed to revert when called by normal users to improve gas costs