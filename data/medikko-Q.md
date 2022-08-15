### Unspecific Compiler Version Pragma

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

_There are **4** instances of this issue:_

```solidity
File: /contracts/VotingEscrow.sol
2:      pragma solidity ^0.8.3;
```

```solidity
File:  /contracts/features/Blocklist.sol
2:      pragma solidity ^0.8.3;
```

```solidity
File: ./contracts/mocks/MockERC20.sol

7:    pragma solidity ^0.8.0;
```

```solidity
File: ./contracts/mocks/MockSmartWallet.sol

2:     pragma solidity ^0.8.3;
```
