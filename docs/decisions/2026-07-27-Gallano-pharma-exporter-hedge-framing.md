# FX Hedging - EUR 8,000,000 Receivable Due in 1 Year

**Created by:** Clarence Gallano  
**Updated by:** Clarence Gallano  
**Date Created:** 07/27/2026  
**Date Updated:** 07/27/2026  
**Version:** 0.0
**LLM Used:** Claude Sonnet 5

---

## Executive Summary (≤150 words)
The U.S. Pharmaceutical Exporter holds a EUR 8,000,000 receivable due in 12 months, creating transaction exposure: the USD value of this receivable can shift materially between contract inception and settlement due to EUR/USD movement. At today's spot (~1.1370–1.1400), the receivable is worth roughly USD 9.10M–9.12M; at the indicative 12-month forward of 1.0890, it converts to a certain USD 8.712M. This gap reflects the prevailing USD–EUR interest rate differential, not a directional market call. Our objective is to hedge this exposure using one of three standard instruments — forward/futures, money market hedge, or options — each trading off certainty, complexity, and upside participation differently. Selecting the right instrument protects shareholder value amid an unpredictable economic and political landscape. This is Stage 1 of a five-stage plan; Stage 2 will price all three alternatives using live quotes.

---

## Background & Objectives
The U.S. Pharmaceutical Exporter is subject to transaction exposure on its EUR 8,000,000 receivable: because the contract was invoiced in euros but will ultimately be converted to USD, the receivable's dollar value fluctuates with EUR/USD between now and settlement. Recent ECB tightening — driven in part by energy-cost pressure from the Iran conflict — combined with sustained USD strength has pushed the euro toward one-year lows and widened the USD–EUR one-year rate differential to roughly 109 basis points. This is directly reflected in forward pricing: the 12-month forward (1.0890) sits 4.2–4.4% below spot, exposing an unhedged position to a potential USD 380K–390K shortfall relative to today's value.

---

## Methods
We are evaluating three hedge families side by side:

Forward/Futures — locks in a fixed conversion rate today (1.0890, or USD 8.712M), delivering budget certainty. The tradeoff is that both instruments eliminate any upside if the euro strengthens against the dollar before settlement.
Money Market Hedge — borrow EUR today, convert to USD at spot, and invest the proceeds at the USD one-year rate; the EUR loan is repaid at maturity using the receivable. This achieves the same rate certainty as a forward, and is particularly useful when forward contracts are unavailable or mispriced. However, it carries higher operational complexity and is constrained by our available EUR borrowing capacity and credit lines.
Options (EUR Put) — allows the firm to limit downside risk while preserving upside potential if the euro appreciates, in exchange for an upfront premium that reduces net proceeds regardless of outcome.

Stage 2 will obtain live dealer quotes for the money-market leg and representative option premiums, then model all-in net proceeds for each instrument under base, bull, and bear EUR/USD scenarios.

---

## Limitations & Next Steps
This analysis uses indicative market rates rather than firm dealer quotes and assumes the receivable amount and settlement date remain fixed; any change would require re-hedging. The money-market hedge estimate is illustrative pending confirmation of our actual achievable EUR borrowing rate and available credit capacity, and forward mispricing risk (versus money-market replication) has not yet been quantified.

Next steps: Finance will (1) obtain firm quotes for all three instruments (Stage 2), (2) run scenario-based P&L modeling (Stage 3), (3) recommend a hedge ratio and instrument mix for CFO approval (Stage 4), and (4) finalize execution and hedge accounting documentation (Stage 5). Target completion for Stage 2 quotes: within one week, pending CFO guidance on risk tolerance.

---

## References
List any sources, data sets, or foundational research that informed this proposal. Use a consistent citation style (APA, MLA, or Chicago).
