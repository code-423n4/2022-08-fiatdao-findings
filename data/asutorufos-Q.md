N-1 Event is Missing `Indexed` Fields 
Each `event` should three `indexed` fields if there are three or more firleds
[VotingEscrow.sol L#24-31](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=event%20Deposit(,)%3B)
[VotingEscrow.sol L#32-36](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=event%20Withdraw(,)%3B)

N-2 Natspec is incomplete 
missing '@return'
[VotingEscrow.sol L#708](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=function%20_findBlockEpoch(uint256%20_block%2C%20uint256%20_maxEpoch))

[VotingEscrow.sol L#731](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#:~:text=which%20to%20search-,//%20%40param%20_block%20Find%20the%20most%20recent%20point%20history%20before%20this%20block,function%20_findUserBlockEpoch(address%20_addr%2C%20uint256%20_block),-internal)


