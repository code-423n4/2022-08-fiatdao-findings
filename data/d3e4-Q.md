All in VotingEscrow.sol

**Confusingly named variables and mixed soft and hard coding.**
The calculation of `penaltyAmount`(L653) involves `maxPenalty`(L668). However `maxPenalty` is by no means a maximum penalty; the full locked amount is. `maxPenalty` is merely used as a multiplier to represent a rational as an integer. The calculation (L653) is then renormalised instead by a hardcoded value of 10**18, probably to avoid division by zero in the case `maxPenalty` is set to 0 (In `unlock()` L161-L165). This means that `maxPenalty` is used for two different purposes.
The unlocking functionality should be handled independently from the multiplier use of `maxPenalty`, which itself should also be renamed and used also instead of the hardcoded value. In fact, it shares the same value with `MULTIPLIER` (L48) which is already used in exactly this way (L300-L302, L334-L337), so this constant should be used here as well.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L653
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L668
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L161-L165
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L48
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L300-L302
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L334-L337


**Cleanup in `delegate()`**
In `delegate()` (L555-L592) `fromLocked` and `toLocked` are always `locked[delegatee]` and `locked[_addr]` respectively, except for a possible update of its `delegatee` to `_addr`. However, this update applies precisely to `locked[msg.sender]´ in all three cases. Thus the entire if-clause can be replaced by a direct setting of `fromLocked` and `toLocked`, and a final update of `locked[msg.sender].delegatee = _addr;`. This makes the logic much more explicit, as well as saves gas.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L555-L592

**Discompartmentalised delegation with `_delegate()`**
The function `_delegate` (L595-L625) has to be called twice, once to undelegate and once to delegate. It is never used in any other way. These two functions should be both put under the same hood in the `_delegate` function. This makes the code both neater and more robust, and saves gas.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L595-L625

**Counterproductive check.**
In L328-L331, if this indeed is not supposed to happen it might be unwise to allow it to happen unchecked. Consider defining Point.slope as a uint instead of an int128, or converting it to a uint (which has the same effect as L328-L331) during its calculation, or using an assert.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L328-L331

**Loop possibly too short.**
In L309 the stopping condition in the for-loop is ´i < 255´. As warned by the comment on L310-L311 this may be too small. There seems to be no reason for the value 255 specifically, so it may be set arbitrarily higher. Or the for-loop could be transformed into a while-loop, using the already existing internal break at L341.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L309-L311
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L341

**Dangerous use of strict equality.**
In L341 a strict equality is used a a breaking condition, `iterativeTime == block.timestamp`, and similarly in L850. Fortunately `iterativeTime` is checked before for `> block.timestamp` in which case it is set to equal, so the breaking condition will trigger as intended. It seems prudent to check for `>=` or at least make a clear comment about this.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L341
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L850

**Mapping from "indexically" spaced keys.**
`slopeChanges` is implemented as a mapping from timestamps (big numbers), but it is really indexed over weeks (small numbers). Were it not for the flooring to precise weekly timestamps, this usage would fail (one would miss the mapping keys). Consider working with weeks explicitly instead.
https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L60
