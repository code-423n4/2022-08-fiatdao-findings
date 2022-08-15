    # ISSUE LIST

#### C4-001 : Critical changes should use two-step procedure - Non Critical
#### C4-002 : Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom - Low
#### C4-003 : Missing zero-address check in the setter functions and initiliazers - Low
#### C4-004 : Low level calls with solidity version 0.8.14 can result in optimiser bug. - LOW
#### C4-005 : The Contract Should safeApprove(0) first - LOW
#### C4-006 : Use of Block.timestamp - Non-critical
#### C4-007 : Incompatibility With Rebasing/Deflationary/Inflationary tokens - LOW
#### C4-008 : Contract should have pause/unpause functionality
#### C4-009 : Pragma Version - Non Critical

# ISSUES

# C4-001 : Critical changes should use two-step procedure

## Impact - NON CRITICAL

The critical procedures should be two step process. The contracts inherit OpenZeppelin's Ownable contract which enables the onlyOwner role to transfer ownership to another address. It's possible that the onlyOwner role mistakenly transfers ownership to the wrong address, resulting in a loss of the onlyOwner role. The current ownership transfer process involves the current owner calling Unlock.transferOwnership(). This function checks the new owner is not the zero address and proceeds to write the new owner's address into the owner's state variable. If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier. Lack of two-step procedure for critical operations leaves them error-prone
if the address is incorrect, the new address will take on the functionality of the new role immediately

for Ex : -Alice deploys a new version of the whitehack group address. When she invokes the whitehack group address setter to replace the address, she accidentally enters the wrong address. The new address now has access to the role immediately and is too late to revert

## Proof of Concept

1. Navigate to the following contract.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L139
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

# C4-002 : Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom

## Impact

It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Reference: This similar medium-severity finding from Consensys Diligence Audit of Fei Protocol: https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call


## Proof of Concept

1. Navigate to the following contract.

2. transfer/transferFrom functions are used instead of safe transfer/transferFrom on the following contracts.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L426
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Consider using safeTransfer/safeTransferFrom or require() consistently.


# C4-003 : # Missing zero-address check in the setter functions and initiliazers

## Impact

Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.

## Proof of Concept

1. Navigate to the following contracts.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L100
```

## Tools Used

Code Review

## Recommended Mitigation Steps

Consider adding zero-address checks in the discussed constructors:
require(newAddr != address(0));.


# C4-004 : Low level calls with solidity version 0.8.3 can result in optimiser bug.

## Impact

The protocol is using low level calls with solidity version 0.8.3 which can result in optimizer bug.


https://medium.com/certora/overly-optimistic-optimizer-certora-bug-disclosure-2101e3f7994d

## Proof of Concept

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L2
```


## Tools Used

Code Review

## Recommended Mitigation Steps

Consider upgrading to solidity 0.8.15.


# C4-005 : The Contract Should safeApprove(0) first - LOW

## Impact

Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value.
They must first be approved by zero and then the actual allowance must be approved.


When trying to re-approve an already approved token, all transactions revert and the protocol cannot be used.


## Tools Used

None

## Recommended Mitigation Steps

Approve with a zero amount first before setting the actual amount.


# C4-006 : Use of Block.timestamp

## Impact -  Non-Critical

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.


## Proof of Concept

1. Navigate to the following contract.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L111
```

## Tools Used

Manual Code Review

## Recommended Mitigation Steps

Block timestamps should not be used for entropy or generating random numbers—i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.

# C4-007 : Incompatibility With Rebasing/Deflationary/Inflationary tokens

## Impact -  LOW

PrePo protocol do not appear to support rebasing/deflationary/inflationary tokens whose balance changes during transfers or over time. The necessary checks include at least verifying the amount of tokens transferred to contracts before and after the actual transfer to infer any fees/interest.

## Example Test

During the lending, If the inflationary/deflationary tokens are used excepted amount will be lower than deposit.

## Proof of Concept

1. Navigate to the following contract.

```
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L426
https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L486
```

## Tools Used

Manual Code Review

## Recommended Mitigation Steps

- Ensure that to check previous balance/after balance  equals to amount for any rebasing/inflation/deflation
- Add support in contracts for such tokens before accepting user-supplied tokens
- Consider supporting deflationary / rebasing / etc tokens by extra checking the balances before/after or strictly inform your users not to use such tokens if they don't want to lose them.

# C4-008 : Contract should have pause/unpause functionality

## Impact

In case a hack is occuring or an exploit is discovered, the team should be able to pause
functionality until the necessary changes are made to the system. Additionally, the AuraLocker.sol contract should be manged by proxy so that upgrades can be made by the owner.

To use a thorchain example again, the team behind thorchain noticed an attack was going to occur well before
the system transferred funds to the hacker. However, they were not able to shut the system down fast enough.
(According to the incidence report here: https://github.com/HalbornSecurity/PublicReports/blob/master/Incident%20Reports/Thorchain_Incident_Analysis_July_23_2021.pdf)


## Proof of Concept

https://github.com/code-423n4/2022-08-fiatdao/blob/main/contracts/VotingEscrow.sol#L440

## Tools Used

Code Review

## Recommended Mitigation Steps

Pause functionality on the contract would have helped secure the funds quickly.


# C4-009 : # Pragma Version

## Impact

In the contracts, there are multiple version of pragmas are used. The contract is using pragma 0.8.3^. The contracts should be deployed with the consistent pragma.

## ## Proof of Concept

https://swcregistry.io/docs/SWC-103

```
All Contracts
```

## Tools Used
Manual code review

## Recommended Mitigation Steps
Lock the pragma version: delete pragma solidity 0.8.10 in favor of pragma solidity 0.8.10.