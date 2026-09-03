<!-- PR TARGET: https://github.com/justin-lozano/Justin-Lozano | Stage 1.1 (2.5 pts) -->
# Stage 1.1 review — engagement brief

**Brief:** [`docs/briefs/perfect-competition-brief.md`](https://github.com/justin-lozano/Justin-Lozano/blob/main/docs/briefs/perfect-competition-brief.md)

> Re-graded 2026-09-02 against your 1 September revision. Last time the file was the untouched template and there was nothing to score. There is real work in it now — the problem section is genuinely yours and it is largely right — but the hypothesis and the falsification section are still blank, and those two carry 45 of the 100 points.

| Criterion | Earned | Notes |
|---|---|---|
| Problem restated in your own voice | 26 / 30 | This is real and it is yours. You catch the thing that makes the case a decision rather than a calculation: the caps sum to 70 against 64 beds, so "I have 6 beds more than what I'm given if I planted the max for each vegetable" and something has to give. And you frame the question correctly — "not which crop would earn the most, rather, how many beds should be planted for each respective crop before any extra revenue concentrated in a crop would no longer be worth it." That is the marginal question stated in your own words, which is what this criterion is for. Four points off because you do not say what it costs to decide it badly beyond profit being eaten into — the season is planted once and there is no mid-season correction, which is the real answer. |
| Hypothesis names a specific mix | 0 / 25 | The section heading is there and nothing is under it. The frontmatter still reads hypothesis: "TBD". This is the criterion the stage exists for — three real bed counts, committed before you model anything — and it is the largest single block of points on the page. |
| Economic mechanism | 8 / 25 | Partly present, inside the problem section rather than its own. You have the compounding rates right and you understand what they do: tomatoes at 10% per bed against 2.5% for carrots and 1.25% for mesclun, and the recognition that the answer is a crossover point rather than a ranking. Nothing is quantified, and one factual claim is wrong — see below. |
| Falsifiability and process | 4 / 20 | The section heading is there and nothing is under it. Four points for having the file at the canonical path, committed before any modeling, with a clean history. |
| **Final** | **38 / 100** | entered |

> Raw total 38 of 100. There is no floor available here: the floor applies to a committed brief that restates the problem in your own words and names a specific mix, and the mix is the half that is missing. Two paragraphs would change that.

### The factual correction, and it is the thing blocking you

You write: "No starting labor info or labor cost has been provided so I cannot tell how costly the compounding rates would be. If I had more time, I would like to know the starting labor info and costs."

Both are in the case, in the same table you took the prices from. Labor hours per week per bed: tomatoes 2.50, carrots 0.833, mesclun 1.25. And the labor cost: the farmer is paid $50,000 for the season and spends 720 hours in the field, and up to four temporary workers are available at $25,000 each for 1,440 hours each — which works out to about $34.72 an hour for her and $17.36 for them.

That is not a small correction, because it is the only thing standing between you and the rest of the brief. You already worked out that the answer is a crossover between rising labor cost and fixed revenue. With those four numbers you can actually locate it.

### The arithmetic, so you can start from something concrete

One bed of tomatoes takes 2.50 hours a week for 36 weeks, so 90 hours a season. The compounding works on the whole crop, not just the new bed: q beds of a crop need q × hours-per-bed-week × 36 × (1 + rate)^q hours in total. So ten tomato beds are not 900 hours — they are 900 × 1.1^10, about 2,334.

Carrots: one bed is 0.833 × 36, about 30 hours. Mesclun: 1.25 × 36, 45 hours. Both compound far more slowly.

Now the question you already framed correctly has a shape. Tomatoes earn $8,800 a bed against labor that roughly doubles every seven or eight beds. Carrots earn $2,094 against labor that barely moves. Where does the tomato bed stop being worth planting?

### What to write, and it is two paragraphs

- Under Hypothesis: three numbers. How many beds of tomatoes, carrots, mesclun. Then one paragraph saying why those numbers and not others, using the rates. You are not graded on being right. A hedged prediction that would survive any outcome is the only kind that is worthless.

- Under How I would know I was wrong: two or three sentences, each naming a result the model could actually produce and the claim of yours it would break. "If the model shows a different mix" is true of every hypothesis ever written and tests nothing. "If the model plants more than 14 tomato beds, I underestimated how fast the 10% penalty compounds" is a real test.

- Then update the frontmatter hypothesis line so it matches the body rather than reading TBD.

### Why this is worth an hour rather than ten minutes

Stage 1.3 asks you to explain why your prediction and your model disagreed. That reflection is only worth writing if there was a real prediction to disagree with, and it has to have been committed before the model existed — your commit history is the proof, which is why a hypothesis written after the Solver run is worth nothing even when it is correct.

Your Stage 0 was a 100. The standard you hold yourself to is not in question here. This is a page of writing that has not happened yet, and Stage 1.2 is due 6 September, which means the brief needs to close this week.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your brief into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then write the changes yourself.** For a brief, this matters more than usual: a hypothesis you did not generate cannot be honestly compared against your model in Stage 3, and that comparison is the entire point of writing the brief first.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*One standing rule for this stage: do not revise your hypothesis to match what your model later tells you. If the model contradicts the brief, that is a finding, not an error — Stage 3 asks you to explain the gap, and a brief quietly edited to be right afterwards has nothing left to explain.*

— Adam
