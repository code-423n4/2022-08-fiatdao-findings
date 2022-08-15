## FINDINGS

## Using immutable on variables that are only set in the constructor and never after 
Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

File: Blocklist.sol [Line 11](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L11)
```solidity
    address public manager;
```

File: Blocklist.sol [Line 12](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L12)
```solidity
    address public ve;
```

File: VotingEscrow.sol [Line 45](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L45)
```solidity
    IERC20 public token;
```

## Cache storage values in memory to minimize SLOADs
The code can be optimized by minimizing the number of SLOADs.

SLOADs are expensive (100 gas after the 1st one) compared to MLOADs/MSTOREs (3 gas each). Storage values read multiple times should instead be cached in memory the first time (costing 1 SLOAD) and then read from this cache to avoid multiple SLOADs.

File: VotingEscrow.sol [Line 673-678](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L673-L678)
### VotingEscrow.sol.collectPenalty(): penaltyRecipient should be cached(Saves 1 sload : ~94 gas)
```solidity
    function collectPenalty() external {
        uint256 amount = penaltyAccumulated;
        penaltyAccumulated = 0;
        require(token.transfer(penaltyRecipient, amount), "Transfer failed");
        emit CollectPenalty(amount, penaltyRecipient);
    }
```

SLOAD 1: [Line 676](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L676)
SLOAD 2: [Line 677](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L677)

## Pack structs by putting data types in ascending size

When defining a struct, pack the values so that the data types are in ascending order. This will make sure that data types that can be put into the same slot are packed together instead of each variable having a separate storage slot.

**Affected code**
File: VotingEscrow.sol [Line 75-80](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L75-L80)
```solidity
    struct LockedBalance {
        int128 amount;
        uint256 end;
        int128 delegated;
        address delegatee;
    }
```

The above should be modified to:
```solidity
    struct LockedBalance {
        int128 amount;
        int128 delegated;
        uint256 end; 
        address delegatee;
    }
```

