<div align="center">

<img src="https://raw.githubusercontent.com/ARSUI-Team/fe-suivan/main/public/suivan-logo.png" alt="Suivan Logo" width="120" />

# SUIVAN

### **The Global ROSCA Protocol on Sui**

*Rotating Savings. Reimagined for the Blockchain Age.*

[![Sui](https://img.shields.io/badge/Powered%20by-Sui-4DA90B?style=for-the-badge&logo=sui&logoColor=white)](https://sui.io)
[![Move](https://img.shields.io/badge/Smart%20Contracts-Move-1E1E2E?style=for-the-badge)](https://move-language.github.io/move/)
[![Next.js](https://img.shields.io/badge/Frontend-Next.js%2016-000?style=for-the-badge&logo=nextdotjs)](https://nextjs.org)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](./LICENSE)

[Smart Contracts](https://github.com/ARSUI-Team/sc-suivan) · [Frontend](https://github.com/ARSUI-Team/fe-suivan) · [Telegram](https://t.me/suivan) · [Discord](https://discord.gg/suivan)

</div>

---

## The Problem

**1.7 billion adults** are unbanked. Yet **500 million+** participate in ROSCAs (Rotating Savings and Credit Associations) — locally known as Arisan (Indonesia), Chit Fund (India), Tanda (Mexico), Susu (West Africa), Hui (China), and Gam'eya (Egypt).

These informal savings circles are:
- **Trust-dependent** — no transparency, no enforcement
- **Cash-only** — no yield, no compound growth
- **Local-only** — can't cross borders or scale

**Suivan fixes all of this. On-chain. On Sui.**

---

## What Suivan Does

Suivan is a **Sui-native protocol** that transforms informal rotating savings groups into transparent, yield-generating, globally accessible smart contracts.

| Traditional ROSCA | Suivan |
|---|---|
| Cash under the mattress | **On-chain pooled USDC** |
| Trust-based enforcement | **Smart contract collateral** |
| Zero yield | **AI-optimized DeFi yield** (Scallop, NAVI, Cetus) |
| Local, in-person only | **Global, zkLogin access** |
| Manual bookkeeping | **Walrus blob metadata** |
| User pays gas | **Sponsored transactions — gasless** |

---

## Architecture

```
                        ┌─────────────────────────────────┐
                        │         SUIVAN FRONTEND          │
                        │   Next.js 16 · React 19 · GSAP  │
                        │   @mysten/dapp-kit · Walrus SDK  │
                        └────────────┬────────────────────┘
                                     │
                    ┌────────────────┼────────────────────┐
                    ▼                ▼                     ▼
          ┌──────────────┐  ┌──────────────┐  ┌───────────────────┐
          │  SUI NETWORK │  │  WALRUS BLOB │  │  DeFiLlama API    │
          │  Move Contracts│  │  STORAGE     │  │  Yield Data Feed  │
          └──────┬───────┘  └──────────────┘  └───────────────────┘
                 │
   ┌─────────────┼──────────────────┐
   ▼             ▼                  ▼
┌──────────┐ ┌──────────┐ ┌────────────────┐
│  Factory  │ │   Pool    │ │  Yield Engine  │
│  Templates│ │  Join/    │ │  Scallop/NAVI/ │
│  Create   │ │  Deposit/ │ │  Cetus routing │
│  List     │ │  Payout   │ │  APY switching │
└──────────┘ └──────────┘ └────────────────┘
```

---

## Sui-Native Features

### zkLogin — Zero-Friction Onboarding
Users sign in with **Google** — no wallet extension needed. Suivan leverages Sui's zkLogin via `@mysten/dapp-kit` to onboard the next billion users who have never installed MetaMask.

### Sponsored Transactions — Gasless UX
Suivan backend **pays gas fees** for users via Ed25519 keypair sponsorship. Join a pool, make deposits, receive payouts — **zero SUI required** in the user's wallet.

### Walrus Blob Storage — Cheap Metadata
Pool descriptions, cycle histories, and legal agreements stored as **Walrus blobs**. The contract stores only a 32-byte blob ID — keeping gas costs minimal while enabling rich off-chain metadata.

### Parallel Execution — Sub-second Finality
Sui's Narwhal-Bullshark consensus delivers **<1 second finality**. Cycle transitions, winner selection, and yield distribution happen faster than any EVM chain.

---

## Smart Contracts

| Contract | Description | Lines | Tests |
|----------|-------------|-------|-------|
| `arisan_factory` | Pool template creation & registry | ~200 | 8 |
| `arisan_pool` | Core ROSCA logic: join, deposit, payout, collateral | ~900 | 31 |
| `yield_strategy` | Simulated APY engine with protocol switching | ~250 | 10 |
| `protocol_vault` | Per-protocol yield vault (Scallop, NAVI, Cetus) | ~180 | 7 |
| `test_usdc` | Testnet faucet token | ~50 | — |
| `seal_randomness` | Commit-reveal winner selection via Seal | ~200 | 12 |
| `deepbook_yield` | DeepBook V3 flash loan arbitrage | ~230 | 8 |
| `walrus_store` | Walrus blob management on-chain | ~140 | 5 |

**Total: 81+ tests passing. All 7 audit issues fixed (critical → low).**

### Audit Fixes

| # | Severity | Issue | Fix |
|---|----------|-------|-----|
| 1 | Critical | Yield inflation breaks withdrawals | `simulate_yield` no longer inflates `total_deposits` |
| 2 | High | Factory doesn't track pool IDs | `create_pool` returns `ID`, factory stores it |
| 3 | High | `end_pool` locks remaining funds | Distributes remaining pool_funds + yield to active participants |
| 4 | Medium | `select_winner` event after mutation | Event emitted before state mutation |
| 5 | Medium | `is_cycle_complete` underflow | Guard: `current_time_ms < pool_start_time_ms` |
| 6 | Medium | `max_participants` = 1 creates stuck pool | `assert!(max_participants >= 2)` |
| 7 | Low | `collateral_multiplier` = 0 allowed | `assert!(collateral_multiplier >= 100)` |

---

## Frontend

Built with **Next.js 16**, **React 19**, **Tailwind CSS 4**, **GSAP**, and **Lenis**.

| Route | Feature |
|-------|---------|
| `/` | Landing page — GSAP motion + Lenis smooth scroll + Sui branding |
| `/pools` | Pool explorer — grid, filters, real-time Sui object reads |
| `/pools/[address]` | Pool detail — participants, collateral, yield analytics |
| `/ai` | AI yield dashboard — live DeFiLlama APY signals for 7 Sui protocols |
| `/demo` | Interactive 5-step walkthrough for new users |
| `/faq` | Multi-language FAQ with accordion |
| `/leaderboard` | Earnings leaderboard |

### UI/UX Highlights
- **Brutalist premium design** — bold borders, sharp shadows, high contrast
- **Dark mode** — system detection + manual toggle
- **i18n** — English + Bahasa Indonesia
- **GSAP ScrollTrigger** — cinematic entrance animations
- **Lenis** — buttery-smooth scroll physics
- **Sui fee profile widget** — real-time gas comparison vs EVM chains

---

## Testnet Deployment

| Object | ID |
|---------|-----|
| **Package** | `0x72bbfae9cd90e62b1cecb9db218eb52ac6135d322d232eb5e8a35a9b1d41bb1b` |
| **ArisanFactory** | `0xd45cfd2dcc4be81c17f44f3e5f934605c7d09bcf1adaeadab576607493383867` |
| **YieldStrategy** | `0xf374ed58edfbf394acca7b352ef0c6738f4b5c5dd378df28c7f69cdd0611b5e2` |
| **TEST_USDC** | `0x72bbfae9cd90e62b1cecb9db218eb52ac6135d322d232eb5e8a35a9b1d41bb1b::test_usdc::TEST_USDC` |

[View on Suiscan](https://suiscan.xyz/testnet/package/0x72bbfae9cd90e62b1cecb9db218eb52ac6135d322d232eb5e8a35a9b1d41bb1b) · [View on Suivision](https://suivision.xyz/testnet/package/0x72bbfae9cd90e62b1cecb9db218eb52ac6135d322d232eb5e8a35a9b1d41bb1b)

---

## AI Yield Optimization

Suivan's AI engine fetches **real-time APY data** from DeFiLlama across 7 Sui DeFi protocols:

| Protocol | Type | Feed |
|----------|------|------|
| Scallop | Lending | Live |
| NAVI | Lending | Live |
| Cetus | DEX + CLMM | Live |
| Aftermath | Yield Aggregator | Live |
| Turbos | DEX | Live |
| Bluefin | DEX | Live |
| Suilend | Lending | Live |

The AI optimizer automatically **switches pool funds to the highest-yielding protocol**, maximizing returns for all pool participants.

---

## Repositories

| Repo | Description |
|------|-------------|
| [**Suivan**](https://github.com/ARSUI-Team/Suivan) | This repo — project overview & documentation |
| [**sc-suivan**](https://github.com/ARSUI-Team/sc-suivan) | Move smart contracts — pool, factory, yield, vault, Seal, Walrus |
| [**fe-suivan**](https://github.com/ARSUI-Team/fe-suivan) | Next.js frontend — wallet, pools, AI dashboard, demo |

---

## Quick Start

### Smart Contracts
```bash
git clone https://github.com/ARSUI-Team/sc-suivan.git
cd sc-suivan/contracts
sui move test
```

### Frontend
```bash
git clone https://github.com/ARSUI-Team/fe-suivan.git
cd fe-suivan
npm install
cp .env.example .env.local
npm run dev
```

Open `http://localhost:3000` — connect Sui Wallet or use zkLogin.

---

## Why Suivan Wins

| Judging Criteria | Suivan |
|------------------|--------|
| **Sui-Native** | zkLogin, Sponsored Tx, Walrus, Seal randomness |
| **Real Product** | Audited contracts, deployed testnet, running frontend |
| **Global Impact** | ROSCA serves 500M+ users across 80+ countries |
| **Technical Depth** | 8 contracts, 81 tests, multi-protocol yield engine, flash loans |
| **AI Integration** | Live DeFiLlama feed, automated yield switching |
| **UX Excellence** | Gasless onboarding, GSAP motion, dark mode, i18n |
| **Ecosystem Fit** | Walrus storage, DeepBook integration, Sui parallel execution |

---

## Team

**ARSUI Team** — Building the future of collective savings on Sui.

- [Faiz Abdurrachman](https://github.com/Faiz-abdurrachman)
- [Handiya](https://github.com/Handiya-lab)
- [Nabila](https://github.com/nabilarhnn)
- [Naufal](https://github.com/naufalham)
- [YT](https://github.com/yt2025id-lab)

---

<div align="center">

**Built for SUIOverflow 2026**

*Saving together. Winning together. On Sui.*

</div>
