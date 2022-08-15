### [L-01] Unsafe ERC20 approve operation

#### Impact
ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

To circumvent ERC20's `approve` functions race-condition vulnerability use OpenZeppelin's SafeERC20 library's `safe{Increase|Decrease}Allowance` functions.


#### Findings:
```
contracts/mocks/MockSmartWallet.sol::20 => fdt.approve(ve, amount);
contracts/mocks/MockSmartWallet.sol::25 => fdt.approve(ve, amount);
```

### [L-02] Unspecific Compiler Version Pragma

#### Impact
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.
#### Findings:
```
contracts/VotingEscrow.sol::2 => pragma solidity ^0.8.3;
contracts/features/Blocklist.sol::2 => pragma solidity ^0.8.3;
contracts/interfaces/IBlocklist.sol::2 => pragma solidity ^0.8.3;
contracts/interfaces/IERC20.sol::2 => pragma solidity ^0.8.3;
contracts/interfaces/IVotingEscrow.sol::2 => pragma solidity ^0.8.3;
contracts/mocks/MockERC20.sol::7 => pragma solidity ^0.8.0;
contracts/mocks/MockSmartWallet.sol::2 => pragma solidity ^0.8.3;
```
