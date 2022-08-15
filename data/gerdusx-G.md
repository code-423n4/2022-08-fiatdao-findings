# Gas Optimazations
# [G-01] Use prefix not postfix in loops 
Using a prefix increment (++i) instead of a postfix increment (i++) saves gas for each loop cycle and so can have a big gas impact when the loop executes on a large number of elements.


There are 4 occurrences

**VotingEscrow.sol**
[L309](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L309) `        for (uint256 i = 0; i < 255; i++) {`
[L717](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717) `        for (uint256 i = 0; i < 128; i++) {`
[L739](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739) `        for (uint256 i = 0; i < 128; i++) {`
[L834](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834) `        for (uint256 i = 0; i < 255; i++) {`


#### Recommended Mitigation Steps
Use prefix not postfix to increment in a loop




# [G-02] Use Custom Errors instead of revert()/require() 
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)


There are 38 occurrences

**Blocklist.sol**
[L24](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/features/Blocklist.sol#L24) `        require(msg.sender == manager, "Only manager");`
[L25](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/features/Blocklist.sol#L25) `        require(_isContract(addr), "Only contracts");`

**VotingEscrow.sol**
[L116](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L116) `        require(decimals <= 18, "Exceeds max decimals");`
[L140](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L140) `        require(msg.sender == owner, "Only owner");`
[L147](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L147) `        require(msg.sender == owner, "Only owner");`
[L154](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L154) `        require(msg.sender == owner, "Only owner");`
[L162](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L162) `        require(msg.sender == owner, "Only owner");`
[L171](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L171) `        require(msg.sender == blocklist, "Only Blocklist");`
[L412](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L412) `        require(_value > 0, "Only non zero amount");`
[L413](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L413) `        require(locked_.amount == 0, "Lock exists");`
[L414](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L414) `        require(unlock_time >= locked_.end, "Only increase lock end"); // from using quitLock, user should increaseAmount instead`
[L415](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L415) `        require(unlock_time > block.timestamp, "Only future lock end");`
[L416](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L416) `        require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");`
[L425](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L425) `        require(`
[L448](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L448) `        require(_value > 0, "Only non zero amount");`
[L449](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L449) `        require(locked_.amount > 0, "No lock");`
[L450](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L450) `        require(locked_.end > block.timestamp, "Lock expired");`
[L485](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L485) `        require(`
[L502](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L502) `        require(locked_.amount > 0, "No lock");`
[L503](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L503) `        require(unlock_time > locked_.end, "Only increase lock end");`
[L504](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L504) `        require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");`
[L529](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L529) `        require(locked_.amount > 0, "No lock");`
[L530](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L530) `        require(locked_.end <= block.timestamp, "Lock not expired");`
[L531](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L531) `        require(locked_.delegatee == msg.sender, "Lock delegated");`
[L546](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L546) `        require(token.transfer(msg.sender, value), "Transfer failed");`
[L563](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L563) `        require(!IBlocklist(blocklist).isBlocked(_addr), "Blocked contract");`
[L564](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L564) `        require(locked_.amount > 0, "No lock");`
[L565](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L565) `        require(locked_.delegatee != _addr, "Already delegated");`
[L587](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L587) `        require(toLocked.amount > 0, "Delegatee has no lock");`
[L588](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L588) `        require(toLocked.end > block.timestamp, "Delegatee lock expired");`
[L589](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L589) `        require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");`
[L635](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L635) `        require(locked_.amount > 0, "No lock");`
[L636](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L636) `        require(locked_.end > block.timestamp, "Lock expired");`
[L637](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L637) `        require(locked_.delegatee == msg.sender, "Lock delegated");`
[L657](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L657) `        require(token.transfer(msg.sender, remainingAmount), "Transfer failed");`
[L676](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L676) `        require(token.transfer(penaltyRecipient, amount), "Transfer failed");`
[L776](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L776) `        require(_blockNumber <= block.number, "Only past block number");`
[L877](https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L877) `        require(_blockNumber <= block.number, "Only past block number");`


#### Recommended Mitigation Steps
Recommended to replace revert strings with custom errors.




