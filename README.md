<div align="center">

# 🟡 Kudi Arc

### Kudi Arc is a hybrid on-chain FX desk and remittance engine for Africa.

**It combines:**
On-chain stablecoin swaps,
Off-chain fiat settlement,
Yield on idle balances.

**Built on Arc Network · Powered by Circle USDC & EURC**

[![Testnet Demo](https://img.shields.io/badge/Live%20Demo-Testnet-27AE74?style=for-the-badge)](http://129.80.199.123)
[![Contract](https://img.shields.io/badge/Contract-Verified-27AE74?style=for-the-badge)](https://testnet.arcscan.app/address/0x8a10D0e61201000B5456EC725165892B08832C5f)
[![Audit](https://img.shields.io/badge/Security%20Audit-SolidityScan-F5A623?style=for-the-badge)](https://solidityscan.com)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

---

*Swap USDC ↔ EURC · Send to 10 African countries · Earn yield via USYC*

</div>

---

## 📌 What is Kudi Arc?

Kudi Arc is an Africa-first stablecoin FX and remittance dApp built on **Arc Network** — the first blockchain where gas is paid in USDC. We solve three real problems for African users:

1. **FX Swap** — Swap between USDC and EURC at live market rates, with near-zero fees
2. **Remittance** — Send USDC/EURC → receive NGN, KES, GHS, XOF, FCFA and more in your bank account or mobile wallet
3. **Yield** — Earn 4.86% APY on your USDC via USYC (tokenized US Treasury Bills)

> *"Kudi"* means **money** in Hausa and Yoruba — Nigeria's two most spoken languages.

---

## 🌍 Why Africa? Why Now?

| Problem | Scale |
|---|---|
| Nigeria forex scarcity | CBN restricts USD access — parallel market premium 30%+ |
| Remittance fees | World Bank avg: 8.5% to send money to Africa |
| No yield on savings | Nigerian savings accounts: 4-6% in a devaluing currency |
| Stablecoin adoption | Nigeria #3 globally for crypto adoption |

Arc Network's USDC-as-gas design makes it the perfect infrastructure for Africa — users only need USDC, nothing else.

---

---

## 🧩 Why Kudi Arc Wins

USDC-native chain (Arc) → no gas friction
Africa-first design → not retrofitted like global apps
Hybrid model → combines speed of crypto with reach of fiat
Built-in yield (USYC) → idle capital becomes productive
Transparent backend + on-chain auditability

---

## ✨ Features

### ⇄ FX Swap
- Swap USDC ↔ EURC at live market rates
- 0.30% fee — lowest in the market
- Slippage protection built into smart contract
- Real-time NGN equivalent shown

### 📤 Send Money
- Send USDC or EURC → recipient gets local cash
- **10 African countries** supported:
  🇳🇬 Nigeria · 🇰🇪 Kenya · 🇬🇭 Ghana · 🇿🇦 South Africa
  🇸🇳 Senegal · 🇨🇮 Côte d'Ivoire · 🇨🇲 Cameroon
  🇹🇿 Tanzania · 🇺🇬 Uganda · 🇷🇼 Rwanda
- Bank account or mobile money delivery
- Wallet signature required for every transfer
- Reference tracking for every transaction

### 📈 Yield (USYC)
- Earn **4.86% APY** on your USDC
- Backed by short-duration US Treasury Bills
- Issued by Circle on Arc Testnet
- No lock-up — withdraw anytime
- Earnings shown in NGN equivalent

### 📋 Transaction History
- Full history of swaps and sends
- Clickable TX hashes → Arc Explorer
- Per-wallet history stored on-chain + backend

---

## 🏗️ Architecture
```
┌─────────────────────────────────────────────────────┐
│                    Frontend (PWA)                    │
│         index.html · send.html · yield.html          │
│              Ethers.js · MetaMask/Rabby              │
└──────────────────┬──────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────┐
│              Flask Backend (Python)                  │
│        Live FX Rates · Multi-currency API            │
│        Bitnob Integration · TX History DB            │
│           Oracle Cloud Ubuntu (Gunicorn)             │
└──────────────────┬──────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────┐
│           KudiSwap V2 Smart Contract                 │
│              Arc Testnet (EVM)                       │
│  ✅ Reentrancy Guard   ✅ Rate Timelock (5 min)      │
│  ✅ Withdrawal Timelock (24hr)  ✅ SafeERC20         │
│  ✅ Rate Bounds (±20%, abs 0.5-2.0)                 │
│  ✅ Min Liquidity (1 USDC)  ✅ 50% Withdrawal Cap   │
│  ✅ Two-step Ownership  ✅ Fee Separation            │
└─────────────────────────────────────────────────────┘
```

---

## 📊 Live Stats (Arc Testnet)

| Metric | Value |
|---|---|
| Total Swaps | 5 |
| USDC Swapped | 56.07 USDC |
| EURC Swapped | 31.00 EURC |
| Fees Collected | 0.26 USDC |
| USDC Pool | 520.29 USDC |
| EURC Pool | 400.19 EURC |
| Contract Fee | 0.30% |
| Network | Arc Testnet (Chain 5042002) |

---

## 📜 Smart Contracts

| Contract | Address | Status |
|---|---|---|
| **KudiSwap V2** | [`0x8a10D0e61201000B5456EC725165892B08832C5f`](https://testnet.arcscan.app/address/0x8a10D0e61201000B5456EC725165892B08832C5f) | ✅ Verified |
| **KudiSwap V1** (deprecated) | [`0xb138329BF67cFe121A37D9947922D2B7278BdC29`](https://testnet.arcscan.app/address/0xb138329BF67cFe121A37D9947922D2B7278BdC29) | ⚠️ Deprecated |
| **USDC (Arc)** | `0x3600000000000000000000000000000000000000` | Circle Official |
| **EURC (Arc)** | `0x89B50855Aa3bE2F677cD6303Cec089B5F319D72a` | Circle Official |
| **USYC (Arc)** | `0xe9185F0c5F296Ed1797AaE4238D26CCaBEadb86C` | Circle Official |

### Security

- **Compiler:** Solidity 0.8.24
- **Security Score:** 60.58/100 (SolidityScan)
- **Threat Score:** 97/100 — Low Risk ✅
- **Critical Vulnerabilities:** 0
- **Audit Tool:** SolidityScan + manual review

Key security features in V2:
- `nonReentrant` modifier on all fund-moving functions
- 5-minute timelock on rate updates
- 24-hour timelock on liquidity withdrawals
- Maximum 50% pool withdrawal per transaction
- Minimum 1 USDC liquidity always maintained
- Absolute rate bounds (0.5 — 2.0)
- Relative rate bounds (±20% per update)
- SafeERC20 for all token transfers
- Two-step ownership transfer

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Blockchain | Arc Network (EVM, Chain 5042002) |
| Smart Contract | Solidity 0.8.24 |
| Frontend | HTML/CSS/JS, Ethers.js 6.7 |
| Backend | Python 3.10, Flask, Gunicorn |
| Database | SQLite (transaction history) |
| Process Manager | systemd |
| Web Server | Nginx |
| FX Rates | Open Exchange Rates API |
| Wallet Support | MetaMask, Rabby |

---

## 🚀 Getting Started

### Prerequisites
- MetaMask or Rabby wallet
- Arc Testnet configured (auto-added when you visit the dApp)
- Testnet USDC from [Circle Faucet](https://faucet.circle.com)

### Use the dApp
1. Visit **http://129.80.199.123** (testnet)
2. Click **Connect Wallet** — Arc Testnet is added automatically
3. Get testnet USDC from [faucet.circle.com](https://faucet.circle.com)
4. Start swapping, sending, or depositing for yield

### Run Locally
```bash
# Clone the repo
git clone https://github.com/KudiArc/kudi-arc.git
cd kudi-arc

# Install Python dependencies
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your values

# Initialize database
python3 db.py

# Start backend
python3 app.py
```

---

---

## 🔄 How It Works

1. User connects wallet
2. User swaps USDC ↔ EURC (on-chain)
3. User initiates remittance
4. Backend processes fiat payout via partners
5. Recipient receives local currency

---

## 📁 Project Structure
```
kudi-arc/
├── contracts/
│   ├── KudiSwapV2.sol      # Main swap contract (current)
│   └── KudiSwap.sol        # V1 (deprecated)
├── docs/
│   ├── WHITEPAPER.md       # Technical whitepaper
│   ├── ARCHITECTURE.md     # System architecture
│   └── API.md              # Backend API reference
├── app.py                  # Flask backend
├── db.py                   # SQLite database
├── deploy.py               # Contract deployment script
├── add_liquidity.py        # Liquidity management
├── index.html              # Swap dApp
├── send.html               # Remittance dApp
├── yield.html              # Yield dApp
├── requirements.txt        # Python dependencies
└── .env.example            # Environment template
```

---

## 🗺️ Roadmap

| Phase | Description | Status |
|---|---|---|
| Phase 1 | USDC ↔ EURC swap on Arc Testnet | ✅ Complete |
| Phase 2 | NGN remittance + 10 country support | ✅ Complete |
| Phase 3 | USYC yield tab | ✅ Complete |
| Phase 4 | Multi-currency (KES, GHS, XOF, FCFA) | ✅ Complete |
| Phase 5 | Bitnob/Flutterwave live integration | 🔄 In Progress |
| Phase 6 | Arc Mainnet deployment | ⏳ Awaiting Arc Mainnet |
| Phase 7 | AMM model + open liquidity | 📋 Planned |
| Phase 8 | Chainlink oracle integration | 📋 Planned |
| Phase 9 | Multi-chain (Base, Stellar, Polygon) | 📋 Planned |
| Phase 10 | Mobile app (React Native) | 📋 Planned |

---

## 🤝 Contributing

We welcome contributions from the African developer community.

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-feature`
3. Commit: `git commit -m 'Add my feature'`
4. Push: `git push origin feature/my-feature`
5. Open a Pull Request

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 👨‍💻 Builder

Built by **Musa AIS**

- X: [@KudiArc](https://twitter.com/KudiArc)
- Discord: [discord.gg/kudiarc](https://discord.gg/kudiarc)

---

<div align="center">

**Built on Arc Network**

*Kudi means money. Arc means infrastructure. Together — financial freedom for Africa.*

</div>
