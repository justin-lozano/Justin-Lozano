<!-- PR TARGET: https://github.com/justin-lozano/Justin-Lozano | Stage 1.2 -->
# Stage 1.2 review — spec, build, audit

**Spec:** [`capabilities/marginal-analysis/spec.md`](https://github.com/justin-lozano/Justin-Lozano/blob/main/capabilities/marginal-analysis/spec.md)

> Graded 2026-09-07 against the specification and workbook you committed on 5 and 6 September. Every figure in your Checks sheet reproduces exactly against my own model of this case, and the specification is one a stranger could build from without asking you a question. Two small things are open and neither is structural.

| Criterion | Where it stands |
|---|---|
| Spec completeness — inputs, structure, calculation flow | The named contract carries a value, a unit and a source for all twenty-three inputs, and the Source precision convention goes further than anyone asked by naming the unrounded constants — carrot hours as 2.5/3 rather than 0.833, and both wage rates as 50000/1440 and 25000/1440. That single decision is why your figures land on the published ones instead of near them. The five sheets are named exactly and each is described by what it must contain rather than what it is about. The calculation logic gives the labor function, the farmer-first allocation rule, the blended rate with its zero-hours guard, the standalone schedules and the Solver setup, all as formulas. Conventions cover rounding, boundaries, named ranges, the treatment of fixed costs and fractional worker equivalents. |
| Spec validation rules | Eight rules, every one with a tolerance and a stated failure mode. Two of them are what separates a specification from a wish list. Rule 4 runs Solver from two starting points and says what to do when they disagree — report both, take the higher, and log the disagreement as a finding — which means the document has an answer ready for a failure it has not had yet. Rule 5 cross-checks the sixth tomato bed's marginal cost against an independent source within a dollar. Rule 8 requires the workbook to flag the marginal-cost dip with a formula and record both values, and explicitly forbids explaining it at this stage, which is the right boundary between building and analysing. |
| Workbook satisfies the contract | Five sheets with the specified names, fifty named ranges, and every calculated cell driven by a formula referencing those names. I reproduced the numbers against my own model and they hold exactly: 5,277.22 total labor hours, 3.16 temporary-worker equivalents, a $19.73 blended rate, $42,761.66 season profit, and a tomato marginal cost of $7,660.86 at bed 5, $4,906.27 at bed 6, $8,248.59 at bed 10 and $9,390.72 at bed 11. The crossings land at 10, 10 and 6 as specified. The workbook has been through Excel, so the cached values are real rather than a library's guess. The one thing costing you here is in Checks!B22: the formula-errors row holds the literal =0 rather than a count, so it reports PASS because it was told to, not because anything was examined. |
| Audit note | Five findings, each one saying what the check would have caught and what you changed as a result — including the four where the honest answer was that you changed nothing because it passed. That is the harder discipline and most people skip it. The two-starting-point table is recorded evidence rather than an assertion: both runs, both mixes, both profits and both statuses, in a table anyone can read. |

### The one cell that asserts rather than checks

Checks!B22 contains =0 and E22 compares it to zero, so the formula-errors row will report PASS on any workbook, including one full of #REF!. Your own validation rule 6 asks the workbook to review every designated calculated cell, and that row is where the review was supposed to happen.

It is one formula. Something of the shape =SUMPRODUCT(--ISERROR('Cost Structure'!B4:B18))+SUMPRODUCT(--ISERROR('Marginal-Cost Schedules'!B5:W35))+SUMPRODUCT(--ISERROR(Optimization!B10:B28)) counts the actual errors across the ranges your rule names, and then E22 is testing something.

This is the distinction your specification already makes better than anyone else's: a check that cannot fail is not a check. Fix the rule in the spec first — name the exact ranges — and then make the cell match it.

### The mesclun crossing sentence says two different things

Under the standalone schedules your specification says: report the largest bed quantity q for which MC(q) is at or below price, before the marginal cost of the next bed exceeds price. Those are two different rules, and on the mesclun schedule they disagree.

Mesclun marginal cost climbs past $2,700 at bed 7, which is why your crossing is 6. But the farmer's 720 hours run out on the standalone mesclun schedule between beds 13 and 14, the marginal hour gets cheaper, and marginal cost drops back to about $2,523 at bed 14 and never rises above $2,700 again through bed 30. So "the largest q with MC(q) at or below price" is 30, while "the first crossing" is 6.

Your workbook implements the first crossing and it is right. The sentence should say so: report the smallest q at which MC(q+1) exceeds price. Same behaviour, one rule instead of two.

The reason this matters beyond wording is that the same effect is already in your tomato schedule and you flagged it — marginal cost falling from $7,660.86 at bed 5 to $4,906.27 at bed 6 is the farmer's hours running out and the cheaper temporary rate taking over. Your rule 8 records it without explaining it, which was correct for this stage. It is the same mechanism twice.

### What this sets up

The analysis stage compares your model against the brief you committed before you built it, and yours predicted 18 tomato, 16 carrot and 30 mesclun. The model returned 10, 20 and 30. That gap is the assignment, and you now have the schedule that explains it — tomato marginal cost passes $8,800 between bed 10 and bed 11, which is where tomatoes stop, and it has nothing to do with running out of hours. Carrots and mesclun stop at their caps instead, with marginal cost still well under price.

Do not go back and adjust the brief. The gap is the finding.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your spec into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then correct the spec, not the workbook.** This is the rule that makes the stage work: when a check fails, you fix the specification and regenerate, so the document keeps describing what was actually built.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam
