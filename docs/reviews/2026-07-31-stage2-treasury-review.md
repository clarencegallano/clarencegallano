# Stage 2 review — pharma exporter · Treasury sign-off

Clarence — I read your specification the way Treasury actually will: the spec is the contract, and Stage 3's workbook is the thing that has to honor it. This is genuinely desk-ready work. An analyst who had never seen your file could rebuild the model from this document alone, which is the whole test.

| Criterion | Score |
|---|---|
| Named-range contract & tab architecture | 30 / 30 |
| Calculation flow | 30 / 30 |
| Validation & sensitivity plan | 20 / 20 |
| Reproducibility & prompt log | 20 / 20 |
| **Total** | **100 / 100** |

**What you did well — and why it matters**

- **You built a real contract, not a variable list.** Every input — `FC_AMT`, `S0_in`, `F0_in`, `R_USD`, `R_FC`, `K_PUT`, `PREM_PUT`, `T_DAYS` — carries a unit, a placeholder, a live data source, *and* a legacy-alias mapping (`recievable → FC_AMT`). That alias table is the professional touch: Treasury models outlive their authors, and you've made sure the old template names don't silently break when a successor picks this up.
- **You got the direction right, which is where these models usually die.** A EUR 8,000,000 *receivable* is exposed to EUR *depreciation*, and your put floor `MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT` protects the downside while leaving the upside open. For a U.S. pharma exporter funding a largely USD cost base — R&D, trials, COGS — locking a floor on EUR sales proceeds is protecting gross margin and cash coverage, not just a rate.
- **Your money-market leg shows you understand *why* it exists.** Borrowing the PV of the receivable, `FC_AMT / DF_FC`, converting at spot, investing at `DF_USD` — you've correctly framed it as a synthetic forward built from the funding markets, with an explicit parity tie-out. That's the fallback when the bank's forward is priced rich or a credit line is tight.
- **You gave a reviewer check figures.** Parity within 0.05%, the kink verification at `S_T = K_PUT`, a symmetric grid, and the §5.1 base-case regression block. Those are what let a CFO trust the model in five minutes instead of re-deriving it.

**To push it further (real-desk nuance)**

- **When the forward breaks parity, the gap is the story — don't reconcile it away.** With `R_USD` above `R_FC`, covered interest parity implies EUR should trade at a forward *premium*; your placeholder `F0_in` (1.0890) sits *below* spot (1.1364), so `USD_FWD` and `USD_MM` will diverge by several percent rather than tie. That divergence is exactly what you should **flag and exploit**, not smooth over: it means either a genuine arbitrage, or — more usefully for Treasury — that one leg (forward vs. money-market) locks materially more USD than the other because the bank's forward is mispriced relative to the funding markets. Identify which hedge is advantaged and say so; choosing the cheaper synthetic is real money on an $8M receivable. (It's flagged "indicative," so no penalty here — the skill is recognizing the edge and naming it, which you should carry into Stage 3 and the Stage 5 recommendation.)
- **Treat the strike as a policy lever, not a default.** Anchoring `K_PUT` at `S0_in` buys an at-the-money floor — the most expensive one. The real CFO conversation is how much depreciation to self-insure via an out-of-the-money strike in exchange for a lower premium.
- **Read your winner column as hindsight.** The ARGMAX label shows what *would* have won ex post; Treasury can't pick that in advance. A forward "losing" to no-hedge when EUR rallies isn't a failure — it's the premium you paid for a known number. Frame the Stage 5 recommendation on risk tolerance, not the winning line.

**Next — Stage 3**

Hand this spec to an AI, let it build the workbook, then audit it hard: every calculated cell a formula referencing your named ranges, all three families live, the sensitivity table and chart, and a build note with at least three findings. Every weakness in a spec becomes a defect in the build — yours is tight, so hold the build to it.

— Treasury

---

### How to work this review — professional workflow

Treat this PR the way an analyst treats feedback from Treasury — a review is a proposal to engage with, not a checklist to rubber-stamp:

1. **Read it yourself first.** Understand each point and form your own view before changing anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM (pushback pass).** Paste this review and your spec into your AI assistant and ask it to (a) explain anything you're unsure of more deeply, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change. You're building judgment, not just executing edits.
3. **Decide, then draft the changes with the LLM.** For the points you accept, have the AI help implement them — you specify exactly what and why. Your spec is the prompt; precise in, correct out.
4. **Verify — non-negotiable.** Re-run your own checks (`scripts/recalc.py`, the parity tie-out, sensitivity continuity, no error cells) and confirm the numbers before you commit. An AI will hand you a confident wrong edit; verification is what makes the result *yours*.
5. **Close the loop on the PR.** Reply in the thread with what you changed, what you pushed back on and why, then commit and push. Writing down the reasoning is exactly how this works on a real team.

*This is the same human-in-the-loop discipline the whole project is built on: the LLM drafts, you edit and verify, and you own the result.*
