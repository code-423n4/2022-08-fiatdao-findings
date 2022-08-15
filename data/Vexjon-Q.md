## Impact
When checking for onlyOwner conditions it's a good practice to create an onlyOwner modifier and not duplicate the code. It also makes the code simpler to read.

## Proof of Concept
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L140
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L154
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L162

## Tools Used
Manual analysis

## Recommended Mitigation Steps
Create an onlyOwner modifier and apply it to above functions