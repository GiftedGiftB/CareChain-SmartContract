# CareChain-SmartContract

## Project Overview

CareChain-SmartContract is the blockchain infrastructure repository for the CareChain decentralized caregiving platform. It houses the Stellar Soroban smart contracts written in Rust, which serve as the foundational trust, escrow, and payment infrastructure powering the CareChain ecosystem.

The role of blockchain in CareChain is to provide a decentralized, immutable, and transparent ledger for managing caregiving service agreements and settlements. By utilizing Stellar's high-speed, low-cost network, CareChain-SmartContract introduces a decentralized escrow mechanism that ensures secure payment routing between families, patients, and caregivers without relying on traditional centralized financial intermediaries.

Decentralized escrow matters profoundly in caregiving systems because it guarantees that funds are securely locked and only released when predetermined service milestones are met. This creates a trustless environment that protects both parties and ensures fair compensation for critical care work.

---

## Core Problem the Platform Solves

The global caregiving industry is hindered by systemic inefficiencies and trust deficits, including:

* **Payment Disputes**: Disagreements over service completion frequently result in withheld or delayed payments.
* **Lack of Trust**: Families struggle to trust independent caregivers, and caregivers risk non-payment for completed work.
* **Delayed Settlements**: Traditional banking systems and centralized platforms often impose long payout delays on caregivers.
* **Fraud Risks**: Both clients and caregivers are exposed to fraudulent claims or fake service representations.
* **Centralized Payment Limitations**: Intermediaries take significant cuts, reducing the net income of caregivers and increasing costs for families.
* **Cross-Border Payment Barriers**: Global caregivers and immigrant support workers face high remittance fees and slow international transfers.

---

## Proposed Solution

CareChain leverages the **Stellar blockchain and Soroban smart contracts** to solve these problems by introducing:

* **Escrow Payments**: Client funds are secured in a decentralized smart contract upon booking, ensuring caregivers that the funds exist and will be paid upon completion.
* **Decentralized Settlement**: Payments are routed peer-to-peer, bypassing expensive centralized payment gateways.
* **Smart Contract Verification**: Service completion requires cryptographic validation and multi-party sign-off to trigger payment release.
* **Blockchain Transparency**: Every milestone, verification, and payment is recorded immutably on-chain.
* **Immutable Records**: Dispute resolution is backed by an unalterable history of interactions, significantly reducing fraud.

---

## How the Smart Contract System Works

* **Escrow Lifecycle**: A client initiates a care booking by depositing Stellar-native assets (e.g., USDC or XLM) into a dynamically generated escrow contract. The funds are locked until conditions are met.
* **Transaction Flow**: The contract emits an event signaling the booking is funded. The caregiver accepts the booking and begins service.
* **Booking-to-Payment Flow**: The frontend and backend interact with the Soroban RPC to track the status of the escrow.
* **Care Service Verification Flow**: Once the service is completed, the caregiver submits a completion proof. The client signs the transaction approving the completion.
* **Settlement Process**: Upon verification, the smart contract automatically executes the settlement, transferring the funds directly to the caregiver's Stellar wallet.
* **Refund/Dispute-Ready Architecture**: If a dispute is raised, the funds remain locked. A decentralized or designated arbitration role can resolve the dispute, executing a refund or partial payment based on the outcome.

---

## Financial Inclusion & Economic Impact

* **Global Caregiver Participation**: Anyone with a Stellar-compatible wallet can participate, enabling caregivers in underbanked regions to offer services.
* **Cross-Border Payments**: Families can securely pay caregivers across the globe with near-zero transaction fees, completely bypassing traditional remittance networks.
* **Reduced Transaction Costs**: By removing intermediary platforms taking 20-30% cuts, caregivers retain significantly more of their earnings.
* **Economic Empowerment**: Direct peer-to-peer payments accelerate liquidity for care workers who live paycheck to paycheck.
* **Decentralized Trust Systems**: An on-chain reputation system allows independent caregivers to build portable, verifiable profiles without being locked into a single centralized platform.

---

## Smart Contract Architecture

The CareChain-SmartContract repository is built on a highly modular architecture designed for security, scalability, and maintainability:

* **Soroban Contract Architecture**: Contracts are designed as small, composable units utilizing standard Soroban interfaces.
* **Rust Modular Organization**: The workspace is split into core business logic (`contracts/escrow`, `contracts/payments`) and shared utilities (`contracts/shared`, `contracts/errors`).
* **Contract Separation Strategy**: Dedicated contracts handle distinct domains (Escrow, Reputation, Verification) to limit the attack surface and simplify upgrades.
* **Event-Driven Architecture**: Contracts rely heavily on emitting structured events (`contracts/events`) to synchronize the off-chain CareChain-Backend via Soroban RPC.
* **Security-First Design**: The architecture enforces strict access control (`contracts/auth`, `contracts/access-control`) and utilizes established patterns to prevent reentrancy and unauthorized state manipulation.
* **Upgrade Strategy**: Contracts utilize proxy patterns and structured state migrations to allow bug fixes and feature additions without compromising the integrity of locked funds.
* **Storage Architecture**: Efficient use of Soroban's `Instance` and `Persistent` storage models optimizes for low fees while ensuring data availability.

