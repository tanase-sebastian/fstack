---
name: fstack-check
description: Review finished work with three questions before shipping. Use when work is done and needs review, or after /fstack-build finishes all plan steps.
---

# /fstack-check

Review finished work before shipping. Answer three questions with evidence. Report and stop. Do not fix anything.

The goal is a report the user can trust without re-checking the work. Every claim needs proof. If you can't prove it, say so.

## When to use

- The user invokes `/fstack-check`.
- `/fstack-build` finished all plan steps.
- `/fstack` routed here because work is done and needs review.

## Before you start

Read two things:

- The **"Done when"** list in `PLAN.md`. These are this task's criteria.
- The **"Always done when"** section in `AGENTS.md`. These are the project's baseline checks. If it's missing, say so and use: typecheck, lint, tests.

## Steps

Answer exactly three questions, in order, with evidence.

### 1. Does it work?

First, the baseline. Run every check from `AGENTS.md`. Paste the last few lines of each output.

Then, the task. Go through each "Done when" item. For each one, prove it:

- Run a test, a command, or the app.
- Paste the output, or describe exactly what you saw.
- Mark it **proven**, **failed**, or **not proven**.

**Not proven** is an honest answer. Use it when you couldn't check something, and say why. Never mark something proven because the code looks right.

"It should work" is not an answer.

### 2. Does it match the plan?

Diff the work against `PLAN.md`. Flag anything built that was not planned. Flag anything planned that was not built.

Check the "NOT doing" section. Confirm none of it crept in.

Then check blast radius. List what else this change could affect: callers, styles, routes, configs, data. Confirm each is fine, with evidence.

### 3. Is it simple?

Quick pass with the `/fstack-simplify` smell list. If something smells, say so and suggest running `/fstack-simplify`. Do not run it automatically.

## Your eyes on

After the three answers, list what the user should look at themselves. Keep it short. Only include:

- Anything marked **not proven**.
- Anything hard to undo: migrations, data changes, deletions, auth, payments, public API changes.
- Anything where you made a judgment call the plan didn't cover.

If the list is empty, write "Nothing — all proven." That line is the point of this skill.

## Verdict

End with one line:

- **Ready to push** — or —
- **Not ready: [reason]**

Anything **failed** means Not ready. Anything **not proven** is the user's call, not yours.

**Stop and ask** the user what to do next. Do not fix what you found.

## Must NOT

- Fix things it finds. Report, suggest, stop. The user decides.
- Skip the "prove it works" step, even for small changes.
- Mark a criterion proven without output or an observation to back it.
- Add review dimensions. Three questions only. No security theater, no style nits.

## Example

> **Does it work?**
> Baseline: `npm run typecheck` clean. `npm run lint` clean. `npm test` 42 passed, 0 failed.
> - Theme persists after refresh — **proven**. Toggled dark, refreshed, still dark.
> - New test covers the toggle — **proven**. `theme.test.ts` failed before the change, passes now.
> - Works in Safari — **not proven**. No Safari in this environment.
>
> **Does it match the plan?** All 4 steps done. Nothing extra built. Nothing from "NOT doing" crept in. Blast radius: header component and global CSS — header snapshot test passes, no other files import the CSS vars I changed.
>
> **Is it simple?** `ThemeProvider` wrapper has one caller. Consider `/fstack-simplify` to drop it.
>
> **Your eyes on:** Safari check.
>
> **Ready to push**