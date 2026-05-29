# Warisan — Defensibility & Velocity Strategy

*How to stay uncopyable and ship twice a month*

---

## Part 1 — The Honest Truth About "Can't Be Copied"

A front-end HTML page **can always be View-Source'd**. There is no client-side trick — obfuscation, minification, license checks — that stops a determined developer from copying a static page. Anyone who promises otherwise is selling snake oil.

So we stop trying to protect the *code* and instead build a moat around the things that are genuinely hard to copy. This is how every durable software business actually defends itself.

### The Four Real Moats

| Moat | Why a copier can't lift it | How we build it |
|------|----------------------------|-----------------|
| **1. Data network effect** | A clone starts with zero rental history, zero tenant records, zero estate cases. Our value compounds with every month of data. | Every payment logged, every disbursement, every resolved estate makes the product smarter and stickier. The dataset *is* the product. |
| **2. Server-side logic** | Payment processing, document generation, notification dispatch, and the Faraid engine's edge cases run behind an authenticated API. The browser only sees the thin UI shell. | Move all mutations server-side. The HTML is a dumb terminal; the brain is private. |
| **3. Switching cost** | Once a family has 18 months of rental records, beneficiary statements, and tenancy agreements in Warisan, leaving means losing their entire estate history. | Make the product the system of record. Export is allowed (trust), but nobody wants to re-enter years of data. |
| **4. Trust & distribution** | Estate matters are intimate. Families recommend Warisan to relatives — who are often co-heirs to the *same* estate. A clone has no trust and no referral loop. | Lean into the emotional, dignified brand. Build the referral loop into the disbursement flow (every beneficiary who receives a statement sees Warisan). |

### What the client-side "gate" actually does

In `warisan-property.html` there is a `WARISAN_LICENSE` integrity check and an origin allowlist. **Be clear about what this does and doesn't do:**

- ❌ It does **not** stop someone copying the HTML.
- ✅ It **does** make a copied static file non-functional, because all write operations (logging payments, generating documents, sending links) require a signed license token validated *server-side*. A clone with no valid token gets rejected by the API on every mutation.

The copier ends up with a pretty but dead shell. To make it work, they'd have to rebuild the entire backend — at which point they're not copying, they're competing, and they're 18 months behind on data.

### Practical anti-lift hardening (defence in depth)

1. **Thin client, fat server.** Never put business logic (pricing rules, Faraid edge cases, disbursement math) in the browser. Compute server-side, return only results.
2. **Authenticated API for every mutation.** Reading the demo UI is free; *doing* anything requires a token tied to a paying account.
3. **Signed, short-lived tokens.** License tokens expire hourly and are re-issued server-side. A scraped token dies fast.
4. **Rate limiting + anomaly detection.** Bulk scraping patterns get flagged and throttled.
5. **Watermarked generated documents.** Every PDF (tenancy agreement, statement) carries an invisible per-account fingerprint — if a clone's output appears, you know the source.
6. **Minify + bundle the shipped JS** so casual "vibe coders" can't trivially read structure. (Slows them; doesn't stop them — which is fine, because the backend is the real gate.)

> **Bottom line:** Speed and data win, not secrecy. Ship faster than anyone can copy, and make your accumulated data impossible to replicate.

---

## Part 2 — The Property & Rental Module (What's Built)

`warisan-property.html` is the front-end MVP of the rental management feature. It demonstrates:

- **Single-property free tier** with a hard paywall to add a 2nd property
- **Income vs cost chart** (6-month history)
- **Rental ledger** with paid / due / overdue status
- **Cost & maintenance tracking** (utilities, repairs, assessment, quit rent)
- **Beneficiary disbursement** — net income split by Faraid share, with "send statements" action
- **Tenant management** with lease tracking and arrears
- **Quick actions**: generate tenancy agreement, create payment link, send due reminder, log repair
- **Payment link generator** with copy-to-share
- **Paywall modal** with Pro (RM 29/mo) and Estate (RM 79/mo) tiers

### Monetisation in this module

| Plan | Price | Properties | Key unlocks |
|------|-------|-----------|-------------|
| **Free** | RM 0 | 1 | Track income/cost, manual ledger, basic disbursement view |
| **Pro** | RM 29/mo | up to 10 | Auto reminders, beneficiary reports, agreement generator, payment links, 0.5% txn fee |
| **Estate** | RM 79/mo | unlimited | Multi-estate, beneficiary portal, tax-ready exports, 0% txn fee, priority support |

Plus a **transaction fee** on collected rent (1% free, 0.5% Pro, 0% Estate) — a usage-based revenue stream that scales with customer success.

---

## Part 3 — The Twice-a-Month Feature Pipeline

Shipping velocity *is* the moat. Here is a concrete 6-month, 12-feature roadmap. Each is scoped to be shippable in ~2 weeks by a small team.

### Month 1
- **F1 — Auto payment reminders** (WhatsApp/SMS/email when rent is due/overdue). *Highest-value, drives Pro upgrades.*
- **F2 — Beneficiary statement PDF** (auto-generated monthly disbursement statement per heir).

### Month 2
- **F3 — Payment link + collection** (Billplz/Stripe integration, real money in).
- **F4 — Utilities bill OCR** (snap a TNB/Air Selangor bill → auto-logs the cost).

### Month 3
- **F5 — Tenancy agreement e-signing** (generate → send → tenant signs digitally).
- **F6 — Multi-property dashboard** (portfolio view across all estate properties).

### Month 4
- **F7 — Beneficiary portal** (read-only login for heirs to see their share & statements).
- **F8 — Tax-ready annual report** (LHDN-formatted rental income summary).

### Month 5
- **F9 — Maintenance vendor directory** (book vetted plumbers/electricians in-app, take a referral margin).
- **F10 — Arrears escalation workflow** (auto reminder → notice → demand letter generation).

### Month 6
- **F11 — Multi-estate management** (manage several deceased estates from one account — for amil, lawyers, large families).
- **F12 — Open banking auto-reconciliation** (match incoming bank transfers to expected rent automatically).

### Pipeline discipline

- **Ship every 1st and 15th.** Public changelog. Visible momentum signals to users (and would-be copiers) that you're uncatchable.
- **Each feature must either** (a) drive an upgrade, (b) add usage-fee revenue, or (c) deepen data lock-in. No vanity features.
- **Dogfood the disbursement loop.** Every new feature should, where possible, surface Warisan to *beneficiaries* — your built-in referral engine.

---

## Part 4 — Why This Beats a Copier Every Time

A vibe-coder clones your page on a Friday. By the time they've reverse-engineered the backend, integrated a payment gateway, handled Faraid edge cases, set up notification infra, and earned a single user's trust — you've shipped 4 more features, accumulated thousands of rental records, and your users have 3 months of history they'd never abandon.

**They're copying a photograph of a moving train.**

---

*Warisan Protocol — Speed is the moat.*
