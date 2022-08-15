# [G-01] Prefix increment costs less gas than postfix increment

There are 4 instances of this issue.

```
File: contracts/VotingEscrow.sol
309: for (uint256 i = 0; i < 255; i++) {
717: for (uint256 i = 0; i < 128; i++) {
739: for (uint256 i = 0; i < 128; i++) {
834: for (uint256 i = 0; i < 255; i++) {
```

# [G-02] The increment in for loop post condition can be made unchecked to save gas

There are 4 instances of this issue.

```
File: contracts/VotingEscrow.sol
309: for (uint256 i = 0; i < 255; i++) {
717: for (uint256 i = 0; i < 128; i++) {
739: for (uint256 i = 0; i < 128; i++) {
834: for (uint256 i = 0; i < 255; i++) {
```

# [G-03] Initializing a variable with the default value wastes gas

There are 10 instances of this issue.

```
File: contracts/VotingEscrow.sol
298: uint256 blockSlope = 0; // dblock/dt
309: for (uint256 i = 0; i < 255; i++) {
714: uint256 min = 0;
717: for (uint256 i = 0; i < 128; i++) {
737: uint256 min = 0;
739: for (uint256 i = 0; i < 128; i++) {
793: uint256 dBlock = 0;
794: uint256 dTime = 0;
834: for (uint256 i = 0; i < 255; i++) {
889: uint256 dTime = 0;
```

# [G-04] Use != 0 instead of > 0 to save gas.

Replace `> 0` with `!= 0` for unsigned integers.

```
File: contracts/VotingEscrow.sol
288: if (epoch > 0) {
412: require(_value > 0, "Only non zero amount");
448: require(_value > 0, "Only non zero amount");
```

```
File: contracts/features/Blocklist.sol
42: return size > 0;
```

# [G-05] Use a more revent version of solidity and replace require/revert strings with custom errors 

Custom errors are available in solidity starting from version 8.0.4 and provide a more gas efficient method to execute error handling.

There are 39 instances of this issue.

```
File: contracts/VotingEscrow.sol
116: require(decimals <= 18, "Exceeds max decimals");
140: require(msg.sender == owner, "Only owner");
147: require(msg.sender == owner, "Only owner");
154: require(msg.sender == owner, "Only owner");
162: require(msg.sender == owner, "Only owner");
171: require(msg.sender == blocklist, "Only Blocklist");
412: require(_value > 0, "Only non zero amount");
413: require(locked_.amount == 0, "Lock exists");
414: require(unlock_time >= locked_.end, "Only increase lock end"); // from using quitLock, user should increaseAmount instead
415: require(unlock_time > block.timestamp, "Only future lock end");
416: require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
448: require(_value > 0, "Only non zero amount");
449: require(locked_.amount > 0, "No lock");
450: require(locked_.end > block.timestamp, "Lock expired");
469: require(locked_.amount > 0, "Delegatee has no lock");
470: require(locked_.end > block.timestamp, "Delegatee lock expired");
502: require(locked_.amount > 0, "No lock");
503: require(unlock_time > locked_.end, "Only increase lock end");
504: require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
511: require(oldUnlockTime > block.timestamp, "Lock expired");
529: require(locked_.amount > 0, "No lock");
530: require(locked_.end <= block.timestamp, "Lock not expired");
531: require(locked_.delegatee == msg.sender, "Lock delegated");
546: require(token.transfer(msg.sender, value), "Transfer failed");
563: require(!IBlocklist(blocklist).isBlocked(_addr), "Blocked contract");
564: require(locked_.amount > 0, "No lock");
565: require(locked_.delegatee != _addr, "Already delegated");
587: require(toLocked.amount > 0, "Delegatee has no lock");
588: require(toLocked.end > block.timestamp, "Delegatee lock expired");
589: require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");
635: require(locked_.amount > 0, "No lock");
636: require(locked_.end > block.timestamp, "Lock expired");
637: require(locked_.delegatee == msg.sender, "Lock delegated");
657: require(token.transfer(msg.sender, remainingAmount), "Transfer failed");
676: require(token.transfer(penaltyRecipient, amount), "Transfer failed");
776: require(_blockNumber <= block.number, "Only past block number");
877: require(_blockNumber <= block.number, "Only past block number");
```

```
File: contracts/features/Blocklist.sol
24: require(msg.sender == manager, "Only manager");
25: require(_isContract(addr), "Only contracts");
```

# [G-06] Use right/left shift instead of division/multiplication to save gas

There are 2 instances of this issue.

```
File: contracts/VotingEscrow.sol
719: uint256 mid = (min + max + 1) / 2;
743: uint256 mid = (min + max + 1) / 2;
```

# [G-07] Repeated require statements should be refactored into a modifier

The logic `msg.sender == owner` can be refactor into a single modifier.

```
File: contracts/VotingEscrow.sol
function transferOwnership(address _addr) external {
    require(msg.sender == owner, "Only owner");
    owner = _addr;
    emit TransferOwnership(_addr);
}

/// @notice Updates the blocklist contract
function updateBlocklist(address _addr) external {
    require(msg.sender == owner, "Only owner");
    blocklist = _addr;
    emit UpdateBlocklist(_addr);
}

/// @notice Updates the recipient of the accumulated penalty paid by quitters
function updatePenaltyRecipient(address _addr) external {
    require(msg.sender == owner, "Only owner");
    penaltyRecipient = _addr;
    emit UpdatePenaltyRecipient(_addr);
}

/// @notice Removes quitlock penalty by setting it to zero
/// @dev This is an irreversible action
function unlock() external {
    require(msg.sender == owner, "Only owner");
    maxPenalty = 0;
    emit Unlock();
}
```

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L139-L165

## [G-08] x += y costs more gas than x = x + y for state variables

There 1 instance with this issue.

```
File: contracts/VotingEscrow.sol
654: penaltyAccumulated += penaltyAmount;
```

## [G-09] Using private rather than public for constants, saves gas

If needed, the values can be inspected on the source code.

```
46: uint256 public constant WEEK = 7 days;
47: uint256 public constant MAXTIME = 365 days;
48: uint256 public constant MULTIPLIER = 10**18;
```