# Mercury OMS — 15-Minute Demo Script

**"One order, front to back — on one golden dataset."**

| | |
|---|---|
| **Audience** | Mixed senior room: C-suite/COO/CIO, desk heads & RMs, compliance, tech team |
| **Duration** | 15:00 hard stop (14:00 of content + buffer) |
| **Build status** | Script-first — every "System does" block below is the build spec |
| **Voice language** | English (pre-recorded call audio; no live mic) |
| **Structure** | One continuous order thread. The rule authored in Scene 3 fires on the order captured in Scene 1. |
| **Apps on stage** | **Mercury Wealth** (advisor portal, live at mercury-wealth.vercel.app) + **Mercury OMS** (this build) — separate apps, one shared seeded dataset, deep-linked both ways |

---

## The thesis (memorize this)

> Every vendor in this market — including the one you run today — has bolted AI *onto* the order workflow as a chat sidebar. Mercury makes AI *the* workflow: voice becomes a compliance-checked ticket, compliance writes rules in English, and the audit file writes itself. Seven of the features you're about to see are shipped by **zero of the 11 vendors** we studied. And it runs front-to-back on one golden dataset — advisory cockpit and OMS reading the same book, the same suitability profiles, the same product shelf.

The demo covers white-space rows **A12, G4** (voice), **G3, B12** (suitability copilot), **B13** (NL rules), **B9** (elderly safeguards), **F4, F2** (audit narrative + call linkage) — plus table-stakes routing done at Gen-3 polish. Keep `OMS-Market-Intelligence-Dashboard.html` loaded in a browser tab for the close.

---

## Integration architecture (agreed 2026-09-04)

Mercury OMS is a **separate app** that imports the mercury-wealth domain — `src/types` (71 entities), `src/data` (the full seeded book), `src/lib` (Money/FX/ids), and key selectors — as a shared package. Both apps boot the identical dataset with identical stable ids, so cross-app deep links resolve without any backend integration:

- Portal → OMS: *"Send to execution"* on a proposal → `oms…/tickets/new?proposalId=…`
- OMS → Portal: *"View in client cockpit"* after a fill → `mercury-wealth…/clients/hh_yeung`

The portal stays frontend-only (its non-negotiable). The OMS may add a thin edge backend for live LLM calls (rule compile, extraction, narrative). To the room, the two apps present as **one platform** — the "single golden dataset front-to-back" pattern (Enfusion/Limina) from the market research, which no HK incumbent has.

---

## Cast — all real seed data from mercury-wealth (one addition)

**People (seeded users; password `mercury`):**
- **Wong Mei-Ling** — Relationship Manager, Greater China Private Clients (`adv_wong` / `meiling.wong@mercurywealth.hk`). *Presenter plays her in Scenes 0–2, 4.*
- **Lau Ka-Ming** — Team Lead, Greater China Private Clients (`adv_lau`). *Wong's real seeded team lead — approves the four-eyes hold.*
- **Ng Sui-Fong** — Compliance (`usr_ng`, `role_compliance`). *Presenter switches persona for Scene 3.*
- *(Reserve: Audrey Kwan, principal (`usr_kwan`), if a firm-wide view is asked for in Q&A.)*

**Clients:**
- **Yeung Siu-Ha** — 72 (b. 1954), retired school principal. ⚠️ **NEW seed to add** (`hh_yeung` / `cli_yeung_sh`) in Wong Mei-Ling's book — full spec in Appendix A. AUM USD 3.2M, risk_3 (Balanced), knowledge Limited, retail (no PI). **The order thread runs through her.**
- **Cheung Ka-Fai** — existing seed (`cli_cheung_ka`), 68, >USD 50m PI, Extensive knowledge. Appears in the Scene 3 back-test hits (age ≥ 65).
- The rest of the existing 20-household book provides blotter life and back-test depth.

