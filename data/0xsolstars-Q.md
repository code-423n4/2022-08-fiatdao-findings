
Unfortunately, we did quite have the bandwidth to attack this with more detail but this appeared to be a mostly well-written repository!

Issue #1 :
It's best practice to not have commented code in production code base. Remove commented code in test case or add relevant uncommented code.

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/test/votingEscrowTest.ts#L565-L567

Issue #2:
Missing zero address checks in constructor as well as in a number of admin functions. A zero-address check is a best security practice in initializations and contractors. There are several places where a zero address check is missing. Here are a few examples: 

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L15-L16
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L120-L121
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L107
