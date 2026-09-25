---
name: fstack-teach
description: Turn finished work into a short lesson for the human, so they learn from what was built. Use after /fstack-check, or when the user asks to understand a change, a pattern, or a concept in the code.
---

# /fstack-teach

The agent writes the code. The human should still grow. This skill turns real work into one short lesson the user can learn from.

Teach from this codebase, not from textbooks. Every lesson points at real lines.

## When to use

- The user invokes `/fstack-teach`.
- After `/fstack-check` says Ready to push, and the task touched something non-trivial.
- The user asks "why did you do it this way?" or "explain this".

## Before you start

Read two things:

- The **"Learner"** section in `AGENTS.md`. What the user knows well, and what they want to get better at. If it's missing, ask two quick questions and add it.
- `learning/INDEX.md`. Past lessons. Do not repeat a topic unless this task adds something new.

## Steps

### 1. Pick one thing

Look at the diff and the plan. Find the one idea most worth learning. Good picks:

- An architecture decision and its trade-off.
- A pattern or technique the user hasn't used much.
- A bug's real root cause.
- Something the agent was unsure about or had to choose between.

Bad picks: syntax, trivia, things the Learner section says the user already knows.

If nothing is worth teaching, say so in one line and stop. That's a fine answer.

### 2. Ask before telling

Before explaining, ask the user one question about it. Examples:

- "Why do you think this uses a queue instead of calling the service directly?"
- "What would break if we removed this index?"

Wait for their answer. Then write the lesson, building on what they said.

If the user says "just teach", skip the question.

### 3. Write the lesson

Keep it under 300 words. Use this shape:

- **The idea** — in plain words, two or three sentences.
- **Where it is** — file and line in this codebase.
- **Why here** — why this choice fits this situation.
- **The other option** — what else could work, and when it would be better.
- **Try it yourself** — one small exercise the user does by hand. It should take 10–20 minutes.
- **Go deeper** — one good resource, only if you're sure it exists.

### 4. Save it

Write the lesson to `learning/YYYY-MM-DD-short-topic.md`.

Add one line to `learning/INDEX.md`: date, topic, file link.

Stop. Do not start the exercise.

## Must NOT

- Solve the exercise. If the user asks for help, give a hint first. Give the answer only if they ask again.
- Write more than one lesson per run.
- Teach something that isn't in the code. No invented examples.
- Lecture. If it's over 300 words, cut it.

## Example

> **The idea:** An optimistic update shows the change in the UI before the server confirms it, then rolls back if the server fails. It makes the app feel instant.
>
> **Where it is:** `src/items/useDeleteItem.ts:14–31`.
>
> **Why here:** Deleting a pantry item almost never fails, and waiting for the server felt slow on mobile.
>
> **The other option:** Wait for the server, show a spinner. Better when failures are common or the action is expensive to undo — like payments.
>
> **Try it yourself:** Make the rename action optimistic too. Force the API to fail and check the rollback works.
>
> Saved to `learning/2026-09-24-optimistic-updates.md`.