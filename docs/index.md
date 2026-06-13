---
title: "What I would tell a team standing up evaluations for Claude"
date: 2026-06-13
---

# What I would tell a team standing up evaluations for Claude

<p class="dek">How to know whether an AI you have shipped into real work is actually working, and how to keep that answer trustworthy when the prompts and the models shift underneath you.</p>

<p class="meta">Kevin Madson · June 2026 · 5 min read</p>

> **If someone forwarded this to you:** I build software with AI agents, programs
> that write and modify code on their own, across several different models. The
> review discipline below is the one I run on my own work before I trust it. Every
> claim is grounded in a public, runnable write-up, linked inline.

<p class="contact-card">
<a href="https://github.com/KiwiMaddog2020/evaluating-claude">github.com/KiwiMaddog2020/evaluating-claude</a>
<span class="sep">·</span>
<a href="mailto:kevinmadson@protonmail.com">kevinmadson@protonmail.com</a> <!-- pragma: allowlist -->
</p>

---

Suppose Claude is already in a real workflow. It drafts the support reply, grades
the document, writes the first pass of the code. The model is good, the team is
happy, and then someone asks the question that decides whether you have an asset or
a slow liability: how do we know it works, and how do we keep knowing after we
rewrite the prompt, swap the model, or hand it a new tool?

Teams get this wrong in a handful of predictable ways. None of the failures are
about the model. They live in the evaluation harness wrapped around it, the
machinery that decides whether an output passes, and every one of them is
preventable. Here is the guide I would hand a team starting this week. A worked,
runnable example sits behind each point.

## 1. The thing that did the work cannot grade the work

The most common check is also the weakest: ask the model whether its own output was
good. It says yes, fluently, with reasons. That is not a measurement. It is one
system scoring itself with its own blind spots intact, which is the student who
grades their own exam and somehow always passes.

The fix is structural. A better prompt will not save you. One model produces the
work, a *different* model grades it, and which model holds the red pen is fixed by
the harness, not asserted by the artifact under review. Work cannot certify its own
independence. In a loop I run, a change written by one model gets reviewed by two
others, and any review that traces back to the same model family, or to an
unidentified source, is discarded rather than weighted. The full write-up, including
the rule that the doer never rates its own output, is
[here](https://kiwimaddog2020.github.io/trust-weighted-evals/). First thing I would
audit in your setup: is anything grading itself.

## 2. One score launders the tradeoff that matters

A single number folds two unrelated questions into one. "Is this good" and "is this
the right thing to build" are different questions, and collapsing them lets a
gorgeous answer to the wrong problem post the same score as a plain answer to the
right one.

So score two axes, never one. Craft: would a domain expert call this well made,
intent aside. Fit: does it serve the actual goal. High craft with low fit is not a
respectable average. The lower axis is the ceiling, and you report the ceiling.
Splitting the two is the single change that catches the most self-deception, because
most self-deception hides in the gap between the well-made and the right. The method,
with the rules that keep a self-assigned score honest, is
[here](https://kiwimaddog2020.github.io/craft-fit-rating/).

## 3. You will pay, over and over, to re-derive what you already know

Across a long run the expense is not generating answers. It is grading them, and the
naive harness re-grades everything on every pass. I once ran a model through eleven
rounds of revising one hard build. A full panel of AI judges kept re-deriving the
previous panel's verdict, and by the middle of the run the judges agreed within half
a point of each other. They were paying real tokens to repeat one another.

Push every check you can onto something fixed and free. In that run the spine became
a suite of 71 deterministic checks that drive the system from a fixed starting state
in roughly six seconds, no model in the loop at all, and the expensive judges were
held in reserve for what genuinely changed between rounds. The cost curve, and how
it bends once you do this, is [here](https://kiwimaddog2020.github.io/oneshot-bench/).
The cheapest trustworthy check is the one with no AI in it. Build that floor before
anything else.

## 4. A check is blind to whatever it never exercises

This is the one I would slap a warning label on. In that same run, the build carried
a broken control path that survived nine consecutive rounds of automated checking.
The checks hit a test back door that worked perfectly, while the real path, the one a
human actually drives, was broken the entire time. The harness was testing the spare
key. It turned. The lock was jammed.

No volume of additional automated checking closes that gap, because the gap is the
part the automation structurally never touches. The fix is a human posted in front of
exactly that part, early and on a schedule, with the standing authority to overrule
the automated verdict when the two disagree. The companion habit is plain honesty:
a result that was not reproduced, or a sample of one, or a run that was paused instead
of finished, gets labeled as precisely that and nothing cleaner. A check you can trust
is one that tells you what it did *not* measure.

## 5. When the system can rewrite its own rules, guard that seam hardest

If your harness ever grows into a loop that can edit its own rules, the failure to
fear is not a bad change. It is a change that quietly edits the check meant to catch
it. The guard I trust there is a small piece of code that judges and never acts. It
evaluates each proposed change against the protected list read from the locked
original, not the copy the proposal ships with itself, so no proposal can shorten the
list of things it is forbidden to touch. The strongest verdict that code can return is
"eligible." Never "done." The design, and the honest reason I have left the acting
half switched off, is [here](https://kiwimaddog2020.github.io/self-improvement-gate/).

## What good looks like, as a checklist

If your team takes nothing else from this:

- Nothing grades its own output. The reviewer is independent, from a different model,
  assigned by the harness rather than by the work.
- Every score carries two axes, good and right-for-the-goal, and cites evidence. A
  grade with no fact under it gets marked down until it has one.
- A fixed, no-AI check is the floor and it runs on every change. The expensive judges
  weigh in only on what is new.
- A human sits in front of the path automation cannot reach, and overrules it on
  disagreement.
- Results state what was not measured. "Estimate," "sample of one," and "paused" ride
  along with the number, never stripped off.

## The through-line

A trustworthy check has three properties, and a strong model hands you none of them
for free. It can disagree with you, because the grader is not the thing being graded.
It costs almost nothing to re-run, because its floor needs no model. And it tells you
what it did not measure, because the alternative is a number that lies by omission.
Build those three in and the model can change underneath you without dragging your
confidence down with it. That is what checking is for. Not to bless the work. To stay
the one thing that still tells you the truth after everything else has shifted.

---

<p class="byline"><em>I build agentic systems across multiple coding LLMs. More of my research notes are <a href="/">here</a>.</em></p>
