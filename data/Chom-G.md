## Add unchecked

![image](https://user-images.githubusercontent.com/4103490/184679678-c05b2d7a-6fec-40e3-92da-56246ba609bd.png)

## For loop postcondition can be made unchecked and using ++i is better than i++

This reduce gas cost as show here https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5

Caching the length in for loops:
1. if it is a storage array, this is an extra sload operation (100 additional extra gas (EIP-2929 2) for each iteration except for the first),
2. if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first),
3. if it is a calldata array, this is an extra calldataload operation (3 additional gas for each iteration except for the first)

for loop postcondition can be made unchecked `Gas savings: roughly speaking this can save 30-40 gas per loop iteration. For lengthy loops, this can be significant!`

![image](https://user-images.githubusercontent.com/4103490/184679613-b8875aee-7c6b-40e2-8353-8cbf6418568e.png)

![image](https://user-images.githubusercontent.com/4103490/184679712-d6fe5e53-9d73-40cd-bfa5-453220d752a0.png)

## Optimization result when apply above two

### Before
![image](https://user-images.githubusercontent.com/4103490/184679078-ebbfd102-33dc-4f44-a938-f40217d8daf7.png)

### After
![image](https://user-images.githubusercontent.com/4103490/184679482-8d2c37b7-f3f0-46e5-98de-f9bcbd4262f2.png)

## Consider using custom errors instead of revert strings

This reduce gas cost as show here https://forum.openzeppelin.com/t/a-collection-of-gas-optimisation-tricks/19966/5

`Solidity 0.8.4 introduced custom errors. They are more gas efficient than revert strings, when it comes to deployment cost as well as runtime cost when the revert condition is met. Use custom errors instead of revert strings for gas savings.`

Any require statement in your code can be replaced with custom error for example,

```
require(locked_.amount > 0, "No lock");
```

Can be replaced with

```
// declare error before contract declaration
error NoLock();

if (locked_.amount == 0) revert NoLock();
```