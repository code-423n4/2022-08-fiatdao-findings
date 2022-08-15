## Table of Contents:
L-01 approve() should be replaced with safeincreaseallowance() / safedecreaseallowance()
L-02 decimals() should be replaced with Safe decimals()

N-01 Use a more recent version of solidity
N-02 File is missing NatSpec
N-03 Event is missing indexed fields
N-04 Large multiple of 10 should use scientific notation





## L-01 approve should be replaced with safeincreaseallowance() / safedecreaseallowance()

There is 1 instance in 1 file:
File: contracts/interfaces/IERC20.sol
    function approve(address spender, uint256 amount) external returns (bool);
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IERC20.sol#L21


## L-02 decimals() should be replaced with Safe decimals()

not all ERC20 contracts define decimals() since it’s optional in the spec. Use safeDecimals() instead.

There is 1 instance: 
File: contracts/VotingEscrow.sol
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L115



## N-01 Use a more recent version of solidity

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

There are 5 instances in 5 files: 
File: contracts/VotingEscrow.sol
	pragma solidity ^0.8.3;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L2

File: contracts/interfaces/IBlocklist.sol
	pragma solidity ^0.8.3;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IBlocklist.sol#L2

File: contracts/interfaces/IVotingEscrow.sol
	pragma solidity ^0.8.3;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IVotingEscrow.sol#L2

File: contracts/interfaces/IERC20.sol
	pragma solidity ^0.8.3;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IERC20.sol#L2

File: contracts/interfaces/IBlocklist.sol
	pragma solidity >=0.8.4;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IBlocklist.sol#L2


## N-02 File is missing NatSpec

Instance
File: contracts/interfaces/IERC20.sol


## N-03 Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

There 3 instances in 2 files:
File: contracts/VotingEscrow.sol
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L25-L31
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L32-L37

File: contracts/interfaces/IERC20.sol
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IERC20.sol#L30-L34



## N-04 Large multiple of 10 should use scientific notation (eg 1e6) rather than decimat literals (eg 10000000, for readability

Instances include:
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L57
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L58



