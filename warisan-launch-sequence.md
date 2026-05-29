# Warisan — Phased Launch Sequence

*Order features so each phase pays for the next. Spend money only after the previous phase proves revenue.*

> **Core principle:** Build cheap-first, regulated-last. Every phase must be cash-flow positive (or break-even) before you fund the next. Never build the expensive, regulated parts on hope.

---

## The Sequencing Logic

Three axes decide what gets built when:

1. **Cost to build & run** — static < server-light < server-heavy < money-movement
2. **Revenue unlocked** — does it create a reason to pay *today*?
3. **Regulatory risk** — does it touch money, personal data, or legal liability?

The sequence below is ordered so you climb the cost curve **only after revenue justifies it**.

---

## PHASE 0 — Validate (Week 0–4)
### "Does anyone actually want this?"

**Build:** Nothing new. You already have it.
**Ship:** The MVP landing page + free calculator + roadmap.

| | |
|---|---|
| **Cost** | ~RM 20/mo (domain only — stays static) |
| **Revenue** | RM 0 (intentional) |
| **Regulatory** | None |
| **Goal** | Traffic + a waitlist + manual sales conversations |

**What you're testing:** Will people use the free calculator? Do they ask "what do I do next?" Do they say "can you just do the paperwork for me?"

**The manual hack:** When someone asks for help, do it *by hand* over WhatsApp and charge RM 99–149. No software. You are the backend. This validates willingness-to-pay before you write a line of backend code.

✅ **Gate to Phase 1:** You've manually sold ≥10 Resolution Packs. People pay for the paperwork. Proceed.

---

## PHASE 1 — Document Packs (Month 1–2)
### "Sell the paperwork — server-light, high margin"

**Build:** Document generation + a simple payment button (one-time, not recurring).

| | |
|---|---|
| **Cost** | ~RM 100–250/mo (light backend + doc generation + Stripe/Billplz one-time) |
| **Revenue** | RM 149 × packs sold. **~95% margin.** |
| **Regulatory** | Low — one-time payment *to you* for a service. No holding third-party money. |
| **Break-even** | ~2 packs/month |

**Why this is first:** Highest margin, lowest regulatory risk, and it monetises the traffic Phase 0 already proved. One-time payments to your own account are clean — no e-money issues.

**Tech:** A serverless function (Vercel/Cloudflare Workers) that fills PDF templates. Stripe/Billplz Checkout for the RM149. That's it.

✅ **Gate to Phase 2:** Consistent pack sales + users asking "can I track the property's rent here too?" Proceed.

---

## PHASE 2 — Rental Tracking (Month 2–4)
### "The sticky core — cheap to run, builds the data moat"

**Build:** The property manager (you already have the front-end). Add a real database + accounts. **No money movement yet** — just *tracking*.

| | |
|---|---|
| **Cost** | ~RM 150–350/mo (database + auth + storage) |
| **Revenue** | Pro RM29/mo subscriptions |
| **Regulatory** | Medium — PDPA applies (you now store personal data). Get a privacy policy. |
| **Break-even** | ~10 Pro subscribers |

**Why this is second:** This is your **retention engine and data moat**. It's cheap (tracking is just database reads/writes — no expensive notifications, no payment rails). Every month of data deepens lock-in. Recurring revenue starts here.

**Critical scope discipline:** In this phase, users *log* payments manually. They *generate* tenancy agreements. They *see* disbursement splits. But Warisan does **not** send money or send automated notifications yet — those are the expensive Phase 3/4 items.

✅ **Gate to Phase 3:** Pro subscribers retain past month 2. Users ask "can it remind my tenant automatically?" Proceed.

---

## PHASE 3 — Notifications (Month 4–5)
### "Automate the nagging — the first variable cost"

**Build:** Automated reminders (rent due, overdue, disbursement-ready) via WhatsApp/SMS/email.

| | |
|---|---|
| **Cost** | ~RM 300–900/mo at scale (this is the first *per-message* cost) |
| **Revenue** | Drives Pro→Estate upgrades; reminders are a top-requested paid feature |
| **Regulatory** | Low-medium — consent for messaging (PDPA), WhatsApp Business API approval |
| **Margin watch** | ⚠️ Email first (near-free), WhatsApp only on paid tiers |

**Why this is third, not earlier:** Notifications are the **first feature with real per-unit cost**. 1,000 users × 3 messages/mo can be RM 300–900/mo. You only fund this once recurring subscription revenue (Phase 2) can absorb it.