**Instruments (all existing seeds):**
- **`inst_sn_autocall_tencent`** — *UBS Autocallable Note on Tencent 2027*. 8.0% p.a. conditional coupon (barrier 80%), autocall 100% quarterly, **65% European knock-in, 0% capital protection, product risk 6/7 (SRI)**. *What Mrs. Yeung asks for.*
- **`inst_sn_cpn_hsi`** — *JPMorgan Capital-Protected Note on HSI 2029*. **100% capital-protected**, 70% upside participation, no coupon, **risk 4/7**. *What the copilot proposes.*
- **`inst_0941_hk`** — *China Mobile Ltd*, HKEX, min lot 500, risk 2/5. *The funding sale — the FIX/blotter showcase.*

---

## Timing map

| Clock | Scene | App | White space hit |
|---|---|---|---|
| 0:00–1:30 | 0 · Morning cockpit + the call | Portal → OMS | (golden dataset) |
| 1:30–4:00 | 1 · Voice order capture | OMS | A12, G4, F2 |
| 4:00–7:00 | 2 · Suitability copilot | OMS | G3, B12 |
| 7:00–10:00 | 3 · Natural-language rule authoring | OMS | B13, B9 |
| 10:00–12:30 | 4 · Routing & fills on the live blotter | OMS | (table stakes, done right) |
| 12:30–14:00 | 5 · AI audit narrative + back to cockpit | OMS → Portal | F4, F2 |
| 14:00–15:00 | Close — the white-space matrix | Dashboard tab | all of the above |

---

## Scene 0 — Morning cockpit + the call (0:00–1:30)

**On screen:** The **deployed Mercury Wealth portal** (mercury-wealth.vercel.app), logged in as Wong Mei-Ling. Advisor Home morning briefing: her book, attention feed, agenda — Yeung Siu-Ha appears in the feed (review due / idle cash, per seed). No slides, ever.

**What I say:**
> "No slides — this is Mercury, live. This is Wong Mei-Ling's morning: her book, what needs attention, who's on the agenda. Notice Mrs. Yeung Siu-Ha in the feed — seventy-two, retired school principal, three point two million with us, sitting on idle cash. Hold that thought, because her phone call is about to become the next fifteen minutes."

*(Incoming-call card appears — click it.)*

> "That call just opened a ticket in Mercury OMS — different app, same brain: the same book, the same suitability profiles, the same product shelf. One golden dataset front to back. Watch."

**What the system does (build spec):**
- Portal is the real deployed app, untouched. The "incoming call" card is an OMS-side entry: a small demo trigger (keyboard shortcut) fires the call overlay and deep-links to `oms…/calls/live?clientId=cli_yeung_sh`.
- OMS opens already authenticated as the same user (shared mock-auth convention).

**Audience beat:** C-suite hears "one platform" before a single feature is shown. Tech sees two apps resolving the same ids.

---

## Scene 1 — Voice order capture (1:30–4:00)

**Seeded client:** Yeung Siu-Ha (`cli_yeung_sh`).

**On screen:** OMS call view: live transcript scrolling as the audio plays; beside it a **draft order ticket materializes field by field** — instrument resolving against the shelf, notional, side — with three pre-check chips already spinning: *Suitability · Cash · Concentration*.

**Audio (pre-recorded, English, ~35 seconds):**
> **Mrs. Yeung:** "Mei-Ling, good morning. My friend at bridge told me about a Tencent note paying eight percent — the one that pays every quarter. I'd like to put five hundred thousand US dollars into it."
> **Wong:** "Good morning Mrs. Yeung — let me bring that up while we talk. That's the Tencent autocallable note, eight percent conditional coupon, five hundred thousand US. One moment."

**What I say (over/after the audio):**
> "Nobody typed anything. Mercury listened, resolved 'a Tencent note paying eight percent' to the UBS autocallable on our shelf, structured the ticket, and — this is the part that matters — started running pre-trade checks *while the client was still talking*. And see this link icon? The recording and transcript are attached to the order from the moment it's born. HKMA and SFC expect suitability evidence kept for at least seven years. Today that evidence lives in a phone system nobody can search. Here, the order and the conversation are one object."

