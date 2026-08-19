# rigor

A fact-checking protocol for AI chatbots. Two forms, same rules:

- **A [Claude Code](https://code.claude.com) skill** ([`SKILL.md`](SKILL.md)) — adds a `/rigor`
  mode that verifies claims against primary sources with live web search.
- **A portable system prompt** ([`references/portable-prompt.md`](references/portable-prompt.md)) —
  paste into ChatGPT custom instructions, Gemini saved info, a Claude Project, or any
  system-prompt box. Self-contained; degrades honestly when the bot can't browse.

## What it enforces

Chatbots fail at facts in predictable ways: they agree with whoever's asking, cite papers they
haven't read, present one loud study as settled science, and report relative risks without base
rates. `rigor` is a fixed procedure targeting each failure:

1. **Decompose** input into atomic claims; classify each as empirical, interpretive, or a value
   judgment. Only empirical claims get verdicts — opinions never get dressed up as findings.
2. **Pre-commit** what evidence would support *and* refute each claim before looking, so the
   search can't stop the moment the preferred side is ahead.
3. **Verify** against primary sources (the paper, not the press release), ranked by an explicit
   evidence hierarchy — meta-analyses down to anecdote — with retraction, replication, and
   funding checks. Fabricated citations are banned; unread sources get labeled as unread.
4. **Disconfirm**: a mandatory search for the strongest case against the emerging conclusion,
   presented in its best form.
5. **Verdict** per claim — Supported / Likely / Contested / Unsupported / Refuted /
   Unverifiable — with calibrated confidence and a "what would change this" line.
6. **Bias sweep**: a sycophancy check (would the answer differ if the user wanted the
   opposite?), a valence-flip symmetry test, absolute effects alongside relative ones, named
   confounds on causal claims, and consensus reported separately from the verdict.

## What it doesn't claim

No prompt removes bias. Training data, source availability, and question framing all leak in.
This protocol *reduces* bias through procedure and **discloses the remainder** — every answer
ends with a Limits line naming its weakest link and what wasn't checked. A tool claiming zero
bias would be the least scientific thing about it.

## Install (Claude Code)

```sh
git clone https://github.com/retrocodes12/rigor ~/.claude/skills/rigor
```

Then in any session:

```
/rigor <claim or question>   # fact-check something; mode stays on until /rigor off
/rigor export                # print the portable prompt for other chatbots
```

## Install (everything else)

Copy the block below the divider in
[`references/portable-prompt.md`](references/portable-prompt.md) into the bot's system prompt or
custom-instructions field.

## License

[MIT](LICENSE)
