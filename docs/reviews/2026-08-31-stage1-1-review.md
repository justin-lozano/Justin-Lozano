<!-- PR TARGET: https://github.com/justin-lozano/Justin-Lozano | Stage 1.1 (2.5 pts) -->
# Stage 1.1 review — engagement brief

**Not yet graded — the brief file is still the unfilled template. Held for revision; the deadline has not passed.**

**Brief:** [`docs/briefs/perfect-competition-brief.md`](https://github.com/justin-lozano/Justin-Lozano/blob/main/docs/briefs/perfect-competition-brief.md)

> Checked 2026-08-31. You committed docs/briefs/perfect-competition-brief.md on 30 August with the message "Create perfect competition engagement brief." The file is the template with nothing filled in — every section still holds its instruction text and the frontmatter hypothesis reads "TBD." I am holding this rather than scoring it, because scoring a template tells you nothing you do not already know.

### What is in the file right now

Under "The problem" it says: "What is being decided, by whom, and what happens if it is decided badly." Under "Hypothesis" it says: "I expect X because Y." Under "How I would know I was wrong" it says: "The observation that would falsify the hypothesis above." Those are the prompts, not answers.

I mention it in this much detail because it is the kind of thing that happens when a file is committed to establish the path and the writing is meant to follow. Nothing is lost — the file is at the correct path and the commit is clean.

### What the stage actually asks for

About a page, in your own words, written before you build anything.

- The problem. What is being decided, by whom, and what it costs to decide it badly. What is fixed, what you get to choose, and what limits the choice. The test is whether you can state it without re-reading the case page — if you cannot, you do not have it yet.

- What you are assuming. The things you are taking as given, and which of them you would want to test with more time.

- Your hypothesis. Three real numbers — how many beds of tomatoes, carrots, and mesclun — and the mechanism you think decides it. You are not graded on being right. A hedged prediction that would survive any outcome is the only kind that is worthless.

- How you would know you were wrong. The specific result that would falsify what you just wrote. This is where most of this cohort loses points, so write it carefully: "if the model shows a different mix" is true of every hypothesis ever written and tests nothing. "If the model plants more than 14 tomato beds, I underestimated how much the 10 percent labor penalty compounds" is a real test.

### The shape of the problem, so you can start from something

The farm has 64 beds and a 36-week season, and it cannot influence prices — it takes what the market gives. Tomatoes earn $8,800 a bed, carrots $2,094, mesclun $2,700. The bed caps are 20, 20, and 30, which sum to 70 against 64 beds, so all three cannot be maxed and something has to give.

The thing that makes it interesting is that labor compounds. Each additional bed of a crop raises the labor required for every bed of that crop, at 10 percent per bed for tomatoes, 2.5 percent for carrots, and 1.25 percent for mesclun. So the crop that earns the most per bed also gets expensive the fastest, and the question is where those two things cross.

Pick a mix, say why, and say what result would tell you it was wrong. That is the whole deliverable, and an hour of honest thinking beats a polished page.

### Why it is worth doing properly rather than quickly

In Stage 3 you are asked to explain why your prediction and your model disagreed. That reflection is only worth writing if there was a real prediction to disagree with, and it has to have been committed before the model existed — the commit history is the proof.

Your Stage 0 was a 100, so the standard you hold yourself to is not in question. This is a page of writing that has not happened yet, and it is due before the model.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your brief into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then write the changes yourself.** For a brief, this matters more than usual: a hypothesis you did not generate cannot be honestly compared against your model in Stage 3, and that comparison is the entire point of writing the brief first.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*One standing rule for this stage: do not revise your hypothesis to match what your model later tells you. If the model contradicts the brief, that is a finding, not an error — Stage 3 asks you to explain the gap, and a brief quietly edited to be right afterwards has nothing left to explain.*

— Adam
