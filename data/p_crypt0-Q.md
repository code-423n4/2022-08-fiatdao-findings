-----
-----
# [Low] Blocklist.sol: not implementing IBlocklist.sol via the 'is' keyword.

## Lines of code

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L9
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L5


## Impact

Could lead to undesired behaviour when interacting with VotingEscrow.sol and specifically, with the 'checkBlocklist()' modifier.

Due to time constraints, I cannot test extent of the impact on this (and have rated a medium out of hesitance and to draw attention), but provided a list of affected areas below:

## Specific functionality potentially affected by this

checkBlocklist() modifier: https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L126

delegate(): https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L563

## Proof of Concept
Blocklist contract does not implement IBlocklist.sol as expected
````
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.3;


import { IVotingEscrow } from "../interfaces/IVotingEscrow.sol";


/// @title Blocklist Checker implementation.
/// @notice Checks if an address is blocklisted
/// @dev This is a basic implementation using a mapping for address => bool
contract Blocklist {
    mapping(address => bool) private _blocklist;
    address public manager;
    address public ve;


    constructor(address _manager, address _ve) {
        manager = _manager;
        ve = _ve;
    }

````

## Tools Used

VS code and conversing with @elnilz on discord to confirm that this is an oversight.

## Recommended Mitigation Steps


Add the import and the 'is' keyword as follows:

````
// SPDX-License-Identifier: Apache-2.0
pragma solidity ^0.8.3;


import { IVotingEscrow } from "../interfaces/IVotingEscrow.sol";
import {IBlocklist} from "../interfaces/IBlocklist.sol";

/// @title Blocklist Checker implementation.
/// @notice Checks if an address is blocklisted
/// @dev This is a basic implementation using a mapping for address => bool
contract Blocklist is IBlocklist {
    mapping(address => bool) private _blocklist;
    address public manager;
    address public ve;


    constructor(address _manager, address _ve) {
        manager = _manager;
        ve = _ve;
    }

````
------
-----
# [Low/Medium] VotingEscrow.sol: increaseUnlockTime() in VotingEscrow.sol incorrectly uses new unlock_time for oldLock.

## Lines of Code


1) 
-----

locked is set to locked[msg.sender]
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L499

-----
2)
-----

oldUnlockTime is set to locked_.end
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L506

-----
3)
-----

locked_.end is set to unlock_time (the function param)
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L507

-----
4)
-----

IF delegatee is msg.sender and so long as oldUnlockTime is greater than block.timestamp (not expired):

LockedBalance oldLocked = _copyLock(locked_)
old_locked.end=unlock_time

checkpoint(msg.sender, oldLocked, locked_)

https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L511-L514

-----

## Impact

Any logic using oldLock could be impacted by this bug, since the oldLock.end would be incorrect.

Owing to a lack of time to find a potential for loss of funds through this logic error in the current contract, however, with respect to the potential for knock-on effects of biases and slopes being calculated incorrectly for oldLock as a result of this bug, I have labelled it as a 'low/medium' severity bug.

Additionally, balanceOfAt() and totalSupplyAt() functions would be affected by this bug.

Any contracts interacting with VotingEscrow.sol, unaware of this bug but performing calculations or logic functions with a checkpoint, would also be incorrect as it stands.

## Tools used

Visual studio code and confirming with @elnilz, who kindly provided the spec (https://github.com/code-423n4/2022-08-fiatdao/blob/main/CheckpointMath.md#increaseunlocktime) to confirm this is indeed a bug.

-----
-----
# [Low] VotingEscrow.sol: updateBlocklist() no check for owner being added to blocklist (assuming owner is a contract)

## Issue
Owner could be added to blocklist by calling updateBlocklist(owner) assuming the owner is not an EOA

## Mitigation plan
Add a requirement that address being added to blocklist is not owner

````
require(addr != msg.sender, "owner would be blocked!")
````
## Link
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L148

-----
-----
# [Low] VotingEscrow.sol: updatePenaltyRecipient() does not check for param zero address.

## Impact
The penalty recipient could be set to zero address, meaning that any penalties that ought to be received could be lost.

## Affected code
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L155

## Mitigation plan
Create a default penalty recipient, which the owner can control.

Alternatively, add a:

````
require(addr != address(0), "Penalty would be lost")
````
such that the transaction fails and penalties don't get lost.