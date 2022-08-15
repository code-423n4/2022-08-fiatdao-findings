## Low Risk Issues

### [L-01] Use two-phase ownership transfers

Currently it doesn't validate the new address when change the owner and recommend using two-phase ownership transfers for safety.

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L120

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
120:         owner = _owner;
```

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L139-L143

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
139:     function transferOwnership(address _addr) external {
140:         require(msg.sender == owner, "Only owner");
141:         owner = _addr;
142:         emit TransferOwnership(_addr);
143:     }
```

### [L-02] Check address(0) for VotingEscrow.penaltyRecipient.

There is no address(0) validation for `penaltyRecipient` and penalty might be transferred to zero address [here](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L676).

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L121

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
121:         penaltyRecipient = _penaltyRecipient;
```

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L153-L157

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
153:     function updatePenaltyRecipient(address _addr) external {
154:         require(msg.sender == owner, "Only owner");
155:         penaltyRecipient = _addr;
156:         emit UpdatePenaltyRecipient(_addr);
157:     }
```

### [L-03] Different require() in VotingEscrow.createLock()

In the [CheckpointMath](https://github.com/code-423n4/2022-08-fiatdao/blob/main/CheckpointMath.md#create-lock), it requires `T>owner.end` but it's implemented as `unlock_time >= locked_.end`.

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L414

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
414:         require(unlock_time >= locked_.end, "Only increase lock end"); // from using quitLock, user should increaseAmount instead 
```

### [L-04] Different require() in VotingEscrow.delegate()

In the [CheckpointMath](https://github.com/code-423n4/2022-08-fiatdao/blob/main/CheckpointMath.md#delegate), it requires `to.end > from.end` but it's implemented as `toLocked.end >= fromLocked.end`.

After I confirm with sponsor, `>=` should be used instead of `>`. Otherwise, a delegator can't change the delegatee when the previous delegatee keeps updating fromLocked.end = the latest week before block.timestamp + MAXTIME.

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L589

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
589:         require(toLocked.end >= fromLocked.end, "Only delegate to longer lock");
```

### [L-05] Wrong comment

[This comment](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L647) is wrong because we check `locked_.end > block.timestamp` [here](https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L636).

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L647

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
647:         // oldLocked can have either expired <= timestamp or zero end
```

## Non-critical Issues

### [N-01] Each event should use three indexed fields if there are three or more fields

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L25-L31

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
25:     event Deposit(
26:         address indexed provider,
27:         uint256 value,
28:         uint256 locktime,
29:         LockAction indexed action,
30:         uint256 ts
31:     );
```

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L32-L37

```
File: 2022-08-fiatdao\contracts\VotingEscrow.sol
32:     event Withdraw(
33:         address indexed provider,
34:         uint256 value,
35:         LockAction indexed action,
36:         uint256 ts
37:     );
```

### [N-02] Immutable addresses should be 0-checked
`manager` and `ve` can be declared as immutable variables because they are set only once in the constructor and they should be 0-checked.

- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/features/Blocklist.sol#L15-L16

```
File: 2022-08-fiatdao\contracts\features\Blocklist.sol
15:         manager = _manager;
16:         ve = _ve;
```