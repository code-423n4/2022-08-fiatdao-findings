G-1 USING `PRIVATE` RATHER THAN `PUBLIC` FOR `CONSTANTS`, SAVES GAS
[VotingEscrow.sol L#46-48](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=uint256%20public%20constant,**18%3B)

G-2 MULTIPLE `ADDRESS` `MAPPINGS` CAN BE COMBINED INTO A SINGLE `MAPPING` OF AN `ADDRESS` TO A `STRUCT`, WHERE APPROPRIATE
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.
[VotingEscrow.sol L#58-59,61](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=mapping(address,LockedBalance)%20public%20locked%3B)

G-3 IT COSTS MORE GAS TO INITIALIZE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED
[VotingEscrow.sol L#229-230](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=int128%20oldSlopeDelta%20%3D,%3D%200%3B)
[VotingEscrow.sol L#309](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=_floorToWeek(lastCheckpoint)%3B-,for%20(uint256%20i%20%3D%200%3B%20i%20%3C%20255%3B%20i%2B%2B)%20%7B,-//%20Hopefully%20it%20won%27t)
[VotingEscrow.sol L#704](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=//%20Binary%20search-,uint256%20min%20%3D%200%3B,-uint256%20max%20%3D)