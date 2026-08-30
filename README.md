# learn

[![video](assets/thumbnail.png)](https://www.youtube.com/watch?v=kzcI5F4tGiU)

My AI learning system from this video: [How I Use AI to Learn Things](https://www.youtube.com/watch?v=kzcI5F4tGiU).

This is a personal system I built for myself, shared as-is. Built as a pi configuration: the teaching philosophy encoded in a skill, a few small extensions, and agent definitions.

## What's in it

- `skills/teach/` — the philosophy and the process
- `skills/visualize/` — adds a correct, minimal diagram to a lesson when an idea is clearer as a picture
- `extensions/ask-user-question/` — the agent asks you questions through a UI popup
- `extensions/quiz/` — graded questions with instant feedback (✓/✗, correct answer, explanation)
- `extensions/md-log/` — link a markdown file to the session
- `extensions/visual-tools/` — tools for visualization subagents
- `agents/` — `researcher`, `svg-maker`, `mermaid-maker`: the subagents the system delegates to

## Install

This repo **is** a `.pi` directory. From your learning project's root:

```bash
git clone https://github.com/amosblomqvist/learn .pi
```

Then open pi in that directory. (Or copy the pieces you want into your existing project config.)

### Dedicated Linux workspace

The visual tools accept two optional environment variables for installations
where the Pi workspace and Obsidian vault are separate:

- `PI_LEARN_STAGING_ROOT` stores transient preview sources and renders under an
  approved scratch directory instead of the operating-system default.
- `PI_LEARN_PUBLISH_ROOT` publishes final images into `<root>/viz` instead of
  `<cwd>/viz`.

Linux Chrome and Chromium installations in the usual `/usr/bin` and `/snap/bin`
locations are detected automatically. SVG rendering still requires
`rsvg-convert` (provided by `librsvg2-bin` on Ubuntu and Debian).

The bundled agent definitions intentionally inherit Pi's selected default model
instead of requiring separate OpenRouter and Anthropic accounts.

This fork also includes `scripts/understanding`, a launcher for the dedicated
workspace. It sets `TMPDIR`, `TMP`, `TEMP`, and the visual staging root under
the DAS scratch filesystem, publishes final images into the Obsidian
`Understanding` folder, and then starts Pi. Override its defaults with
`UNDERSTANDING_WORKSPACE`, `UNDERSTANDING_SCRATCH_BASE`, or
`PI_LEARN_PUBLISH_ROOT` when needed.

## Requirements

- [pi](https://github.com/earendil-works/pi)
- A subagent implementation, so the system can spawn the researcher and the visual makers. Recommended: [pi-interactive-subagents](https://github.com/amosblomqvist/pi-interactive-subagents) (tmux only). With it, everything works out of the box. Any other implementation works too, but expect to adapt the agent definitions, e.g. `agents/researcher.md` lists `safe_bash` in its tools, which is specific to that extension.
- `ask-user-question` — use the copy bundled here. If your setup already has an `ask-user-question` extension, use **this** one in its place. Popups from different extensions serialize through a shared UI lock, which only works when it's the same implementation.

## Notes

You can run the system without subagents. The main session does the teaching. You just lose the researcher (truth verification) and the generated visuals.

The teaching skill is written for one learner (me). Edit the skill to fit how you learn best.