## Using unchecked blocks to save gas
Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block
[see resource](https://github.com/ethereum/solidity/issues/10695)

File: VotingEscrow.sol [Line 302](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L302)
```solidity
                (block.timestamp - lastPoint.ts);
```

The above operation cannot underflow due to the check on [Line 299](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L299) which ensures that `block.timestamp` is greater than `lastPoint.ts`

## Using unchecked blocks to save gas - Increments in for loop can be unchecked  ( save 30-40 gas per loop iteration)
The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop . eg.

e.g Let's work with a sample loop below.

```solidity
for(uint256 i; i < 10; i++){
//doSomething
}

```
can be written as shown below.
```solidity
for(uint256 i; i < 10;) {
  // loop logic
  unchecked { i++; }
}
```

We can also write  it as an inlined function like below.

```solidity
function inc(i) internal pure returns (uint256) {
  unchecked { return i + 1; }
}
for(uint256 i; i < 10; i = inc(i)) {
  // doSomething
}
```

**Affected code**

File:VotingEscrow.sol [Line 309](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309)
```solidity
        for (uint256 i = 0; i < 255; i++) {
```

**Other Instances to modify**
File:VotingEscrow.sol [Line 717](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L717)
```solidity
        for (uint256 i = 0; i < 128; i++) {
```

File:VotingEscrow.sol [Line 739](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L739)
```solidity
        for (uint256 i = 0; i < 128; i++) {
```

File: VotingEscrow.sol [Line 834](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L834)
```solidity
        for (uint256 i = 0; i < 255; i++) {
```

[see resource](https://github.com/ethereum/solidity/issues/10695)

## Internal/Private functions only called once can be inlined to save gas
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

Affected code:
File: VotingEscrow.sol [Line 662-669](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L662-L669)
```solidity
    function _calculatePenaltyRate(uint256 end)
        internal
        view
        returns (uint256)
    {
        // We know that end > block.timestamp because expired locks cannot be quitted
        return ((end - block.timestamp) * maxPenalty) / MAXTIME;
    }
```

The above function is only used once on File: VotingEscrow.sol at  [Line 652](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L652)

File: Blocklist.sol [Line 37-43](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L37-L43)
```solidity
    function _isContract(address addr) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
    }
```

## ++i costs less gas compared to i++ or i += 1  (~5 gas per iteration)

++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

i++ increments i and returns the initial value of i. Which means:

```solidity
uint i = 1;  
i++; // == 1 but i == 2  
```

But ++i returns the actual incremented value:

```solidity
uint i = 1;  
++i; // == 2 and i == 2 too, so no need for a temporary variable  
```

In the first case, the compiler has to create a temporary variable (when used) for returning 1 instead of 2

Instances include:
File:VotingEscrow.sol [Line 309](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309)
```solidity
        for (uint256 i = 0; i < 255; i++) {
```

File:VotingEscrow.sol [Line 717](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L717)
```solidity
        for (uint256 i = 0; i < 128; i++) {
```

File:VotingEscrow.sol [Line 739](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L739)
```solidity
        for (uint256 i = 0; i < 128; i++) {
```

File: VotingEscrow.sol [Line 834](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L834)
```solidity
        for (uint256 i = 0; i < 255; i++) {
```

## Comparisons: != is more efficient than > in require (6 gas less)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas.

Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you're in a require statement, this will save gas. You can see this tweet for more proofs: https://twitter.com/gzeon/status/1485428085885640706

I suggest changing > 0 with != 0 here:

File: VotingEscrow.sol [Line 412](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L412)
```solidity
        require(_value > 0, "Only non zero amount");
```

File:VotingEscrow.sol [Line 448](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L448)
```solidity
        require(_value > 0, "Only non zero amount");
```


## Using bools for storage incurs overhead
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) 
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

**Instances affected include**
File: Blocklist.sol [Line 10](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L10)
```solidity
    mapping(address => bool) private _blocklist;
```

## Use Shift Right/Left instead of Division/Multiplication
A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

[relevant source](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)

File: VotingEscrow.sol [Line 719](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L719)
```solidity
            uint256 mid = (min + max + 1) / 2;
```

File:VotingEscrow.sol [Line 743](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L743)
```solidity
            uint256 mid = (min + max + 1) / 2;
```

## x += y costs more gas than x = x + y for state variables

File:VotingEscrow.sol [Line 654](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L654)
```solidity
        penaltyAccumulated += penaltyAmount;
```

**penaltyAccumulated** is a state variable as such we should modify the above as 

```solidity
        penaltyAccumulated = penaltyAccumulated + penaltyAmount;
```

## Duplicated require()/revert() checks should be refactored to a modifier or function
This saves deployement gas

File: VotingEscrow.sol [Line 140](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L140)
```solidity
        require(msg.sender == owner, "Only owner");
```
The above require statement is repeated 3 more times in the following lines
File: VotingEscrow.sol [Line 147](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L147)
File: VotingEscrow.sol [Line 154](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L154)
File: VotingEscrow.sol [Line 162](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L162)

## Use Custom Errors instead of Revert Strings to save Gas(~50 gas)
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)
Custom errors save ~50 gas each time they’re hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).
see [Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)

File: Blocklist.sol [Line 24](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L24)
```solidity
        require(msg.sender == manager, "Only manager");
```

File: Blocklist.sol [Line 25](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L25)
```solidity
        require(_isContract(addr), "Only contracts");
```

File: VotingEscrow.sol [Line 116](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L116)
```solidity
        require(decimals <= 18, "Exceeds max decimals");
```

File: VotingEscrow.sol [Line 125-128](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L125-L128)
```solidity
        require(
            !IBlocklist(blocklist).isBlocked(msg.sender),
            "Blocked contract"
        );
```

File: VotingEscrow.sol [Line 140](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L140)
```solidity
        require(msg.sender == owner, "Only owner");
```

