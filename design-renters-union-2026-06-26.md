# Renters Union — Design Doc
**Date:** 2026-06-26  
**Branch:** claude/office-hours-7n50z8  
**Mode:** Startup (non-profit)  
**Stage:** Pre-product, idea stage  

---

## Vision

Renters have never had access to the same quality of market intelligence that landlords use every day. The mission of Renters Commons is to build a public-interest housing intelligence platform that gives every renter access to transparent pricing data, negotiation tools, and community knowledge — so they can make informed housing decisions.

---

## The Problem

Landlords have algorithmic pricing tools — RealPage, YieldStar — that aggregate actual occupancy and rent data across competing properties to maximize extraction. The DOJ sued RealPage. New York became the first state to ban algorithmic rent-setting tools (S.7882, late 2025). Portland and Washington State are following. These tools cost renters an estimated $70/month average nationally.

Renters have nothing equivalent. StreetEasy covers NYC listings. Rentometer gives rough estimates. Zumper tracks trends. No one has: actual paid rent at the building level, accumulated over time, with an AI layer that converts it into negotiating leverage.

**The founder spent two months collecting Chicago rental data manually using Claude and used it to negotiate a $200/month reduction on their own lease.** That's the proof of concept. The product is that workflow, productized and accessible to renters who don't know how to build it themselves.

---

## What I Noticed (Founder Signals)

Strong session. Signals observed:
- **Real problem, personal evidence** — used the tool themselves, got a result ($200/month saved), not a hypothesis
- **Named a specific market** — Chicago, specific buildings (Solvere and 811), now expanding
- **Accurate self-assessment** — "not a billion-dollar market cap idea but a billion-dollar impact idea" — shows clear thinking about what this is
- **Mission alignment is the decision driver, not money** — non-profit choice came from trust and values, not inability to monetize
- **Already has working infrastructure** — automated Windows Task Scheduler pipeline, 44 days of data, 4 buildings
- **Strong instinct on who needs it most** — lower-income Chicago neighborhoods, large buildings, people with the most to gain on a percentage basis

The instinct that led to the non-profit decision is correct: renters will share sensitive lease data with a non-profit they trust. They won't share it with a VC-backed startup they suspect will monetize it. The non-profit structure IS the moat for data quality.

---

## Agreed Premises

1. **The real moat is the lease upload corpus (actual paid rent), not scraped listing data.** *(Premise revised after cross-model second opinion — see below.)* Scraped listing data is the bootstrap mechanism that gives renters something to access before the corpus exists. A funded competitor can replicate the scraping pipeline within 18 months. No one can replicate years of user-submitted actual paid rents from a trusted non-profit. Time-series listing data is still valuable and has no equivalent elsewhere at this granularity — but it's the on-ramp, not the destination.

