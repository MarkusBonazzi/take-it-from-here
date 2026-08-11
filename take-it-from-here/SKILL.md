---
name: take-it-from-here
description: Prepare the user's remaining work for a fresh local task or session, transfer a compact continuation context without creating Git branches or worktrees, and offer to continue without starting work until the user explicitly confirms. Use when the user invokes $take-it-from-here or /take-it-from-here, asks to prepare a fresh session, move context to a new task and wait, or expresses equivalent intent in any language, including "подготовь продолжение в новой задаче," "перенеси контекст и жди моей команды," or "предложи продолжить со следующего шага."
---

# Take It From Here

Transfer the minimum execution-critical state into a fresh task without copying the conversation. Offer one concrete next action, then wait for explicit user confirmation before doing it.

## Host adaptation

1. Inspect the tools and capabilities already available in the current AI host. Do not assume product-specific task or session tools exist.
2. If the host can create a new task, chat, or session in local mode, create it in the same saved project and exact current working directory. Use the continuation note as its initial prompt and tell the receiving agent to offer the next action and wait.
3. Never create or select a Git branch or worktree, and never fork the current conversation merely to create the receiving session. If the host cannot explicitly guarantee a local session without those side effects, return the complete continuation note in one copy-ready fenced block instead. Tell the user to paste it into a fresh local session. The prompt must still require confirmation before work begins.
4. If project identity cannot be preserved automatically, include the exact working directory or workspace identity in the continuation note.
5. Use title-setting tools only when available. Never fail the prepared continuation merely because title-setting is unavailable.

## Repository safety

1. Treat task creation as context transfer only. Do not run Git commands merely to create the task.
2. Do not create, switch, rename, or delete branches.
3. Do not create or remove Git worktrees.
4. Keep the receiving task attached to the same local checkout and working directory whenever the host supports that mode.
5. When local-only creation cannot be guaranteed, stop at the copy-ready continuation note instead of attempting automatic task creation.

## Language

1. Accept explicit commands and equivalent natural-language requests in any language.
2. Write the continuation note in the language currently used by the user unless another language is requested.
3. Preserve commands, code, paths, filenames, identifiers, error messages, and quoted literals exactly unless translation is explicitly requested.
4. Localize the section labels and final instruction when useful to the receiving task.

## Workflow

1. Use the current conversation and already collected evidence. Do not rescan the repository merely to prepare the handoff.
2. If inside a Git repository, inspect only concise `git status` and relevant `git diff --stat` when the current state is uncertain.
3. Prepare a continuation note of at most 800 tokens containing:
   - objective and current phase;
   - completed work;
   - exact important or changed file paths;
   - confirmed decisions and constraints;
   - completed verification and its result;
   - rejected approaches that must not be repeated;
   - blockers, if any;
   - one concrete next action.
4. Never include the full conversation, large logs, full file contents, secrets, credentials, or speculative history.
5. Follow the host-adaptation path above: create a new local task or session only when the host guarantees no branch, worktree, or history fork; otherwise return a copy-ready continuation note.
6. Tell the receiving session to verify the listed working files briefly, avoid repeating completed discovery, state the proposed next action, and ask whether to continue. Do not execute the next action until the user explicitly confirms.
7. Give the new task or session a short title based on the objective when title-setting is supported.
8. Return the created-session link or directive when available, otherwise return the copy-ready block, plus a one-sentence summary. Leave the old session unchanged, do not archive it, and do not resume work in either session without explicit user confirmation.

## Initial prompt format

Start the receiving prompt with `TAKE IT FROM HERE - CONTEXT FROM PREVIOUS SESSION`, followed by the compact sections above and this final instruction:

`Context only. Stay in the same local checkout and working directory. Do not create or switch Git branches or worktrees. Briefly verify the listed working state, offer to continue from the listed next action, and wait. Do not start work until the user explicitly confirms.`
