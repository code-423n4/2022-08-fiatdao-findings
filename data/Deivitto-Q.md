## Use of hardcoded amount of days
### Summary 
Formula uses a hardcoded value of 365 (days) which would be wrong applied in a leap year (366 days)
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L47
### Mitigation
Consider using an oracle for this
Consider using a method that change the value between 365 and 366 for the operations in leap years and regular years

## Missing checks for address(0x0) when assigning values to address state or immutable variables 
### Summary
Zero address should be checked for state variables and immutable variables. A zero address can lead into problems.
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L14-L17
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L101-L103

### Mitigation
Check zero address before assigning or using it


## Missing checks for address(0x0) on transferOwnership
### Summary
Zero address should be checked for some function parameters. For example in functions like mints, withdrawals... 

A zero address can lead into serious problems as locking eth or correct functioning.

### Details
`owner` is being assigned to an address parameter and 0 address value is not being checked

### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L139-L157

### Mitigation
Check zero address before assigning or using it

## block.timestamp used as time proxy 
###  Summary:
Risk of using block.timestamp for time should be considered. 
###  Details:
block.timestamp is not an ideal proxy for time because of issues with synchronization, miner manipulation and changing block times. 

The use of block.timestamp with a strict equality is prone to not be accomplished. The block.timestamp can be manipulated.

block.timestamp is used through all the code in `VotingEscrow.sol`. Consider the fact that this can be manipulated to bypass some require and other conditions.

### References
SWC ID: 116

###  Github Permalinks: 
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L341-L346

###  Mitigation:
Consider using an oracle for precision
Consider the risk of using block.timestamp as time proxy and evaluate if block numbers can be used as an approximation for the application logic. Both have risks that need to be factored in. 


## Missing function on interface
### Summary:
IERC20 interface should include totalSupply() as it not an optional function
### Details
Also, I would like to mention that none of the expected ERC20 tokens with or without the scope are protected from race condition (checked with slither).
For more info about this typical erc20 issue:
https://github.com/0xProject/0x-monorepo/issues/850 
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/interfaces/IERC20.sol#L4

### Mitigation
Consider following the ERC20 interface in his totality.


# Informational
## Missing inheritance
###  Summary:
Contract is missing to inherit its dedicated interface
### Details
BlockList is not inheriting from IBlocklist
### Github Permalink
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L9-L44
### Mitigation
Add the inheritance to the contract

##  Use of magic numbers is confusing and risky
###  Summary:
Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
###  Details:
Values are hardcoded and would be more readable and maintainable if declared as a constant

In several locations in the code numbers like 18, 128, 255, 10**18 are used. It's quite easy to make a mistake somewhere when using hardcoded numbers.

###  Github Permalinks:
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L116
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L309
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L834
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L717
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L739
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L653

###  Mitigation:
Replace magic hardcoded numbers with declared constants.
Define constants for the numbers used throughout the code.
Comment what this numbers are intended for

## Missing indexed event parameters
###  Summary:
Events without indexed event parameters make it harder and
inefficient for off-chain tools to analyze them.
###  Details:
Indexed parameters (“topics”) are searchable event parameters.
They are stored separately from unindexed event parameters in an efficient manner to allow for faster access. This is useful for efficient off-chain-analysis, but it is also more costly gas-wise.
 
###  Github Permalinks:
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L38-L42

###  Mitigation:
Consider which event parameters could be particularly useful to off-chain tools and should be indexed.




## Use of a more recent of solidity
### Summary
Rather than using require/revert messages, since pragma version 0.8.4, custom Errors are available, this can improve the gas cost and readability.
### Details
Custom errors reduce 38 gas if the condition is met and 22 gas otherwise.
Also reduces contract size and deployment costs.
This can improve VotingEscrow.sol and Blocklist.sol gas cost
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L2

### Mitigation
Consider changing to pragma 0.8.4



## Missing Natspec 
 ###  Summary: 
 Missing Natspec and regular comments affect readability and maintainability of a codebase. 
 ###  Details: 
 Contracts has partial or full lack of comments
 ###  Github Permalinks: 
 #### Some Natspec @params
- exact param

#### Natspec @param
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L146-L157

#### Natspec @return value
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L699-L751
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L33-L35

#### Natspec in general 
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L871
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L863-L904
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L753-L819
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L684-L697
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L396-L669
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L167-L183
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L37-L43

#### More documentation needed
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L124-L130

 ###  mitigation
 Add @param descriptors
 Add @return descriptors
 Add Natspec comments. 
 Add inline comments. 
 Add comments for what the contract does






## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots
### Details

Events are declared before the state variables, but also the state variables are declared without order affecting readability of the code (for example: 3 uint variables, 2 addresses, 2 more uints, 1 address, etc). Variables of the same type should be put together. 

Modifier defined after constructor
### github permalink
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L25-L130


### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.15/style-guide.html

## Function shadows built-in symbol
 ###  Summary: 
Name shadowing where two or more variables/functions share the same name could be confusing to developers and/or reviewers
 ###  Details: 
 Use of `block` keyword as a function name
 ###  Github Permalinks: 
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/features/Blocklist.sol#L23
 ###  Mitigation
 Replace `block` variable in the function parameter to `blockAddress` or a similar substitution

## Large multiples of ten should use scientific notation (e.g. 1e6) rather than decimal literals (e.g. 1000000), for readability
### Summary:
Multiples of 10 can be declared as constants with scientific notation so it's easier to read them and less prone to miss/exceed a 0 of the expected value.

### Details
Values 1000000000000000000 and 1000000000 can be used in scientific notation
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L57-L58

### Mitigation
Replace hardcoded numbers with constants that represent the scientific corresponding notation

## Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)
### Summary:
Multiples of 10 can be declared as constants with scientific notation so it's easier to read them and less prone to miss/exceed a 0 of the expected value.

### Details
Values 10**18 and 1000000000 can be used in scientific notation
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L48
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L51
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L653
### Mitigation


## ERC20 decimals
### Summary:
ERC20 decimals expect to use uint8
### Details
The code is interacting with ERC20 decimals but using decimals uint256 = 18
In one hand, several tokens incorrectly return a uint256. If this is the case, ensure the value returned is below 255.
In the other hand to assume that 18 is the ERC20 value of decimals is also incorrect as some ERC20 tokens doesn't use 18 but less 
### Github Permalinks
https://github.com/code-423n4/2022-08-fiatdao/blob/5a254ab15a387bd65a7dc50ac8371cb77de1e5d5/contracts/VotingEscrow.sol#L66

### Mitigation
Consider if the interaction with some ERC20 token decimals() can affect the code.

