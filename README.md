# WARISAN — وارثان

**Estate Resolution Infrastructure for Malaysia**

> A blockchain-anchored framework for resolving intestate estates — serving both Syariah (Faraid) and civil law tracks, handling liquid and non-liquid assets, with settlement issuance as a verifiable on-chain financial instrument.

---

## Live Pages

| Page | Description |
|------|-------------|
| [**System Concept**](warisan-concept.html) | Full system design brief — architecture, dual-track compliance, illiquid asset resolution, partner ecosystem, revenue model, and investment thesis |
| [**Distribution Calculator**](warisan-calculator.html) | Interactive Faraid & civil intestate calculator with trilingual support (EN / BM / AR) and non-liquid asset ledger (Mode A/B/C) |
| [**API Reference**](warisan-api.html) | Developer documentation for all 18 REST endpoints across Identity, Estate, Asset Registry, Distribution Engine, SBT Settlement, and Institutional API layers |

---

## What is Warisan?

Malaysia holds an estimated **RM 9B+** in economically frozen unclaimed estate assets. The existing system has no digital resolution pathway, no self-service mechanism for heirs, and no way for verified asset holders to leverage what they legitimately own.

Warisan is a consortium-chain estate resolution protocol designed to solve this end-to-end:

1. **Identity verification** — MyKad NFC + JPN death registry integration
2. **Asset mapping** — connects to Land Office, EPF, Bursa, LHDN, and Amanah Raya APIs
3. **Dual-track distribution** — Faraid engine (Quran 4:11–12) for Muslim estates; Distribution Act 1958 for civil intestate
4. **Non-liquid asset resolution** — three resolution modes (Collective Sale / Buyout / Co-ownership) with liquidity discount calculation
5. **Settlement SBT** — Soulbound Token issued per heir, Ethereum-anchored, accepted by partner FIs as collateral
6. **Institutional API** — banks, EPF, insurers push unresolved cases; Warisan resolves and returns a settlement hash

**Target institutional partner: Amanah Raya Berhad**

---

## Repository Structure

```
warisan/
├── index.html                  # Landing page
├── warisan-concept.html        # System design brief
├── warisan-calculator.html     # Distribution calculator (EN / BM / AR)
├── warisan-api.html            # API reference documentation
└── README.md                   # This file
```

All files are self-contained HTML — no build step, no dependencies, no server required. Deploy directly via GitHub Pages or Cloudflare Pages.

---

## The Distribution Calculator

The calculator implements two complete distribution engines:

### Syariah Track — Faraid Engine
- Full Ashab al-Furud (fixed-share) table: husband, wives (up to 4), father, mother, daughters, granddaughters, maternal half-siblings, full/paternal sisters
- Asabah (residuary) engine: sons + daughters at 2:1 ratio, full/paternal brothers + sisters at 2:1 ratio, father residue
- Hajb Hirman (blocking) rules fully enforced — blocked heirs listed explicitly
- Bayt al-Mal residue when no Asabah heir exists
- Paternal grandfather steps in if no father; maternal grandmother blocked by mother

### Civil Track — Distribution Act 1958
- All 9 intestate scenarios per Malaysian law
- Spouse + children (1/4 : 3/4), spouse + parents (1/2 : 1/2), spouse alone, children alone, parents alone, siblings, grandparents, uncles/aunts
- Grandchildren and nephews/nieces with prerequisite warnings

### Asset Ledger (Non-Liquid Assets)
- Add any number of assets with appraised value, type, and resolution mode
- **Mode A** — Collective Sale: liquidity discount 0–40% based on sale urgency
- **Mode B** — Buyout (افتداء): one heir compensates the rest, 0–20% discount
- **Mode C** — Co-ownership / Musya' (مشاع): undivided fractional title, 5–20% discount per NLC s.342
- Full equation: `NDE = GEV × (1 − Liquidity Discount) − Liabilities`
- Per-heir entitlement breakdown by asset with SBT badge

### Languages
Fully translated across all UI, heir labels, legal citations, warnings, and results:
- 🇬🇧 English (default)
- 🇲🇾 Bahasa Malaysia
- 🇸🇦 Arabic (with full RTL layout)

---

## The API

The Warisan REST API (`/v1/`) covers six layers:

