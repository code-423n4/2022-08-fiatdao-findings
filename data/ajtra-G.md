# Index
1. Post-increment/decrement cost more gas then pre-increment/decrement
2. Operatos <= or >= cost more gas than operators < or >
3. != 0 is cheaper than > 0
4. Variable1 = Variable1 + (-) Variable2 is cheaper in gas cost than variable1 += (-=) variable2.
5. Use custom errors rather than REVERT()/REQUIRE() strings to save deployment gas
6. Using private rather than public for constants, saves gas
7. Usage of uints/ints smaller than 32 Bytes (256 bits) incurs overhead
8. Use a more recent version of solidity
9. Initialize variables with default values are not needed
10. Using bools for storage incurs overhead
11. Use unchecked when it's not possible to overflow.
12. Duplicated require()/revert() checks should be refactored to a modifier or function
13. Redundant code
14. Shift Right instead of Dividing by 2
15. Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
16. Not caching variable when it's going to use just once to save gas
17. Refactoring code to save gas
18. Reordering variables save gas

# Details
## 1. Post-increment/decrement cost more gas then pre-increment/decrement
### Description
++var (--var) cost less gas than var++ (var--). Save 5 gas per iteration in the for-loop.

### Total gas saved
(5 * 255) + (5 * 128) + (5 * 128) + (5 * 255) = 3830 gas saved.

