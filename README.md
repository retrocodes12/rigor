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

## Install (Claude app — claude.ai)

Claude.ai accepts real skill uploads (same format as Claude Code), on every plan including Free:

1. Grab **`rigor.zip`** from the [latest release](https://github.com/retrocodes12/rigor/releases/latest)
   — don't use GitHub's *Download ZIP* button; it wraps the files in a `rigor-main/` folder,
   which can fail skill validation.
2. On **claude.ai in a browser**, open **Settings → Capabilities** and enable
   *code execution and file creation* (skills require it).
3. Go to **Customize → Skills → "+" → Create skill → Upload a skill** and upload the zip.
4. Claude now invokes it automatically when a request looks like fact-checking — or say
   *"use my rigor skill on this claim."*

**Mobile caveat:** Anthropic's docs don't state whether uploaded skills fire in the mobile app,
and reports conflict — test on your phone after uploading. Guaranteed mobile fallback: create a
**Project** named "Rigor" on claude.ai, paste the portable prompt (everything below the `---`
divider in [`references/portable-prompt.md`](references/portable-prompt.md)) into its project
instructions, and chat inside that project — Projects sync to the mobile app. Pasting it into
**Settings → Profile → personal preferences** applies it to every chat instead.

## Install (ChatGPT)

The paste block is ~3,300 characters, which overflows ChatGPT's 3,000-character
custom-instructions field. Use either of these instead:

- **Project (recommended, works on Free):** New project → name it "Rigor" → paste the portable
  prompt into the project's **Instructions** → chat inside the project. Works in the mobile app.
- **Custom GPT (needs Plus to create):** Explore GPTs → Create → paste the prompt into
  **Instructions** (8,000-char limit) → save. Summon it in any chat with `@Rigor`.

## Install (Gemini)

Create a **Gem**: [gemini.google.com](https://gemini.google.com) → **Gems → New Gem** → paste the
portable prompt as its instructions → save. Gems work in the Gemini mobile app too.

## Install (any other chatbot)

Open the
[raw portable prompt](https://raw.githubusercontent.com/retrocodes12/rigor/main/references/portable-prompt.md),
copy everything **below the `---` divider**, and paste it into the bot's system-prompt or
custom-instructions box — Claude Projects, Copilot, Grok, Perplexity Spaces, LM Studio,
Open WebUI, a Discord bot's persona field, anywhere.

On surfaces where the bot can't browse the web, the prompt degrades honestly by design: claims
get labeled `[trained]` / `[inference]` instead of being presented as verified. Same protocol,
different ceiling.

## License

[MIT](LICENSE)
