---
title: "What I would tell a team standing up evaluations for Claude"
date: 2026-06-13
---

# What I would tell a team standing up evaluations for Claude

<p class="dek">A field guide to measuring whether an LLM deployment actually works, and keeping that answer trustworthy as the prompts and the models change underneath you.</p>

<p class="meta">Kevin Madson · June 2026 · 5 min read</p>

> **If someone forwarded this to you:** I build and operate agentic systems
> across multiple coding LLMs, and the evaluation discipline below is the one I
> run on my own code before I trust any of it. Every point is grounded in a
> public, runnable write-up, linked inline.

<p class="contact-card">
<a href="https://github.com/KiwiMaddog2020/evaluating-claude">github.com/KiwiMaddog2020/evaluating-claude</a>
<span class="sep">·</span>
<a href="mailto:kevinmadson@protonmail.com">kevinmadson@protonmail.com</a> <!-- pragma: allowlist -->
</p>

---

Say you have put Claude into a real workflow. It drafts the support reply, grades
the document, writes the first pass of the code. The model is good, your team is
happy, and then someone asks the question that decides whether this is an asset
or a slow-moving liability: how do we know it is working, and how do we keep
knowing after we change the prompt, swap the model, or add a tool?

Most teams answer that question wrong in one of a few predictable ways. None of
the mistakes are about the model. They are about the evaluation around it, and
every one of them is avoidable. Here is the field guide I would hand a team
starting this week, with a worked, runnable example behind each point.

## 1. Do not let the thing that did the work grade the work

The most common eval is also the weakest: ask the model whether its own output
was good. It will tell you yes, fluently, with reasons. Self-assessment from a
generator is not a measurement; it is the same distribution scoring itself.

The fix is structural, not a better prompt. The output from one model is graded
by a *different* model family, and the grader's identity is established by your
harness, not asserted by the work under review. A proposal cannot vouch for its
own independence. In a loop I run, a change written by one model is reviewed by
two others, and a same-family or unknown-provenance review fails closed rather
than open. The full write-up, with the rule that the doer never rates its own
work, is
[here](https://kiwimaddog2020.github.io/trust-weighted-evals/). The first thing I
would check in your setup is whether anything is grading itself.

## 2. One score hides the tradeoff that actually matters

A single number quietly launders two different questions into one. "Is this good"
and "is this right for what we are doing" are not the same axis, and averaging
them lets a beautiful solution to the wrong problem score the same as the right
one.

Score two axes, never one: craft (would an expert in the discipline call this
well made, independent of intent) and fit (does it serve your stated purpose).
A category at high craft and low fit is not a middling average; it is a
fit problem, and the weaker axis is what holds the work back. Splitting the axes
is the single change that catches the most self-deception, because most of it
lives in the slash between the two. The rubric, with the anti-inflation rules
that keep a self-assigned score honest, is
[here](https://kiwimaddog2020.github.io/craft-fit-rating/).

## 3. You will pay to re-verify what you already know

The expensive part of a long-running eval is not generating outputs, it is
checking them, and the naive setup re-checks everything every time. When I ran a
model through eleven rounds of iteration on one hard build, a full panel of
LLM-judges re-derived most of what the previous panel already knew; by the
middle of the run the judges agreed within half a point, which means most of
them were paying full price to re-confirm each other.

Push every check you can onto something deterministic and free. In that run the
backbone became a suite of 71 assertions that drive the system on a fixed seed in
about six seconds with no model in the loop, and the expensive judges were
reserved for what genuinely changed. The published numbers on this, including how
the cost curve bends when you do it, are
[here](https://kiwimaddog2020.github.io/oneshot-bench/). For your deployment: the
cheapest trustworthy eval is the one with no model in it. Build that floor first.

## 4. Your evals are blind to whatever they cannot exercise

This is the one I would attach a warning label to. In that same run, the system
had a broken real-input path that hid from nine consecutive rounds of automated
checking, because the checks exercised an injected test path that worked
perfectly while the real path a human drives was broken. The graders were testing
the thing they could reach, and it was not the thing that was broken.

No amount of additional automated checking closes that gap. The fix is to
schedule a human against the part the harness structurally cannot see, early and
on a cadence, and to let that human outrank the automated verdict when they
disagree. The companion habit is disclosure: a result that was not reproduced,
or a sample of one, or a run that was paused rather than finished, gets labeled
as exactly that. An eval you can trust is one that tells you what it did *not*
measure.

## 5. When the system can change its own rules, gate that hardest

If your deployment ever graduates to a loop that can modify its own operating
rules, the failure you should fear is not a bad change; it is a change that edits
the check meant to stop it. The guardrail I trust most there is a pure, tested
gate that classifies but never writes: it evaluates a proposed change against the
protected list from the *trusted base*, not the candidate's own copy, so a
proposal cannot shorten the list of things it is forbidden to touch. Its best
possible verdict is "eligible," never "applied." The design, and the honest
reason I have not turned the acting half on, is
[here](https://kiwimaddog2020.github.io/self-improvement-gate/).

## What good looks like, in a checklist

If your team adopts nothing else, adopt these:

- Nothing grades its own output. Independent, cross-family review, with identities
  the harness controls.
- Every score has two axes and cites evidence; a grade with no fact behind it gets
  lowered until it does.
- A deterministic, model-free suite is the floor, and it runs on every change.
  Expensive judges only price what is new.
- A human is scheduled against the path the automation cannot exercise, and
  outranks it on disagreement.
- Results disclose what was not measured. "Estimate," "n of one," "paused" stay
  on the number.

## The through-line

An evaluation you can trust has three properties, and a frontier model gives you
none of them for free. It can disagree with you, because the grader is not the
thing being graded. It costs almost nothing to re-run, because its floor is
deterministic. And it tells you what it did not measure, because the alternative
is a number that lies by omission. Build those three in, and the model can change
underneath you without taking your confidence with it. That is the whole point of
an eval: not to bless the work, but to be the thing that still tells you the
truth after everything else has changed.

---

<p class="byline"><em>I build agentic systems across multiple coding LLMs. More of my research notes are <a href="/">here</a>.</em></p>
