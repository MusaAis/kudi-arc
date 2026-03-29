**🟡 KUDI ARC**

Whitepaper

**Stablecoin FX & Remittance Protocol for Africa**

Version 1.0 · March 2026 · Arc Testnet (Mainnet Pending)

Built on Arc Network · Powered by Circle USDC & EURC

*github.com/KudiArc/kudi-arc · \@KudiArc · discord.gg/kudiarc*

**Table of Contents**

Abstract 3

1\. Problem Statement 3

2\. Solution Overview 4

3\. Competitive Landscape 5

4\. Smart Contract Architecture 6

5\. Backend Architecture 7

6\. Fiat Settlement Layer 8

7\. Supported Countries & Currencies 9

8\. Fee Structure & Protocol Economics 9

9\. Security Model 10

10\. Governance & Upgrade Path 11

11\. Market Opportunity (TAM) 11

12\. Roadmap 12

13\. Team 13

14\. Why Arc Network 13

15\. Risks & Mitigations 14

16\. Legal Disclaimer 15

**Abstract**

Kudi Arc is a hybrid on-chain FX desk and remittance engine for Africa
--- built on Arc Network, the first EVM-compatible blockchain that uses
USDC as its native gas token. By combining on-chain stablecoin swaps,
off-chain fiat settlement, and tokenized yield, Kudi Arc addresses three
structural failures in African financial infrastructure: prohibitive
remittance costs, forex scarcity, and the absence of accessible low-risk
yield products.

The protocol deploys KudiSwap V2 --- a Solidity 0.8.24 smart contract
--- on Arc Testnet (Chain ID 5042002), enabling USDC ↔ EURC swaps with
live FX rates, a 0.30% fee, and multi-layered security controls. A
Flask-based backend bridges on-chain activity to licensed fiat payout
partners (Bitnob, Flutterwave, Wave) across 10 African countries. USYC
integration provides 4.86% APY on idle USDC balances via Circle\'s
tokenized US Treasury product.

This document describes the protocol\'s architecture, economic model,
security design, competitive positioning, risks, and roadmap to Arc
Mainnet.

**1. Problem Statement**

**1.1 The African Forex Crisis**

Nigeria --- Africa\'s largest economy with 220 million people ---
operates under severe foreign currency restrictions. The Central Bank of
Nigeria (CBN) strictly limits USD access for individuals and businesses,
creating a parallel market that routinely trades at a 30%+ premium over
the official rate. This forex scarcity forces businesses to absorb
currency risk and individuals to pay extortionate rates just to preserve
purchasing power.

This is not a Nigeria-only problem. Across sub-Saharan Africa, 14 of 54
countries maintain fixed or managed exchange rate regimes, creating
endemic FX friction for cross-border commerce.

**1.2 Remittance Costs**

Africa received over \$50 billion in inbound remittances in 2024 (World
Bank). The average cost to send money to sub-Saharan Africa remains 8.5%
--- the most expensive corridor in the world. A Nigerian diaspora worker
sending \$200 home loses \$17 in fees. Traditional corridors through
Western Union, MoneyGram, and local banks account for the majority of
this flow.

The UN Sustainable Development Goal target is 3% by 2030. Current
infrastructure is not on track.

**1.3 The Yield Gap**

Nigerian savings accounts offer 4--6% annual yield in NGN --- a currency
that devalued 70% against the USD in 2023 alone. In real USD-adjusted
terms, most Nigerian savers experience negative returns. There is no
accessible, low-risk, dollar-denominated yield product for ordinary
Africans without navigating complex crypto infrastructure.

**1.4 The Gas Problem in DeFi**

