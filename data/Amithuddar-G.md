1) ++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP


File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  309,38:         for (uint256 i = 0; i < 255; i++) {
  717,38:         for (uint256 i = 0; i < 128; i++) {
  739,38:         for (uint256 i = 0; i < 128; i++) {
  834,38:         for (uint256 i = 0; i < 255; i++) {
  
  
2)++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{++I} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS  

File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  309,38:         for (uint256 i = 0; i < 255; i++) {
  717,38:         for (uint256 i = 0; i < 128; i++) {
  739,38:         for (uint256 i = 0; i < 128; i++) {
  834,38:         for (uint256 i = 0; i < 255; i++) {
  
3)It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: for (uint256 i = 0; i < numIterations; ++i) { should be replaced with for (uint256 i; i < numIterations; ++i) {  



File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  309,38:         for (uint256 i = 0; i < 255; i++) {
  717,38:         for (uint256 i = 0; i < 128; i++) {
  739,38:         for (uint256 i = 0; i < 128; i++) {
  834,38:         for (uint256 i = 0; i < 255; i++) {
  
4)Use custom errors rather than revert()/require() strings to save gas

File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  116,9:         require(decimals <= 18, "Exceeds max decimals");
  125,9:         require(
                           !IBlocklist(blocklist).isBlocked(msg.sender),
            "Blocked contract"
        );  
  140,9:         require(msg.sender == owner, "Only owner");
  147,9:         require(msg.sender == owner, "Only owner");
  154,9:         require(msg.sender == owner, "Only owner");
  162,9:         require(msg.sender == owner, "Only owner");
  171,9:         require(msg.sender == blocklist, "Only Blocklist");
  412,9:         require(_value > 0, "Only non zero amount");
  413,9:         require(locked_.amount == 0, "Lock exists");
  414,9:         require(unlock_time >= locked_.end, "Only increase lock end"); // from using quitLock, user should increaseAmount instead
  415,9:         require(unlock_time > block.timestamp, "Only future lock end");
  416,9:         require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
  425,9:         require(
                           token.transferFrom(msg.sender, address(this), _value),
            "Transfer failed"
        );
  448,9:         require(_value > 0, "Only non zero amount");
  449,9:         require(locked_.amount > 0, "No lock");
  450,9:         require(locked_.end > block.timestamp, "Lock expired");
  469,13:             require(locked_.amount > 0, "Delegatee has no lock");
  470,13:             require(locked_.end > block.timestamp, "Delegatee lock expired");
  485,9:         require(
                           token.transferFrom(msg.sender, address(this), _value),
            "Transfer failed"
        );
  502,9:         require(locked_.amount > 0, "No lock");
  503,9:         require(unlock_time > locked_.end, "Only increase lock end");
  504,9:         require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
  511,13:             require(oldUnlockTime > block.timestamp, "Lock expired");
  529,9:         require(locked_.amount > 0, "No lock");
  530,9:         require(locked_.end <= block.timestamp, "Lock not expired");
  531,9:         require(locked_.delegatee == msg.sender, "Lock delegated");
  546,9:         require(token.transfer(msg.sender, value), "Transfer failed");
  563,9:         require(!IBlocklist(blocklist).isBlocked(_addr), "Blocked contract");
  564,9:         require(locked_.amount > 0, "No lock");
  565,9:         require(locked_.delegatee != _addr, "Already delegated");
  587,9:         require(toLocked.amount > 0, "Delegatee has no lock");
  588,9:         require(toLocked.end > block.timestamp, "Delegatee lock expired");
  589,9:         require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");
  635,9:         require(locked_.amount > 0, "No lock");
  636,9:         require(locked_.end > block.timestamp, "Lock expired");
  637,9:         require(locked_.delegatee == msg.sender, "Lock delegated");
  657,9:         require(token.transfer(msg.sender, remainingAmount), "Transfer failed");
  676,9:         require(token.transfer(penaltyRecipient, amount), "Transfer failed");
  776,9:         require(_blockNumber <= block.number, "Only past block number");
  877,9:         require(_blockNumber <= block.number, "Only past block number"); 

File: 2022-08-fiatdao\contracts\features\Blocklist.sol
  24,9:         require(msg.sender == manager, "Only manager");
  25,9:         require(_isContract(addr), "Only contracts");  
  
5)X = X + Y IS CHEAPER THAN X += Y
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  418,24:         locked_.amount += int128(int256(_value));
  420,27:         locked_.delegated += int128(int256(_value));
  460,30:             newLocked.amount += int128(int256(_value));
  461,33:             newLocked.delegated += int128(int256(_value));
  465,28:             locked_.amount += int128(int256(_value));
  472,33:             newLocked.delegated += int128(int256(_value));
  603,33:             newLocked.delegated += value;
  654,28:         penaltyAccumulated += penaltyAmount; 

6)Using private rather than public for constants, saves gas

If needed, the value can be read from the verified contract source code.
Savings are due to the compiler not having to create non-payable getter
functions for deployment calldata, and not adding another entry to the method ID table
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  46,12:     uint256 public constant WEEK = 7 days;
  47,12:     uint256 public constant MAXTIME = 365 days;
  48,12:     uint256 public constant MULTIPLIER = 10**18;  
  
7) USE A MORE RECENT VERSION OF SOLIDITY


Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

File: 2022-08-fiatdao\contracts\VotingEscrow.sol
  2,1: pragma solidity ^0.8.3; 

File: 2022-08-fiatdao\contracts\features\Blocklist.sol
  2,1: pragma solidity ^0.8.3; 

File: 2022-08-fiatdao\contracts\interfaces\IBlocklist.sol
  2,1: pragma solidity ^0.8.3;

File: 2022-08-fiatdao\contracts\interfaces\IERC20.sol
  2,1: pragma solidity ^0.8.3;  
  
File: 2022-08-fiatdao\contracts\interfaces\IVotingEscrow.sol
  2,1: pragma solidity ^0.8.3; 

8) USING > 0 COSTS MORE GAS THAN != 0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

This change saves 6 gas per instance


File: 2022-08-fiatdao\contracts\VotingEscrow.sol

  412,24:         require(_value > 0, "Only non zero amount");
  448,24:         require(_value > 0, "Only non zero amount");
  449,32:         require(locked_.amount > 0, "No lock");
  469,36:             require(locked_.amount > 0, "Delegatee has no lock");
  502,32:         require(locked_.amount > 0, "No lock");
  529,32:         require(locked_.amount > 0, "No lock");
  564,32:         require(locked_.amount > 0, "No lock");
  587,33:         require(toLocked.amount > 0, "Delegatee has no lock");
  635,32:         require(locked_.amount > 0, "No lock");  
  