**What the system does (build spec):**
- Plays bundled audio; transcript rendered word-by-word from pre-aligned timestamps (no live ASR on stage; live ASR is a component swap — Appendix B).
- LLM extraction (real call, cached fallback): utterance → `{instrumentId: inst_sn_autocall_tencent, side: Buy, notional: USD 500,000, clientId: cli_yeung_sh}` with a confidence chip; "eight percent, pays quarterly" shown resolving against `structuredProductDetails` (couponPerAnnumPct 8.0, quarterly observations).
- Ticket state `Draft` (the seeded `OrderStatus` union); async pre-checks fire, results land in Scene 2.
- Order stores `sourceType: VOICE`, recording id, transcript hash; classification chip **unsolicited** (client-initiated).

**Audience beat:** RMs: two workflows became one. Compliance: evidence + solicited/unsolicited captured at source. Tech: extraction is function-calling against the real instrument master, not a chatbot guessing.

**If it breaks:** audio file + "replay extraction" button rendering the cached result instantly.

---

## Scene 2 — Suitability copilot (4:00–7:00)

**Seeded client:** Yeung Siu-Ha.

**On screen:** Pre-check chips resolve: **Suitability ✕ (red)** — *product risk 6/7 exceeds the risk_3 (Balanced) ceiling of 4/7; complex product vs knowledge: Limited; 65% knock-in with no capital protection* — **Cash ✕ (amber)** — *USD 160k free vs 500k required* — **Concentration ✓**. The copilot explains each failure in plain English over a **"What can Mrs. Yeung buy?"** eligibility strip. **Suggest alternatives** → comparison cards. Wong picks the capital-protected HSI note; the ticket amends in place; a rationale box captures the reasoning; the classification chip flips to **solicited**.

**What I say:**
> "Here's where every legacy OMS says 'ORDER REJECTED — CODE 4471' and the RM phones compliance. Mercury says *why*, in words Mei-Ling can read to the client: this note is risk six of seven with a sixty-five percent knock-in and zero capital protection; Mrs. Yeung is a Balanced-profile retail client with limited derivatives experience. And instead of a dead end — watch. [click] From our own shelf: the JPMorgan capital-protected note on the Hang Seng. A hundred percent of her capital protected, seventy percent of the index upside, risk four — inside her band. Same China view her friend was excited about, in a wrapper she can actually own. She agrees on the call — and notice Mercury just flipped the order from unsolicited to *solicited*, because the recommendation is now ours, and it's drafting the rationale for the file. Compliance just stopped being the department of no."
>
> "One more thing — the cash check. She has a hundred and sixty thousand free. Mercury already found the funding trade: sell forty thousand China Mobile from her income sleeve — eighty board lots, about three hundred and sixty thousand US. Mei-Ling accepts it as a linked funding leg. That's the pair we'll route in a minute."

**What the system does (build spec):**
- Deterministic rule engine returns structured failures; the LLM narrates them (engine decides, model explains). Risk-band mapping (OMS config): client `risk_1..risk_5` → max product SRI `2/3/4/6/7`.
- Eligibility strip (B12): filter over the seeded shelf for the loaded client's profile.
- Alternatives (G3): shelf query constrained by profile; card compares `inst_sn_autocall_tencent` vs `inst_sn_cpn_hsi` (protection / coupon vs participation / risk / tenor / issuer).
- Ticket amend → CPN USD 500k; rationale pre-drafted by copilot, **editable by the RM**, saved as an event; chip animates unsolicited → solicited.
- Cash check spawns linked child order: `Sell 40,000 × inst_0941_hk` (80 × min lot 500), `orderType: Limit`, tagged *funding leg*.
- Both orders now `Draft → pending release` on the OMS blotter.

**Audience beat:** C-suite: compliance friction becomes a cross-sell surface. Compliance: rationale + reclassification at the moment of advice. Tech: LLM never overrides the engine.