2. **Scraped listing data is the MVP.** Lease uploads come later as a network contribution mechanic (Glassdoor-style gate: upload your lease to access others' data). Cold start solved by launching with scraped data alone — the pipeline already exists.

3. **Chicago is the wedge city.** NYC already has StreetEasy, Openigloo, and the country's strongest tenant protections. Chicago has almost nothing. The founder has 2 months of real data there.

4. **The AI layer produces intelligence, not letters.** The output is not "here's your negotiation letter." It's: *"Your building has increased vacancy from 3 to 11 units in the last 28 days. Similar units are now listed for $187 less. The leasing office has added two concessions. Ask for: $175 reduction, waived renewal fee, reserved parking, 14-month lease."* That's market intelligence delivered as a specific ask. Text generation is the last step, not the product.

5. **Distribution via both search and renegotiation.** Renegotiation (lease renewal moment) has the clearest ROI story and the highest motivation. Apartment search has higher volume. Both are valid use cases with different urgency.

---

## Two Products, Not One

The platform is actually two distinct products serving different audiences. Recognizing the separation clarifies every engineering and product decision.

**Product A — Real Rent (consumer)**
The renter-facing negotiation tool. Audience: someone renewing their lease who wants to pay less. The question it answers: *"What should I ask for, and why?"* Every feature should be evaluated against: does this help a renter save money at lease renewal? If not, it's not MVP.

**Product B — Renters Commons (infrastructure)**
The housing intelligence platform. Audience: researchers, city housing departments, journalists, tenant unions, policy organizations, universities. The question it answers: *"What is actually happening in Chicago's rental market?"* This is the long-term data trust — the thing that produces grant revenue, city partnerships, and academic credibility.

Both products are built on the same data. But they have different interfaces, different users, and different value propositions. Building them as one blurs the priorities. Phase A builds Product A. Phase B builds the infrastructure that makes Product B possible.

---

## The Landscape

**Direct competitors:** Weaker than expected.
- **StreetEasy** — listing prices, NYC only, no actual paid rent
- **Rentometer** — rough rent comparisons, no time-series, no negotiation layer
- **Openigloo** — Brooklyn, rent-stabilized apartments only, no lease data aggregation
- **Apartment List / Zumper** — trend data, no building-level granularity

**No one is doing:** building-level time-series listing data + actual paid rent (lease uploads) + AI negotiation assistant. The space is clearer than expected.

**Regulatory tailwind:** NY banned algorithmic rent-setting (2025). Portland followed. DOJ sued RealPage. Public awareness of algorithmic rent manipulation is at an all-time high. A non-profit renter data platform is politically viable in a way it wasn't 3 years ago.

**Funding landscape:** MacArthur Foundation (headquartered in Chicago) funds housing justice work. Ford Foundation, Kresge, JPMorgan Chase Community Development are all active in this space. A Chicago-based non-profit building renter data infrastructure for lower-income neighborhoods is a strong grant candidate.

---

## The Data Foundation (What Exists Now)

- **44 days of data** on Solvere and 811 (original 2 buildings, same management company)
- **2 additional buildings** recently added in the area
- **~20 data points/day** across the original 2 buildings combined
- **Tracks:** unit price changes, listing specials, days on market, unit absorption (when a unit disappears = rented)
- **Pipeline:** automated via Windows Task Scheduler — runs daily without manual intervention
- **Format:** living in a Claude Code project on founder's Windows desktop (needs migration to server for production)

**Assessment:** Sufficient for MVP demo and grant application. Insufficient for a product renters across Chicago rely on. The methodology is proven; coverage needs to scale.

---

## Recommended Approach: A → B

### Phase A (Now): AI Negotiation MVP — "The Scout"

Use the existing 44 days of Chicago data to build the simplest possible AI negotiation tool. A user enters their building address, gets a comparison of their current rent vs. what that building has been listing at over time, sees the occupancy/absorption signal (how fast units are moving), and receives an AI-generated negotiation framework.

**This is not a product launch.** It's a demo that proves the concept well enough to:
- Show to MacArthur Foundation as a grant application
- Show to Metropolitan Tenants Organization Chicago as a partnership pitch
- Use to recruit a technical co-founder or first volunteer engineer

**Revenue/Funding in Phase A:** Single seed grant from a Chicago community foundation ($25–75K). Enough to run the infrastructure and one part-time person.

**What to build:** Web form → address lookup → market comparison → AI negotiation letter. No accounts, no database at scale. The founder's existing data as the backend.

### Phase B (Funded): Public Interest Data Trust — "The Registry"

A continuously-running scraping infrastructure covering large buildings in Chicago's lower-income neighborhoods (Pilsen, Englewood, South Shore, Austin, Humboldt Park). Time-series database of listing prices, unit availability, and absorption rates at building level. AI negotiation assistant. Building/landlord reviews as community layer. Lease uploads added once there's enough traffic to make the gate valuable.

**Why lower-income neighborhoods first:**
- Most to gain on a percentage basis (saving $150 on $1,100 rent = 14%)
- Most under-documented (listing sites focus coverage on affluent areas)
- Strongest mission story for foundation funding
- Strongest political protection

**Revenue/Funding in Phase B:** MacArthur Foundation and Ford Foundation grants ($250K–1M). Data licensing to researchers, journalists, and policy organizations (not landlords, never landlords). City of Chicago partnership potential (city wants this data for housing policy decisions). Optional renter membership ($5–10/month, voluntary).

**What to build:** Scraping pipeline on cloud server, time-series database (Postgres), building-level dashboard, AI negotiation assistant, building review layer.

---

## Legal Considerations

**Scraping public listing data:** Defensible under current federal law. *HiQ v. LinkedIn* (9th Circuit, 2022) established that scraping publicly available data does not violate the CFAA. *Van Buren v. United States* (Supreme Court, 2021) further narrowed the CFAA. The realistic risk is Terms of Service violations (civil, not criminal) leading to IP blocks and C&D letters — not criminal prosecution.

**Actual paid rent data:** Cannot be scraped — it doesn't exist on any public site. Must be collected via lease uploads (user-submitted). This is clearly legal and actually produces better data.

**Copyright:** Factual data (rent amounts, addresses, availability dates) is not copyrightable under *Feist Publications v. Rural Telephone* (1991). Storing analysis rather than verbatim listing pages reduces exposure further.

**Non-profit status:** Does not create special scraping rights. Does create political protection — a non-profit fighting for housing justice in lower-income neighborhoods is an extremely unsympathetic defendant for any listing site to sue in 2026.

**501(c)(3) vs. 501(c)(4):** If advocacy (lobbying for rent control, campaigning against algorithmic pricing legislation) is part of the mission, file as (c)(4). Donations are not tax-deductible under (c)(4), but advocacy rights are full. A (c)(3) is limited to educational/charitable activities and cannot lobby. Given the political context (NY ban, DOJ action), (c)(4) is likely the right structure.

**Risk mitigation:** Scrape only public pages (no login-required data). Rate-limit aggressively. Store analysis, not raw listings. Get a housing/tech lawyer to review before launch — one hour of legal advice now is worth 100 hours later. EFF has defended public-interest scrapers before.

---

## Architecture Notes

**Current state:** Windows Task Scheduler running a Claude Code project daily on the founder's desktop.

**What needs to change for production:**
1. Move scraping pipeline to a cloud server (Digital Ocean $10/month, or AWS). The desktop going offline or restarting breaks data collection continuity.
2. Structured database (Postgres or SQLite to start) with building/unit as first-class entities and time as the primary dimension.
3. Simple web frontend for the negotiation tool (Next.js or plain HTML/JS to start).
4. The Claude API stays as the AI negotiation layer — that architecture is proven.

**Scaling to more buildings:** The pipeline is already automated. Expanding coverage is a configuration change (adding building addresses to the scrape list), not a rebuild. Priority: large residential buildings in Pilsen, Englewood, South Shore, Austin, Humboldt Park.

**Housing Weather Report (UX concept):** Market conditions expressed as an instantly readable signal, not a table of numbers. Example:

> **Pilsen** — Vacancy ↑ · Prices ↓ · *Negotiation strength: High*
> **South Loop** — Inventory shrinking · Expect increases · *Negotiation strength: Low*

Stocks have Bloomberg. Weather has radar. Housing has nothing. This is the consumer-facing surface of the intelligence layer.

**Lease upload field extraction:** Beyond the rent amount, AI extraction of uploaded leases should capture: concessions, pet fees, parking fees, renewal increase amounts, move-in credits, application fees, utility clauses, internet requirements, lease duration, and early termination terms. These become the richest dataset in the corpus — things that never appear in listing data.

---

## Distribution

**Primary channels:**
1. **Metropolitan Tenants Organization** (Chicago) — the city's main tenant advocacy organization. They have members who are exactly this product's users. A partnership here solves cold-start distribution.
2. **Foundation networks** — grantees of MacArthur and Ford in housing justice tend to know each other and share tools.
3. **Chicago Reader / local journalism** — the story ("founder saves $200/month by building renter AI, now making it free for everyone") is a local news story that writes itself.
4. **Word of mouth at lease renewal time** — the product's primary trigger.

**The pitch to Metropolitan Tenants Org:** Don't ask for users. Ask to co-design. "We have 2 months of data on buildings in your area. We're building a tool for renters at lease renewal time. We want your help shaping it — what questions do your members ask every day? What information do they wish they had? What would make this trustworthy enough for you to recommend?" If they help shape it, they'll advocate for it. That changes the relationship from customer to partner.

---

## Open Questions

1. **501(c)(3) vs. 501(c)(4)?** Need a lawyer conversation before filing. Key question: is advocacy part of the mission?
2. **What listing sources feed the scraper?** Craigslist, Zillow, Apartments.com — which ones? ToS exposure varies by source.
3. **Data storage format?** What format does the current pipeline produce? (CSV, JSON, SQLite?) Determines migration complexity.
4. **Is there a co-founder?** Building this alone as a non-profit is very hard. The combination of technical pipeline + grant writing + tenant org relationships is more than one person.
5. **City of Chicago partnership?** Chicago's Department of Housing has data that could augment the scraping. Worth a conversation early.

---

## The Assignment

One concrete action this week — not a strategy, an action:

**Contact Metropolitan Tenants Organization Chicago.** Find the executive director (currently connected to housing justice orgs in Chicago), send a 3-paragraph email: (1) what you built, (2) what you saved on your own rent, (3) ask for 20 minutes to show them the tool.

That conversation will tell you more about product-market fit than another month of building.

After that:
- [ ] Consult a lawyer: 501(c)(3) vs. 501(c)(4) decision
- [ ] Move scraping pipeline from Windows desktop to a cloud server
- [ ] Add 5-10 large buildings in Pilsen or Englewood to the pipeline
- [ ] Build the simplest possible negotiation web demo on existing data
- [ ] Research MacArthur Foundation housing grant application timeline

---

## What This Is Not

This is not a startup optimizing for growth metrics. It is not a product that treats renter data as an asset to monetize. It is not competing with Zillow.

It is an attempt to build the renter-side equivalent of what landlords already have — and give it away to the people who need it most. The non-profit structure is not a constraint; it's the thing that makes the product trustworthy enough to actually work.

---

---

## Cross-Model Second Opinion (2026-06-26)

Run as a structured adversarial review against all five agreed premises. Four findings:

**1. Strongest version of the idea:**  
A non-profit intelligence commons doing for renters what RealPage does for landlords. The non-profit structure is not a constraint — it's the mechanism that generates the lease upload data that makes the product irreplaceable. For-profit competitors cannot get renters to share actual lease details. The non-profit can.

**2. The product is already proven:**  
"I spent 2 months collecting Chicago rental data manually using Claude and used it to negotiate a $200/month reduction on my own lease." The product is that workflow, end-to-end, accessible to renters who can't build it themselves. This is the demo. This is the grant pitch. This is the story for MTO.

**3. Premise 1 is wrong as stated (revised above):**  
Time-series scraped listing data is not the moat. A well-funded competitor (or a PE firm backing one) can replicate the scraping infrastructure within 18 months. The moat is the lease upload corpus — actual paid rents from actual leases, submitted by renters who trust a non-profit with their sensitive data. That corpus takes years to build and cannot be bought. The scraping is the bootstrap; the corpus is the defensible position.

**4. 48-hour prototype spec:**  
Stack: Next.js + Vercel + Claude API + SQLite/Supabase.  
Flow: address input form → lookup against the 44-day Chicago dataset → negotiation brief with specific dollar ask → export as PDF.  
Scope: Do not build user accounts, map UI, or anything outside the existing 44-day dataset. The prototype's job is to generate a compelling negotiation brief. One flow, no edge cases.

---

## External Review — ChatGPT (2026-06-26)

Rating: **9/10 for an idea-stage project.** "One of the strongest early-stage nonprofit product concepts I've read in a while." Key observations:

- The founder story ($200/month saved) is the grant story. Put it in every presentation.
- The nonprofit reasoning is a *network-effect argument*, not a moral argument. Trust produces better data. That's much stronger than "we're mission-driven."
- Starting with Chicago is exactly right. Every successful data company begins somewhere tiny.
- The remaining gap is evidence: demonstrate that the workflow that saved one renter $200 can reliably help many renters negotiate better outcomes.

Additions incorporated from this review: Vision section, two-product framing (Real Rent vs. Renters Commons), AI intelligence framing (specific ask > letter generation), Housing Weather Report concept, expanded lease upload fields, co-design framing for MTO.

---

## Decisions Made This Session (2026-06-26)

- **Name:** Renters Commons
- **Product tool name:** Real Rent (under consideration)
- **Structure:** Non-profit, 501(c)(4) likely (allows advocacy)
- **Wedge city:** Chicago, starting with lower-income neighborhoods (Pilsen, Englewood, Humboldt Park, South Shore, Austin)
- **Domains registered:** renterscommons.org (primary), renterscommons.com (redirect) — Namecheap, order 206537909, auto-renewal ON
- **Data sources:** Domu.com + Zillow (medium risk, HiQ precedent), avoid Apartments.com/CoStar
- **First action:** Contact Metropolitan Tenants Organization Chicago

---

*Design doc generated via /office-hours on 2026-06-26.*  
*Approach: A → B (MVP → non-profit data platform). Non-profit (501c4 likely). Wedge: Chicago lower-income neighborhoods.*
