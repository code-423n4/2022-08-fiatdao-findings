### [G-01] Don't Initialize Variables with Default Value

#### Impact
Uninitialized variables are assigned with the types default value.

Explicitly initializing a variable with it's default value costs unnecesary gas.

#### Findings:
```
contracts/VotingEscrow.sol::298 => uint256 blockSlope = 0; // dblock/dt
contracts/VotingEscrow.sol::309 => for (uint256 i = 0; i < 255; i++) {
contracts/VotingEscrow.sol::714 => uint256 min = 0;
contracts/VotingEscrow.sol::717 => for (uint256 i = 0; i < 128; i++) {
contracts/VotingEscrow.sol::737 => uint256 min = 0;
contracts/VotingEscrow.sol::739 => for (uint256 i = 0; i < 128; i++) {
contracts/VotingEscrow.sol::793 => uint256 dBlock = 0;
contracts/VotingEscrow.sol::794 => uint256 dTime = 0;
contracts/VotingEscrow.sol::834 => for (uint256 i = 0; i < 255; i++) {
contracts/VotingEscrow.sol::889 => uint256 dTime = 0;
```


### [G-02] Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.


#### Findings:
```
contracts/VotingEscrow.sol::412 => require(_value > 0, "Only non zero amount");
contracts/VotingEscrow.sol::448 => require(_value > 0, "Only non zero amount");
contracts/VotingEscrow.sol::449 => require(locked_.amount > 0, "No lock");
contracts/VotingEscrow.sol::469 => require(locked_.amount > 0, "Delegatee has no lock");
contracts/VotingEscrow.sol::502 => require(locked_.amount > 0, "No lock");
contracts/VotingEscrow.sol::529 => require(locked_.amount > 0, "No lock");
contracts/VotingEscrow.sol::564 => require(locked_.amount > 0, "No lock");
contracts/VotingEscrow.sol::587 => require(toLocked.amount > 0, "Delegatee has no lock");
contracts/VotingEscrow.sol::635 => require(locked_.amount > 0, "No lock");
```


### [G-03] Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

#### Findings:
```
contracts/VotingEscrow.sol::719 => uint256 mid = (min + max + 1) / 2;
contracts/VotingEscrow.sol::743 => uint256 mid = (min + max + 1) / 2;
```