---

## Scene 3 — Natural-language rule authoring (7:00–10:00)

**Persona switch:** Ng Sui-Fong, Compliance. **Back-test** runs across the real seeded book; the deployed rule fires on **Yeung Siu-Ha's** pending order; **Lau Ka-Ming** (her actual team lead) approves.

**On screen:** OMS compliance workspace. Ng types a rule in plain English; Mercury compiles it into a **human-readable structured rule** shown side-by-side for review. **Back-test** → hits table across the last 12 months of seeded orders — including structured-product tickets for **Cheung Ka-Fai, 68**. **Deploy** → cut to the blotter: Mrs. Yeung's pending note order flips to **⏸ FOUR-EYES REQUIRED**; the approval request lands with Lau Ka-Ming, who approves with the vulnerable-client checklist.

**What I say:**
> "Now the scene every compliance officer in this room has lived. The HKMA turns up the heat on elderly and vulnerable clients, and you need a new control. On your current system that's a vendor change request: sixty to ninety days, a five-figure invoice, a test cycle. Watch Ng Sui-Fong do it before lunch."
>
> *[types]* **"For clients aged 65 or above, any structured product order over 5% of their AUM requires four-eyes approval and a completed vulnerable-client checklist before release."**
>
> "Mercury compiles that into a precise rule — and shows it back in English, because compliance should never have to trust a black box. Before it goes live she back-tests it against our actual book: it would have caught these orders — there's Mr. Cheung Ka-Fai, sixty-eight, a fifty-million-dollar professional investor; the rule bites on age, exactly as the regulator intends, regardless of sophistication. Deploy. Now watch the blotter — [cut] — Mrs. Yeung is seventy-two, and the note is fifteen point six percent of her AUM. Her order, captured eight minutes ago, just paused itself under a rule written ninety seconds ago. Lau Ka-Ming — Mei-Ling's actual team head — gets the request with the whole story attached: the call, the alternative, the rationale. Checklist, approve. **That** is the difference between AI as a sidebar and AI as the workflow."

**What the system does (build spec):**
- NL → rule compile (real LLM call, cached fallback): structured rule (`scope / conditions / actions`) rendered as reviewable plain-English clauses with editable thresholds.
- Back-test evaluates against the seeded 12-month order/activity window (mercury-wealth Phase 3b ledger + any OMS-added history); hits table shows date, client, instrument, notional, %AUM — `cli_cheung_ka` rows visible.
- Deploy: rule versioned `VC-1 v1 · author: Ng Sui-Fong · 14:32 HKT`; pending-release re-evaluation runs; Yeung CPN (500,000 / 3,200,000 = **15.6% AUM**) → `HELD — FOUR_EYES`.
- Approval request for Lau Ka-Ming with embedded context + checklist (second browser window, or timed "approved" event).
- Funding equity leg is *not* held (not a structured product) — precision detail if asked.

**Audience beat:** C-suite: the vendor change-request line item dies. Compliance: author, test, prove, deploy — versioned. Tech: NL compiles to a deterministic rule artifact; no LLM in the enforcement path.

**If it breaks:** composer has a pre-cached compilation for exactly this sentence; ghost-text type-ahead keeps typing fast and typo-proof.

---

## Scene 4 — Routing & fills on the live blotter (10:00–12:30)

**Seeded client:** Yeung Siu-Ha (both legs).

**On screen:** The OMS blotter — dense, dark, alive with the seeded book's working orders (a "keep" from the research: density is a feature). Wong releases the pair. The **China Mobile funding sale** routes via FIX to the simulated HKEX broker — `NEW → ACKED`, then three partial fills stream in with running average price. In parallel the **CPN order** goes out as an **RFQ to three issuing desks**; three quotes land staggered, best highlighted, Wong lifts it. A FIX inspector side panel shows the raw `35=D` / `35=8` traffic. Positions and cash projections update live.

