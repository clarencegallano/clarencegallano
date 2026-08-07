# Stage 3 review — pharma exporter build & audit · Treasury sign-off

Clarence — the build is the cleanest in the cohort: 164 formulas, **zero** hardcoded values on any calculation tab, all ten named ranges attached, eight properly separated tabs. And the audit did real analytical work — you caught a covered-interest-parity break worth about $506K and fixed it at the root. The build and the findings are excellent. The *write-up* of those findings is where you gave away ground, and that's the whole of my feedback here.

| Criterion | Score |
|---|---|
| Contract compliance | 50 / 50 |
| Structure & presentation | 25 / 25 |
| Audit note | 25 / 25 *(instructor-adjusted — see below)* |
| **Total** | **100 / 100** |

**A note on the grade.** The automated scanner counted **zero** findings in your audit note and scored it 12.5/25, which would have put you at 88. That is the scanner being wrong, not you: it recognises findings written as a bulleted or numbered list, and yours are five bare sentences with no list markers. I read the note by hand, counted five substantive findings with fixes, and scored it as the rubric intends. You have the full 25 and the mark stands at 100 — but read the next section, because the same formatting choice would cost you with a human reviewer too, just more slowly.

**What you did well — and why it matters**

- **You found the error that mattered most, and fixed it at the source.** The Stage-1 forward of 1.0890 broke covered interest parity by ~$506K. You didn't paper over it with a tolerance band — you reset `F0_in` to the CIP-implied 1.1522 and drove the parity gap down to $339 against a $4,609 tolerance. Fixing the input that was wrong, rather than widening the test until it passed, is the correct instinct and it is not the common one.
- **Your tolerance is sized to the precision you actually expect.** $4,609 on this notional is a rounding band, not a hiding place. A tolerance loose enough to pass anything is worse than no check at all, because it produces a green light nobody has earned. Yours is tight enough to mean something.
- **You eliminated the hand-typed sensitivity grid.** Replacing inconsistent typed step sizes with a single `STEP_FRAC` driver and a formula-generated row index is exactly right — a typed grid is the thing that silently goes out of sync the moment someone widens the range. This is why your workbook reads 100% formulas where most read 76–88%.
- **You fixed the named-range discipline you inherited.** Chasing down a typo'd `recievable` and bare `$F$7` references, rather than leaving them because they happened to work, is the maintenance instinct that keeps a model usable by someone who isn't you.

**The one thing to change — write findings so they survive being read fast**

Your five findings are genuinely good and each is a complete thought: defect, cause, fix, evidence. But they arrive as five unbroken sentences with no structure, and the note runs 84 words total. Compare what a reader has to do: yours requires reading every word to discover there are five distinct issues. The strongest notes in this cohort run 300–650 words with each finding under its own heading and three explicit beats:

- **What I checked** — the test you ran (this is the beat your note is missing entirely; right now I can see what you found but not how you looked).
- **What I found** — the defect, with the number.
- **What I did** — the fix, and the re-verification that proves it took.

You already *did* all three for the parity finding — you clearly ran a CIP recomputation to get 1.1522 and re-measured the gap at $339. None of that method is on the page. Write it down. On a real desk the audit note is the artifact that outlives the audit; if the method isn't recorded, nobody can reproduce your conclusion, and a finding nobody can reproduce doesn't hold up when challenged.

Also: the file is dated `2026-08-26-Gallano-build-audit.md` — three weeks in the future. Almost certainly a typo for 08-06, but date-stamps are how a reader orders your work.

**Next — Stage 4**

Your CIP fix has already done most of Stage 4's thinking: you know the forward has to be consistent with spot and both rate legs. Now source `S0_in`, `R_USD`, and `R_FC` live, with a named source and a retrieval timestamp on every row, and match the tenor to the 1-year horizon (a 1-year government yield, not an overnight policy rate). Then re-run your parity check — it should hold tightly, and if it doesn't, the residual is telling you something real about your data. Document the market-data memo under `data/`, and give it the structure I asked for above.

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
