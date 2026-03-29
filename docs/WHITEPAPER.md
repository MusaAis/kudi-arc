# Kudi Arc — Technical Whitepaper

**Version:** 1.0
**Date:** March 2026
**Network:** Arc Testnet (Mainnet pending)

---

## Abstract

Kudi Arc is a hybrid on-chain FX desk and remittance engine for Africa🌍 — the first EVM-compatible blockchain that uses USDC as its native gas token. By leveraging Arc's unique infrastructure, Kudi Arc eliminates the traditional barriers to African financial access: high remittance fees, forex scarcity, and the complexity of managing multiple crypto assets just to pay for gas.

---

## 1. Problem Statement

### 1.1 The African Forex Crisis
Nigeria — Africa's largest economy — faces severe foreign currency restrictions. The Central Bank of Nigeria (CBN) limits USD access for individuals and businesses, forcing a parallel market with a 30%+ premium over official rates. This affects 220 million Nigerians and over 1.4 billion Africans across the continent.

### 1.2 Remittance Cost
Africa receives $50B+ in annual remittances. The average cost to send money to Africa is 8.5% (World Bank, 2024) — one of the highest in the world. A Nigerian diaspora worker sending $200 home loses $17 in fees alone.

### 1.3 Yield Gap
Nigerian savings accounts offer 4-6% annual yield in a currency (NGN) that devalued 70% against the USD in 2023. There is no accessible, regulated, low-risk yield product for ordinary Africans.

### 1.4 The Gas Problem
Every Ethereum-compatible dApp requires ETH for gas. For African users unfamiliar with crypto, this creates an impossible UX barrier: to use USDC, you must first acquire ETH. Arc Network solves this entirely — gas is paid in USDC.

---

## 2. Solution

Kudi Arc provides three financial primitives on Arc Network:

### 2.1 FX Swap (KudiSwap)
A USDC ↔ EURC swap contract with:
- Live rates fed from open FX APIs
- 0.30% fee (vs 2-5% on traditional exchanges)
- Reentrancy protection + rate timelocks
- Slippage protection for every trade

### 2.2 Remittance (KudiSend)
A crypto-to-fiat remittance flow:
1. User sends USDC/EURC onchain (with wallet signature)
2. Backend routes to local payout partner (Bitnob, Flutterwave, Wave)
3. Recipient receives NGN/KES/GHS/XOF/FCFA in bank or mobile wallet

Supported: 10 African countries, expanding to 20+ on mainnet.

### 2.3 Yield (KudiYield)
Integration with Circle's USYC — a tokenized money market fund backed by US Treasury Bills:
- Current APY: 4.86%
- 1 USYC ≈ $1.02 USDC
- Accessible to non-US institutions (Africa qualifies)
- Earnings shown in NGN equivalent for local context

---

## 3. Smart Contract Architecture

### 3.1 KudiSwap V2

**Address:** `0x8a10D0e61201000B5456EC725165892B08832C5f`
**Compiler:** Solidity 0.8.24
**Network:** Arc Testnet (Chain 5042002)

#### Core Functions
```solidity
swapUSDCforEURC(uint256 amountIn, uint256 minAmountOut)
swapEURCforUSDC(uint256 amountIn, uint256 minAmountOut)
previewSwapUSDCtoEURC(uint256 amountIn)
previewSwapEURCtoUSDC(uint256 amountIn)
getPoolBalances()
getStats()
```

#### Security Model
| Feature | Implementation |
|---|---|
| Reentrancy | `nonReentrant` modifier + `_locked` bool |
| Rate manipulation | 5-minute timelock + ±20% bounds + abs 0.5-2.0 |
| Liquidity drain | 24-hour timelock + 50% cap per withdrawal |
| Min liquidity | 1 USDC always maintained |
| Token transfers | SafeERC20 inline library |
| Ownership | Two-step transfer + pending owner pattern |
| Fee updates | Event emitted + max 2% cap |

### 3.2 Trust Model

Kudi Arc is a **custodial FX desk**, not a trustless AMM. This is by design:
- NGN offramp requires licensed partner integration (always centralized)
- Mobile-first African users need simplicity over decentralization
- Comparable to Bitnob, Yellowcard, Grey — all trusted operators

**Transparency measures:**
- Contract source code publicly verified on Arcscan
- All admin actions emit onchain events
- Timelocks give users 5-24 hours to react to any owner action
- Rate bounds prevent extreme manipulation