Every EVM-compatible dApp requires ETH (or the chain\'s native token)
for gas. For African users new to crypto, this creates an insurmountable
UX barrier: to use USDC, you must first acquire ETH, understand wallets,
manage two assets, and handle gas estimation. Arc Network eliminates
this entirely --- all gas is paid in USDC, meaning users only ever need
a single asset.

**2. Solution Overview**

Kudi Arc provides three financial primitives on Arc Network, each
targeting one of the problems above:

**2.1 FX Swap (KudiSwap)**

A USDC ↔ EURC swap contract with live rates sourced from the Open
Exchange Rates API, refreshed every 5 minutes. Key parameters:

-   Fee: 0.30% (vs 2--5% on traditional exchanges and 1--3% on competing
    crypto desks)

-   Slippage protection: minAmountOut parameter enforced on-chain

-   Rate timelock: 5-minute delay on any rate update --- prevents flash
    manipulation

-   Rate bounds: absolute (0.5--2.0 USDC/EURC) and relative (±20% per
    update) enforced in contract

**2.2 Remittance (KudiSend)**

A crypto-to-fiat flow that bridges on-chain stablecoin transfers to
local bank and mobile wallet payouts:

-   User initiates send with wallet signature (on-chain authorization)

-   Backend receives signed intent, validates, routes to licensed payout
    partner

-   Recipient receives NGN / KES / GHS / XOF / FCFA in their bank
    account or mobile wallet

-   Reference tracking on every transaction --- both on-chain TX hash
    and payout partner reference stored

-   Supports 10 African countries at launch; planned expansion to 20+ on
    mainnet

**2.3 Yield (KudiYield)**

Integration with Circle\'s USYC --- a tokenized money market fund backed
by short-duration US Treasury Bills, issued directly on Arc Network:

-   Current APY: 4.86% (variable, tracks Fed Funds Rate)

-   1 USYC ≈ \$1.02 USDC --- accrual-based pricing, not rebase

-   Eligible to non-US institutions (African entities qualify under
    Circle\'s allowlist)

-   No lock-up period --- withdrawals processed on demand

-   Earnings displayed in NGN equivalent to provide local context

Note: USYC access requires Circle allowlist approval. Kudi Arc has
applied and approval is pending. Until approved, the yield tab displays
estimated returns but deposits are not live.

**3. Competitive Landscape**

Kudi Arc occupies a distinct position: a crypto-native FX and remittance
protocol built specifically for Africa, on infrastructure (Arc) that
removes the gas complexity that has historically prevented adoption.

  -------------------------------------------------------------------------------------------------------
  **Provider**   **Type**      **Fee**    **Crypto-Native**   **Yield**   **On-Chain   **Africa-First**
                                                                          Audit**      
  -------------- ------------- ---------- ------------------- ----------- ------------ ------------------
  Kudi Arc       Hybrid        0.30%      ✅ USDC-only        ✅ 4.86%    ✅ Arcscan   ✅
                 DeFi/CeFi                                    APY                      

  Yellow Card    Crypto        1.5--3%    ✅ Multi-token      ❌          ❌           ✅
                 Exchange                                                              

  Chipper Cash   Mobile Money  Free--2%   ❌ Fiat-only        ❌          ❌           ✅

  Grey Finance   Stablecoin    1%         ✅ USDC/USDT        ❌          ❌           ✅
                 Wallet                                                                

  Western Union  Traditional   5--9%      ❌ Fiat-only        ❌          ❌           ❌

  Lemfi          Neobank       0.5--1%    ❌ Fiat-only        ❌          ❌           Partial
  -------------------------------------------------------------------------------------------------------

Key differentiators over existing solutions:

-   Only USDC-as-gas chain (Arc) --- zero gas friction for non-crypto
    users

-   Only protocol offering on-chain FX swap + remittance + yield in a
    single interface

-   All swap logic is publicly verifiable on Arcscan --- no black-box
    rate manipulation

-   0.30% fee is among the lowest in the market for stablecoin FX

**4. Smart Contract Architecture**

**4.1 KudiSwap V2**

Contract Address: 0x8a10D0e61201000B5456EC725165892B08832C5f

Compiler: Solidity 0.8.24 · Network: Arc Testnet (Chain 5042002) ·
Status: Verified

**Core Functions**

  -----------------------------------------------------------------------
  **Function**                       **Description**
  ---------------------------------- ------------------------------------
  swapUSDCforEURC(amountIn,          Swap USDC to EURC with slippage
  minAmountOut)                      protection

  swapEURCforUSDC(amountIn,          Swap EURC to USDC with slippage
  minAmountOut)                      protection

  previewSwapUSDCtoEURC(amountIn)    Preview output amount before
                                     committing

  previewSwapEURCtoUSDC(amountIn)    Preview output amount before
                                     committing

  getPoolBalances()                  Returns current USDC and EURC pool
                                     reserves

  getStats()                         Returns cumulative swap volume and
                                     fee totals

  withdrawFees()                     Owner-only: extract accrued protocol
                                     fees

  updateRate(newRate)                Owner-only: update USDC/EURC rate
                                     (5-min timelock)
  -----------------------------------------------------------------------

**Security Controls**

  -----------------------------------------------------------------------
  **Threat**       **Control**      **Detail**
  ---------------- ---------------- -------------------------------------
  Reentrancy       nonReentrant     Custom \_locked bool guard on all
  attack           modifier         fund-moving functions

  Rate             5-min timelock + ±20% relative + 0.5--2.0 absolute per
  manipulation     bounds           update

  Liquidity drain  24-hr timelock + Maximum 50% pool withdrawal per
                   cap              transaction

  Token transfer   SafeERC20        Inline library --- reverts on false
  failure                           return values

  Ownership        Two-step         Pending owner must accept ---
  takeover         transfer         prevents typo lockout

  Extreme fee      2% cap           setFee() reverts above 200 basis
  setting                           points

  Min liquidity    1 USDC floor     Contract reverts if withdrawal would
  breach                            drain pool

  Flash price      Rate timelock    5-minute delay gives users time to
  manipulation                      react or exit
  -----------------------------------------------------------------------

**4.2 Trust Model**

Kudi Arc is deliberately custodial, not a trustless AMM. This is a
considered design choice, not a limitation:

-   NGN offramp always requires a licensed, centralized payout partner
    --- full decentralization of fiat settlement is not feasible

-   Mobile-first African users need simplicity; requiring private key
    management for every action increases abandonment

-   Comparable to Bitnob, Yellow Card, and Grey --- all trusted
    operators with similar hybrid models

Transparency measures that compensate for custodial nature:

-   Contract source code publicly verified on Arcscan

-   All admin actions (rate updates, withdrawals) emit on-chain events
    with timestamps

-   5--24 hour timelocks give users advance warning of any owner action

-   Rate bounds (absolute and relative) prevent extreme manipulation
    even by the owner

The roadmap toward progressive decentralization is outlined in Section
10.

**5. Backend Architecture**

The Flask backend (Python 3.10) bridges on-chain activity to off-chain
payout infrastructure and FX data sources.

**5.1 API Endpoints**

  ---------------------------------------------------------------------------------
  **Endpoint**           **Method**   **Description**
  ---------------------- ------------ ---------------------------------------------
  /api/rates             GET          Live USD/EUR/NGN rates --- refreshed every 5
                                      minutes via Open Exchange Rates

  /api/countries         GET          10-country payout configs with live local
                                      currency rates

  /api/banks             GET          Nigerian bank list (26 banks) for account
                                      validation

  /api/send/payout       POST         Universal payout endpoint --- routes to
                                      correct partner per country

  /api/swap/record       POST         Records on-chain swap event to local database

  /api/history/:wallet   GET          Per-wallet transaction history (swaps +
                                      sends)

  /api/yield/stats       GET          USYC APY and pool data from Circle

  /api/yield/balance     GET          User\'s USYC position by wallet address
  ---------------------------------------------------------------------------------

**5.2 Database**

Current: SQLite (appropriate for testnet/beta scale). The transactions
table records swap events (tx_hash, amount_in, amount_out, rate,
timestamp) and send events (recipient, bank_code, reference, status,
timestamp), indexed by wallet address.

Production migration plan: PostgreSQL on Arc Mainnet deployment.
SQLite\'s single-writer model is not suitable for concurrent production
traffic. This is a known testnet limitation and is addressed in the
Phase 6 mainnet checklist.

**5.3 Infrastructure**

Current deployment: Oracle Cloud Free Tier Ubuntu VM (129.80.199.123),
running Flask via Gunicorn with Nginx reverse proxy, managed by systemd.
No load balancing or failover at testnet stage.

Mainnet target: Dedicated VM with PostgreSQL, Redis for rate caching,
and Nginx with SSL termination.

**6. Fiat Settlement Layer**

This is the highest-risk component of the protocol and deserves detailed
treatment. Fiat settlement depends on licensed third-party payout
partners --- Bitnob, Flutterwave, and Wave --- each covering specific
country corridors.

**6.1 Transaction Flow**

A complete remittance transaction follows this sequence:

-   1\. User connects wallet and inputs: destination country, recipient
    bank details, amount in USDC/EURC

-   2\. User signs a message with their wallet (authorization --- no
    on-chain transfer at this step)

-   3\. Backend validates: signature, KYC check (basic), amount within
    daily limit, partner availability

-   4\. Backend locks the user\'s USDC by initiating the on-chain
    transfer

-   5\. Backend calls the relevant partner API (Bitnob for NGN,
    Flutterwave for KES/GHS/ZAR, Wave for XOF/XAF)

-   6\. Partner processes payout to recipient\'s bank account or mobile
    wallet

-   7\. Backend receives webhook confirmation and updates transaction
    status in DB

-   8\. User sees confirmed status with partner reference number

**6.2 Partner Coverage**

  -------------------------------------------------------------------------
  **Partner**   **Countries Covered**    **Payout Methods** **Integration
                                                            Status**
  ------------- ------------------------ ------------------ ---------------
  Bitnob        Nigeria (NGN)            Bank transfer,     Live (testnet)
                                         mobile money       

  Flutterwave   Kenya, Ghana, South      Bank transfer,     Live (testnet)
                Africa, Tanzania,        M-Pesa, MTN Mobile 
                Uganda, Rwanda           Money              

  Wave          Senegal, Côte d\'Ivoire, Wave mobile        Live (testnet)
                Cameroon                 wallet, Orange     
                                         Money              
  -------------------------------------------------------------------------

**6.3 Failure Modes & Recovery**

Partner API failures are the primary operational risk. The following
failure handling is implemented:

-   Partner timeout (\>30s): Transaction is flagged as \'pending\' ---
    user receives reference number, team manually resolves within 2
    hours

-   Bank rejection (invalid account): USDC is returned to user wallet
    within 10 minutes --- no funds lost

-   Partial payout: Backend detects mismatch via webhook amount,
    escalates to manual review

-   Partner downtime: Country routes are disabled automatically ---
    users see \'temporarily unavailable\' for affected countries

Current SLA (testnet): Best-effort 2-hour resolution. Mainnet target:
automated retry with 30-minute maximum resolution time for all
reversible failures.

**6.4 KYC & Compliance**

At testnet stage, KYC is minimal (wallet address + phone number for
mobile payouts). Mainnet compliance architecture will depend on Arc
Network\'s regulatory framework and partner requirements.
Nigeria-specific: CBN\'s Virtual Asset Service Provider (VASP)
regulations will apply to NGN offramp operations. Kudi Arc is monitoring
the regulatory landscape and will integrate full KYC/AML before NGN
mainnet launch.

**7. Supported Countries & Currencies**

  -------------------------------------------------------------------------------------
  **Country**     **Currency**   **Symbol**   **Payout Methods**        **Partner**
  --------------- -------------- ------------ ------------------------- ---------------
  🇳🇬 Nigeria      NGN            ₦            Bank transfer, mobile     Bitnob
                                              money                     

  🇰🇪 Kenya        KES            KSh          Bank transfer, M-Pesa     Flutterwave

  🇬🇭 Ghana        GHS            GH₵          Bank transfer, MTN MoMo   Flutterwave

  🇿🇦 South Africa ZAR            R            Bank transfer (EFT)       Flutterwave

  🇸🇳 Senegal      XOF            CFA          Wave wallet, Orange Money Wave

  🇨🇮 Côte         XOF            CFA          Wave wallet, MTN MoMo     Wave
  d\'Ivoire                                                             

  🇨🇲 Cameroon     XAF            FCFA         Wave wallet, Orange Money Wave

  🇹🇿 Tanzania     TZS            TSh          Bank transfer, M-Pesa     Flutterwave

  🇺🇬 Uganda       UGX            USh          Bank transfer, MTN MoMo   Flutterwave

  🇷🇼 Rwanda       RWF            RF           Bank transfer, MTN MoMo   Flutterwave
  -------------------------------------------------------------------------------------

FX data sourced from Open Exchange Rates API, covering NGN, KES, GHS,
ZAR, XOF, XAF, TZS, UGX, and RWF --- 9 currencies refreshed every 5
minutes. Mainnet expansion target: 20+ countries including Ethiopia,
Mozambique, and Egypt.

**8. Fee Structure & Protocol Economics**

**8.1 Fee Schedule**

  ---------------------------------------------------------------------------
  **Action**         **Fee**   **Collected   **Recipient**
                               In**          
  ------------------ --------- ------------- --------------------------------
  USDC → EURC Swap   0.30%     USDC          Protocol treasury

  EURC → USDC Swap   0.30%     EURC          Protocol treasury

  Remittance send    0.30%     USDC          Protocol treasury + partner

  USYC deposit       0%        ---           ---

  USYC withdrawal    0%        ---           ---
  ---------------------------------------------------------------------------

**8.2 Fee Mechanics**

Swap fees are collected in the swap token and tracked separately via the
accruedFees state variable. Fees cannot be withdrawn together with
liquidity --- they are extracted exclusively via the withdrawFees()
function. This separation prevents the owner from obscuring fee
extraction within liquidity operations.

The fee cap is enforced in-contract at 2% (200 basis points) --- no fee
above this can be set regardless of owner intent.

**8.3 Protocol Economics (No Token)**

Kudi Arc does not have a protocol token at this stage. All protocol fees
accumulate in the contract treasury (USDC/EURC) and are used to fund
development, partner integrations, and operational costs.

Token considerations: A future governance token (KUDI) is under
consideration for Phase 7+ to facilitate decentralized LP governance and
fee distribution. This is not committed --- any token launch would be
announced with a separate tokenomics document.

**8.4 Revenue Projections**

At 0.30% fee on \$1M monthly swap volume: \$3,000/month in protocol
fees. At 1% market penetration of Nigeria\'s \~\$20B annual inbound
remittance corridor: \$600,000/year in remittance fees. These are
illustrative projections only, not forecasts.

**9. Security Model**

**9.1 Audit Results**

KudiSwap V2 was audited via SolidityScan (automated static analysis):

-   Security Score: 60.58 / 100 --- this score reflects the automated
    tool\'s weighting, which penalises custodial design patterns
    (owner-controlled rate updates). These patterns are intentional and
    documented.

-   Threat Score: 97 / 100 --- Low Risk

-   Critical Vulnerabilities: 0

-   High Vulnerabilities: 0

The gap between the security score (60.58) and threat score (97)
reflects SolidityScan\'s methodology: custodial controls (setRate,
setFee, withdrawLiquidity) reduce the \'decentralization score\'
component but do not represent active threats. All owner-callable
functions have explicit timelocks and bounds. A manual audit by an
independent firm is planned before mainnet deployment.

**9.2 Known Limitations**

-   Oracle dependency: FX rates are sourced from a centralized API (Open
    Exchange Rates). A compromised or stale feed could result in
    unfavorable swap rates. Chainlink oracle integration (Phase 8) will
    address this.

-   Custodial rate control: The owner can update rates within bounds,
    with a 5-minute delay. Users who disagree with a new rate have 5
    minutes to exit.

-   Single-key ownership: Current deployment uses a single EOA owner.
    Mainnet will use a 3-of-5 multisig (Phase 6).

-   No formal audit: Automated tooling is not a substitute for manual
    review. A third-party audit is planned for Q2 2026.

**10. Governance & Upgrade Path**

Kudi Arc launches as a centralized protocol with a clear, time-bound
path toward progressive decentralization. This mirrors the model used by
Uniswap, Aave, and Compound --- all of which launched with centralized
control before transitioning to governance.

  -----------------------------------------------------------------------
  **Phase**     **Governance Model**                       **Timeline**
  ------------- ------------------------------------------ --------------
  Testnet (Now) Single EOA owner --- full control with     Current
                timelocked actions                         

  Mainnet       3-of-5 multisig (Gnosis Safe) --- team + 2 Q3 2026
  Launch (Phase external signers                           
  6)                                                       

  V3 (Phase 9)  Chainlink oracle anchors rates --- owner   Q4 2026
                can no longer set arbitrary rates          

  V4 (Phase 7+) AMM formula replaces manual rates ---      2027
                owner rate control removed                 

  DAO (Future)  KUDI governance token --- community fee    TBD
                votes, LP governance                       
  -----------------------------------------------------------------------

Admin action event log: Every privileged action (rate update, fee
change, liquidity withdrawal) emits an on-chain event. Users can monitor
the contract\'s event log on Arcscan at any time to verify admin
behavior.

**11. Market Opportunity (TAM)**

  ------------------------------------------------------------------------
  **Market Segment**       **Size**            **Kudi Arc Relevance**
  ------------------------ ------------------- ---------------------------
  Sub-Saharan Africa       \$54B/year (World   Primary: KudiSend
  inbound remittances      Bank 2024)          

  Nigeria inbound          \$20B+/year         Primary: KudiSend (NGN
  remittances                                  corridor)

  Africa stablecoin        \$180B+/year        Primary: KudiSwap
  transaction volume       (Chainalysis 2024)  

  Africa crypto user base  53M+ users (Nigeria Addressable user base
                           #3 globally)        

  Tokenized T-Bill market  \$500M+ AUM and     KudiYield integration
  (USYC)                   growing             

  Africa FX market         \$50B+/year         KudiSwap primary target
  (informal)               parallel market     
                           volume              
  ------------------------------------------------------------------------

Conservative scenario: Capture 0.1% of Nigeria\'s \$20B remittance
corridor = \$20M annual volume = \$60,000/year in fees at 0.30%.

Growth scenario: 1% of Nigeria + 0.5% of remaining 9 countries =
\~\$400M annual volume = \$1.2M/year in fees.

These figures are illustrative and subject to regulatory, technical, and
market risks outlined in Section 15.

**12. Roadmap**

**Testnet --- Q1 2026 (Current)**

  -----------------------------------------------------------------------
  **Milestone**                                 **Status**
  --------------------------------------------- -------------------------
  KudiSwap V2 deployed & verified on Arcscan    ✅ Complete

  10-country remittance support (KudiSend)      ✅ Complete

  USYC yield integration (KudiYield)            ✅ Complete --- pending
                                                Circle allowlist

  Transaction history per wallet                ✅ Complete

  SolidityScan security audit                   ✅ Complete

  Bitnob API integration (NGN live)             ✅ Complete

  Flutterwave API integration                   ⏳ In Progress --- Q1
                                                2026

  Circle USYC allowlist approval                ⏳ Pending --- applied

  User beta testing program                     ⏳ In Progress
  -----------------------------------------------------------------------

**Mainnet --- Q3 2026**

  -----------------------------------------------------------------------
  **Milestone**                                 **Target Date**
  --------------------------------------------- -------------------------
  Arc Mainnet deployment                        Q3 2026

  Real NGN/KES/GHS/XOF live payouts             Q3 2026

  Custom domain + SSL + CDN                     Q3 2026

  PostgreSQL migration (replace SQLite)         Q3 2026

  3-of-5 multisig ownership transfer            Q3 2026

  Third-party smart contract audit              Q2 2026 (pre-mainnet)

  20-country expansion                          Q4 2026
  -----------------------------------------------------------------------

**V3 --- Q4 2026**

  -----------------------------------------------------------------------
  **Milestone**                                 **Target Date**
  --------------------------------------------- -------------------------
  Chainlink oracle integration (replace         Q4 2026
  centralized FX feed)                          

  AMM model (x\*y=k) replacing manual rate      Q4 2026
  setting                                       

  Open liquidity provision + LP token system    Q4 2026

  Multi-chain deployment (Base, Stellar)        Q4 2026
  -----------------------------------------------------------------------

**V4 --- 2027**

  -----------------------------------------------------------------------
  **Milestone**                                 **Target Date**
  --------------------------------------------- -------------------------
  Mobile app (React Native) --- iOS & Android   Q1 2027

  KUDI governance token consideration           Q2 2027

  DAO governance framework                      Q3 2027

  Expand to 30+ African countries               2027
  -----------------------------------------------------------------------

**13. Team**

**Musa AIS --- Founder & Lead Developer**

Full-stack developer based in Nigeria with expertise spanning smart
contract development (Solidity/Foundry), backend systems
(Python/FastAPI/Flask), web frontend (React/ethers.js), and mobile
(Flutter). Previous projects include:

-   Vortix Signal Bot --- crypto futures signal service for Gate.io USDT
    Perpetuals, using ICT/SMC analysis with backtesting infrastructure
    and Twitter/X integration

-   ArcPay DeFi Protocol --- PaymentHub and LendingPool smart contracts
    on Arc Testnet with USDC payments, escrow, subscriptions, and 75%
    LTV lending

-   VTU Platform --- Virtual Top-Up platform for Nigerian mobile
    data/airtime resellers, built on FastAPI + React + Flutter

GitHub: github.com/KudiArc · X (Twitter): \@KudiArc · Discord:
discord.gg/nEJfqrAsqc 

*Advisory & Hiring: Kudi Arc is actively seeking advisors with
backgrounds in African fintech regulation, Bitnob/Flutterwave
partnership experience, and Arc Network ecosystem development. Community
contributors are welcomed via GitHub.*

**14. Why Arc Network**

Arc Network is the only blockchain that satisfies all requirements for
an Africa-first stablecoin protocol simultaneously:

  -----------------------------------------------------------------------------
  **Requirement**    **Arc         **Ethereum L1** **Polygon**   **Base**
                     Network**                                   
  ------------------ ------------- --------------- ------------- --------------
  Gas paid in USDC   ✅ Native     ❌ Requires ETH ❌ Requires   ❌ Requires
                                                   MATIC         ETH

  Circle             ✅            Bridged only    Bridged only  Bridged only
  USDC/EURC/USYC     First-class                                 
  native                                                         

  EVM compatible     ✅ Yes        ✅ Yes          ✅ Yes        ✅ Yes

  Deterministic      ✅ Yes        Probabilistic   ✅ Yes        ✅ Yes
  finality                                                       

  Low transaction    ✅ \<\$0.01   ❌ \$1--50+     ✅ \<\$0.01   ✅ \<\$0.01
  cost               USDC                                        

  MetaMask / Rabby   ✅ Yes        ✅ Yes          ✅ Yes        ✅ Yes
  support                                                        
  -----------------------------------------------------------------------------

The USDC-as-gas design is not a minor convenience --- it is the
foundational unlock for African adoption. Requiring users to hold ETH
before they can use USDC has historically been the primary barrier to
DeFi adoption in developing markets. Arc eliminates this barrier at the
infrastructure level.

**15. Risks & Mitigations**

  ----------------------------------------------------------------------------------------------------
  **Risk**               **Category**     **Likelihood**   **Impact**   **Mitigation**
  ---------------------- ---------------- ---------------- ------------ ------------------------------
  CBN regulatory action  Regulatory       Medium           High         Monitor VASP framework;
  against crypto-to-NGN                                                 partner with CBN-licensed
  offramp                                                               Bitnob; structure as
                                                                        stablecoin bridge not exchange

  Arc Mainnet delayed or Infrastructure   Low              High         Protocol contracts are
  cancelled                                                             EVM-compatible; can deploy to
                                                                        Base or Polygon with minor
                                                                        configuration changes

  Payout partner         Operational      Medium           Medium       Multi-partner routing;
  (Bitnob/Flutterwave)                                                  fallback to manual resolution;
  API failure                                                           SLA monitoring with automated
                                                                        country disable

  FX oracle manipulation Technical        Low              Medium       ±20% rate bounds in contract
  (Open Exchange Rates)                                                 prevent extreme manipulation;
                                                                        Chainlink integration in Phase
                                                                        8

  Smart contract exploit Technical        Low              High         Reentrancy guards, timelocks,
                                                                        SafeERC20; third-party audit
                                                                        pre-mainnet; bug bounty
                                                                        program planned

  Circle USYC allowlist  Business         Medium           Low          USYC yield is additive, not
  rejection                                                             core; swap and remittance
                                                                        features are fully independent

  Single-founder key     Team             Medium           High         Open-source codebase; seeking
  person risk                                                           co-founder and advisors;
                                                                        multisig on mainnet

  SQLite data loss at    Technical        High (testnet    Low          PostgreSQL migration is Phase
  scale                                   only)                         6 pre-mainnet requirement ---
                                                                        already planned

  NGN devaluation        Market           High             Low          USYC yield is USD-denominated;
  reduces real yield                                                    NGN display is informational
                                                                        only --- users earn in USDC
  ----------------------------------------------------------------------------------------------------

**16. Legal Disclaimer**

This whitepaper is provided for informational purposes only and does not
constitute financial advice, investment advice, or a solicitation to
purchase any securities or financial instruments. Nothing in this
document should be construed as a promise or representation about future
performance, returns, or outcomes.

Kudi Arc is currently deployed on Arc Testnet only. All figures (swap
volumes, fees, APY) reflect testnet activity and are not indicative of
mainnet performance. Testnet tokens have no real monetary value.

Participation in DeFi protocols involves significant risk, including but
not limited to: smart contract vulnerabilities, regulatory changes,
counterparty risk with fiat payout partners, and total loss of funds.
Users should conduct their own research and consult qualified advisors
before engaging with any financial protocol.

Kudi Arc does not hold a money transmission license, virtual asset
service provider license, or any financial services license in any
jurisdiction as of the date of this document. Mainnet operations will be
structured in compliance with applicable regulations, particularly CBN
VASP guidelines in Nigeria.

All contract addresses, APY figures, and partner integrations referenced
in this document are subject to change. The most current information is
always available at github.com/KudiArc/kudi-arc.

*--- End of Whitepaper ---*

**Kudi Arc · Built in Africa · github.com/KudiArc/kudi-arc**
