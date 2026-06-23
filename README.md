# ⛓️ SupplyProof

> **Decentralized Supplier Identity & Composable ESG Compliance Protocol on Stellar**  
> The trust layer for carbon-accountable supply chains — from raw material to retail shelf.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Stellar Network](https://img.shields.io/badge/Network-Stellar-7B2D8B)](https://stellar.org)
[![Soroban](https://img.shields.io/badge/Contracts-Soroban-FF6B35)](https://soroban.stellar.org)
[![Rust](https://img.shields.io/badge/Contracts-Rust-orange)](https://www.rust-lang.org/)
[![TypeScript](https://img.shields.io/badge/Backend%20%2F%20Frontend-TypeScript-3178C6)](https://www.typescriptlang.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## 📖 Table of Contents

- [Overview](#-overview)
- [The Problem](#-the-problem)
- [Our Solution](#-our-solution)
- [How It Works](#-how-it-works)
- [The Composable ESG Score](#-the-composable-esg-score)
- [Architecture](#-architecture)
- [Repository Structure](#-repository-structure)
- [Smart Contracts (Soroban)](#-smart-contracts-soroban)
- [Backend Services](#-backend-services)
- [Frontend Application](#-frontend-application)
- [Protocol Flows](#-protocol-flows)
- [Supported Verticals](#-supported-verticals)
- [Credential Types](#-credential-types)
- [Getting Started](#-getting-started)
- [Environment Configuration](#-environment-configuration)
- [Deployment](#-deployment)
- [API Reference](#-api-reference)
- [SDK](#-sdk)
- [Testing](#-testing)
- [Security](#-security)
- [Regulatory Alignment](#-regulatory-alignment)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌍 Overview

**SupplyProof** is an open, permissionless protocol that gives every supplier in a global supply chain a **verifiable decentralized identity (DID)** — anchored to Stellar — containing audited ESG credentials: carbon footprint attestations, labor compliance records, environmental certifications, and ISO standards.

Brands don't have to trust a supplier's self-reported sustainability claims. They query the supplier's DID and receive a **Composable ESG Score** — a single, aggregated, on-chain metric computed from verified third-party attestations across every tier of the supply chain.

This is not a sustainability reporting tool. This is **trust infrastructure** — the layer that makes ESG claims verifiable, portable, and composable across the entire global trade ecosystem.

### Who SupplyProof Is For

| Actor | Role in the Protocol |
|---|---|
| **Suppliers** | Create a DID; receive credential attestations from auditors; prove ESG compliance to any buyer without re-auditing |
| **Brands / Buyers** | Query supplier DIDs at sourcing time; enforce ESG thresholds programmatically; reduce Scope 3 liability |
| **Auditors / Certifiers** | Issue signed, on-chain credentials after conducting real-world assessments; become trusted registry issuers |
| **Regulators** | Access aggregated, immutable audit trails for CSRD, CBAM, and SEC climate disclosure compliance |
| **Developers** | Build lending, insurance, and trade finance products that consume the ESG score as a composable primitive |

---

## 🔴 The Problem

### The Carbon Accountability Gap

Over **90% of a company's carbon footprint** lies in Scope 3 emissions — the upstream suppliers and downstream value chains they don't directly control. Yet today:

- Supplier ESG data is **self-reported**, unverified, and easy to fabricate
- Certifications (ISO 14001, SA8000, GHG Protocol) are issued as **PDF documents** — forgeable, unrevocable, and invisible to third parties
- Every brand re-audits the same supplier independently, creating **$50B+ in duplicated audit costs** annually
- There is **no shared infrastructure** for ESG credentials — every buyer gets a different answer about the same supplier
- Multi-tier supply chains (Tier 1 → Tier 2 → Tier 3 raw materials) are completely opaque to brands

### The Greenwashing Crisis

- The EU CSRD mandates **double materiality reporting** for 50,000+ companies from 2025
- The US SEC's climate disclosure rule requires **Scope 3 emissions data** from all public companies
- The EU Carbon Border Adjustment Mechanism (**CBAM**) taxes imported goods based on embedded carbon — requiring verifiable proof of origin carbon intensity
- H&M, Shell, and Volkswagen have each faced **nine-figure greenwashing fines and settlements** for unverifiable sustainability claims

### Why Blockchain Hasn't Solved This Before

Existing blockchain-based supply chain solutions fail because they:
- Store full data on-chain, creating **privacy and competitive intelligence leaks**
- Are **permissioned, enterprise-only** chains that exclude small suppliers
- Produce certifications that are **not interoperable** across chains or platforms
- Have **no aggregation layer** — they verify point facts, not supply chain-wide ESG posture
- Are built on expensive chains with transaction costs that exclude suppliers in the Global South

---

## ✅ Our Solution

SupplyProof introduces three primitives the industry has never had simultaneously:

### 1. Supplier DID (`did:supplyproof:<stellar_account_id>`)
Every supplier entity — from a tier-3 cobalt mine to a tier-1 electronics assembler — gets a self-sovereign identity anchored to a Stellar keypair. The DID is the root of trust: it controls what credentials get attached, what data gets disclosed, and who gets to query it.

### 2. On-Chain ESG Credential Registry
Trusted auditors (SGS, Bureau Veritas, DNV, local certifiers) issue signed credentials directly to a supplier's DID on-chain. Every credential has a type, issuer, issuance timestamp, expiry, and hash of the underlying audit report. Nothing is self-reported. Every claim has an issuer signature.

### 3. Composable ESG Score
The protocol's crown jewel. An algorithmic, on-chain score (0–1000) computed from the full credential graph of a supplier and all of their upstream suppliers. A brand buying from a Tier-1 supplier gets a score that reflects the verified ESG posture of every entity in their supply chain — not just the first link. This is the **Scope 3 transparency** that regulators are demanding and no solution currently provides.

---

## 🔬 How It Works

```
                        SupplyProof Protocol
    ┌────────────┐                               ┌──────────────┐
    │  Auditor   │ — issues signed credentials → │   Supplier   │
    │ (SGS, DNV) │                               │    DID       │
    └────────────┘                               └──────┬───────┘
                                                        │
                              Soroban Score Contract    │
                           ┌───────────────────────────►│
                           │  Aggregates credential      │
                           │  graph across all tiers     │
                           └───────────────────────────►│
                                                        │
    ┌────────────┐                               ┌──────▼───────┐
    │   Brand    │ ←— queries ESG score  ——————— │  Composable  │
    │  (Buyer)   │                               │  ESG Score   │
    └────────────┘                               └──────────────┘
```

**Step-by-step:**

1. A supplier registers a DID on Stellar (`did:supplyproof:G...`)
2. They undergo real-world audits by trusted certifiers (carbon audits, labor inspections, ISO assessments)
3. Auditors issue signed, on-chain credentials to the supplier's DID
4. The ESG Score Contract reads the credential graph and computes a score
5. Brands query the supplier's DID + score at sourcing time via the SupplyProof SDK
6. Brands can set **ESG thresholds** as procurement policy — suppliers below threshold are flagged automatically
7. All credential issuances and queries are logged immutably on Stellar for regulatory audit trails

---

## 📊 The Composable ESG Score

This is what makes SupplyProof structurally different from any existing solution.

### Score Scale: 0 – 1000

| Score Range | Rating | Meaning |
|---|---|---|
| 850 – 1000 | **Platinum** | Exemplary ESG posture; audit-ready for strictest global standards |
| 700 – 849 | **Gold** | Strong compliance; minor gaps in tertiary supply chain tiers |
| 550 – 699 | **Silver** | Adequate compliance; improvement plan required for premium buyers |
| 400 – 549 | **Bronze** | Partial compliance; significant Scope 3 gaps exist |
| 0 – 399 | **Unrated / At Risk** | Insufficient verified credentials; high greenwashing liability |

### Score Components

```
┌─────────────────────────────────────────────────────────────────┐
│                  ESG Score Component Weights                    │
├─────────────────────────────────────────────────────────────────┤
│  Carbon Footprint & Emissions      ████████████████████  30%  │
│  Labor & Human Rights              ██████████████        25%  │
│  Environmental Certifications      ████████████          20%  │
│  Governance & Anti-Corruption      ████████              15%  │
│  Supply Chain Tier Coverage        ██████                10%  │
└─────────────────────────────────────────────────────────────────┘
```

**Carbon Footprint & Emissions (30%)**
Verified Scope 1, 2, and 3 GHG emissions data; year-over-year reduction trajectory; carbon offset verification; CBAM-relevant embedded carbon intensity per product unit; Science Based Targets (SBTi) commitments.

**Labor & Human Rights (25%)**
SA8000 and SMETA audit results; living wage compliance; no child or forced labor attestations; worker grievance mechanism verification; Modern Slavery Act compliance declarations.

**Environmental Certifications (20%)**
ISO 14001 Environmental Management System; water stewardship (AWS certification); deforestation-free sourcing (FSC, RSPO, RTRS); hazardous waste management certifications; biodiversity impact assessments.

**Governance & Anti-Corruption (15%)**
ISO 37001 Anti-Bribery Management System; beneficial ownership transparency; trade compliance (sanctions screening); whistleblower protection attestations; board ESG oversight verification.

**Supply Chain Tier Coverage (10%)**
Percentage of upstream Tier 1, 2, and 3 suppliers also registered on SupplyProof. A supplier with fully credentialed upstream chains scores higher, incentivizing cascade adoption through the supply network.

### Composability: The Tier Aggregation Model

When a brand queries a Tier-1 supplier's score, SupplyProof recursively aggregates across all registered upstream suppliers:

```
Brand
  └── Tier 1 Supplier (Score: 820)
        ├── Tier 2 Supplier A (Score: 750)  ← weighted 40% into Tier 1 score
        ├── Tier 2 Supplier B (Score: 690)  ← weighted 35%
        └── Tier 2 Supplier C (not on SupplyProof) ← penalized, caps Tier-1 score
              └── Tier 3 Raw Material (Score: 880) ← weighted 25% into Tier 2
```

Suppliers have a direct financial incentive to onboard their own suppliers — their score is penalized for unregistered upstream entities.

---

## 🏗 Architecture

```
┌──────────────────────────────────────────────────────────────────────────┐
│                         SupplyProof Protocol                             │
│                                                                          │
│  ┌──────────────┐   ┌──────────────┐   ┌───────────────────────────┐   │
│  │   Frontend   │   │   Backend    │   │    Soroban Contracts       │   │
│  │  (Next.js)   │◄─►│  (Node.js /  │◄─►│  Identity | Score |       │   │
│  │              │   │  TypeScript) │   │  Registry | Governance     │   │
│  └──────────────┘   └──────┬───────┘   └───────────────────────────┘   │
│                             │                        │                   │
│                    ┌────────┴────────┐      ┌────────┴────────┐         │
│                    │  Data Layer     │      │  Stellar Network │         │
│                    │  PostgreSQL     │      │  Horizon API     │         │
│                    │  Redis          │      │  Soroban RPC     │         │
│                    │  IPFS / Arweave │      │  Event Stream    │         │
│                    └─────────────────┘      └─────────────────┘         │
│                                                                          │
│  ┌──────────────┐   ┌──────────────┐   ┌───────────────────────────┐   │
│  │  Auditor     │   │  Oracle      │   │  Brand / Buyer SDK        │   │
│  │  Portal      │   │  Adapters    │   │  (@supplyproof/sdk)        │   │
│  │  (Next.js)   │   │  IoT, GHG    │   │  REST + Soroban client     │   │
│  └──────────────┘   └──────────────┘   └───────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────────┘
```

### Protocol Layers

```
Layer 5: Consumer Layer       [Brand Portal, Supplier Portal, Auditor Portal, Public API]
Layer 4: SDK Layer            [@supplyproof/sdk — npm package for buyers and integrators]
Layer 3: Service Layer        [Score API, DID Resolver, Credential Indexer, Oracle Bridge]
Layer 2: Contract Layer       [Identity, Score, Registry, Governance Soroban contracts]
Layer 1: Stellar Network      [Soroban, Ledger, Horizon API, SEP-0010 Auth, Event Streams]
Layer 0: Storage              [IPFS for audit reports, Arweave for permanent archival]
```

---

## 📁 Repository Structure

```
supplyproof/
│
├── contracts/                              # Soroban smart contracts (Rust)
│   │
│   ├── identity/                           # Supplier DID & credential management
│   │   ├── src/
│   │   │   ├── lib.rs                      # Contract entry point & function dispatch
│   │   │   ├── did.rs                      # DID creation, update, deactivation
│   │   │   ├── credentials.rs              # ESG credential storage & revocation
│   │   │   ├── disclosure.rs               # Selective disclosure rules per credential
│   │   │   ├── attestation.rs              # Issuer attestation logic & signatures
│   │   │   ├── supply_chain.rs             # Supplier ↔ supplier linkage (tier graph)
│   │   │   └── errors.rs                   # Custom error types
│   │   ├── Cargo.toml
│   │   └── README.md
│   │
│   ├── esg-score/                          # Composable ESG scoring engine
│   │   ├── src/
│   │   │   ├── lib.rs                      # Contract entry point
│   │   │   ├── score.rs                    # Core scoring algorithm
│   │   │   ├── components/
│   │   │   │   ├── carbon.rs               # Carbon & emissions component
│   │   │   │   ├── labor.rs                # Labor & human rights component
│   │   │   │   ├── environment.rs          # Environmental certifications component
│   │   │   │   ├── governance.rs           # Governance & anti-corruption component
│   │   │   │   └── tier_coverage.rs        # Supply chain tier coverage component
│   │   │   ├── aggregation.rs              # Recursive tier aggregation algorithm
│   │   │   ├── snapshot.rs                 # Historical score snapshots
│   │   │   └── oracle.rs                   # Oracle data interface
│   │   ├── Cargo.toml
│   │   └── README.md
│   │
│   ├── registry/                           # Trusted auditor & schema registry
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── auditors.rs                 # Trusted auditor/certifier management
│   │   │   ├── schemas.rs                  # ESG credential schema definitions
│   │   │   └── standards.rs                # Recognized ESG standard mappings
│   │   ├── Cargo.toml
│   │   └── README.md
│   │
│   ├── governance/                         # Protocol governance
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── proposals.rs                # Proposal creation & lifecycle
│   │   │   ├── voting.rs                   # Token-weighted voting
│   │   │   └── timelock.rs                 # 48h timelock enforcement
│   │   ├── Cargo.toml
│   │   └── README.md
│   │
│   └── shared/                             # Shared types across all contracts
│       ├── src/
│       │   ├── lib.rs
│       │   ├── types.rs                    # DID, Credential, Score shared types
│       │   ├── standards.rs                # ESG standard enum (ISO14001, SA8000...)
│       │   └── constants.rs                # Score weights, thresholds
│       └── Cargo.toml
│
├── backend/                                # Node.js / TypeScript backend
│   ├── src/
│   │   │
│   │   ├── api/                            # REST API (Express.js)
│   │   │   ├── v1/
│   │   │   │   ├── identity.ts             # Supplier DID endpoints
│   │   │   │   ├── score.ts                # ESG score query endpoints
│   │   │   │   ├── credentials.ts          # Credential management endpoints
│   │   │   │   ├── registry.ts             # Auditor registry endpoints
│   │   │   │   ├── supply-chain.ts         # Tier linkage endpoints
│   │   │   │   └── webhooks.ts             # Brand/buyer webhook subscriptions
│   │   │   └── router.ts
│   │   │
│   │   ├── services/                       # Core business logic
│   │   │   ├── did-resolver.ts             # did:supplyproof resolution
│   │   │   ├── score-engine.ts             # Off-chain score assist & caching
│   │   │   ├── tier-graph.ts               # Supply chain graph traversal
│   │   │   ├── credential-verifier.ts      # Credential signature verification
│   │   │   ├── ipfs.ts                     # IPFS audit report storage/retrieval
│   │   │   ├── arweave.ts                  # Permanent archival service
│   │   │   ├── stellar-indexer.ts          # Soroban event listener & indexer
│   │   │   └── oracle-bridge.ts            # IoT / GHG data oracle adapter
│   │   │
│   │   ├── oracles/                        # External data feed adapters
│   │   │   ├── ghg-protocol.ts             # GHG Protocol data ingestion
│   │   │   ├── cdp.ts                      # CDP (Carbon Disclosure Project) API
│   │   │   ├── sbti.ts                     # Science Based Targets initiative API
│   │   │   └── iot-adapter.ts              # Generic IoT sensor data adapter
│   │   │
│   │   ├── workers/                        # Background jobs (BullMQ)
│   │   │   ├── score-refresh.ts            # Periodic score recomputation
│   │   │   ├── credential-expiry.ts        # Expiry monitoring & alerts
│   │   │   ├── tier-sync.ts                # Supply chain graph sync
│   │   │   └── event-listener.ts           # Real-time Soroban event processor
│   │   │
│   │   ├── models/                         # Prisma database models
│   │   │   ├── supplier.model.ts           # Supplier entity
│   │   │   ├── credential.model.ts         # ESG credential record
│   │   │   ├── score-snapshot.model.ts     # Historical score snapshots
│   │   │   ├── auditor.model.ts            # Trusted auditor registry
│   │   │   ├── tier-link.model.ts          # Supplier ↔ supplier relationships
│   │   │   └── brand.model.ts              # Brand/buyer accounts
│   │   │
│   │   ├── middleware/
│   │   │   ├── auth.ts                     # SEP-0010 JWT auth
│   │   │   ├── auditor-auth.ts             # Auditor-specific auth middleware
│   │   │   ├── rate-limit.ts
│   │   │   └── logger.ts
│   │   │
│   │   └── utils/
│   │       ├── stellar.ts                  # Stellar SDK helpers
│   │       ├── crypto.ts                   # Signature verification utilities
│   │       └── score-weights.ts            # Score weight constants & calculators
│   │
│   ├── prisma/
│   │   └── schema.prisma
│   ├── package.json
│   ├── tsconfig.json
│   └── .env.example
│
├── frontend/                               # Next.js 14 frontend (App Router)
│   ├── src/
│   │   ├── app/
│   │   │   ├── page.tsx                    # Landing page
│   │   │   │
│   │   │   ├── supplier/                   # Supplier-facing portal
│   │   │   │   ├── dashboard/page.tsx      # Supplier dashboard & score overview
│   │   │   │   ├── register/page.tsx       # DID registration wizard
│   │   │   │   ├── credentials/page.tsx    # Credential status & management
│   │   │   │   ├── supply-chain/page.tsx   # Link & manage upstream suppliers
│   │   │   │   └── reports/page.tsx        # Audit report upload & history
│   │   │   │
│   │   │   ├── brand/                      # Brand/buyer portal
│   │   │   │   ├── dashboard/page.tsx      # Brand sourcing dashboard
│   │   │   │   ├── verify/page.tsx         # Supplier ESG verification tool
│   │   │   │   ├── watchlist/page.tsx      # Monitor supplier scores over time
│   │   │   │   ├── thresholds/page.tsx     # Set ESG procurement thresholds
│   │   │   │   └── reports/page.tsx        # Regulatory export (CSRD, CBAM)
│   │   │   │
│   │   │   ├── auditor/                    # Auditor/certifier portal
│   │   │   │   ├── dashboard/page.tsx      # Pending & issued credentials
│   │   │   │   ├── issue/page.tsx          # Issue new credential wizard
│   │   │   │   ├── revoke/page.tsx         # Revoke compromised credentials
│   │   │   │   └── registry/page.tsx       # Auditor profile & scope of accreditation
│   │   │   │
│   │   │   ├── explore/page.tsx            # Public supplier explorer (anonymized)
│   │   │   └── governance/page.tsx         # Protocol governance & voting
│   │   │
│   │   ├── components/
│   │   │   ├── ui/                         # shadcn/ui base components
│   │   │   ├── score/
│   │   │   │   ├── ESGScoreGauge.tsx        # Animated score dial (0–1000)
│   │   │   │   ├── ComponentBreakdown.tsx  # Radar/spider chart of score components
│   │   │   │   ├── TierTreeMap.tsx          # Supply chain tier visualization
│   │   │   │   └── ScoreHistory.tsx         # Line chart of score over time
│   │   │   ├── credentials/
│   │   │   │   ├── CredentialCard.tsx       # Individual credential display
│   │   │   │   ├── CredentialTimeline.tsx   # Issuance/expiry timeline
│   │   │   │   └── IssuerBadge.tsx          # Trusted auditor badge
│   │   │   ├── supply-chain/
│   │   │   │   ├── TierGraph.tsx            # Interactive D3 supply chain graph
│   │   │   │   └── SupplierSearchModal.tsx
│   │   │   └── wallet/
│   │   │       └── WalletConnect.tsx        # Stellar wallet integration
│   │   │
│   │   ├── hooks/
│   │   │   ├── useStellarWallet.ts
│   │   │   ├── useSupplierDID.ts
│   │   │   ├── useESGScore.ts
│   │   │   ├── useTierGraph.ts
│   │   │   └── useContracts.ts
│   │   │
│   │   └── lib/
│   │       ├── stellar.ts
│   │       ├── soroban.ts
│   │       └── api.ts
│   │
│   ├── next.config.js
│   ├── tailwind.config.ts
│   └── package.json
│
├── sdk/                                    # @supplyproof/sdk (npm package)
│   ├── src/
│   │   ├── index.ts                        # Public exports
│   │   ├── supplier-client.ts              # Supplier DID operations
│   │   ├── score-client.ts                 # ESG score queries
│   │   ├── credential-client.ts            # Credential verification
│   │   ├── tier-client.ts                  # Supply chain graph queries
│   │   └── types.ts                        # All exported types
│   ├── package.json
│   └── README.md
│
├── docs/
│   ├── architecture.md
│   ├── did-method-spec.md                  # did:supplyproof specification
│   ├── esg-scoring-model.md                # Full scoring model documentation
│   ├── credential-schemas/                 # JSON schemas for each credential type
│   │   ├── carbon-footprint.json
│   │   ├── labor-compliance.json
│   │   ├── iso-14001.json
│   │   └── ...
│   ├── api-reference.md
│   ├── integration-guide.md
│   └── regulatory-mapping.md               # CSRD, CBAM, SEC mapping guide
│
├── scripts/
│   ├── deploy-contracts.sh
│   ├── seed-auditors.ts                    # Seed trusted auditor registry
│   ├── seed-standards.ts                   # Seed ESG standard schemas
│   └── migrate-db.sh
│
├── .github/
│   └── workflows/
│       ├── test.yml
│       ├── deploy-testnet.yml
│       └── deploy-mainnet.yml
│
├── docker-compose.yml
├── docker-compose.prod.yml
├── Makefile
└── README.md
```

---

## 📜 Smart Contracts (Soroban)

All contracts are written in **Rust** and deployed to **Stellar's Soroban** smart contract platform. The contract set is designed to be composable — external protocols (lending, insurance, trade finance) can read ESG scores as a primitive.

---

### 1. Identity Contract (`contracts/identity/`)

Creates and manages supplier DIDs and their associated ESG credential sets.

**Key Functions:**

```rust
/// Register a new supplier DID
fn register_supplier(
    env: Env,
    owner: Address,
    entity_name: String,
    country_of_registration: String,
    industry_codes: Vec<NACECode>,
) -> DIDDocument;

/// Add a verified ESG credential to a supplier DID
fn add_credential(
    env: Env,
    supplier: Address,
    issuer: Address,                    // must be in trusted registry
    credential_type: ESGCredentialType,
    standard: ESGStandard,
    scope: CredentialScope,
    credential_hash: BytesN<32>,        // SHA-256 of full audit report
    ipfs_cid: String,                   // encrypted report on IPFS
    issued_at: u64,
    expires_at: Option<u64>,
) -> CredentialId;

/// Revoke a credential (issuer-only)
fn revoke_credential(
    env: Env,
    issuer: Address,
    credential_id: CredentialId,
    revocation_reason: RevocationReason,
);

/// Link an upstream supplier (creates tier graph edge)
fn link_upstream_supplier(
    env: Env,
    downstream: Address,
    upstream: Address,
    relationship_type: SupplierRelationship,
    spend_percentage: u32,             // % of COGS from this supplier
);

/// Resolve full DID document
fn resolve(env: Env, supplier: Address) -> DIDDocument;

/// Get all active credentials for a supplier
fn get_credentials(
    env: Env,
    supplier: Address,
    credential_type: Option<ESGCredentialType>,
) -> Vec<ESGCredential>;
```

**Core Data Structures:**

```rust
pub struct DIDDocument {
    pub did: String,                           // did:supplyproof:G...
    pub controller: Address,
    pub entity_name: String,
    pub country: String,
    pub industry_codes: Vec<NACECode>,
    pub active_credentials: Vec<CredentialRef>,
    pub upstream_suppliers: Vec<SupplierLink>,
    pub registered_at: u64,
    pub last_updated: u64,
    pub status: SupplierStatus,               // Active | Suspended | Deactivated
}

pub enum ESGCredentialType {
    // Carbon
    GHGInventoryScope1,
    GHGInventoryScope2,
    GHGInventoryScope3,
    CarbonOffsetVerification,
    ScienceBasedTarget,
    CBAMProductCarbon,                        // EU CBAM embedded carbon
    // Labor
    SA8000Certification,
    SMETAAudit,
    LivingWageAttestation,
    ModernSlaveryCompliance,
    // Environment
    ISO14001Certification,
    WaterStewardship,
    DeforestationFree,
    HazardousWasteCompliance,
    // Governance
    ISO37001Certification,
    BeneficialOwnershipDisclosure,
    SanctionsScreeningClearance,
    // Custom
    Custom(String),
}

pub enum ESGStandard {
    GHGProtocol,
    ISO14064,
    TCFD,
    GRI,
    SASB,
    SBTI,
    SA8000,
    SMETA,
    ISO14001,
    ISO37001,
    CBAM,
    EUCSRD,
    Custom(String),
}
```

---

### 2. ESG Score Contract (`contracts/esg-score/`)

The algorithmic scoring engine. Reads credential graphs and computes the Composable ESG Score.

**Key Functions:**

```rust
/// Compute and store the ESG score for a supplier
fn compute_score(
    env: Env,
    subject: Address,
    include_tiers: bool,               // whether to recurse into upstream graph
    max_tier_depth: u32,               // how many tiers deep to aggregate (max: 5)
) -> ESGScore;

/// Get current score (from latest snapshot)
fn get_score(env: Env, subject: Address) -> Option<ESGScore>;

/// Get score history
fn get_score_history(
    env: Env,
    subject: Address,
    from: Option<u64>,
    limit: u32,
) -> Vec<ScoreSnapshot>;

/// Get the full score breakdown with per-component explanations
fn get_score_report(env: Env, subject: Address) -> ESGScoreReport;

/// Check whether a supplier meets a buyer's ESG threshold policy
fn meets_threshold(
    env: Env,
    supplier: Address,
    policy: BuyerESGPolicy,
) -> ThresholdResult;
```

**Score Data Structures:**

```rust
pub struct ESGScore {
    pub subject: Address,
    pub score: u32,                           // 0–1000
    pub rating: ESGRating,                    // Platinum | Gold | Silver | Bronze | AtRisk
    pub components: ScoreComponents,
    pub tier_aggregation: TierAggregation,
    pub data_points: u32,
    pub credential_count: u32,
    pub last_computed: u64,
    pub valid_until: u64,                     // score expires if credentials expire
}

pub struct ScoreComponents {
    pub carbon:        ComponentScore,         // 300 pts max (30%)
    pub labor:         ComponentScore,         // 250 pts max (25%)
    pub environment:   ComponentScore,         // 200 pts max (20%)
    pub governance:    ComponentScore,         // 150 pts max (15%)
    pub tier_coverage: ComponentScore,         // 100 pts max (10%)
}

pub struct ComponentScore {
    pub score: u32,
    pub max_score: u32,
    pub verified_standards: Vec<ESGStandard>,
    pub missing_standards: Vec<ESGStandard>,
    pub last_audit_date: Option<u64>,
    pub next_expiry: Option<u64>,
}

pub struct TierAggregation {
    pub own_score: u32,
    pub tier1_weighted_score: Option<u32>,
    pub tier2_weighted_score: Option<u32>,
    pub tier3_weighted_score: Option<u32>,
    pub unregistered_supplier_penalty: u32,
    pub total_coverage_percentage: u32,
}

pub struct BuyerESGPolicy {
    pub minimum_total_score: Option<u32>,
    pub minimum_carbon_score: Option<u32>,
    pub minimum_labor_score: Option<u32>,
    pub required_credentials: Vec<ESGCredentialType>,
    pub required_standards: Vec<ESGStandard>,
    pub max_unregistered_tier_percentage: Option<u32>,
}
```

---

### 3. Registry Contract (`contracts/registry/`)

Manages trusted auditors, certifiers, and credential schemas.

**Key Functions:**

```rust
/// Register a trusted auditor (governance-gated)
fn register_auditor(
    env: Env,
    admin: Address,
    auditor: Address,
    name: String,
    accreditation_body: String,         // e.g. "UKAS", "DAkkS", "COFRAC"
    accreditation_id: String,
    authorized_credential_types: Vec<ESGCredentialType>,
    authorized_standards: Vec<ESGStandard>,
    geographic_scope: Vec<String>,      // ISO 3166-1 country codes
);

/// Check if an auditor is authorized for a specific credential type
fn is_authorized_auditor(
    env: Env,
    auditor: Address,
    credential_type: ESGCredentialType,
    standard: ESGStandard,
    country: String,
) -> bool;

/// Register a credential schema
fn register_schema(
    env: Env,
    schema: CredentialSchema,
    regulatory_mappings: Vec<RegulatoryMapping>,
) -> SchemaId;

/// Get all authorized auditors for a credential type
fn get_auditors_for_credential(
    env: Env,
    credential_type: ESGCredentialType,
    country: Option<String>,
) -> Vec<AuditorProfile>;
```

---

### 4. Governance Contract (`contracts/governance/`)

All protocol parameter changes — score weights, auditor additions, fee structures — require on-chain governance.

**Governable Parameters:**

```rust
pub struct ProtocolParameters {
    pub score_weights: ScoreWeights,
    pub tier_aggregation_weights: TierWeights,
    pub credential_expiry_grace_period: u64,
    pub unregistered_tier_penalty: u32,
    pub auditor_registration_fee: i128,
    pub score_query_fee: i128,                // charged to brands (in XLM)
    pub min_governance_quorum: u32,           // percentage of supply
    pub proposal_timelock_seconds: u64,
}
```

---

### Building & Testing Contracts

```bash
# Install Stellar CLI
cargo install --locked stellar-cli --features opt

# Build all contracts
cd contracts && make build

# Run unit tests
make test

# Run integration tests (requires local Stellar node)
stellar network start local
make test-integration

# Generate contract bindings (TypeScript)
make bindings

# Deploy to testnet
make deploy-testnet

# Deploy to mainnet (requires multisig)
make deploy-mainnet
```

---

## 🖥 Backend Services

The backend is a **Node.js / TypeScript** service using **Express.js**, **Prisma** (PostgreSQL), **BullMQ** (Redis) for job queues, and **IPFS / Arweave** for decentralized document storage.

### Service Overview

| Service | Responsibility |
|---|---|
| `DIDResolverService` | Resolves `did:supplyproof:G...` to full DID documents |
| `ScoreEngineService` | Caches and serves ESG scores; triggers recomputation on credential events |
| `TierGraphService` | Traverses the supplier–supplier graph; computes aggregated tier scores |
| `CredentialVerifierService` | Verifies issuer signatures and on-chain credential integrity |
| `IPFSService` | Stores encrypted audit reports; returns content-addressed CIDs |
| `ArweaveService` | Permanent archival of issued credentials for regulatory retention |
| `StellarIndexerService` | Streams and indexes Soroban contract events in real-time |
| `OracleBridgeService` | Adapters for GHG Protocol, CDP, SBTi, and IoT data feeds |
| `WebhookService` | Notifies brands on score changes or credential events for watched suppliers |

### Key API Endpoints

```
# Supplier Identity
GET    /api/v1/supplier/:address                    # Resolve DID document
POST   /api/v1/supplier/register                    # Register new supplier DID
PATCH  /api/v1/supplier/:address                    # Update supplier metadata
POST   /api/v1/supplier/:address/link               # Link upstream supplier

# ESG Credentials
GET    /api/v1/supplier/:address/credentials        # List all credentials
POST   /api/v1/credentials/issue                    # Issue credential (auditors only)
DELETE /api/v1/credentials/:id                      # Revoke credential (auditors only)
GET    /api/v1/credentials/:id/report               # Retrieve audit report from IPFS

# ESG Score
GET    /api/v1/score/:address                       # Get current ESG score
GET    /api/v1/score/:address/report                # Full score breakdown
GET    /api/v1/score/:address/history               # Score history
GET    /api/v1/score/:address/tiers                 # Tier-aggregated score breakdown
POST   /api/v1/score/threshold-check                # Check supplier against buyer policy

# Supply Chain Graph
GET    /api/v1/supply-chain/:address/graph          # Full tier graph for a supplier
GET    /api/v1/supply-chain/:address/coverage       # Registration coverage by tier

# Registry
GET    /api/v1/registry/auditors                    # List trusted auditors
GET    /api/v1/registry/auditors/:address           # Auditor profile & scope
GET    /api/v1/registry/schemas                     # Credential schema catalog
GET    /api/v1/registry/standards                   # Recognized ESG standards

# Brand / Buyer
POST   /api/v1/brand/watchlist                      # Add supplier to watchlist
GET    /api/v1/brand/watchlist                      # Get watchlist with scores
POST   /api/v1/brand/policy                         # Set ESG procurement policy
GET    /api/v1/brand/report/csrd                    # Export CSRD-formatted report
GET    /api/v1/brand/report/cbam                    # Export CBAM declaration data
```

### Running the Backend

```bash
cd backend
npm install
cp .env.example .env         # configure environment
npm run db:migrate            # apply Prisma migrations
npm run db:seed               # seed auditors and schemas
npm run dev                   # http://localhost:4000
npm run test
npm run test:coverage
```

---

## 🎨 Frontend Application

Built with **Next.js 14** (App Router), **Tailwind CSS**, and **shadcn/ui**.  
Three distinct portals: **Supplier**, **Brand/Buyer**, and **Auditor** — each with role-gated access via Stellar wallet signatures (SEP-0010).

### Supplier Portal Features
- **DID Registration Wizard** — step-by-step entity registration with NACE code selection
- **ESG Score Dashboard** — animated score gauge (0–1000), component radar chart, tier tree
- **Credential Manager** — status, expiry countdown, and renewal prompts per credential
- **Audit Report Vault** — encrypted report storage and access control
- **Supply Chain Manager** — link upstream suppliers, view tier coverage gaps

### Brand / Buyer Portal Features
- **Supplier Verification Tool** — enter any Stellar address, get instant ESG score + credential list
- **Supplier Watchlist** — track scores across your supplier network with alert subscriptions
- **Procurement Policy Engine** — set minimum score thresholds and required credentials
- **Regulatory Report Export** — one-click CSRD, CBAM, and SEC climate disclosure exports
- **Tier Transparency View** — visual D3 graph of your full supply chain with ESG color-coding

### Auditor Portal Features
- **Credential Issuance Wizard** — guided flow to attach audit results to a supplier DID
- **Active Credential Dashboard** — all issued credentials with expiry monitoring
- **Revocation Manager** — immediately revoke compromised or expired certifications
- **Registry Profile** — manage accreditation scope, geographic authorization, and public profile

### Running the Frontend

```bash
cd frontend
npm install
cp .env.example .env.local
npm run dev        # http://localhost:3000
npm run build
npm run test
npm run test:e2e
```

---

## 🔄 Protocol Flows

### Flow 1: Supplier Onboarding & Credential Issuance

```
Supplier           SupplyProof Frontend    Backend              Identity Contract
    │                      │                  │                        │
    │  Connect Stellar wallet                 │                        │
    │─────────────────────►│                  │                        │
    │  Fill entity details  │                  │                        │
    │─────────────────────►│  POST /register  │                        │
    │                       │─────────────────►│  register_supplier()  │
    │                       │                  │──────────────────────►│
    │                       │                  │◄── DIDDocument        │
    │  DID: did:supplyproof:G...               │                        │
    │◄──────────────────────│                  │                        │
    │                       │                  │                        │
    │  Upload ISO 14001 cert│                  │                        │
    │─────────────────────►│  POST /report    │                        │
    │                       │─────────────────►│  store on IPFS        │
    │                       │                  │◄── CID                │
    │                       │                  │                        │

Auditor (SGS)      Auditor Portal          Backend              Identity Contract
    │                      │                  │                        │
    │  Log in, find supplier│                  │                        │
    │─────────────────────►│                  │                        │
    │  Issue ISO14001 cred  │                  │                        │
    │─────────────────────►│ POST /credentials/issue                   │
    │                       │─────────────────►│  verify auditor auth  │
    │                       │                  │  add_credential()     │
    │                       │                  │──────────────────────►│
    │                       │                  │  ◄── CredentialId     │
    │  Credential issued ✓  │◄─────────────────│                        │
    │◄──────────────────────│                  │                        │
```

### Flow 2: Brand ESG Verification at Sourcing

```
Brand              Brand Portal        Score API           Score Contract
    │                   │                  │                     │
    │  Enter supplier address              │                     │
    │──────────────────►│                  │                     │
    │                    │  GET /score/:addr│                     │
    │                    │─────────────────►│  get_score()       │
    │                    │                  │────────────────────►│
    │                    │                  │◄── ESGScore         │
    │                    │  GET /score/:addr/tiers               │
    │                    │─────────────────►│  compute tiers      │
    │                    │                  │  (recursive graph)  │
    │                    │◄─────────────────│                     │
    │  Score: 847 Gold   │                  │                     │
    │  Carbon: 285/300   │                  │                     │
    │  Labor: 230/250    │                  │                     │
    │  Tier 1 coverage: 94%                │                     │
    │  Tier 2 coverage: 71%                │                     │
    │◄───────────────────│                  │                     │
    │                    │                  │                     │
    │  Meets policy? ✓   │                  │                     │
    │  → Approve supplier│                  │                     │
```

### Flow 3: Credential Expiry & Score Decay

```
Background Worker       Score Contract       Brand Webhook
       │                      │                    │
       │  Daily expiry scan   │                    │
       │─────────────────────►│                    │
       │  Finds: SA8000 exp   │                    │
       │  in 30 days          │                    │
       │                      │                    │
       │  Recompute score      │                    │
       │  (reduced by pending  │                    │
       │  expiry penalty)      │                    │
       │─────────────────────►│                    │
       │                       │                   │
       │  Notify affected brands────────────────►  │
       │                       │  Score Alert:      │
       │                       │  Supplier G...     │
       │                       │  Score: 847 → 801  │
       │                       │  Reason: SA8000    │
       │                       │  expiring in 30d   │
```

---

## 🏭 Supported Verticals

SupplyProof is purpose-built for **carbon-intensive industries** facing the sharpest regulatory pressure.

### ⚡ Energy & Utilities
Critical for CBAM compliance. Credentials: GHG inventory (Scope 1–3), renewable energy certification (I-REC, REGO), methane leak attestation, decommissioning plans. Buyers: utilities, grid operators, industrial offtakers.

### 🏗 Heavy Manufacturing & Steel
The highest-scrutiny CBAM category. Credentials: embedded carbon per ton (GHG Protocol), blast furnace vs. electric arc attestation, ISO 14001, water discharge compliance. Buyers: automotive OEMs, construction companies, defense contractors.

### 🚢 Shipping & Logistics
IMO 2030/2050 targets create acute credential needs. Credentials: vessel carbon intensity (CII rating), fuel type attestation (bio/LNG/ammonia), port emissions compliance, modern slavery in crew management. Buyers: retailers, commodity traders, ports.

### 🌾 Agriculture & Food
Deforestation-free sourcing under EU law from 2025. Credentials: EUDR deforestation-free attestation, FSC/RSPO certification, water stewardship (AWS), GHG emissions per kg of product, fair trade labor. Buyers: FMCG brands, food manufacturers, retailers.

### 🏗 Construction & Building Materials
Cement and concrete are responsible for 8% of global CO₂. Credentials: embodied carbon declaration (EPD), low-carbon cement specification, BREEAM/LEED material compliance, aggregate sourcing attestation. Buyers: property developers, infrastructure contractors, governments.

---

## 📋 Credential Types

A comprehensive catalog of all ESG credentials the protocol supports:

### Carbon & Climate

| Credential | Standard | Issuer Type | CBAM Relevant |
|---|---|---|---|
| Scope 1 GHG Inventory | GHG Protocol / ISO 14064 | Accredited verifier | ✅ |
| Scope 2 GHG Inventory | GHG Protocol / ISO 14064 | Accredited verifier | ✅ |
| Scope 3 GHG Inventory | GHG Protocol | Accredited verifier | ⚠️ |
| Product Carbon Footprint | PAS 2050 / ISO 14067 | Accredited verifier | ✅ |
| Science Based Target | SBTi | SBTi directly | ❌ |
| Carbon Offset Verification | Gold Standard / VCS | Accredited verifier | ❌ |
| Renewable Energy Certificate | I-REC / REGO | Issuing body | ❌ |

### Labor & Human Rights

| Credential | Standard | Issuer Type |
|---|---|---|
| Social Audit | SMETA (2 or 4 pillar) | SMETA-licensed auditor |
| Social Management System | SA8000 | SAI-accredited auditor |
| Living Wage Attestation | WageIndicator | Accredited HR auditor |
| Modern Slavery Compliance | UK MSA / AUS MSA | Compliance firm |
| Responsible Recruitment | Responsible Business Alliance | RBA-approved auditor |

### Environmental

| Credential | Standard | Issuer Type |
|---|---|---|
| Environmental Management | ISO 14001 | UKAS/IAF-accredited CB |
| Deforestation Free (Timber) | FSC Chain of Custody | FSC-accredited CB |
| Deforestation Free (Palm) | RSPO | RSPO-approved auditor |
| EUDR Compliance | EU Deforestation Regulation | Registered verifier |
| Water Stewardship | AWS Standard | AWS-licensed verifier |

### Governance

| Credential | Standard | Issuer Type |
|---|---|---|
| Anti-Bribery Management | ISO 37001 | Accredited CB |
| Beneficial Ownership | FATF / local law | Registered legal firm |
| Sanctions Screening | OFAC / EU / UN lists | Compliance provider |
| Conflict Minerals | OECD DDG / RMI | RMAP auditor |

---

## 🚀 Getting Started

### Prerequisites

| Tool | Version | Purpose |
|---|---|---|
| Rust | 1.75+ | Soroban contracts |
| `stellar-cli` | latest | Contract deploy & test |
| Node.js | 20.x LTS | Backend & frontend |
| PostgreSQL | 15+ | Database |
| Redis | 7+ | Job queues & caching |
| Docker | 24+ | Local dev stack |
| IPFS (Kubo) | 0.27+ | Credential document storage |

### Quick Start (Docker)

```bash
git clone https://github.com/your-org/supplyproof.git
cd supplyproof

cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env.local

docker-compose up -d

# Frontend:    http://localhost:3000
# API:         http://localhost:4000
# API Docs:    http://localhost:4000/docs
# BullMQ UI:   http://localhost:4001
```

### Manual Setup

```bash
# 1. Build and deploy contracts to testnet
cd contracts
make build
make deploy-testnet
# Copy contract addresses to .env files

# 2. Start backend
cd ../backend
npm install && npm run db:migrate && npm run db:seed
npm run dev

# 3. Start frontend
cd ../frontend
npm install && npm run dev
```

---

## ⚙️ Environment Configuration

### Backend (`backend/.env`)

```env
PORT=4000
NODE_ENV=development

DATABASE_URL=postgresql://postgres:password@localhost:5432/supplyproof
REDIS_URL=redis://localhost:6379

STELLAR_NETWORK=testnet
STELLAR_RPC_URL=https://soroban-testnet.stellar.org
STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org

# Contract addresses (from deployment)
IDENTITY_CONTRACT_ID=C...
SCORE_CONTRACT_ID=C...
REGISTRY_CONTRACT_ID=C...
GOVERNANCE_CONTRACT_ID=C...

# Storage
IPFS_API_URL=http://localhost:5001
IPFS_GATEWAY_URL=http://localhost:8080
ARWEAVE_WALLET_PATH=./arweave-keyfile.json

# Oracle integrations
CDP_API_KEY=
SBTI_API_KEY=
GHG_PROTOCOL_API_KEY=

# Auth
JWT_SECRET=your-secret
JWT_EXPIRY=7d

# Webhooks
WEBHOOK_SECRET=your-webhook-secret
```

### Frontend (`frontend/.env.local`)

```env
NEXT_PUBLIC_API_URL=http://localhost:4000/api/v1
NEXT_PUBLIC_STELLAR_NETWORK=testnet
NEXT_PUBLIC_STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org
NEXT_PUBLIC_IPFS_GATEWAY=http://localhost:8080

NEXT_PUBLIC_IDENTITY_CONTRACT_ID=C...
NEXT_PUBLIC_SCORE_CONTRACT_ID=C...
NEXT_PUBLIC_REGISTRY_CONTRACT_ID=C...
```

---

## 🚢 Deployment

### Testnet (Automated — CI/CD)

Pushed to `develop` branch → GitHub Actions deploys contracts and backend to testnet automatically.

### Mainnet

Pushed to `main` → requires:
1. Passing governance vote (>50% quorum)
2. 48-hour timelock after vote passes
3. 3-of-5 protocol multisig signature
4. Successful automated security checks

```bash
# Manual mainnet deploy (emergency only)
STELLAR_SECRET_KEY=<COLD_KEY> NETWORK=mainnet make deploy-mainnet
```

---

## 📡 API Reference

Full OpenAPI 3.0 spec available at `/docs` when running locally.

### SDK Quick Reference (`@supplyproof/sdk`)

```typescript
import { SupplyProof } from '@supplyproof/sdk';

const client = new SupplyProof({
  network: 'mainnet',
  apiKey: 'your-api-key',
});

// Get a supplier's full ESG score
const score = await client.score.get('GAHT...');
console.log(score);
// {
//   score: 847,
//   rating: "Gold",
//   components: {
//     carbon:      { score: 285, max: 300, standards: ["GHGProtocol", "ISO14064"] },
//     labor:       { score: 230, max: 250, standards: ["SA8000", "SMETA"] },
//     environment: { score: 190, max: 200, standards: ["ISO14001", "FSC"] },
//     governance:  { score: 112, max: 150, standards: ["ISO37001"] },
//     tierCoverage:{ score: 30,  max: 100, coveragePercent: 71 }
//   },
//   tierAggregation: {
//     ownScore: 820,
//     tier1WeightedScore: 780,
//     tier2WeightedScore: 690,
//     unregisteredPenalty: 47
//   },
//   lastComputed: "2025-06-15T12:00:00Z",
//   validUntil: "2025-09-15T12:00:00Z"
// }

// Check if a supplier meets your procurement policy
const check = await client.score.meetsPolicy('GAHT...', {
  minimumTotalScore: 700,
  minimumCarbonScore: 200,
  requiredCredentials: ['GHGInventoryScope1', 'SA8000Certification'],
  maxUnregisteredTierPercentage: 30,
});
// { approved: true, gaps: [], score: 847 }

// Watch a supplier for score changes
client.watchlist.subscribe('GAHT...', (event) => {
  console.log(`Score changed: ${event.previousScore} → ${event.newScore}`);
  console.log(`Reason: ${event.reason}`);
});

// Resolve a supplier's full DID document
const did = await client.identity.resolve('GAHT...');
```

---

## 🧪 Testing

### Contract Tests

```bash
cd contracts
cargo test                         # unit tests
cargo test --features integration  # integration tests (needs local node)
cargo tarpaulin --out Html         # coverage report
```

### Backend Tests

```bash
cd backend
npm run test                       # all tests
npm run test:unit                  # unit tests only
npm run test:integration           # requires PostgreSQL + Redis
npm run test:e2e                   # full API test suite
npm run test:coverage
```

### Frontend Tests

```bash
cd frontend
npm test                           # Vitest component tests
npm run test:e2e                   # Playwright end-to-end
npm run test:visual                # Visual regression
```

---

## 🔒 Security

### Protocol Security Properties

- **Issuer authorization:** Only registry-approved auditors can issue credentials to any DID — self-issuance is cryptographically impossible
- **Credential integrity:** Every credential includes a SHA-256 hash of the underlying audit report; any document tampering is immediately detectable
- **Score determinism:** ESG score computation is fully deterministic and on-chain verifiable; no black-box manipulation
- **Non-repudiation:** All credential issuances carry issuer address + timestamp, permanently on-chain
- **Privacy by design:** No PII or commercially sensitive data is stored on-chain; only hashes and IPFS CIDs
- **Revocation propagation:** Revoking a credential triggers automatic score recomputation within one block finality cycle

### Audits

| Firm | Scope | Status |
|---|---|---|
| *TBD* | Soroban Contracts | Planned Q3 2025 |
| *TBD* | Backend API | Planned Q3 2025 |

**Responsible Disclosure:** `security@supplyproof.io` — bug bounty program details at [supplyproof.io/security](https://supplyproof.io/security)

---

## ⚖️ Regulatory Alignment

SupplyProof is designed to generate evidence for, or directly feed into, the following regulatory frameworks:

| Regulation | Jurisdiction | Effective | SupplyProof Coverage |
|---|---|---|---|
| **CSRD** — Corporate Sustainability Reporting Directive | EU | 2025 (phased) | Full Scope 1–3 credential trail; CSRD report export |
| **CBAM** — Carbon Border Adjustment Mechanism | EU | 2026 (full) | Embedded carbon credentials per product; CBAM declaration export |
| **SEC Climate Disclosure Rule** | US | 2025 | Scope 3 supply chain data; audit trail for material disclosures |
| **UK Modern Slavery Act** | UK | In force | MSA compliance credential; supply chain transparency reporting |
| **EU EUDR** — Deforestation Regulation | EU | 2025 | Deforestation-free credentials; geolocation attestation |
| **German Supply Chain Act (LkSG)** | Germany | In force | Labor compliance credentials; due diligence documentation |
| **Norway Transparency Act** | Norway | In force | Living wage and working conditions attestations |

---

## 🗺 Roadmap

### Phase 1 — Foundation ✅ (Q2–Q3 2026)
- [x] Supplier DID contract (identity + credentials)
- [x] ESG Score contract (single-tier)
- [x] Trusted auditor registry
- [x] Backend API (score, identity, credentials)
- [x] Supplier and Brand portals (MVP)
- [x] Testnet deployment

### Phase 2 — Composability (Q4 2026)
- [ ] Tier aggregation algorithm (Tier 1–3 recursive scoring)
- [ ] Auditor portal + credential issuance flow
- [ ] Oracle adapters (GHG Protocol, CDP, SBTi)
- [ ] Brand procurement policy engine
- [ ] CSRD & CBAM regulatory export
- [ ] `@supplyproof/sdk` npm package v1.0
- [ ] Mainnet launch with 5 anchor auditors (SGS, Bureau Veritas, DNV...)

### Phase 3 — Scale (2027)
- [ ] IoT sensor data oracle (real-time emissions from connected assets)
- [ ] ZK proofs for competitive-sensitive credential disclosure
- [ ] Cross-chain bridges (EVM supply chain protocols ↔ SupplyProof)
- [ ] DAO governance transition
- [ ] Integration with EU CBAM registry API
- [ ] Tier 4–5 aggregation + incentive program for Global South suppliers
- [ ] Mobile app for field auditors

---

## 🤝 Contributing

```bash
git clone https://github.com/your-org/supplyproof.git
git checkout -b feat/my-feature
# make changes, write tests
git commit -m "feat(score): add tier-3 aggregation penalty curve"
git push origin feat/my-feature
# open PR
```

**Commit convention:** `feat | fix | docs | test | refactor | chore (scope): message`

All PRs require: passing CI, test coverage maintained, `cargo clippy` zero warnings, ESLint clean. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📄 License

MIT License — see [LICENSE](LICENSE).  
The SupplyProof name and logo are trademarks. Protocol usage is permissionless; the brand is not.

---

## 🙏 Acknowledgements

- [Stellar Development Foundation](https://stellar.org) — Soroban and the Stellar network
- [W3C DID Working Group](https://www.w3.org/TR/did-core/) — DID specification
- [GHG Protocol](https://ghgprotocol.org) — emissions accounting standards
- [Social Accountability International](https://sa-intl.org) — SA8000 standard
- [Science Based Targets initiative](https://sciencebasedtargets.org) — SBTi framework

---

<div align="center">

**Making ESG claims as verifiable as financial statements.**

[Website](https://supplyproof.io) · [Docs](https://docs.supplyproof.io) · [Discord](https://discord.gg/supplyproof) · [Twitter](https://twitter.com/supplyproof)

</div>
