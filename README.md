# Solve Research Problems

A Codex skill for difficult problems in mathematics, physics, software engineering, and data analysis. It helps Codex define what would count as a solution, test promising hypotheses, and delegate an independent check when it could change the conclusion.

## Installation

Clone the repository into your Codex skills directory.

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/qqhgdkktu/solve-research-problems.git "$HOME/.agents/skills/solve-research-problems"
```

If you already installed the skill elsewhere, update that copy. To update a clone installed with the commands above, run `git -C "$HOME/.agents/skills/solve-research-problems" pull --ff-only`. Restart Codex if the skill does not appear. [Codex skills documentation](https://developers.openai.com/codex/skills)

## Usage

Include `$solve-research-problems` in your prompt and provide the problem statement, code, data, or proof. For example, you can ask Codex to review a proof.

> Use $solve-research-problems. Check whether the attached proof holds for all positive parameters. If you find a gap, identify the exact step and try to construct a counterexample. If an independent derivation could resolve that step, ask a subagent to check it.

For debugging, ask Codex to find the smallest input that reproduces the bug, exercise the normal entry point, and run the test from the final deliverable. For data analysis, provide the source rows and the denominator, then ask whether the conclusion holds within each segment.

## How it works

1. **Define the check.** Start with a small case, limiting case, counterexample, executable test, or independent calculation that can distinguish plausible answers.
2. **Work independently by default.** Delegate a specific uncertainty that another method, dataset, or reviewer could resolve. Give the subagent one question, the relevant inputs, a verifiable deliverable, and a stopping condition. For an independent derivation, withhold the expected answer.
3. **Change course when a check fails.** Try a different method, input, or test. Keep a one-line record of a dead end if it prevents duplicated work.
4. **Verify the final deliverable.** Run the code from the file being handed over, compare the formal statement with the original claim, or recompute the result from the source data.

The [skill instructions](SKILL.md) and [three delegation templates](references/assignments.md) are written in Russian. The main instruction is about 200 words, and the templates are available on demand.

## Evidence

Parisi and Zamponi used a numerical observation to frame a problem for an analytical proof. They flagged gaps in the resulting proof and published the conversations. In MOSAIC, the benefit of delegation varied with team size and token budget. These cases informed the skill's emphasis on verifiable results and targeted delegation. [Parisi and Zamponi's conversations](https://zenodo.org/records/20478428) · [MOSAIC, Appendix S-V.4](https://arxiv.org/html/2610.35934v1)

In a small Codex evaluation, each setup attempted the same six tasks twice. The plain prompt scored **11/12**, the short skill with one agent scored **12/12**, and the short skill with a subagent scored **12/12**. The baseline lost one point because the submitted Python test had a syntax error. The subagent runs used more tokens on average. These results cover one small set of tasks.
