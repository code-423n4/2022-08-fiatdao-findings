# Summary

| Id | Title |
| -- | ----- |
| 1 | An address can be blocked 2 times |
| 2 | Penalty can be up to 100% of lock amount |

## 1. An address can be blocked 2 times

In function `block()`, an address can be blocked even when it's already been blocked because it didn't check `_blocklist[addr]` first

### Affected Codes

- https://github.com/code-423n4/2022-08-foundation/blob/792e00df429b0df9ee5d909a0a5a6e72bd07cf79/contracts/mixins/nftDropMarket/NFTDropMarketFixedPriceSale.sol#L156

### Recommended Mitigation Steps

- Check `_blocklist[addr]` before block an address
```solidity
require(_blocklist[addr] == false);
```

## 2. Penalty can be up to 100% of lock amount

When users lock for maximum duration, and use `quitLock()`, the penalty they might get can be up to 100% of lock amount.

```solidity
return ((end - block.timestamp) * maxPenalty) / MAXTIME;
```

In case maximum duration, `_calculatePenaltyRate()` will return `maxPenalty`, which is `1e18` and this value is used to calculate penalty as

```solidity
uint256 penaltyAmount = (value * penaltyRate) / 10**18;
```

So `penaltyAmount` can equal to `value` and users will receive nothing when they call `quitLock()` function.

### Affected Codes
- https://github.com/code-423n4/2022-08-fiatdao/blob/fece3bdb79ccacb501099c24b60312cd0b2e4bb2/contracts/VotingEscrow.sol#L652-L653

### Recommended Mitigation Steps

Should consider reduce value of `maxPenalty` and add a limit for this value.



