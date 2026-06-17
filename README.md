# £ FallAdviser · sovereign UK financial adviser

> Tax · pension · portfolio · Q&A · all on your device · UK 2025-26 rules · estate-aware · informational, not regulated advice.
>
> v1 · prime **719** · MIT · ◊·κ=1

**Live:** [sjgant80-hub.github.io/falladviser](https://sjgant80-hub.github.io/falladviser/)
**Source:** [github.com/sjgant80-hub/falladviser](https://github.com/sjgant80-hub/falladviser)

---

## What it is

A sovereign single-HTML financial-adviser tool calibrated to the **UK 2025-26 tax year**. Replaces the part of an IFA that's just calculations and rule-of-thumb recommendations — at £0/year vs £1,500-3,000 typical IFA fees.

**Knows:**
- UK 2025-26 income tax bands (incl. 60% PA-taper trap)
- National insurance (employed Class 1)
- Dividend tax + £500 allowance
- CGT 18%/24% + £3,000 allowance
- ISA £20,000 + LISA £4,000 sub-allowance
- Pension annual allowance £60,000 (incl. tapering above £260k adjusted)
- State pension qualifying years
- Asset allocation by risk profile (cautious / balanced / adventurous / aggressive)

**Does NOT do:**
- Regulated advice (you're the decision-maker; this is informational)
- Live market data (you enter your own positions)
- Scottish income tax bands (England/Wales/NI only · noted on profile)
- Specific platform integrations (works with any account)
- IHT planning (basic NRB tracked, full estate-planning future feature)

## Tabs

| | tab | what it does |
|---|---|---|
| ◐ | **Dashboard** | tax position · allowance bars · portfolio summary · top recommendations |
| ◊ | **Profile** | age · income · contributions · risk profile · NI years |
| £ | **Tax** | band breakdown · liability composition · what-if pension contribution modeller |
| ☷ | **Pension** | state forecast · workplace + SIPP projection (3 growth scenarios) · annuity / drawdown income |
| ▦ | **Portfolio** | holdings table · asset allocation · tax-wrapper split · concentration warnings · rebalance suggestion |
| ? | **Q & A** | ask any UK-finance question · T0 deterministic for 4+ common topics · T3 (BYOK) for nuance |
| ↓ | **Reports** | annual tax summary (.md or PDF via FallPDF) · holdings CSV · full state JSON · Mansoor P3 audit chain |

## The recommender

Real-time T0 deterministic suggestions based on your profile:

- ISA allowance use + LISA bonus availability
- Higher-rate / 60%-trap pension relief opportunities
- Pension AA usage and approaching ceiling
- Emergency fund gap vs target
- Workplace employer match capture
- Portfolio drift vs risk target
- Concentration risk (any position >10% of total)
- State pension NI years shortfall
- Spending-to-income ratio check

Each rec includes priority (high / med / low) + **why** in plain English with specific £ amounts from your numbers.

## Q&A · the cascade

**T0** (offline · default): hard rules for the most-asked UK questions:
- "ISA vs SIPP for my profile" — uses your marginal rate
- "CGT on £X sale" — full band breakdown
- "60% tax trap" — modelled against your income
- "State pension qualifying years" — shortfall + top-up math

**T3** (BYOK · opt-in): when an API key is set (Anthropic / Gemini *free* / OpenAI / OpenRouter), Q&A becomes free-form. The model receives your full anonymised context (profile, contributions, portfolio, marginal rate) + the UK 2025-26 rules + a strict prompt: *informational only, always say so, end with "verify with HMRC or an authorised adviser before acting."*

## Estate awareness

On boot, FallAdviser pulls `fall-registry/index.json` and indexes your estate tools. The advisor knows you have:
- **FallAccount** / **FallAccount-Trades** for trading and books
- **FallLedger** for the double-entry GL (auto-imports from FallInvoice etc.)
- **FallFlow** for cashflow / runway
- **FallInvoice** + **FallAP** for receivables and payables

Recommendations can route to these (e.g. "open FallLedger to record this VAT entry").

## si-didy integration

The full `postMessage` API:

```javascript
// Ask the adviser a question
{ target: 'falladviser', action: 'ask', question: 'should I top up state pension?' }

// Get the current tax position
{ target: 'falladviser', action: 'get-tax' }

// Get portfolio analysis
{ target: 'falladviser', action: 'get-portfolio' }

// Get prioritised suggestions
{ target: 'falladviser', action: 'get-suggestions' }

// Read/write profile (with consent)
{ target: 'falladviser', action: 'get-profile' }
{ target: 'falladviser', action: 'set-profile', profile: {...} }
```

Plus it listens on `BroadcastChannel('fall-signal')` for si-didy `query` intents and responds with `answer` events.

## Mansoor's 6 · client-data hardening

| | check | how |
|---|---|---|
| P1 | data sync | dashboard pulls from same IDB store as profile |
| P2 | rule enforce | event-driven recompute on every profile change |
| P3 | **audit chain** | prevHash + docHash + ts on every save · exportable JSON · togglable in Settings |
| P4 | surface tailor | UK-finance specific UI (allowance bars, marginal rate, 60% trap warning) |
| P5 | multi-device | JSON export/import covers everything (keys stripped on export) |
| P6 | economics gate | KCC not surfaced (personal-use tool) |

## Build gate · 14 points

All 14 honored. 14-point gate · single HTML · vanilla JS · no deps · file:// works · IDB primary · mobile-responsive · KONOMI shim · fall-signal mesh · postMessage API · PWA manifest · MIT · README · .nojekyll Pages legacy.

## Use

1. Open [the tool](https://sjgant80-hub.github.io/falladviser/) or `index.html` from `file://`
2. Profile → enter your numbers (none of it leaves your device)
3. Dashboard for the snapshot
4. Q&A for any specific question
5. Ctrl+K anywhere for the ask palette

## Honest scope

This is **informational**, calibrated to UK 2025-26 rules. Not a substitute for an FCA-authorised adviser when binding decisions are on the table. Tax law is fiendishly detailed and the rules change every Budget — verify everything with HMRC or a regulated professional before acting.

That said, for the day-to-day work of *"am I using my allowances · is my portfolio aligned · what's my marginal rate · should I prioritise X or Y this year"*, this replicates ~80% of what a £150-300/hour IFA conversation delivers, except you can re-run it at midnight with updated numbers.

---

## For developers

```
index.html      one file · ~62KB · vanilla JS · no deps
README.md       this
LICENSE         MIT
.nojekyll       Pages legacy deploy
```

### Updating tax constants

`RULES` at the top of the inline script is the year-config. Each Budget, update:
- `personalAllowance`, `basicRateBand`, `higherRateStart`, `additionalRateStart`
- `isaAllowance`, `lisaAllowance`, `pensionAnnualAllowance`
- `cgtAllowance`, `cgtBasicRate`, `cgtHigherRate`
- `dividendAllowance`, dividend rates
- `statePensionAnnual`, `statePensionQualifyingYears`
- `taxYear` string and `TAX_YEAR` constant

That's it. One block per year.

### Adding a Q&A T0 pattern

Add an entry to `T0_PATTERNS`:

```javascript
{ match: /your-regex/i, answer: q => `**Your topic title**\n\nResponse text...` }
```

Each pattern returns markdown that gets rendered with `**bold**` and line breaks.

### Adding a suggestion rule

Add to `generateSuggestions()`:

```javascript
out.push({ prio: 'med', title: '...', why: '...' });
```

Priority sorts the output (high → med → low).

## Credit

- **HMRC** for publishing the rules
- The fall* estate doctrine — every gate point honoured
- Same cascade pattern as [FallOffice](https://github.com/sjgant80-hub/falloffice), [FallMap](https://github.com/sjgant80-hub/fallmap), [TemuOracle](https://github.com/sjgant80-hub/temuoracle), [fall-prompt-gate](https://github.com/sjgant80-hub/fall-prompt-gate)

⚒ Part of the [fall* estate](https://github.com/sjgant80-hub) · prime 719 · ◊·κ=1
