# Testing Architecture Planning

This document outlines the testing architecture for the CareChain-SmartContract repository. A robust testing strategy is critical to ensure the safety of funds in the decentralized escrow system.

## Testing Layers

### 1. Unit Testing
* **Scope**: Isolated business logic within individual Rust modules.
* **Framework**: Native Rust `#[test]` attribute.
* **Focus**: Arithmetic safety, state transitions, validation logic, and error handling.
* **Execution**: Run via `cargo test`.

### 2. Integration Testing
* **Scope**: Cross-contract calls and complete workflows (e.g., Escrow contract interacting with Reputation contract).
* **Framework**: Soroban SDK testing utilities (`soroban_sdk::testutils`).
* **Focus**: Authentication (`require_auth`), event emission verification, and storage state verification after complex interactions.
* **Location**: `integration-tests/` directory.

### 3. Smart Contract Simulation Testing
* **Scope**: Simulating transactions against a local Soroban environment before network deployment.
* **Framework**: Soroban CLI (`soroban contract invoke`).
* **Focus**: Gas estimation, parameter serialization/deserialization, and contract initialization workflows.

### 4. Escrow Lifecycle & Transaction Validation Testing
* **Focus**: End-to-end testing of the core CareChain use case.
* **Workflow**:
  1. Initialize Escrow.
  2. Fund Escrow (simulating client deposit).
  3. Emit booking event.
  4. Simulate caregiver verification.
  5. Validate client sign-off.
  6. Assert final settlement transfer and zeroed escrow balance.
  7. Test dispute resolution paths (refunds and arbitrator interventions).

### 5. Wallet Interaction Testing
* **Focus**: Simulating the exact payload structure that will be sent to Stellar wallets (Freighter, xBull).
* **Goal**: Ensure the authorization payloads are correctly formatted and prevent signature replay attacks.

### 6. Security & Performance Testing
* **Fuzz Testing**: Utilizing tools like `cargo-fuzz` to test contract inputs with random data to uncover edge-case panics.
* **Gas Profiling**: Monitoring instruction count limits and storage read/write limits to ensure transactions remain affordable and within network bounds.

## CI Integration
All tests are strictly enforced in the CI pipeline (`.github/workflows/test.yml`). Pull requests will not be merged unless coverage is maintained and all tests pass.
