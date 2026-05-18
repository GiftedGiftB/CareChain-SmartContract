# Contributing to CareChain-SmartContract

Thank you for your interest in contributing to **CareChain-SmartContract**!

We welcome developers, blockchain engineers, security researchers, and open-source contributors who want to help build a decentralized caregiving payment and escrow infrastructure powered by the Stellar blockchain.

CareChain-SmartContract is designed to provide secure, transparent, and trustless payment workflows for caregiving services using Stellar Soroban smart contracts and Rust.

Whether you are:

* improving escrow workflows,
* optimizing smart contract performance,
* enhancing security,
* designing wallet integrations,
* improving testing infrastructure,
* or proposing new decentralized trust features,

your contribution is highly valuable to the CareChain ecosystem.

---

# Getting Started

## 1. Fork the Repository

Fork the repository on GitHub.

---

## 2. Clone Your Fork

```bash
git clone https://github.com/your-username/CareChain-SmartContract.git
cd CareChain-SmartContract
```

---

## 3. Create a Feature Branch

```bash
git checkout -b feature/my-new-feature
```

Use descriptive branch names such as:

* feature/escrow-release-flow
* feature/reputation-events
* fix/payment-validation
* refactor/shared-types
* docs/contract-architecture

---

# Development Workflow

The CareChain Smart Contract repository is structured as a **Cargo Workspace**.

This architecture allows multiple Soroban smart contracts and shared blockchain modules to live within a unified workspace while sharing dependencies, utilities, and common blockchain types.

The workspace is organized around modular smart contract responsibilities located inside the `contracts/` directory.

---

# Core Workspace Structure

## shared_types/

This is one of the most important crates in the workspace.

It acts as the shared communication layer between all CareChain smart contracts.

This crate contains:

* Shared structs
* Common enums
* Blockchain event definitions
* Custom contract errors
* Escrow transaction types
* Booking/payment metadata
* Reputation-related data structures
* Wallet interaction types

If a data structure needs to be shared between contracts, it MUST be defined here.

Examples include:

* BookingDetails
* EscrowStatus
* TransactionState
* CaregiverVerificationStatus
* PaymentReleaseCondition

---

## escrow_contract/

Responsible for decentralized payment escrow management.

This contract will eventually handle:

* Locking caregiver payments
* Escrow creation
* Payment release conditions
* Refund workflows
* Booking settlement logic
* Milestone-based payments
* Dispute-aware payment architecture

---

## verification_contract/

Handles care service verification and transaction confirmation workflows.

Responsibilities include:

* Care service completion confirmation
* Client verification approval
* Service validation logic
* Transaction settlement authorization
* Caregiver confirmation workflows

---

## reputation_contract/

Responsible for decentralized trust and reputation infrastructure.

This contract manages:

* Reputation scoring preparation
* Trust-related event structures
* Caregiver verification records
* Immutable reputation tracking
* On-chain trust indicators

---

# Prerequisites

You will need the following installed:

## Rust

Install the latest stable Rust toolchain:

```bash
https://www.rust-lang.org/tools/install
```

---

## WASM Target

Required for compiling Soroban smart contracts.

```bash
rustup target add wasm32-unknown-unknown
```

---

## Stellar CLI

Required for:

* contract compilation,
* deployment,
* invocation,
* testnet interactions,
* and blockchain management.

```bash
cargo install --locked stellar-cli
```

---

# Building the Contracts

Because this repository is a Cargo Workspace, you can build all contracts directly from the root directory.

---

# Linux / macOS Users (Using Make)

## Build all contracts

```bash
make build
```

---

## Build specific contracts

```bash
make build-escrow
make build-verification
make build-reputation
```

---

# Windows Users (or users without Make)

## Build all contracts

```bash
cargo build --target wasm32-unknown-unknown --release
```

---

## Build specific contracts

```bash
cargo build -p escrow_contract --target wasm32-unknown-unknown --release

cargo build -p verification_contract --target wasm32-unknown-unknown --release

cargo build -p reputation_contract --target wasm32-unknown-unknown --release
```

---

# Testing Guidelines

Testing is a critical part of decentralized application development and blockchain security.

Every contribution should include proper testing where applicable.

---

# Where to Write Tests

Tests should be written inside the corresponding contract directory.

Examples:

* `contracts/escrow_contract/test.rs`
* `contracts/verification_contract/test.rs`
* `contracts/reputation_contract/test.rs`

---

# Shared Types Testing

Although `shared_types/` is not a deployable contract, all helper functions, validation logic, and reusable blockchain structures added there must include unit tests.

---

# Running Tests

## Using Make

```bash
make test
```

---

## Using Cargo

```bash
cargo test
```

---

# Important Note

Running tests may generate ledger snapshot `.json` files.

Examples:

* `test_escrow_flow.1.json`
* `test_payment_release.1.json`

These files are already ignored in `.gitignore` and should NOT be committed.

---

# Smart Contract Contribution Areas

We encourage contributions in areas such as:

* Escrow payment systems
* Soroban contract optimization
* Blockchain event architecture
* Transaction verification systems
* Wallet interaction architecture
* Reputation and trust systems
* Cross-border payment support
* Security hardening
* Gas optimization
* Testing infrastructure
* Monitoring and analytics preparation

---

# Feature Requests & GitHub Issues

Community-driven development is important to the CareChain ecosystem.

---

# Working on Existing Issues

Before starting new work:

1. Browse the GitHub Issues tab
2. Look for:

   * good first issue
   * smart-contract
   * escrow
   * security
   * shared-types
   * reputation-system

Contributors new to the project are encouraged to start with:

* shared blockchain types,
* testing infrastructure,
* documentation,
* or event definitions.

---

# Requesting New Features

Before opening a feature request:

1. Check existing issues first
2. Ensure the feature aligns with the CareChain mission

When opening an issue, include:

* Clear title
* Problem statement
* Blockchain or caregiving use case
* Proposed solution
* Smart contract impact
* Security considerations
* Cross-contract dependencies

---

# Submitting a Pull Request

Before submitting your PR:

---

## 1. Ensure All Tests Pass

```bash
cargo test
```

---

## 2. Format Your Code

```bash
cargo fmt --all
```

---

## 3. Run Linting

```bash
cargo clippy
```

---

## 4. Update Documentation

If your contribution changes:

* escrow behavior,
* transaction workflows,
* contract interfaces,
* blockchain events,
* or security assumptions,

you MUST update the appropriate documentation inside:

```bash
docs/
```

---

# Pull Request Guidelines

Your PR should include:

* Clear explanation of changes
* Related issue references
* Security considerations
* Cross-contract dependencies
* Testing coverage summary
* Migration or upgrade notes (if applicable)

Submit PRs against the:

```bash
main
```

branch unless instructed otherwise.

---

# Security Policy

Security is a top priority for CareChain.

Please avoid publicly disclosing critical vulnerabilities.

If you discover a security issue involving:

* escrow logic,
* payment release conditions,
* wallet authorization,
* transaction validation,
* replay protection,
* or access control,

please report it responsibly through the maintainers.

---

# Coding Standards

Contributors should follow:

* Clean Rust architecture
* Modular Soroban contract design
* Secure blockchain development practices
* Minimal and efficient storage usage
* Event-driven architecture patterns
* Clear naming conventions
* Documentation-first development

---

# License

By contributing to CareChain-SmartContract, you agree that your contributions will be licensed under the MIT License, the same license used by the project.