**What I say:**
> "Approved — so let's trade, and here's the blotter in anger. The funding leg first: forty thousand China Mobile, out over FIX to the exchange broker. There's the ack… and the fills — ten, fifteen, fifteen thousand. For the tech team, that side panel is the raw FIX — new order single out, execution reports back — today against our simulator, in production against your existing broker connectivity, unchanged."
>
> "And the note doesn't go to an exchange — it goes to competition. Request-for-quote to three issuing desks… three quotes back, best one highlighted — lifted. Two asset classes, two market mechanisms, one blotter, no re-keying, no swivel chair. And every message you just saw — every ack, every fill, every quote — just became evidence. Which brings me to my favourite ninety seconds of this demo."

**What the system does (build spec):**
- Blotter renders the seeded `Order` collection natively (statuses `Working / Partially Filled / Filled…` are the existing union) plus the two live demo orders.
- FIX simulator: NewOrderSingle out; Ack + 3 ExecutionReports on a 1–3s cadence; ClOrdID/ExecID in the inspector; deterministic demo seed so fills complete within ~45s.
- RFQ simulator: 3 issuer quotes (e.g. 99.60 / 99.85 / 99.45) staggered with countdown; lift-best books the note.
- Position/cash projections recompute via the shared selectors on each fill.
- Every message persisted to the event log (feeds Scene 5).

**Audience beat:** Desk heads: fills, average price, no re-keying. Tech: real FIX semantics, swappable counterparty. C-suite: the "swivel chair" from the research is dead.

---

## Scene 5 — AI audit narrative + close the loop (12:30–14:00)

**Persona:** Ng Sui-Fong (or read-only Auditor view). **Seeded client:** Yeung Siu-Ha.

**On screen:** The completed order's lifecycle page. **Generate audit narrative** → a document composes in ~8 seconds: a chronological plain-English account of the entire order, **every sentence carrying a citation chip** — click one and the source opens: the transcript at the exact timestamp, the failed check, rule `VC-1 v1`, Lau's approval, the FIX execution report. **Export evidence pack (PDF)**. Then the last click: **"View in client cockpit"** → the *portal* opens on Mrs. Yeung's household, positions updated.

**What I say:**
> "It's eleven months from now and the SFC asks: *show us this was suitable.* Today that's three days of pulling call recordings, CRM notes and order logs — and praying they agree. Here: one click. [click] Eight seconds. 'Client initiated contact requesting a product outside her risk band… RM proposed a capital-protected alternative, reclassified as solicited, rationale recorded… order held under rule VC-1 version one, authored 14:32 by Ng Sui-Fong… approved by Lau Ka-Ming with vulnerable-client checklist… executed across two legs, best of three quotes.' Every sentence is a claim, and every claim is a link — transcript, check, rule version, approval, FIX message. This isn't AI summarizing what it thinks happened; it's a narrative compiled from the same event log the order ran on. Seven-year obligation, one-click evidence pack."
>
> "And one last click — back to the cockpit. There's Mrs. Yeung's household: the note is in the book, the cash moved, the same numbers the OMS just wrote. One dataset, front to back."

**What the system does (build spec):**
- LLM composes the narrative **grounded exclusively on the order's event log** (structured events in, prose + citation ids out); cached fallback for the demo order.
- Citation chips deep-link: transcript viewer (scrolls to timestamp), rule diff (`VC-1 v1`), approval record, FIX message detail.
- Evidence-pack export: button + toast + pre-built PDF (stub OK for demo).
- "View in client cockpit" → `mercury-wealth…/clients/hh_yeung` (deep link; the portal shows the seeded post-trade state — see Appendix A note on staging the two dataset states).

**Audience beat:** Compliance/C-suite: audit cost and regulatory exposure collapse. Tech: narrative is a projection of the event log, not a hallucination surface.

---

## Close (14:00–15:00)

**On screen:** The tab with `OMS-Market-Intelligence-Dashboard.html`, White Space section — the ten ranked cards.

