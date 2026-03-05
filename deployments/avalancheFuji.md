# Avalanche Fuji Deployment

Deployed: 2026-03-05 (Avax Build Games hackathon submission)

## Addresses

| Contract            | Address |
|---------------------|---------|
| DFSEscrowManager    | `0x3819AC57110F008D491BBBba4fB14EcbFf45E5D0` |
| MockVaultFactory    | `0x1dCE6e45eaf73B15E26139F365d4Bf622D69fff0` |
| USDC (official)     | `0x5425890298aed601595a70AB815c96711a31Bc65` |

Use these in backend/frontend env (Steps 4, 5, 6 of avax_fuji_add_contest_tracker.md).

## Verify on Snowtrace

Both contracts are verified. Hardhat is configured to use `ETHERSCAN_API_KEY` in `.env` (Snowtrace uses Etherscan-compatible API).

Verified contracts:
- [DFSEscrowManager](https://testnet.snowtrace.io/address/0x3819AC57110F008D491BBBba4fB14EcbFf45E5D0#code)
- [MockVaultFactory](https://testnet.snowtrace.io/address/0x1dCE6e45eaf73B15E26139F365d4Bf622D69fff0#code)

To re-verify:

```bash
# DFSEscrowManager (constructor: vaultFactory address)
npx hardhat verify --network avalancheFuji 0x3819AC57110F008D491BBBba4fB14EcbFf45E5D0 0x1dCE6e45eaf73B15E26139F365d4Bf622D69fff0

# MockVaultFactory (no constructor args)
npx hardhat verify --network avalancheFuji 0x1dCE6e45eaf73B15E26139F365d4Bf622D69fff0
```

## Network Details

- **Chain ID**: 43113
- **RPC URL**: `https://api.avax-test.network/ext/bc/C/rpc`
- **Block Explorer**: `https://testnet.snowtrace.io`
- **Explorer API**: `https://api-testnet.snowtrace.io/api`
- **Native Token**: AVAX
- **Stablecoin**: USDC (6 decimals)

## Faucets

- **AVAX Testnet Faucet**: `https://core.app/tools/testnet-faucet/`
- **USDC Testnet Faucet**: `https://faucet.circle.com/`
