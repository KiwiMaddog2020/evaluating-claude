---
title: "What I would tell a team standing up evaluations for Claude"
date: 2026-06-13
---

# What I would tell a team standing up evaluations for Claude

<p class="dek">A field guide to knowing whether an AI you have put into real work is actually working, and keeping that answer trustworthy as the prompts and the models change underneath you.</p>

<p class="meta">Kevin Madson · June 2026 · 5 min read</p>

> **If someone forwarded this to you:** I build software with AI agents, programs
> that write and change code on their own, across several different AI models. The
> review discipline below is the one I run on my own work before I trust it. Every
> point is grounded in a public, runnable write-up, linked inline.

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
the mistakes are about the model. They are about the checking around it, and
every one of them is avoidable. Here is the field guide I would hand a team
starting this week, with a worked, runnable example behind each point.

## 1. Do not let the thing that did the work grade the work

The most common check is also the weakest: ask the model whether its own output
was good. It will tell you yes, fluently, with reasons. An AI grading its own
output is not a measurement; it is the same system scoring itself, with the same
blind spots, the way a student grading their own exam tends to pass.

The fix is structural, not a better prompt. The output from one model is graded
by a *different* AI, and which one is the grader is decided by your setup, not
claimed by the work under review. A piece of work cannot vouch for its own
independence. In a loop I run, a change written by one model is reviewed by two
others, and a review from the same family of model, or from an unclear source,
is thrown out rather than trusted. The full write-up, with the rule that the doer
never rates its own work, is
[here](https://kiwimaddog2020.github.io/trust-weighted-evals/). The first thing I
would check in your setup is whether anything is grading itself.

## 2. One score hides the tradeoff that actually matters

A single number quietly blends two different questions into one. "Is this good"
and "is this right for what we are doing" are not the same question, and averaging
them lets a beautiful answer to the wrong problem score the same as the right one.

Score two things, never one: craft (would an expert in the field call this well
made, setting aside intent) and fit (does it serve your actual goal). Something
with high craft and low fit is not a middling average; the weaker of the two is
what holds it back. Splitting the two is the single change that catches the most
self-deception, because most of it hides in the gap between them. The method, with
the rules that keep a self-given score honest, is
[here](https://kiwimaddog2020.github.io/craft-fit-rating/).

## 3. You will pay to re-check what you already know

The expensive part of checking, over a long run, is not producing answers, it is
checking them, and the naive setup re-checks everything every time. When I ran a
model through eleven rounds of revising one hard build, a full panel of AI judges
kept re-deriving what the last panel already knew; by the middle of the run the
judges agreed within half a point, so most of them were paying to repeat each
other.

Push every check you can onto something fixed and free. In that run the backbone
became a set of 71 checks that drive the system on a set starting point in about
six seconds with no AI involved at all, and the expensive judges were saved for
what genuinely changed. The numbers on this, including how the cost bends when you
do it, are [here](https://kiwimaddog2020.github.io/oneshot-bench/). For your own
setup: the cheapest trustworthy check is the one with no AI in it. Build that floor
first.

## 4. Your checks are blind to whatever they cannot exercise

This is the one I would attach a warning label to. In that same run, the build had
a broken control path that hid from nine straight rounds of automated checking,
because the checks exercised a test back door that worked perfectly while the real
path a human drives was broken. The checkers were testing the spare key, and it
turned, while the actual lock was jammed.

No amount of extra automated checking closes that gap. The fix is to put a human in
front of the part the automation structurally cannot reach, early and on a schedule,
and to let that human overrule the automated verdict when they disagree. The
companion habit is honesty: a result that was not reproduced, or a sample of one, or
a run that was paused rather than finished, gets labeled as exactly that. A check
you can trust is one that tells you what it did *not* measure.

## 5. When the system can change its own rules, guard that hardest

If your setup ever grows into a loop that can change its own rules, the failure to
fear is not a bad change; it is a change that edits the check meant to stop it. The
guard I trust most there is a small piece of code that judges but never acts: it
checks a proposed change against the protected list taken from the locked-down
original, not the copy the change brought along, so a proposal cannot shorten the
list of things it is forbidden to touch. The best verdict it can give is "eligible,"
never "done." The design, and the honest reason I have not switched the acting half
on, is [here](https://kiwimaddog2020.github.io/self-improvement-gate/).

## What good looks like, in a checklist

If your team adopts nothing else, adopt these:

- Nothing grades its own output. An independent reviewer, from a different model,
  chosen by your setup rather than by the work.
- Every score has two parts (good, and right-for-the-goal) and cites evidence; a
  grade with no fact behind it gets lowered until it does.
- A fixed, no-AI check is the floor, and it runs on every change. The expensive AI
  judges only weigh in on what is new.
- A human is put in front of the path the automation cannot reach, and overrules it
  when they disagree.
- Results say what was not measured. "Estimate," "sample of one," and "paused" stay
  on the number.

## The through-line

A check you can trust has three properties, and a powerful model gives you none of
them for free. It can disagree with you, because the grader is not the thing being
graded. It costs almost nothing to re-run, because its floor needs no AI. And it
tells you what it did not measure, because the alternative is a number that lies by
leaving things out. Build those three in, and the model can change underneath you
without taking your confidence with it. That is the whole point of checking: not to
bless the work, but to be the thing that still tells you the truth after everything
else has changed.

---

<p class="byline"><em>I build agentic systems across multiple coding LLMs. More of my research notes are <a href="/">here</a>.</em></p>