**What I say:**
> "Fifteen minutes, one order. Voice capture, a suitability copilot that sells instead of blocks, a compliance rule written in English and live in ninety seconds, multi-asset execution on one blotter, an audit file that writes itself — and an advisory cockpit and an OMS running on one golden dataset. We studied eleven vendors across sixty-five features to build this map — these are the ten white-space rows, and you just watched us demo seven of them. Number of vendors shipping any of these today, including the system this bank runs: **zero of eleven**. The others treat AI as a sidebar on a 2005 workflow. Mercury *is* the workflow. Let's talk about what a pilot on your desk looks like."

---

## Anticipated questions & parries

- **"What if the AI hallucinates a trade?"** — It can't. The model drafts and explains; a deterministic rule engine and a human release every order. Nothing reaches the market without an RM confirm, and held orders need a second pair of human eyes. (Same guardrails-first pattern BlackRock ships in Aladdin Copilot.)
- **"Is the voice recognition real?"** — Extraction and rule compilation are live model calls; the call audio is pre-recorded so we're not demoing your conference-room acoustics. Live ASR is a component swap; Cantonese/English code-switching is on the roadmap — ask us for that demo next.
- **"What's simulated?"** — Market data, the FIX counterparty, and issuer quotes. The FIX semantics are real (4.4 message flows) and the simulator swaps for your broker connectivity. The order lifecycle, rule engine, event log, and the shared client/product dataset are the real product.
- **"Are these two systems or one?"** — Two deployable apps on one shared domain model and dataset — advisory cockpit and OMS read the same book. That's the architecture answer; the demo answer is you just watched an order cross between them without re-keying.
- **"Data residency / can client data hit an LLM?"** — Deployment supports HK data residency; model calls carry no client identifiers beyond what the check requires, and can run against regionally hosted models. (Have the one-pager ready.)
- **"Does this replace Ayers or sit beside it?"** — Either. Land as the advisory/suitability layer routing into existing execution, or as full OMS. Don't get pinned down on stage.
- **"HKMA/SFC view on AI?"** — Both regulators' GenAI guidance expects exactly this posture: human accountability, explainability, audit trail. Scene 5 *is* the compliance story.

---

## Pre-demo checklist

1. Reset OMS demo state to `T-0` snapshot (Yeung order not yet created, rule VC-1 not deployed; seeded blotter staged).
2. Portal tab logged in as `meiling.wong@mercurywealth.hk` (password `mercury`), on Advisor Home.
3. Verify room AV with the Scene 1 audio clip (worst failure mode in the building).
4. Warm the LLM cache: run extraction, rule compile, and narrative once; confirm cached-fallback toggle.
5. Second browser window logged in as Lau Ka-Ming (approval); third tab on the market-intelligence dashboard.
6. Timer visible; if running long, Scene 4's RFQ leg is the designated cut (route only the equity leg — the script still works).

---

## Appendix A — Seed data spec

### A.1 New seed to add to mercury-wealth (the only data change)

**Household `hh_yeung` → client `cli_yeung_sh` — Yeung Siu-Ha**, in Wong Mei-Ling's book (`adv_wong`, team_gc). Follow the repo's seed conventions (base facts only, stable ids, FK-safe order); note this intentionally changes the frozen book snapshot and needs `npm run validate` + snapshot update.

| Field | Value |
|---|---|
| Name / DOB | Yeung Siu-Ha, `1954-03-08` (72) — retired school principal |
| Segment | HNW retail (no PI assessment), HK resident (no tax lots — HK no-CGT rule) |
| Suitability | `risk_3` (Balanced), capacity Medium, knowledge **Limited**, objective Income, horizon Medium, esg null, assessed 2025, review due 2026 |
| AUM | ≈ USD 3.2M total |
| Free cash | ≈ USD 160k (`inst_cash_usd` / `inst_cash_hkd`) |
| Key positions | **55,000 × `inst_0941_hk`** (China Mobile, income sleeve); HK govt + corp bonds (`inst_bd_hkgov_28`, HSBC seniors); a balanced fund; small HK blue-chip sleeve |
| Portfolio | One advisory portfolio, one custodian account, base currency USD |