### Lines in the code
[VotingEscrow.sol#L309](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309)
[VotingEscrow.sol#L717](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L717)
[VotingEscrow.sol#L739](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L739)
[VotingEscrow.sol#L834](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L834)

## 2. Operatos <= or >= cost more gas than operators < or >
### Description
Change all <= / >= operators for < / > and remember to increse / decrese in consecuence to maintain the logic (example, a <= b for a < b + 1)

### Lines in the code
[VotingEscrow.sol#L116](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L116)
[VotingEscrow.sol#L414](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L414)
[VotingEscrow.sol#L416](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L416)
[VotingEscrow.sol#L439](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L439)
[VotingEscrow.sol#L504](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L504)
[VotingEscrow.sol#L530](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L530)
[VotingEscrow.sol#L541](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L541)
[VotingEscrow.sol#L589](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L589)
[VotingEscrow.sol#L647](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L647)
[VotingEscrow.sol#L718](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L718)
[VotingEscrow.sol#L720](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L720)
[VotingEscrow.sol#L740](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L740)
[VotingEscrow.sol#L744](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L744)
[VotingEscrow.sol#L776](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L776)
[VotingEscrow.sol#L814](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L814)
[VotingEscrow.sol#L877](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L877)

## 3. != 0 is cheaper than > 0
### Description
Replace all > 0 for != 0

### Lines in the code
[VotingEscrow.sol#L176](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L176)
[VotingEscrow.sol#L236](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L236)
[VotingEscrow.sol#L244](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L244)
[VotingEscrow.sol#L288](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L288)
[VotingEscrow.sol#L412](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L412)
[VotingEscrow.sol#L448](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L448)
[VotingEscrow.sol#L449](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L449)
[VotingEscrow.sol#L469](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L469)
[VotingEscrow.sol#L502](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L502)
[VotingEscrow.sol#L529](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L529)
[VotingEscrow.sol#L564](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L564)
[VotingEscrow.sol#L587](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L587)
[VotingEscrow.sol#L621](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L621)
[VotingEscrow.sol#L635](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L635)
[VotingEscrow.sol#L814](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L814)
[Blocklist.sol#L42](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L42)

## 4. Variable1 = Variable1 + (-) Variable2 is cheaper in gas cost than variable1 += (-=) variable2.
### Description

### Lines in the code
[VotingEscrow.sol#L418](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L418)
[VotingEscrow.sol#L420](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L420)
[VotingEscrow.sol#L460](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L460)
[VotingEscrow.sol#L461](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L461)
[VotingEscrow.sol#L465](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L465)
[VotingEscrow.sol#L472](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L472)
[VotingEscrow.sol#L537](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L537)
[VotingEscrow.sol#L603](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L603)
[VotingEscrow.sol#L612](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L612)
[VotingEscrow.sol#L642](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L642)
[VotingEscrow.sol#L654](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L654)

## 5. Use custom errors rather than REVERT()/REQUIRE() strings to save deployment gas
### Description
Custom errors are available from solidity version 0.8.4. 
Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. 
Not defining the strings also save deployment gas.

### Lines in the code
[VotingEscrow.sol#L116](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L116)
[VotingEscrow.sol#L125](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L125)
[VotingEscrow.sol#L140](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L140)
[VotingEscrow.sol#L147](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L147)
[VotingEscrow.sol#L154](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L154)
[VotingEscrow.sol#L162](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L162)
[VotingEscrow.sol#L171](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L171)
[VotingEscrow.sol#L412](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L412)
[VotingEscrow.sol#L413](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L413)
[VotingEscrow.sol#L414](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L414)
[VotingEscrow.sol#L415](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L415)
[VotingEscrow.sol#L416](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L416)
[VotingEscrow.sol#L425](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L425)
[VotingEscrow.sol#L448](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L448)
[VotingEscrow.sol#L449](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L449)
[VotingEscrow.sol#L450](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L450)
[VotingEscrow.sol#L469](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L469)
[VotingEscrow.sol#L470](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L470)
[VotingEscrow.sol#L485](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L485)
[VotingEscrow.sol#L502](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L502)
[VotingEscrow.sol#L503](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L503)
[VotingEscrow.sol#L504](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L504)
[VotingEscrow.sol#L511](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L511)
[VotingEscrow.sol#L529](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L529)
[VotingEscrow.sol#L530](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L530)
[VotingEscrow.sol#L531](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L531)
[VotingEscrow.sol#L546](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L546)
[VotingEscrow.sol#L563](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L563)
[VotingEscrow.sol#L564](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L564)
[VotingEscrow.sol#L565](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L565)
[VotingEscrow.sol#L587](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L587)
[VotingEscrow.sol#L588](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L588)
[VotingEscrow.sol#L589](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L589)
[VotingEscrow.sol#L635](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L635)
[VotingEscrow.sol#L636](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L636)
[VotingEscrow.sol#L637](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L637)
[VotingEscrow.sol#L657](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L657)
[VotingEscrow.sol#L676](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L676)
[VotingEscrow.sol#L776](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L776)
[VotingEscrow.sol#L877](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L877)
[Blocklist.sol#L24](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L24)
[Blocklist.sol#L25](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L25)

## 6. Using private rather than public for constants, saves gas
### Description
If needed, the value can be read from the verified contract source code. 
Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, 
and not adding another entry to the method ID table

### Lines in the code
[VotingEscrow.sol#L46](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L46)
[VotingEscrow.sol#L47](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L47)
[VotingEscrow.sol#L48](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L48)

## 7. Usage of uints/ints smaller than 32 Bytes (256 bits) incurs overhead
### Description
When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. 
This is because the EVM operates on 32 bytes at a time.
Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes 
to the desired size. Use a larger size then downcast where needed

### Lines in the code
In the following lines a int/uint smaller than 32 bytes is having used in some way.
[VotingEscrow.sol#L60](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L60)
[VotingEscrow.sol#L70](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L70)
[VotingEscrow.sol#L71](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L71)
[VotingEscrow.sol#L76](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L76)
[VotingEscrow.sol#L78](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L78)
[VotingEscrow.sol#L109](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L109)
[VotingEscrow.sol#L110](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L110)
[VotingEscrow.sol#L174](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L174)
[VotingEscrow.sol#L205](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L205)
[VotingEscrow.sol#L206](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L206)
[VotingEscrow.sol#L229](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L229)
[VotingEscrow.sol#L230](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L230)
[VotingEscrow.sol#L239](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L239)
[VotingEscrow.sol#L242](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L242)
[VotingEscrow.sol#L247](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L247)
[VotingEscrow.sol#L250](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L250)
[VotingEscrow.sol#L313](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L313)
[VotingEscrow.sol#L319](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L319)
[VotingEscrow.sol#L321](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L321)
[VotingEscrow.sol#L418](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L418)
[VotingEscrow.sol#L420](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L420)
[VotingEscrow.sol#L460](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L460)
[VotingEscrow.sol#L461](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L461)
[VotingEscrow.sol#L465](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L465)
[VotingEscrow.sol#L472](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L472)
[VotingEscrow.sol#L533](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L533)
[VotingEscrow.sol#L537](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L537)
[VotingEscrow.sol#L567](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L567)
[VotingEscrow.sol#L598](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L598)
[VotingEscrow.sol#L639](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L639)
[VotingEscrow.sol#L642](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L642)
[VotingEscrow.sol#L762](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L762)
[VotingEscrow.sol#L766](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L766)
[VotingEscrow.sol#L813](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L813)
[VotingEscrow.sol#L815](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L815)
[VotingEscrow.sol#L836](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L836)
[VotingEscrow.sol#L849](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L849)
[VotingEscrow.sol#L860](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L860)
[IERC20.sol#L10](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IERC20.sol#L10)

## 8. Use a more recent version of solidity
### Description
 Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings 
 Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value. 

### Lines in the code
[VotingEscrow.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L2)
[Blocklist.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L2)
[IBlocklist.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IBlocklist.sol#L2)
[IVotingEscrow.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IVotingEscrow.sol#L2)
[IERC20.sol#L2](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IERC20.sol#L2)

## 9. Initialize variables with default values are not needed
### Description
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address,etc). 
Explicitly initializing it with its default value is an anti-pattern and wastes gas.

### Lines in the code
[VotingEscrow.sol#L229](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L229)
[VotingEscrow.sol#L230](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L230)
[VotingEscrow.sol#L298](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L298)
[VotingEscrow.sol#L309](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309)
[VotingEscrow.sol#L313](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L313)
[VotingEscrow.sol#L714](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L714)
[VotingEscrow.sol#L717](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L717)
[VotingEscrow.sol#L737](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L737)
[VotingEscrow.sol#L739](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L739)
[VotingEscrow.sol#L793](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L793)
[VotingEscrow.sol#L794](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L794)
[VotingEscrow.sol#L834](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L834)
[VotingEscrow.sol#L836](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L836)
[VotingEscrow.sol#L889](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L889)

## 10. Using bools for storage incurs overhead
### Description
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from 'false' to 'true', 
after having been 'true' in the past

### Lines in the code
[Blocklist.sol#L10](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L10)
[Blocklist.sol#L33](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L33)
[IBlocklist.sol#L7](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/interfaces/IBlocklist.sol#L7)

## 11. Use unchecked when it's not possible to overflow.
### Description
++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

### Lines in the code
[VotingEscrow.sol#L309](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309)
[VotingEscrow.sol#L717](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L717)
[VotingEscrow.sol#L739](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L739)
[VotingEscrow.sol#L834](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L834)

## 12. Duplicated require()/revert() checks should be refactored to a modifier or function
### Lines in the code
#### "Only owner"
[VotingEscrow.sol#L140](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L140)
[VotingEscrow.sol#L147](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L147)
[VotingEscrow.sol#L154](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L154)
[VotingEscrow.sol#L162](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L162)

#### "Only past block number"
[VotingEscrow.sol#L776](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L776)
[VotingEscrow.sol#L877](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L877)

#### "Only non zero amount"
[VotingEscrow.sol#L412](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L412)
[VotingEscrow.sol#L448](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L448)

#### "Only increase lock end"
[VotingEscrow.sol#L414](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L414)
[VotingEscrow.sol#L503](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L503)

#### "Exceeds maxtime"
[VotingEscrow.sol#L416](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L416)
[VotingEscrow.sol#L504](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L504)

#### "No lock"
[VotingEscrow.sol#L449](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L449)
[VotingEscrow.sol#L502](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L502)
[VotingEscrow.sol#L529](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L529)
[VotingEscrow.sol#L564](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L564)
[VotingEscrow.sol#L635](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L635)

#### "Lock expired"
[VotingEscrow.sol#L450](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L450)
[VotingEscrow.sol#L511](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L511)
[VotingEscrow.sol#L636](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L636)

#### "Delegatee has no lock"
[VotingEscrow.sol#L469](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L469)
[VotingEscrow.sol#L587](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L587)

#### "Delegatee lock expired"
[VotingEscrow.sol#L470](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L470)
[VotingEscrow.sol#L588](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L588)

## 13. Redundant code
### Description
In the code is using a require instead of using the modifier checkBlocklist() that already exist.

### Lines in the code
[VotingEscrow.sol#L563](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L563)

## 14. Shift Right instead of Dividing by 2
### Description
Gas can be saved by using >> operator instead of dividing by 2 on multiple line of codes.

### Lines in the code
[VotingEscrow.sol#L719](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L719)
[VotingEscrow.sol#L743](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L743)

## 15. Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
### Description
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. 
Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate 
the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

### Lines in the code
[VotingEscrow.sol#L58](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L58)
[VotingEscrow.sol#L59](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L59)
[VotingEscrow.sol#L61](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L61)

## 16. Not caching variable when it's going to use just once to save gas

### increaseAmount(uint256 _value)
Remove unlockTime caching from line 453 to use directly locked_.end in line 489

### increaseUnlockTime(uint256 _unlockTime)
```diff
function increaseUnlockTime(uint256 _unlockTime)
    external
    override
    nonReentrant
    checkBlocklist
{
    LockedBalance memory locked_ = locked[msg.sender];
    uint256 unlock_time = _floorToWeek(_unlockTime); // Locktime is rounded down to weeks
    // Validate inputs
    require(locked_.amount > 0, "No lock");
    require(unlock_time > locked_.end, "Only increase lock end");
    require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
    // Update lock
-   uint256 oldUnlockTime = locked_.end;
    locked_.end = unlock_time;
    locked[msg.sender] = locked_;
    if (locked_.delegatee == msg.sender) {
        // Undelegated lock
-       require(oldUnlockTime > block.timestamp, "Lock expired");
+       require(locked_.end > block.timestamp, "Lock expired");
        LockedBalance memory oldLocked = _copyLock(locked_);
        oldLocked.end = unlock_time;
        _checkpoint(msg.sender, oldLocked, locked_);
    }
    emit Deposit(
        msg.sender,
        0,
        unlock_time,
        LockAction.INCREASE_TIME,
        block.timestamp
    );
}
```

### totalSupply()
```diff
function totalSupply() public view override returns (uint256) {
-	uint256 epoch_ = globalEpoch;
-   Point memory lastPoint = pointHistory[epoch_];
-   return _supplyAt(lastPoint, block.timestamp);
+	return _supplyAt(pointHistory[globalEpoch], block.timestamp);
}
```

### Lines in the code
[VotingEscrow.sol#L440-L490](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L493-L523)
[VotingEscrow.sol#L493-L523](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L493-L523)
[VotingEscrow.sol#L864-L868](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L864-L868)

## 17. Refactoring code to save gas
### Description
In the folling cases it's possible to move the variable declaration after if's sentences to save gas when the if's conditions are true.

### Lines in the code

#### VotingEscrow.delegate. Move the variable value after the last require.
[VotingEscrow.sol#L555-L592](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L555-L592)

```diff
    function delegate(address _addr)
        external
        override
        nonReentrant
        checkBlocklist
    {
        LockedBalance memory locked_ = locked[msg.sender];
        // Validate inputs
        require(!IBlocklist(blocklist).isBlocked(_addr), "Blocked contract");
        require(locked_.amount > 0, "No lock");
        require(locked_.delegatee != _addr, "Already delegated");
        // Update locks
-       int128 value = locked_.amount;
        address delegatee = locked_.delegatee;
        LockedBalance memory fromLocked;
        LockedBalance memory toLocked;
        locked_.delegatee = _addr;
        if (delegatee == msg.sender) {
            // Delegate
            fromLocked = locked_;
            toLocked = locked[_addr];
        } else if (_addr == msg.sender) {
            // Undelegate
            fromLocked = locked[delegatee];
            toLocked = locked_;
        } else {
            // Re-delegate
            fromLocked = locked[delegatee];
            toLocked = locked[_addr];
            // Update owner lock if not involved in delegation
            locked[msg.sender] = locked_;
        }
        require(toLocked.amount > 0, "Delegatee has no lock");
        require(toLocked.end > block.timestamp, "Delegatee lock expired");
        require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");
+       int128 value = locked_.amount;
        _delegate(delegatee, fromLocked, value, LockAction.UNDELEGATE);
        _delegate(_addr, toLocked, value, LockAction.DELEGATE);
    }
```

## 18. Reordering variables save gas
### Description
In the following struct if reorder the variables inside the struct can save 1 slot.

```diff
    struct LockedBalance {
        int128 amount;
+		int128 delegated;
        uint256 end;
-       int128 delegated;
        address delegatee;
    }
```
### Lines in the code
[VotingEscrow.sol#L75-L80](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L75-L80)