**Mainnet roadmap toward decentralization:**
- Phase 7: AMM formula (x*y=k) replaces manual rates
- Phase 8: Chainlink oracle anchors rates to market price
- Phase 9: Multisig ownership (3-of-5 signers)

---

## 4. Backend Architecture
```
Flask API (Python 3.10)
├── /api/rates          — Live USD/EUR/NGN rates (refreshed every 5 min)
├── /api/countries      — 10 country payout configs + live rates
├── /api/banks          — Nigerian bank list (26 banks)
├── /api/send/payout    — Universal payout endpoint
├── /api/swap/record    — Record onchain swap to DB
├── /api/history/:wallet — Per-wallet transaction history
├── /api/yield/stats    — USYC APY + pool data
└── /api/yield/balance  — User USYC position

Database: SQLite
├── transactions table
│   ├── Swap records (tx_hash, amount_in, amount_out, rate)
│   └── Send records (recipient, bank, reference, status)
└── Indexed by wallet address

FX Data: Open Exchange Rates API
└── 9 currencies: NGN, KES, GHS, ZAR, XOF, XAF, TZS, UGX, RWF
```

---

## 5. Supported Countries & Currencies

| Country | Currency | Symbol | Payout Partner |
|---|---|---|---|
| 🇳🇬 Nigeria | NGN | ₦ | Bitnob |
| 🇰🇪 Kenya | KES | KSh | Flutterwave |
| 🇬🇭 Ghana | GHS | GH₵ | Flutterwave |
| 🇿🇦 South Africa | ZAR | R | Flutterwave |
| 🇸🇳 Senegal | XOF | CFA | Wave |
| 🇨🇮 Côte d'Ivoire | XOF | CFA | Wave |
| 🇨🇲 Cameroon | XAF | FCFA | Wave |
| 🇹🇿 Tanzania | TZS | TSh | Flutterwave |
| 🇺🇬 Uganda | UGX | USh | Flutterwave |
| 🇷🇼 Rwanda | RWF | RF | Flutterwave |

---

## 6. Fee Structure

| Action | Fee | Recipient |
|---|---|---|
| USDC ↔ EURC Swap | 0.30% | Protocol treasury |
| Remittance send | 0.30% | Protocol treasury |
| USYC deposit | 0% | — |
| USYC withdrawal | 0% | — |

All fees are collected in the swap token and tracked separately via `accruedFees` state variable. Fees cannot be withdrawn together with liquidity — they are extracted via `withdrawFees()` only.

---

## 7. Roadmap

### Testnet (Current)
- [x] KudiSwap V2 deployed + verified
- [x] 10-country remittance support
- [x] USYC yield integration
- [x] Transaction history
- [x] Security audit (SolidityScan)
- [ ] Circle USYC allowlist approval
- [ ] Bitnob API integration (live)
- [ ] Flutterwave API integration
- [ ] User beta testing

### Mainnet (Q3 2026)
- [ ] Arc Mainnet deployment
- [ ] Real NGN/KES/GHS payouts
- [ ] Domain + SSL
- [ ] Multisig ownership
- [ ] Chainlink oracle integration

### V3 (Q4 2026)
- [ ] AMM model (x*y=k)
- [ ] Open liquidity provision
- [ ] LP token system
- [ ] Multi-chain (Base, Stellar)
- [ ] Mobile app (React Native)

---

## 8. Team

**Musa AIS** — Founder & Lead Developer
- Background: Python, Flask, WebSocket systems, algorithmic trading bots
- Location: Nigeria 🇳🇬
- Previous: Built and deployed Vortix Signal Bot (crypto futures signals)

---

## 9. Why Arc Network?

Arc is the ideal infrastructure for Kudi Arc because:

1. **USDC as gas** — Users only need USDC, nothing else
2. **Deterministic finality** — No re-org risk for settlement
3. **Circle-native** — USDC, EURC, USYC all first-class assets
4. **EVM compatible** — Familiar tooling, MetaMask/Rabby support
5. **Stablecoin FX built-in** — Arc's StableFX is our core use case

No other chain combines these properties. For African remittance, Arc is the right foundation.

---

---

## 10. 🧩 Why Kudi Arc Wins
USDC-native chain (Arc) → no gas friction
Africa-first design → not retrofitted like global apps
Hybrid model → combines speed of crypto with reach of fiat
Built-in yield (USYC) → idle capital becomes productive
Transparent backend + on-chain auditability

---

*Kudi Arc — Financial infrastructure for Africa, built on Arc Network.*