```
POST   /identity/verify                         Verify heir or deceased identity (JPN)
POST   /identity/death-certificate              Register and confirm death
GET    /identity/{id}                           Get identity & death status

POST   /estates                                 Create a new estate
GET    /estates/{id}                            Get full estate record
PATCH  /estates/{id}/state                      Advance estate state machine
GET    /estates                                 List estates (paginated)

POST   /estates/{id}/assets                     Register an asset
GET    /ar/query?ic={ic}                        Query Amanah Raya holdings
PUT    /estates/{id}/assets/{asset_id}          Update valuation or resolution mode

POST   /distribution/faraid                     Calculate Faraid distribution
POST   /distribution/civil                      Calculate civil intestate distribution
POST   /distribution/nde                        Compute Net Distributable Estate
POST   /distribution/confirm                    Confirm and lock distribution to chain

POST   /sbt/issue                               Issue Settlement SBT per heir
GET    /sbt/{id}                                Verify SBT (full — partner FIs)
GET    /fi/sbt/verify?sbt_id={id}               FI quick-verify SBT (read-only)

POST   /institutional/cases                     Push unresolved case from partner institution
GET    /fi/sbt/verify                           FI SBT verification endpoint
```

**Request API access:** [carreyume@gmail.com](mailto:carreyume@gmail.com)

---

## Settlement SBT

Each heir receives a **Soulbound Token** on settlement — non-transferable, Ethereum-anchored, and accepted by partner financial institutions as proof of beneficial interest.

```
SettlementSBT {
  sbt_id                // hash(estate_id + heir_address)
  share_bps             // basis points (10000 = 100%)
  transferable          // always false
  resolution_modes      // encoded Mode A | B | C per asset
  sunset_trigger_date   // Mode C compulsory sale trigger
  ipfs_bundle_cid       // IPFS CID of full estate document bundle
  ethereum_anchor       // Ethereum tx hash of Merkle root
  jpn_sig               // JPN digital signature
  issued_at             // immutable issuance timestamp
  last_validated_at     // annual re-attestation timestamp
}
```

**Collateral terms for partner FIs:**
- Mode A/B resolved estate → up to 85% LTV
- Mode C (Musya' co-ownership) → 40–60% LTV haircut

---

## Compliance

| Track | Governing Law | Engine |
|-------|---------------|--------|
| Syariah | Quran 4:11–12, 4:176 · Shafi'i school | Faraid calculator with Hajb rules |
| Civil | Distribution Act 1958 (Act 300) | Intestate succession table |
| Assets | NLC s.342 (co-ownership) · JPPH valuation standards | NDE engine with liquidity discounts |
| Settlement | Consortium chain (Hyperledger Besu) · Ethereum mainnet anchor | SBT + OpenTimestamps |

State Syariah enactments may vary. Mixed-religion estates escalate to human adjudicators. This tool is indicative and not a substitute for qualified legal or Faraid advice.

---

## Deploying

### GitHub Pages (browser only)

1. Fork or create a new repo
2. Upload all four HTML files + `README.md`
3. **Settings → Pages → Source: Deploy from branch (main / root)**
4. Live at `https://[username].github.io/warisan/`

### Cloudflare Pages

1. Connect Cloudflare Pages to your GitHub repo
2. Framework: **None** · Build command: *(blank)* · Output: `/`
3. Deploy — live at `https://warisan.pages.dev` (or attach a custom domain)

No build step. No package.json. No server. Pure static HTML.

---

## Roadmap

- [ ] Amanah Raya API integration (AR Query endpoint)
- [ ] JPN death registry live connection (production)
- [ ] Consortium chain node deployment (Hyperledger Besu)
- [ ] SBT smart contract audit
- [ ] Partner FI onboarding (Maybank, CIMB, BSN pilot)
- [ ] Multilingual PDF export of distribution results
- [ ] Mobile-optimised calculator view
- [ ] Jabatan Agama Islam state validation integration

---

## Contact

**API Access & Partnerships:** [carreyume@gmail.com](mailto:carreyume@gmail.com)

---

*Warisan Protocol — Timeless Proof of Legacy*
*وارثان · Syariah & Conventional · On-Chain Settlement · Amanah Raya Integration*