**Margin protection:**
- **Free/Pro tier:** email reminders (essentially free)
- **Estate tier only:** WhatsApp reminders (the costly channel)
- This makes the expensive channel a paid upsell, not a margin leak

✅ **Gate to Phase 4:** MRR comfortably exceeds total infra + messaging cost with buffer. Legal structure for money-movement confirmed. Proceed.

---

## PHASE 4 — Payment Collection (Month 5–7)
### "Move the money — last, because most expensive + most regulated"

**Build:** Live payment links. Tenant pays → landlord receives → Warisan takes a fee → system auto-reconciles.

| | |
|---|---|
| **Cost** | Gateway 1.5–2.9% + ~RM1/txn (passed through, not absorbed) |
| **Revenue** | Transaction fee (must *exceed* gateway cost) + drives Estate tier (0% fee) |
| **Regulatory** | ⚠️ **HIGH — confirm with fintech lawyer BEFORE building** |
| **Margin watch** | ⚠️ Your fee must clear the gateway fee or you lose money per transaction |

**Why this is LAST:**
1. **Most regulated.** Routing tenant money may trigger Bank Negara e-money / payment-services rules. *Must* be structured so funds flow **directly tenant→landlord** with you taking only a fee — never holding the money.
2. **Most expensive per transaction.** Gateway fees eat margin if mispriced.
3. **Highest liability.** Money disputes are the worst kind.

**The clean structure:** Use a gateway (Billplz/iPay88/Stripe Connect) where the landlord is the merchant of record. Money never touches your account. You're a software facilitator taking a SaaS/usage fee — not a payment institution. Confirm this *exact* structure with a lawyer before writing code.

**Pricing discipline:**
- Free tier: 1% fee (above the ~0.8% gateway floor for FPX)
- Pro: 0.5%
- Estate: 0% (the upsell — heavy users go Estate to kill the fee)

✅ **Gate to Phase 5:** Payment volume flowing, reconciliation working, no regulatory flags. Proceed to expansion.

---

## PHASE 5+ — Expand (Month 7+)
### "Now compound — twice-a-month pipeline kicks in"

Once the money engine works and is legal, layer the high-value features from the twice-a-month pipeline, prioritising those that **deepen lock-in or add usage revenue**:

- Beneficiary portal (heirs log in → referral engine)
- Tax-ready annual reports (LHDN exports → Estate tier stickiness)
- Bill OCR (snap utility bill → auto-log)
- Maintenance vendor directory (referral margin)
- Multi-estate management (amil/lawyer accounts → B2B revenue)
- Open-banking auto-reconciliation

---

## The Whole Picture at a Glance

| Phase | What | Monthly Cost | Revenue Model | Reg Risk | Build before next when… |
|---|---|---|---|---|---|
| **0** | Validate (manual) | ~RM 20 | Manual RM149 sales | None | 10 packs sold by hand |
| **1** | Document packs | ~RM 100–250 | RM149 one-time | Low | Consistent pack sales |
| **2** | Rental tracking | ~RM 150–350 | RM29/mo Pro | Medium (PDPA) | 10 Pro subs retaining |
| **3** | Notifications | ~RM 300–900 | Upsell driver | Low-med | MRR > costs + buffer |
| **4** | Payment collection | gateway % | Txn fee + Estate | **HIGH** | Lawyer-confirmed structure |
| **5+** | Expand | scales | Mixed | Varies | Money engine stable |

---

## Three Rules to Tape to Your Wall

1. **Never build the next phase until the current one pays for it.** Revenue funds growth, not optimism.

2. **Climb the cost curve slowly: static → server-light → server-heavy → money-movement.** Each step is 5–10× the cost and risk of the last.

3. **The expensive, regulated payment piece is LAST — and only after a lawyer confirms you never hold the money.** This is the one mistake that can end the business, not just dent it.

---

## Realistic Money Timeline

- **Month 0:** ~RM 20/mo cost · revenue from manual sales
- **Month 2:** ~RM 250/mo cost · break even at 2 packs + a few Pro subs
- **Month 4:** ~RM 500/mo cost · 20+ Pro subs = comfortably profitable
- **Month 7:** ~RM 1,500–4,000/mo cost · payment fees + 50+ subs = scaling
- **The whole point:** you never front more than ~RM 500/mo of risk until the product has *proven* it can pay for itself.

You can launch Phase 0 **this week** for the price of a domain. Everything after that is funded by the phase before it.

---

*Warisan Protocol — Build cheap-first. Let revenue fund the climb.*
