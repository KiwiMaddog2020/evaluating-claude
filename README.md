# evaluating-claude

A short field guide to measuring whether an LLM deployment actually works, and
keeping that answer trustworthy as the prompts and the models change underneath
you. It is the advisory synthesis of four runnable write-ups; this repo is the
guide, and the worked examples live in their own repositories.

The guide lives at **https://kiwimaddog2020.github.io/evaluating-claude/**.

## The checklist

If a team adopts nothing else:

- Nothing grades its own output. Independent, cross-family review, with the
  grader's identity controlled by the harness, not asserted by the work.
- Every score has two axes (craft and fit) and cites evidence; a grade with no
  fact behind it gets lowered until it does.
- A deterministic, model-free suite is the floor and runs on every change.
  Expensive judges only price what is new.
- A human is scheduled against the path the automation cannot exercise, and
  outranks it on disagreement.
- Results disclose what was not measured. "Estimate," "n of one," and "paused"
  stay on the number.

## The runnable examples behind each point

- **Independent, cross-model review**: [trust-weighted-evals](https://github.com/KiwiMaddog2020/trust-weighted-evals): the doer never rates its own work.
- **Two-axis scoring + anti-inflation**: [craft-fit-rating](https://github.com/KiwiMaddog2020/craft-fit-rating): craft and fit, with a runnable report parser.
- **Cheap deterministic floor + verification economics**: [oneshot-bench](https://github.com/KiwiMaddog2020/oneshot-bench): a 71-assertion suite and the cost curve of a long iteration run.
- **Self-modification guardrail**: [self-improvement-gate](https://github.com/KiwiMaddog2020/self-improvement-gate): a pure, tested promotion gate that classifies but never writes.

## License

MIT (see LICENSE).
