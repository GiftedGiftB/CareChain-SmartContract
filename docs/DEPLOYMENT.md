# Deployment Architecture & Workflow Planning

This document outlines the deployment strategy for the CareChain-SmartContract repository. A secure, reproducible, and automated deployment process is essential for maintaining the integrity of the blockchain infrastructure across different environments.

## Deployment Environments

### 1. Local / Standalone (Development)
* **Target**: Local Soroban standalone node or in-memory Rust test environment.
* **Purpose**: Rapid iteration, frontend integration testing, and debugging.
* **Process**: Developers use `soroban contract deploy` against a local RPC instance. State is ephemeral.

### 2. Stellar Futurenet / Testnet (Staging)
* **Target**: Public Stellar Testnet.
* **Purpose**: Beta testing, wallet integration verification, and staging for the CareChain frontend and backend.
* **Process**: Automated via GitHub Actions (`.github/workflows/testnet-deploy.yml`) upon merging PRs into the `develop` branch.
* **State Management**: Contract IDs are tracked in `environments/testnet/state.json` to allow the backend to synchronize with the latest deployments.

### 3. Stellar Mainnet (Production)
* **Target**: Public Stellar Mainnet.
* **Purpose**: Production release handling real user funds and settlements.
* **Process**: Strictly gated. Triggered manually via GitHub Actions (`.github/workflows/mainnet-deploy.yml`) after a formal release is cut, signed, and approved by the core team.
* **Security Controls**: Deployment requires multi-signature approval from authorized admin accounts. Contract admin keys are stored securely in hardware wallets or enterprise key management systems.

## Deployment Workflow

1. **Compilation**: Contracts are compiled to optimized WebAssembly (`wasm32-unknown-unknown`) using the release profile defined in `Cargo.toml`.
2. **WASM Optimization**: The `soroban-cli` optimizes the WASM binary to reduce size and execution cost.
3. **Installation (Upload)**: The WASM binary is installed on the Stellar network, returning a unique `wasm_hash`.
4. **Instantiation (Deploy)**: A contract instance is created from the `wasm_hash`, returning a unique Contract ID.
5. **Initialization**: The deployment script calls the `init()` function of the contract, passing necessary configuration parameters (e.g., admin public keys, token contract IDs).
6. **State Tracking**: The generated Contract IDs are automatically committed back to the repository in the `deployment/` directory or synced to a secure environment variable registry.

## Contract Upgrade Strategy

* **Proxy Upgrades**: Rather than deploying entirely new contracts and migrating data off-chain, CareChain utilizes Soroban's native contract upgrade mechanism.
* **Process**: 
  1. A new WASM binary is installed on the network.
  2. An authorized admin invokes the `update_current_contract_wasm` function on the existing contract, passing the new `wasm_hash`.
  3. The contract logic is updated while retaining the existing Persistent and Instance storage.
* **Migration Handling**: If storage schemas change, upgrade transactions are bundled with data migration scripts to ensure atomic state transitions without risking locked funds.
