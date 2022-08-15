### Intializing the variables to zero that aren't `constant` or `immutable` will cost more gas rather than use default value of zero.

If you not overwritte the default value you will save 8 gas for stack variables and more for storage and memory variables.

_There are **10** instances of this issue:_

```solidity
File: ./contracts/VotingEscrow.sol

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

### If you use `++i` should be `unchecked{++i}` in `for`-loops

Not using a `unchecked{++i} will cost more gas because the default compiler overflow and underflow safety checks. This is true from version 0.8.0, code below match that requirments.

_There are **4** instances of this issue:_

```solidity
File: ./contracts/VotingEscrow.sol

309: for (uint256 i = 0; i < 255; i++) {

717: for (uint256 i = 0; i < 128; i++) {

739: for (uint256 i = 0; i < 128; i++) {

834: for (uint256 i = 0; i < 255; i++) {
```

### You can use `++i` instead of `i++` to save a gas (same for `--i`/`i--`)

This will save you 6 gas per instance/loop

_There are **4** instances of this issue:_

```solidity
File: ./contracts/VotingEscrow.sol

309: for (uint256 i = 0; i < 255; i++) {

717: for (uint256 i = 0; i < 128; i++) {

739: for (uint256 i = 0; i < 128; i++) {

834: for (uint256 i = 0; i < 255; i++) {
```

### `x = x + y` will be more cheap rather than `x += y` for state variables.

_There are **1** instances of this issue:_

```solidity
File: ./contracts/VotingEscrow

654:    penaltyAccumulated += penaltyAmount;
```

### Use `uint`s/`int`s that aren't 256 bits may cost more gas because of EVM.

EVM operates on 256 bits at the time and to use small varibles than 256 bits, they will need to resize and that may cost more gas.

_There are **2** instances of this issue:_

```solidity
File: ./contracts/VotingEscrow

567:    int128 value = locked_.amount;

836:    int128 dSlope = 0;
```

### Use Shift Right/Left instead of Division/Multiplication if possible Using of bit shifting is more cheap rather than normal Multiplication/Division

Use bit shifting will be may harder to code read rather than normal multiplication/division but it will save some gas. In the EVM `MUl`/`DIV` cost 5 gas rather than `SHL`/`SHR` that costs 3 gas.

_There are **2** instances of this issue:_

```solidity
File: ./contracts/VotingEscrow

719:    uint256 mid = (min + max + 1) / 2;

743:    uint256 mid = (min + max + 1) / 2;
```

### Ordering righ struct will save a slot and instead use 4 you will use 3

Instead of this you can order struct diffrent to save a slot

_There are **1** instances of this issue:_

```solidity
75:    struct LockedBalance {

76:        int128 amount;

77:        uint256 end;

78:        int128 delegated;

79:        address delegatee;

80:    }
```

Order like this will save one slot also and gas.

```solidity
75:    struct LockedBalance {

76:        int128 amount;

77:        int128 delegated;

78:        uint256 end;

79:        address delegatee;

80:    }
```