---

## Blockchain Features

* **Escrow System**: Time-locked and condition-locked fund management.
* **Transaction Verification**: Cryptographic validation of all service milestones.
* **Wallet Integration Readiness**: Abstracted interfaces designed to work with Freighter, xBull, and other Stellar wallets.
* **On-Chain Reputation Preparation**: Foundations for issuing non-transferable reputation tokens or attestations based on completed jobs.
* **Immutable Records**: Permanent storage of service agreements and payment histories.
* **Blockchain Settlement Tracking**: End-to-end visibility of transaction finality on the Stellar ledger.

---

## DEVELOPMENT PHASES

### Phase 1 — Smart Contract MVP Foundation
* **Focus**: Soroban project scaffolding, basic escrow contract structure, mock payment flow, transaction simulation, testnet preparation, and basic deployment workflows.
* **Priority**: Fast iteration, proof of concept, and demo readiness.

### Phase 2 — Escrow & Verification Expansion
* **Focus**: Advanced service verification workflows, payment release conditions, transaction lifecycle management, wallet interaction preparation, event tracking architecture, and booking verification systems.

### Phase 3 — Decentralized Trust & Payments
* **Focus**: Real Stellar escrow payments (USDC/XLM integration), wallet integrations, cross-border transactions, reputation systems, on-chain settlement confirmation, and multi-party transaction architecture.

### Phase 4 — Production & Scale
* **Focus**: Security hardening, third-party smart contract audits, monitoring and analytics, upgrade mechanisms, performance optimization, mainnet deployment readiness, and enterprise-grade blockchain infrastructure.

---

## ADDITIONAL SECTIONS

### Technology Stack
* **Blockchain**: Stellar Network
* **Smart Contract Platform**: Soroban
* **Language**: Rust
* **Testing**: Rust built-in tests, Soroban CLI simulator

### Blockchain Infrastructure Overview
The infrastructure relies on Soroban RPC for querying contract state and submitting transactions. We utilize Stellar's Futurenet/Testnet for staging and will deploy to Mainnet upon successful security audits.

### Soroban Development Environment
Ensure you have the following installed:
* Rust toolchain (latest stable)
* `soroban-cli`
* WebAssembly target: `rustup target add wasm32-unknown-unknown`

### Installation & Setup Instructions
1. Clone the repository.
2. Run `make build` to compile the contracts.
3. Run `make test` to execute the test suite.

### Environment Variables
*(To be populated with RPC endpoints, admin public keys, and network passphrases during Phase 1)*

### Deployment Workflow
Deployment scripts in `scripts/` and `deployment/` handle the instantiation of contracts across different environments. GitHub Actions orchestrate the pipeline from PR to Mainnet deployment.

### Smart Contract Testing Strategy
The repository includes strict testing protocols:
* Unit tests for isolated Rust logic.
* Integration tests (`integration-tests/`) for cross-contract interactions.
* Simulation testing via Soroban CLI.

### Security Overview
* Strict access control definitions.
* Prevention of arithmetic overflows (built into Rust).
* Audit readiness documentation.
* Use of multi-signature requirements for administrative functions.

### Contract Upgrade Strategy
Contracts are designed to be upgradeable by authorized administrators using Soroban's native contract upgrade functions, ensuring the protocol can evolve without migrating state.

### Folder Structure Documentation
Refer to `docs/ARCHITECTURE.md` (to be implemented) for a deep dive into the repository's folder and file organization, which includes separation into `contracts/`, `tests/`, `scripts/`, and `.github/workflows/`.

### Event Architecture
All state changes emit strongly typed events. These events are the primary mechanism for the CareChain backend to index blockchain state changes and update the off-chain database.

### CI/CD & DevOps Strategy
GitHub Actions will be used to enforce code quality, run tests, validate Soroban builds, and manage deployments to Testnet and Mainnet.

### Monitoring & Analytics Preparation
Tools and scripts for querying the Soroban RPC and analyzing event logs will be maintained in the `monitoring/` directory.

### Contribution Guidelines
Contributions must adhere to the Rust style guide (`rustfmt`), pass all tests, and include comprehensive documentation for any new contract logic.

### Future Roadmap
* Decentralized arbitration integration.
* Yield-bearing escrow options for long-term care contracts.
* Advanced zero-knowledge proof verification for patient privacy.

### License
MIT License

### Team & Author Information
Developed by the CareChain Core Team.
