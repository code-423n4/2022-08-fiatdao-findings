
## Table of Contents

G-01 ++i costs less gas than i++, especially when it's used in for loops (4 instances)
G-02 ++i/i++ should be unchecked{++i}/unchecked{i++} (4 instances)
G-03 Comparison operators  (12 instances)
G-04 Use a more recent version of solidity (5 instances)
G-05 Using private rather than public for constants saves gas (3 instances)
G-06 calldata instead of memory for read-only function parameter (5 instances)
G-07 internal functions only called once can be inlined to save gas (7 instances)
G-08 Using > 0 costs more gas than != 0 when used on a uint in a require() statement (2 instances)
G-09 <x> += <y> costs more gas than <x> = <x> + <y> for state variables (6 instances)
G-10 Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead (9 instances)
G-11 Use custom errors rather than revert()/require() strings to save gas (39 instances)
G-12 It costs more gas to initialize variables to zero than to let the default of zero be applied (8 instances)
G-13 Multiplication/division by two should use bit shifting (2 instances)

There are 96 instances in total.


## G-01 ++i costs less gas than i++, especially when it's used in for loops  

There are 4 instances in 1 file:
File: contracts/VotingEscrow.sol

        for (uint256 i = 0; i < 255; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L309

        for (uint256 i = 0; i < 128; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

        for (uint256 i = 0; i < 128; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

        for (uint256 i = 0; i < 255; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834



## G-02 ++i/i++ should be unchecked{++i}/unchecked{i++}

when it is not possible for them to overflow, as is the case when used in for- and while-loops

There is 4 instances in 1 file:
File: contracts/VotingEscrow.sol

        for (uint256 i = 0; i < 255; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L309

        for (uint256 i = 0; i < 128; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L717

        for (uint256 i = 0; i < 128; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L739

        for (uint256 i = 0; i < 255; i++) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L834



## G-03 Comparison operators

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =.
Using strict comparison operators hence saves gas.

There are 12 instances in 1 file:
File: contracts/VotingEscrow.sol

        require(decimals <= 18, "Exceeds max decimals");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L116

        require(unlock_time >= locked_.end, "Only increase lock end"); 
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L414

       require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L416

 	require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L504

        require(locked_.end <= block.timestamp, "Lock not expired");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L530

        require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L589

            if (min >= max) break;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L718

            if (pointHistory[mid].blk <= _block) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L720
            if (min >= max) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L740

        require(_blockNumber <= block.number, "Only past block number");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L776

        if (upoint.bias >= 0) {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L814

        require(_blockNumber <= block.number, "Only past block number");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L877



## G-04 Use a more recent version of solidity

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



## G-05 Using private rather than public for constants saves gas
If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table.

There are 3 instances in 1 file:
File: contracts/VotingEscrow.sol

    uint256 public constant WEEK = 7 days;
    uint256 public constant MAXTIME = 365 days;
    uint256 public constant MULTIPLIER = 10**18;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L46



## G-06 calldata instead of memory for read-only function parameter
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. 
Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.
Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

There are 5 instances in 1 file:
File: contracts/VotingEscrow.sol

        string memory _name,
        string memory _symbol
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L104

        LockedBalance memory _oldLocked,
        LockedBalance memory _newLocked
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L224

        LockedBalance memory _locked,
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L597

    function _copyLock(LockedBalance memory _locked)
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L685

    function _supplyAt(Point memory _point, uint256 _t)
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L825



## G-07 internal functions only called once can be inlined to save gas
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

There are 7 instances in 1 file:
File: contracts/VotingEscrow.sol

    function _checkpoint(
        address _addr,
        LockedBalance memory _oldLocked,
        LockedBalance memory _newLocked
    ) internal {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L222-L226

    function _delegate(
        address addr,
        LockedBalance memory _locked,
        int128 value,
        LockAction action
    ) internal {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L595-L600

    function _calculatePenaltyRate(uint256 end)
        internal
        view
        returns (uint256)
    {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L662-L666

    function _copyLock(LockedBalance memory _locked)
        internal
        pure
        returns (LockedBalance memory)
    {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L685-L689

    function _findBlockEpoch(uint256 _block, uint256 _maxEpoch)
        internal
        view
        returns (uint256)
    {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L708-L712

    function _findUserBlockEpoch(address _addr, uint256 _block)
        internal
        view
        returns (uint256)
    {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L732-L736

    function _supplyAt(Point memory _point, uint256 _t)
        internal
        view
        returns (uint256)
    {
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L825-l829




## G-08 Using > 0 costs more gas than != 0 when used on a uint in a require() statement
This change saves 6 gas per instance.

There are 2 instances in 1 file:
File: contracts/VotingEscrow.sol
require(_value > 0, "Only non zero amount");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L412

require(_value > 0, "Only non zero amount");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L448





## G-09 <x> += <y> costs more gas than <x> = <x> + <y> for state variables

There are 6 instances in 1 file:
File: contracts/VotingEscrow.sol
        locked_.amount += int128(int256(_value));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L418

        locked_.delegated += int128(int256(_value));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L420

        newLocked.amount += int128(int256(_value));
        newLocked.delegated += int128(int256(_value));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L460

        newLocked.delegated += int128(int256(_value));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L472

        newLocked.delegated += value;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L603

        penaltyAccumulated += penaltyAmount;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L654


## G-10 Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. 
Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Use a larger size then downcast where needed

There are 9 instances in 2 files:
File: contracts/VotingEscrow.sol
        int128 bias;
        int128 slope;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L70

        int128 amount;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L76

        int128 bias,
        int128 slope,
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L205

        uint256 value = uint256(uint128(locked_.amount));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L533

        uint256 value = uint256(uint128(locked_.amount));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L639

        return uint256(uint128(lastPoint.bias));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L766

        return uint256(uint128(upoint.bias));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L815

        return uint256(uint128(lastPoint.bias));
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L860


contracts/interfaces/IERC20.sol
    function decimals() external view returns (uint8);
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/interfaces/IERC20.sol#L10



## G-11 Use custom errors rather than revert()/require() strings to save gas

There are 39 instances in 1 file:
File: contracts/VotingEscrow.sol
        require(decimals <= 18, "Exceeds max decimals");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L116

        require(
            !IBlocklist(blocklist).isBlocked(msg.sender),
            "Blocked contract"
        );
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L125

        require(msg.sender == owner, "Only owner");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L140

        require(msg.sender == owner, "Only owner");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L147

        require(msg.sender == owner, "Only owner");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L154

        require(msg.sender == owner, "Only owner");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L162

        require(msg.sender == blocklist, "Only Blocklist");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L171

        require(_value > 0, "Only non zero amount");
        require(locked_.amount == 0, "Lock exists");
        require(unlock_time >= locked_.end, "Only increase lock end"); // from using quitLock, user should increaseAmount instead
        require(unlock_time > block.timestamp, "Only future lock end");
        require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L412

        require(
            token.transferFrom(msg.sender, address(this), _value),
            "Transfer failed"
        );
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L425

        require(_value > 0, "Only non zero amount");
        require(locked_.amount > 0, "No lock");
        require(locked_.end > block.timestamp, "Lock expired");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L448

            require(locked_.amount > 0, "Delegatee has no lock");
            require(locked_.end > block.timestamp, "Delegatee lock expired");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L469

        require(
            token.transferFrom(msg.sender, address(this), _value),
            "Transfer failed"
        );
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L485

        require(locked_.amount > 0, "No lock");
        require(unlock_time > locked_.end, "Only increase lock end");
        require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L502

        require(locked_.amount > 0, "No lock");
        require(locked_.end <= block.timestamp, "Lock not expired");
        require(locked_.delegatee == msg.sender, "Lock delegated");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L529

        require(token.transfer(msg.sender, value), "Transfer failed");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L546

        require(!IBlocklist(blocklist).isBlocked(_addr), "Blocked contract");
        require(locked_.amount > 0, "No lock");
        require(locked_.delegatee != _addr, "Already delegated");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L563

        require(toLocked.amount > 0, "Delegatee has no lock");
        require(toLocked.end > block.timestamp, "Delegatee lock expired");
        require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L587

        require(locked_.amount > 0, "No lock");
        require(locked_.end > block.timestamp, "Lock expired");
        require(locked_.delegatee == msg.sender, "Lock delegated");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L635

        require(token.transfer(msg.sender, remainingAmount), "Transfer failed");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L657

        require(token.transfer(penaltyRecipient, amount), "Transfer failed");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L676

        require(_blockNumber <= block.number, "Only past block number");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L776

        require(_blockNumber <= block.number, "Only past block number");
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L877


G-12 It costs more gas to initialize variables to zero than to let the default of zero be applied

There are 8 instances in 1 file:
File: contracts/VotingEscrow.sol

        int128 oldSlopeDelta = 0;
        int128 newSlopeDelta = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L229

        uint256 blockSlope = 0; // dblock/dt
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L298

            int128 dSlope = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L313

        uint256 min = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L714

        uint256 min = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L737

        uint256 dBlock = 0;
        uint256 dTime = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L793

            int128 dSlope = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L836

        uint256 dTime = 0;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L889


G-13 Multiplication/division by two should use bit shifting

<x> * 2 is equivalent to <x> << 1 and <x> / 2 is the same as <x> >> 1. 
The MUL and DIV opcodes cost 5 gas, whereas SHL and SHR only cost 3 gas

There are 2 instances in 1 file:
File: contracts/VotingEscrow.sol

            uint256 mid = (min + max + 1) / 2;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L719

            uint256 mid = (min + max + 1) / 2;
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L743


