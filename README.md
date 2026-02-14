# Blinkchain Portal

> **The comprehensive Web3 ecosystem portal** — DeFi staking, validator nodes, NFT marketplace, multichain launchpad, DAO governance, Learn-to-Earn, and token migration — all on Base.

![Architecture Overview](public/images/architecture-overview.png)

---

## Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Tech Stack](#tech-stack)
4. [Smart Contracts](#smart-contracts)
5. [Modules](#modules)
   - [Dashboard](#1-dashboard)
   - [DeFi Staking](#2-defi-staking)
   - [Validator Nodes](#3-validator-nodes)
   - [NFT Marketplace](#4-nft-marketplace)
   - [Launchpad](#5-launchpad)
   - [DAO Governance](#6-dao-governance)
   - [Learn-to-Earn](#7-learn-to-earn)
   - [Token Migration](#8-token-migration)
   - [Vesting & Claims](#9-vesting--claims)
   - [Public Profiles](#10-public-profiles)
6. [Admin & Developer Tools](#admin--developer-tools)
7. [Data Layer](#data-layer)
8. [Internationalization](#internationalization)
9. [RBAC System](#rbac-system)
10. [Routing](#routing)
11. [Project Structure](#project-structure)
12. [Getting Started](#getting-started)

---

## Overview

Blinkchain Portal is a full-featured decentralized application (dApp) built on **Base** (Chain ID: 8453). It serves as the unified interface for the Blink Galaxy ecosystem, providing users with access to DeFi yield strategies, validator node management, an NFT marketplace, a multichain token launchpad, decentralized governance, and an educational Learn-to-Earn program.

### Key Highlights

| Feature | Description |
|---------|-------------|
| **6 Staking Strategies** | Flexible to 24-month lock, with dynamic on-chain APY |
| **Validator Nodes** | ERC-721 Node Key NFTs with staking rewards |
| **NFT Marketplace** | Buy, sell, promote listings with BG tokens |
| **Multichain Launchpad** | Token sales across 7 EVM chains with vesting |
| **DAO Governance** | OpenZeppelin Governor + Treasury + Timelock |
| **Learn-to-Earn** | 100 lessons, quizzes, $BG rewards, Scholar NFT |
| **Token Migration** | GQ → BG migration across 3 stages |
| **Multi-language** | English + Spanish (México 🇲🇽 / España 🇪🇸) |

---

## Architecture

The application follows a strictly separated layer architecture where each module (DeFi, Nodes, NFTs, Launchpad, DAO, Learn) operates independently with no cross-dependencies:

- **DeFi** → Yield only (staking/unstaking)
- **Nodes** → Ownership and mint rights
- **NFTs** → Access keys for the network
- **Launchpad** → Tiered token sale entry
- **DAO** → Governance and treasury management
- **Learn** → Education and rewards

### Data Flow

```
┌─────────────────────┐     ┌──────────────────────┐
│   React Frontend    │────▶│   Wagmi + Viem       │────▶ Base Chain (RPC)
│   (Vite + TS)       │     │   (Contract Reads/   │
│                     │     │    Writes)            │
└────────┬────────────┘     └──────────────────────┘
         │
         ▼
┌─────────────────────┐     ┌──────────────────────┐
│   The Graph         │────▶│   Subgraph Studio     │
│   (GraphQL Queries) │     │   (Indexed Events)    │
└─────────────────────┘     └──────────────────────┘
```

### Wallet Integration

Wallet connectivity is powered by **Reown AppKit** (WalletConnect v3) with support for:
- All major browser wallets (MetaMask, Coinbase, Rainbow, etc.)
- WalletConnect QR code pairing
- Social logins (Google, X, Discord, GitHub, Apple)
- Email-based onboarding
- Default network: **Base**
- Additional networks: Ethereum, BSC, Polygon, Arbitrum, Optimism, Avalanche

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | React 18, TypeScript, Vite |
| **Styling** | Tailwind CSS, shadcn/ui, Framer Motion |
| **Wallet** | Reown AppKit, wagmi v3, viem |
| **State** | React hooks, TanStack Query, localStorage |
| **Contracts** | Solidity ^0.8.24, OpenZeppelin |
| **Indexing** | The Graph (Subgraph Studio) |
| **i18n** | react-i18next (EN, ES) |
| **Charts** | Recharts |
| **Routing** | React Router v6 |

---

## Smart Contracts

All contracts are deployed on **Base (Chain ID: 8453)**.

| Contract | Address | Description |
|----------|---------|-------------|
| **BG Token** (ERC-20) | `0x701C8C09fE9F081f9BE69Ab9AF8291c84c09389B` | Native ecosystem token |
| **vBG Vote Token** | `0xf25FfA8Ba979CE4a40A961eA16d6e576493Af1C7` | ERC20Votes governance wrapper |
| **BGGovernor** | `0xd38c2d1079AD76eE0C7FC4C83856Dc83711f40D5` | OpenZeppelin Governor |
| **BGTimelock** | `0xd764E54bB6eB0De23157b6517f0ed72E5d1b6410` | TimelockController |
| **BGTreasury** | `0x849766ae8E89488f8eEd12f621CD257356A4630E` | DAO Treasury |
| **BGNodeKeyNFT** | `0xF8c61bE2c2aB5Faa67ee66Ce81b5FE8c5d96dF09` | ERC-721 Node Key NFTs |
| **BGNodeKeyMarketplace** | `0x940Df8eA38d79a71475886af76dd682B6309D8A9` | NFT Marketplace |
| **BGLaunchpadFactory** | `0xCb005fe90078f130E479756Fbf5047F042A58575` | Launchpad sale factory |
| **BGLearnToEarn** | `0x7aA35a8eAb91830A19aCAB81684a0C27D7310A55` | Learn-to-Earn + Scholar NFT |
| **BGVestingClaim** | — | Token vesting schedules |
| **Node Staking** | `0x94883504Bfd0D87Fd81c6FB7C5bCA9fc5787E315` | Node owner staking |

### DeFi Staking Pool Contracts

| Strategy | Lock | Address |
|----------|------|---------|
| Flexible | None | `0xbBb0230aCBb4FE4058C12Be1DfBAa72b1C29AA7c` |
| Low Risk | 1 month | `0x3b986Af0D3A6B0952fB128Fb0591A29272EAfb44` |
| Balanced | 3 months | `0x51279C25Be164698C555A9130D4e69FEd7cDE140` |
| Yield Focused | 6 months | `0x14a918324A3DA42e9597341F30Ce0AEC2c5aEE7A` |
| Serious Staker | 12 months | `0x027D81154E2f1E2acb5527211eF39DEa83c5FA73` |
| Max Conviction | 24 months | `0x3e0769C4E9fDEb748eE55A6F2B7B824427237bE8` |

### Solidity Source Files

```
contracts/
├── BGGovernor.sol              # OpenZeppelin Governor implementation
├── BGLaunchpadFactory.sol      # Clone-based sale factory
├── BGLaunchpadSale.sol         # Individual sale contract
├── BGLearnToEarn.sol           # Learn-to-Earn + Scholar NFT
├── BGNodeKeyMarketplace.sol    # NFT marketplace
├── BGNodeKeyNFT.sol            # ERC-721 Node Key NFTs
├── BGTimelock.sol              # TimelockController
├── BGTreasury.sol              # DAO treasury
├── BGVestingClaim.sol          # Vesting schedules
├── BGVoteToken.sol             # ERC20Votes wrapper (vBG)
└── BGVoteTokenLocked.sol       # Locked vote token variant
```

---

## Modules

### 1. Dashboard

**Route**: `/`

The central landing page providing a portfolio overview:
- **Portfolio Card**: Total value, BG balance, staking positions
- **Active Positions**: Current staking across all 6 strategies
- **Launchpad Carousel**: Featured upcoming/active token sales
- **Skeleton loading states**: Smooth UX while wallet data loads

### 2. DeFi Staking

**Routes**: `/defi`, `/defi/my-staking`, `/defi/stats`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0117b20-d23a-44a3-9205-801bc7f10363" />


Six progressive staking strategies with dynamic on-chain APY:

| Strategy | Lock Period | Early Exit Penalty | Risk |
|----------|-----------|-------------------|------|
| Flexible | No lock | 0% | Low |
| Low Risk | 1 month | 50% | Low |
| Balanced | 3 months | 45% | Medium |
| Yield Focused | 6 months | 40% | Medium |
| Serious Staker | 12 months | 35% | High |
| Max Conviction | 24 months | 30% | High |

**APY Calculation**: `APY = (rewardPerBlock × blocksPerYear / totalStaked) × 100`  
Base block time: ~2 seconds → ~15,778,800 blocks/year

**Key Features**:
- Stake/Unstake modals with approval flow
- Real-time pending rewards display
- Claim rewards across all pools
- Analytics dashboard (TVL charts, reward distribution, strategy breakdown)
- User positions summary with lock status

**Key Hooks**: `useUserStakingPositions`, `useOnChainAPY`, `usePendingRewards`, `useClaimStakingRewards`, `useStakingAnalytics`

### 3. Validator Nodes

**Routes**: `/validators`, `/validators/:nodeId`, `/validators/my-seats`, `/validators/stats`, `/mint`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53268b6c-ba9e-4c26-ad42-660ec41dbae9" />


Validator nodes are represented as ERC-721 NFTs (Node Keys). Each node type has:
- Configurable mint price and supply
- Staking rewards distribution
- Terms & conditions acceptance flow
- Detailed stats and analytics

**Key Features**:
- Browse available node types with hero section
- Mint Node Key NFTs (with approval + terms flow)
- View owned seats and staking positions
- Claim node staking rewards
- Node stats and analytics dashboards

**Key Hooks**: `useNodeKeyNFT`, `useNodeStaking`, `useUserNodes`, `useNodeStakingAnalytics`

### 4. NFT Marketplace

**Routes**: `/marketplace`, `/marketplace/stats`, `/nfts`, `/nfts/:tokenId`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7cd570ae-c811-4915-b0c7-3a20de04f4d7" />


A peer-to-peer marketplace for Node Key NFTs:

| Config | Value |
|--------|-------|
| Royalty | 10% |
| Platform Fee | 2.5% |
| Currency | BG Token |
| Treasury | `0x9832...dE2` |

**Features**:
- List/delist NFTs for sale
- Buy NFTs with BG tokens
- Promotion tiers: Spotlight (24h, 500 BG), Boost (7d, 2,000 BG), Premium (30d, 5,000 BG)
- NFT detail modal with gallery carousel (keyboard navigable)
- Activity table with on-chain sales history
- Stats page with volume, floor price, sales metrics (derived from `getLogs`)

**Key Hooks**: `useMarketplace`

### 5. Launchpad

**Routes**: `/launchpad`, `/launchpad/claim`, `/investments`, `/admin/launchpad`

A multichain token launchpad supporting 7 EVM chains:

| Chain | Stablecoins |
|-------|-------------|
| Base 🔵 | USDT, USDC |
| Ethereum ⟠ | USDT, USDC, DAI |
| BNB Chain 🟡 | USDT, USDC, BUSD |
| Polygon 🟣 | USDT, USDC |
| Arbitrum 🔷 | USDT, USDC |
| Optimism 🔴 | USDT, USDC |
| Avalanche 🔺 | USDT, USDC |

**Sale Types**: Presale, Fair Launch, Hyper Launch, Linear Launch

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6340589-c3d9-4b8f-b133-6c67e6713e27" />


**Sale Lifecycle**: Created → Active → Sale Ended → Tokens Delivered → Claims Open → Completed (or Cancelled)

**Vesting Configuration**: TGE percentage, cliff period, vesting cycles

**Features**:
- Browse active/upcoming sales with project detail pages
- Invest using native tokens or stablecoins
- Claim purchased tokens with vesting timeline
- Refund mechanism for cancelled sales
- Admin: Create Sale Wizard (4-step), sale lifecycle management, tier snapshots

**Key Hooks**: `useLaunchpadEligibility`, `useLaunchpadInvestment`

### 6. DAO Governance

**Routes**: `/dao`, `/dao/proposals`, `/dao/proposals/:id`, `/dao/create`, `/dao/treasury`, `/dao/grants`, `/dao/admin`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/88aa74ce-8bf6-4bb6-b8bb-7bb0318b5c45" />


Full on-chain governance powered by OpenZeppelin Governor:

**Governance Flow**:
1. **Wrap BG → vBG**: Users wrap BG tokens into vote-enabled vBG tokens
2. **Create Proposal**: Requires minimum vBG balance; wizard-based creation
3. **Vote**: For / Against / Abstain with voting power based on vBG balance
4. **Timelock Queue**: Successful proposals enter timelock delay
5. **Execute**: After timelock, proposals execute via Treasury

**Treasury**:
- Multi-token balances (BG, USDT, USDC)
- Spend request proposals
- Transaction history
- Token registry management

**Additional Features**:
- Delegation panel (delegate voting power)
- Grants program with submission forms
- Governance categories and filters
- Community links
- Comment threads on proposals
- Voting guide

**Key Hooks**: `useDAOProposals`, `useDAOAdmin`, `useDAOEligibility`, `useVotingPower`, `useGovernanceRoles`, `useTreasury`, `useTreasuryTokenRegistry`

### 7. Learn-to-Earn

**Routes**: `/learn`, `/admin/learn`

> 📖 **Detailed documentation**: See [LEARN_README.md](LEARN_README.md) for complete Learn-to-Earn module documentation.

100 lessons across 10 categories with progressive BG token rewards:

| Tier | Lessons | Reward/Lesson | Total |
|------|---------|--------------|-------|
| 1 | 1–20 | 5 BG | 100 BG |
| 2 | 21–40 | 10 BG | 200 BG |
| 3 | 41–60 | 15 BG | 300 BG |
| 4 | 61–80 | 20 BG | 400 BG |
| 5 | 81–100 | 25 BG | 500 BG |
| Scholar NFT | All 100 | +500 BG bonus | 500 BG |
| **Total** | | | **2,000 BG** |

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7ee43230-4cab-43f1-b1bb-65d339fc8d84" />


**Flow**: Read Lesson → Pass Quiz (2 questions) → Share on X → Submit Tweet URL → Admin Verifies → Earn $BG

**Ranking**: Novice (0) → Apprentice (10) → Student (25) → Scholar (50) → Expert (75) → Master (100)

**Scholar NFT**: Soulbound ERC-721 with fully on-chain SVG metadata

### 8. Token Migration

**Route**: `/migration`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/24901603-570c-4071-ad28-868b62ff8c7d" />


GQ → BG token migration from BSC (Chain ID: 56) to Base:

| Stage | Contract | Rate | Status |
|-------|----------|------|--------|
| Stage 1 | `0xb4c0...Ad99` | 5:1 | Ended |
| Stage 2 | `0xAfA3...e974` | 7:1 | Ended |
| Stage 3 | `0x2ec1...d913` | 10:1 | Active |

**Features**:
- Deposit GQ tokens on BSC
- View migration totals (contract + user)
- Info sections explaining the migration process

### 9. Vesting & Claims

**Route**: `/claim`

Token vesting schedule management:
- Vesting overview with schedule visualization
- Claim available tokens
- View vesting timeline and milestones

**Key Hook**: `useVestingSchedule`

### 10. Public Profiles

**Route**: `/profile/:address`

Public user profiles featuring:
- Deterministic generative avatars
- Dynamic achievement badges with requirement tooltips
- Summary statistics (ROI, volume, staked totals)
- Interactive staking distribution donut chart (animated with Framer Motion)
- Unified activity timeline
- NFT grid with detail modal (keyboard-navigable gallery)
- Share button (copies profile URL to clipboard)

---

## Admin & Developer Tools

### Developer Dashboard

**Route**: `/admin/dashboard`

Central protocol command center with:
- Real-time metrics and trend charts (TVL, Activity)
- Quick Actions: Pause/Resume toggles for all 12+ contracts
- Reward rate updates, treasury transfers, emergency token rescues
- Safety confirmation modals for destructive actions
- Persistent Admin Activity Log (localStorage with tx hashes)

### Module Admin Panels

| Route | Module | Tabs |
|-------|--------|------|
| `/dao/admin` | DAO | Governance config, role management |
| `/admin/launchpad` | Launchpad | Metrics, sales table, create wizard, lifecycle, tiers, verification, config, activity |
| `/admin/validators` | Validators | Node creation/edit, moderation, rewards, NFT config |
| `/admin/defi` | DeFi | Config, rewards, moderation, emergency |
| `/admin/marketplace` | Marketplace | Fees, moderation, promotions |
| `/admin/learn` | Learn | Stats, activity (The Graph), verify tweets, config, emergency |

---

## Data Layer

### The Graph Subgraphs

| Subgraph | Purpose | Endpoint |
|----------|---------|----------|
| **DeFi Portal** | Staking events, TVL, user positions | Decentralized gateway (ID: `44jUamW...`) |
| **Governance** | Proposals, votes, treasury transactions | Studio: `governance-blinkchain` |
| **Learn-to-Earn** | Lesson completions, rewards, leaderboard | Studio: `blink-learn-to-earn` |
| **Nodes** | Node staking events (shared with DeFi) | Same deployment |

**Note**: Marketplace financial metrics (Volume, Floor, Sales History) are derived directly from on-chain `Sold` and `Listed` logs via `getLogs` for real-time accuracy.

### On-Chain Reads

All contract interactions use **wagmi v3** hooks:
- `useReadContract` for view functions
- `useWriteContract` for state-changing transactions
- `useWaitForTransactionReceipt` for confirmation tracking

### Local Storage

Used for optimistic caching and user preferences:
- `learn-progress-{address}` — Learn lesson completion cache
- `learn-pending-tweets` — Pending tweet verification submissions
- `admin-activity-log` — Admin action history
- `i18nextLng` — Language preference
- `locale-region` — Regional variant (MX/ES)

---

## Internationalization

The portal uses **react-i18next** with two languages:

| Language | File | Variants |
|----------|------|----------|
| English | `src/i18n/en.json` | Default |
| Spanish | `src/i18n/es.json` | México 🇲🇽, España 🇪🇸 |

Both Spanish variants share the same translation file but persist regional preference for flag/label display.

**Key terminology**: "Validators" → "Validadores", "Forge" → "Forjar", "Staking" remains "Staking"

Regional formatting (e.g., `1.234,56`) is handled via `useLocaleFormatting`.

---

## RBAC System

Role-based access control using wallet address whitelisting:

| Role | Access Level |
|------|-------------|
| `superadmin` | Full access — inherits all permissions |
| `admin` | Admin panels, contract management |
| `deployer` | Launchpad creation, developer tools |
| `finance` | Treasury and financial operations |
| `accounting` | Financial reporting |
| `support` | User support tools |
| `backend` | Backend operations |
| `client` | Launchpad client access |

**Developer sidebar access**: `superadmin`, `admin`, `deployer`, `client`

> **Note**: Currently implemented as a hardcoded wallet whitelist (`src/config/roles.ts`). Planned migration to a database-backed system in the future.

---

## Routing

### Public Routes

| Route | Page | Description |
|-------|------|-------------|
| `/` | Dashboard | Portfolio overview |
| `/buy` | Buy BG | Token purchase |
| `/defi` | DeFi | Staking strategies |
| `/defi/my-staking` | My Staking | User positions |
| `/defi/stats` | Staking Stats | Analytics |
| `/validators` | Validators | Node browser |
| `/validators/:nodeId` | Validator Detail | Node details |
| `/validators/:nodeId/terms` | Terms | Terms & conditions |
| `/validators/my-seats` | My Seats | Owned nodes |
| `/validators/stats` | Validator Stats | Node analytics |
| `/mint` | Mint NFT | Mint node keys |
| `/nfts` | My NFTs | Owned NFTs |
| `/marketplace` | Marketplace | NFT marketplace |
| `/marketplace/stats` | Stats | Marketplace analytics |
| `/launchpad` | Launchpad | Token sales |
| `/launchpad/claim` | Claim Tokens | Claim purchased tokens |
| `/investments` | My Investments | Portfolio |
| `/migration` | Migration | GQ → BG migration |
| `/claim` | Claim | Vesting claims |
| `/dao` | DAO | Governance home |
| `/dao/proposals` | Proposals | Proposal list |
| `/dao/proposals/:id` | Proposal Detail | Vote & discuss |
| `/dao/create` | Create Proposal | Proposal wizard |
| `/dao/treasury` | Treasury | DAO treasury |
| `/dao/grants` | Grants | Grant submissions |
| `/learn` | Learn | Learn-to-Earn hub |
| `/profile/:address` | Profile | Public profile |
| `/settings` | Settings | User preferences |

### Admin Routes (Role-Gated)

| Route | Page |
|-------|------|
| `/admin/dashboard` | Developer Dashboard |
| `/admin/launchpad` | Launchpad Admin |
| `/admin/validators` | Validator Admin |
| `/admin/defi` | DeFi Admin |
| `/admin/marketplace` | Marketplace Admin |
| `/admin/learn` | Learn Admin |
| `/dao/admin` | DAO Admin |

---

## Project Structure

```
├── contracts/                    # Solidity smart contracts (11 contracts)
├── subgraph/                     # The Graph subgraphs
│   ├── defi-portal-blinkchain/   # DeFi staking subgraph
│   ├── governance/               # DAO governance subgraph
│   └── learn-to-earn/            # Learn-to-Earn subgraph
├── public/
│   └── images/                   # Documentation diagrams
├── src/
│   ├── assets/                   # Brand assets (logos, icons)
│   ├── components/
│   │   ├── claim/                # Vesting claim components
│   │   ├── dao/                  # DAO governance components (20+)
│   │   ├── dashboard/            # Dashboard widgets
│   │   ├── defi/                 # DeFi staking + admin (15+)
│   │   ├── developers/           # Developer dashboard components
│   │   ├── launchpad/            # Launchpad + admin (20+)
│   │   ├── layout/               # AppHeader, AppSidebar, AppLayout
│   │   ├── learn/                # Learn-to-Earn + admin (15+)
│   │   ├── marketplace/          # Marketplace + admin
│   │   ├── migration/            # GQ→BG migration
│   │   ├── nfts/                 # NFT gallery
│   │   ├── profile/              # Public profile components
│   │   ├── settings/             # Profile + wallet settings
│   │   ├── shared/               # WalletGuard, ApprovalWarning, etc.
│   │   ├── ui/                   # shadcn/ui primitives (50+)
│   │   └── validators/           # Validator node + admin
│   ├── config/                   # ABIs, addresses, chain configs, roles
│   ├── hooks/                    # 30+ custom hooks
│   ├── i18n/                     # Translation files (en.json, es.json)
│   ├── lib/                      # Utilities (formatting, node utils, etc.)
│   ├── pages/                    # 35+ route pages
│   └── providers/                # Web3Provider (Reown AppKit + wagmi)
├── .deploy/                      # Docker/Nginx/Jenkins deployment
├── LEARN_README.md               # Detailed Learn-to-Earn documentation
└── README.md                     # This file
```

---

## Getting Started

### Prerequisites

- Node.js 18+ with npm
- A Web3 wallet (MetaMask, Coinbase, etc.)
- Base network configured in wallet

### Installation

```bash
# Clone the repository
git clone <YOUR_GIT_URL>
cd blinkchain-portal

# Install dependencies
npm install

# Start development server
npm run dev
```

### Environment

The application runs entirely client-side with no backend server. All data is read from:
- **Base RPC** (via wagmi/viem) for contract reads/writes
- **The Graph** (via fetch) for indexed event data
- **localStorage** for user preferences and optimistic caching

### Build

```bash
npm run build     # Production build
npm run preview   # Preview production build
npm run lint      # ESLint check
```

---

## License

Proprietary — Blink Galaxy / Blinkchain Portal. All rights reserved.