Math that must hold: CPN order USD 500,000 / 3,200,000 = **15.6% AUM** (> 5% rule threshold); funding sale 40,000 × 0941.HK ≈ USD 360k + 160k free cash ≥ 500k.

### A.2 Existing seeds the demo leans on (do not modify)

| Entity | Id | Role in demo |
|---|---|---|
| RM | `adv_wong` — Wong Mei-Ling | Presenter persona, Scenes 0–2, 4 |
| Team lead | `adv_lau` — Lau Ka-Ming (leads `team_gc`) | Four-eyes approver |
| Compliance | `usr_ng` — Ng Sui-Fong (`role_compliance`) | Rule author, Scene 3; auditor view, Scene 5 |
| Back-test hit | `cli_cheung_ka` — Cheung Ka-Fai, b. 1958-03-22 (68), PI | Proves the rule bites on age regardless of sophistication |
| Requested note | `inst_sn_autocall_tencent` — UBS Autocallable on Tencent 2027 | 8% conditional coupon, 65% KI, risk 6/7 — blocked |
| Alternative | `inst_sn_cpn_hsi` — JPM Capital-Protected Note on HSI 2029 | 100% protected, 70% participation, risk 4/7 — executed via RFQ |
| Funding stock | `inst_0941_hk` — China Mobile, min lot 500 | Sell 40,000 (80 lots) via simulated FIX, 3 partial fills |

### A.3 The rule (Scene 3, exact input text)

> "For clients aged 65 or above, any structured product order over 5% of their AUM requires four-eyes approval and a completed vulnerable-client checklist before release."

Compiles to: `scope: client.age >= 65` · `condition: instrument.type == STRUCTURED_PRODUCT && order.notional / client.aum > 0.05` · `actions: [HOLD_FOUR_EYES, REQUIRE_CHECKLIST(vulnerable_client)]` · deployed as `VC-1 v1`. Fires on the Yeung CPN order; does **not** fire on the 0941.HK funding leg.

### A.4 Risk-band mapping (OMS config, not portal data)

Client `risk_1..risk_5` (mercury-wealth `RISK_PROFILE`) → max product risk `2/3/4/6/7` on the SRI-7 scale (equities on INT5 map up-scale). So risk_3 ceiling = SRI 4: the autocallable (6) fails, the CPN (4) passes.

### A.5 Two-app dataset staging

The portal is frontend-only and won't know about the live demo order. Stage it: the deployed portal build for demo day includes Mrs. Yeung's **post-trade** state (note position + reduced China Mobile + cash) behind a `?state=post` flag (or a demo-day deploy), so the Scene 5 "View in client cockpit" link lands on updated numbers. Cheap, honest-to-the-room, zero backend.

---

## Appendix B — Simulation spec (tech honesty)

| Component | Demo | Production path |
|---|---|---|
| Client/product/book data | mercury-wealth shared seed (71 entities) | Core banking / custodian / product-master feeds behind the same domain model |
| Market data | Seeded price series + random-walk ticks | Vendor feed adapter |
| Voice/ASR | Pre-recorded audio + pre-aligned transcript | Streaming ASR component swap |
| Intent extraction | Live LLM (cached fallback) | Same |
| Rule compile & back-test | Live LLM compile (cached fallback); deterministic back-test | Same |
| Rule evaluation at runtime | Deterministic engine — **no LLM in the hot path** | Same |
| FIX counterparty | In-process simulator, real FIX 4.4 message shapes | Bank's existing broker/HKEX connectivity |
| Issuer RFQ | 3 simulated quotes, staggered | Issuer API / email-parse adapters |
| Audit narrative | Live LLM grounded on event log (cached fallback) | Same |
| Cross-app links | URL contract on shared stable ids | Same contract over a real API |

Principle to state on demand: **the model drafts, explains, and narrates; deterministic code and humans decide.**
