# aiSports EVM Escrow Contracts - Avalanche Fuji

Smart contracts for **stablecoin-based fantasy sports contests** on **Avalanche Fuji testnet** (Chain ID: 43113), designed for the Avax Build Games hackathon submission. This is a dedicated escrow contracts repository focused on Avalanche Fuji deployment.

## Overview

The primary contract in this repo is **`DFSEscrowManager`** (`contracts/DFSEscrowManager.sol`). It manages:

- **Escrow creation** for a contest (organizer/authorized creator)
- **Joining** an escrow with **multi-entry** support (up to `maxEntriesPerUser`, default 1000)
- **Pool top-ups** (sponsors/organizer can add funds)
- **Payout distribution** after the contest ends (organizer-triggered)
- **Overflow handling** (any surplus funds go to an overflow recipient; defaults to organizer)
- **Authorized creators**: the owner can whitelist which addresses are allowed to create escrows

Funds are custody’d inside a **Yearn-style ERC-4626 vault per escrow**. On testnets (and networks without an official factory), the deployment uses `MockVaultFactory` / `MockYearnVault`.

> Note: `EscrowManager.sol` remains in the repo as an earlier version; `DFSEscrowManager.sol` is the DFS-specific, current contract.

## What’s deployed

### Avalanche Fuji Testnet (Chain ID: 43113)

Deployment addresses will be documented in `deployments/avalancheFuji.md` after contract deployment.

- **`DFSEscrowManager`**: `TBD` (to be deployed)
- **`MockVaultFactory`**: `TBD` (to be deployed)
- **Official USDC (Avalanche Fuji)**: `0x5425890298aed601595a70AB815c96711a31Bc65` (6 decimals)

To get Avalanche Fuji testnet AVAX for gas, use the faucet at `https://core.app/tools/testnet-faucet/`.  
To get USDC testnet tokens, use the Circle faucet at `https://faucet.circle.com/`.

## Supported networks (Hardhat)

Configured in `hardhat.config.ts`:

- **Avalanche Fuji Testnet**: `avalancheFuji` (chainId 43113; explorer: Snowtrace)
- Additional networks may be configured as needed for testing/deployment

## Project structure

```
aiSports_evm_escrow/
├── contracts/
│   ├── DFSEscrowManager.sol            # Primary contract (DFS + PYUSD 6-decimals + multi-entry)
│   ├── EscrowManager.sol               # Legacy contract
│   ├── MockToken.sol                   # Mock ERC20 used for local/tests
│   ├── interfaces/
│   │   ├── IERC4626.sol
│   │   ├── IVaultFactory.sol
│   │   └── IYearnVault.sol
│   └── mocks/
│       ├── MockVaultFactory.sol
│       └── MockYearnVault.sol
├── scripts/
│   ├── deploy_dfs_escrow_manager.ts    # Deploy DFSEscrowManager (+ vault factory resolution)
│   └── deploy.ts                       # Deploy legacy EscrowManager
├── deployments/
│   └── arbitrumSepolia.md              # Deployed addresses + verification commands
├── test/
│   ├── DFSEscrowManager.ts
│   └── EscrowManager.ts
└── hardhat.config.ts
```

## Setup

Install:

```bash
npm install
```

Create `.env`:

```bash
# Used for Avalanche Fuji testnet deployment
DEPLOYER_PRIVATE_KEY=...

# Optional (contract verification). Hardhat is configured for Snowtrace API.
ETHERSCAN_API_KEY=...
```

## Common commands

```bash
npm run compile
npm run test
npm run node
```

## Deploy

### Deploy `DFSEscrowManager` (recommended)

Local:

```bash
npm run deploy:dfs:localhost
```

Flow:

```bash
npm run deploy:dfs:testnet
npm run deploy:dfs:mainnet
```

Arbitrum / Base:

```bash
npm run deploy:dfs:arbitrumSepolia
npm run deploy:dfs:baseSepolia

# Placeholders (addresses TBD)
npm run deploy:dfs:arbitrumOne
npm run deploy:dfs:base
```

The deploy script prints the values you’ll want to paste into your frontend/backend env vars (for example `NEXT_PUBLIC_EVM_ESCROW_ADDRESS_ARB_SEPOLIA`).

### Deploy legacy `EscrowManager`

```bash
npm run deploy:flowTestnet
npm run deploy:flowMainnet
```

## Verify contracts

Avalanche Fuji verification commands will be documented in `deployments/avalancheFuji.md` after deployment. Example:

```bash
# DFSEscrowManager (constructor arg: vaultFactoryAddress)
npx hardhat verify --network avalancheFuji <DFSEscrowManager_ADDRESS> <vaultFactoryAddress>

# MockVaultFactory (no constructor args)
npx hardhat verify --network avalancheFuji <MockVaultFactory_ADDRESS>
```

## Integration notes (aiSports app)

This repository contains the smart contracts for Avalanche Fuji testnet integration with the aiSports DFS platform. In the broader multi-chain aiSports system, contests carry a `chain_network` field (e.g. `"avalancheFuji"`). The backend/frontend resolve the correct RPC + contract + token addresses from a per-network registry.

**Hackathon Submission**: This dedicated repository is prepared for the Avax Build Games hackathon submission, focusing specifically on Avalanche Fuji testnet deployment.

## License

MIT