File: VotingEscrow.sol [Line 171](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L171)
```solidity
        require(msg.sender == blocklist, "Only Blocklist");
```

File: VotingEscrow.sol [Line 412](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L412)
```solidity
        require(_value > 0, "Only non zero amount");
```

File:VotingEscrow.sol [Line 413-416](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L413-L416)
```solidity
        require(locked_.amount == 0, "Lock exists");
        require(unlock_time >= locked_.end, "Only increase lock end"); // from using quitLock, user should increaseAmount instead
        require(unlock_time > block.timestamp, "Only future lock end");
        require(unlock_time <= block.timestamp + MAXTIME, "Exceeds maxtime");
```

File:VotingEscrow.sol [Line 425-428](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L425-L428)
```solidity
        require(
            token.transferFrom(msg.sender, address(this), _value),
            "Transfer failed"
        );
```

File:VotingEscrow.sol [Line 448](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L448)
```solidity
        require(_value > 0, "Only non zero amount");
```

File:VotingEscrow.sol [Line 449-450](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L449-L450)
```solidity
        require(locked_.amount > 0, "No lock");
        require(locked_.end > block.timestamp, "Lock expired");
```

File: VotingEscrow.sol [Line 469-470](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L469-L470)
```solidity
            require(locked_.amount > 0, "Delegatee has no lock");
            require(locked_.end > block.timestamp, "Delegatee lock expired");
```

File:VotingEscrow.sol [Line 485](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L485-L488)
```solidity
        require(
            token.transferFrom(msg.sender, address(this), _value),
            "Transfer failed"
        );
```

File: VotingEscrow.sol [Lines 502-504](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L502-L504)
File: VotingEscrow.sol [Line 511](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L511)
File: VotingEscrow.sol [Line 529,530,531](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L529-L531)
File: VotingEscrow.sol [Line 546](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L546)
File: VotingEscrow.sol [Line 563,564,565](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L563-L565)
File: VotingEscrow.sol [Line 587,588,589](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L587-L589)
File: VotingEscrow.sol [Line 635,636,637](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L635-L637)
File: VotingEscrow.sol [Line 657](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L657)
File: VotingEscrow.sol [Line 676](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L676)
File: VotingEscrow.sol [Line 776](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L776)

**Proof of custom errors optimizations**

```
contract GasTest is DSTest {
    Contract0 c0;
    Contract1 c1;

    function setUp() public {
        c0 = new Contract0();
        c1 = new Contract1();
    }

    function testFailGas() public {
        c0.stringErrorMessage();
        c1.customErrorMessage();
    }
}

contract Contract0 {
    function stringErrorMessage() public {
        bool check = false;
        require(check, "error message");
    }
}

contract Contract1 {
    error CustomError();

    function customErrorMessage() public {
        bool check = false;
        if (!check) {
            revert CustomError();
        }
    }
}
```

**Gas comparisons**
```
╭────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ Contract0 contract ┆                 ┆     ┆        ┆     ┆         │
╞════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost    ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 34087              ┆ 200             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name      ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ stringErrorMessage ┆ 218             ┆ 218 ┆ 218    ┆ 218 ┆ 1       │
╰────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
╭────────────────────┬─────────────────┬─────┬────────┬─────┬─────────╮
│ Contract1 contract ┆                 ┆     ┆        ┆     ┆         │
╞════════════════════╪═════════════════╪═════╪════════╪═════╪═════════╡
│ Deployment Cost    ┆ Deployment Size ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ 26881              ┆ 164             ┆     ┆        ┆     ┆         │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ Function Name      ┆ min             ┆ avg ┆ median ┆ max ┆ # calls │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌┼╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ customErrorMessage ┆ 161             ┆ 161 ┆ 161    ┆ 161 ┆ 1       │
╰────────────────────┴─────────────────┴─────┴────────┴─────┴─────────╯
```