---
name: take-it-from-here
description: Take over the user's remaining work, create a compact continuation context, transfer it to a fresh task or session when the host supports that, and continue from the next action. Use when the user invokes $take-it-from-here or /take-it-from-here, says "take it from here" or "handle the rest," asks to move the work to a fresh session and keep going, or expresses equivalent intent in any language, including "возьми дальше на себя," "продолжи в новой задаче," or "доделай сам."
---

# Take It From Here

Take responsibility for the remaining work. Transfer the minimum execution-critical state into a fresh task without copying the conversation, then continue from the next action.

## Host adaptation

1. Inspect the tools and capabilities already available in the current AI host. Do not assume product-specific task or session tools exist.
2. If the host can create a new task, chat, or session, create one in the same saved project or workspace, use the continuation note as its initial prompt, and tell the receiving agent to continue immediately.
3. If the host cannot create a new session, return the complete continuation note in one copy-ready fenced block and tell the user to paste it into a fresh session so the agent can continue there.
4. If project identity cannot be preserved automatically, include the exact working directory or workspace identity in the continuation note.
5. Use title-setting tools only when available. Never fail the continuation merely because title-setting is unavailable.

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
5. Follow the host-adaptation path above: create a new task or session when supported; otherwise return a copy-ready continuation note.
6. Tell the receiving session to verify the listed working files briefly, avoid repeating completed discovery, and continue immediately from the listed next action. Ask the user only when genuinely blocked, when required information is missing, or when approval is required.
7. Give the new task or session a short title based on the objective when title-setting is supported.
8. Return the created-session link or directive when available, otherwise return the copy-ready block, plus a one-sentence summary. Leave the old session unchanged, do not archive it, and continue the transferred work only in the fresh session.

## Initial prompt format

Start the receiving prompt with `TAKE IT FROM HERE - CONTEXT FROM PREVIOUS SESSION`, followed by the compact sections above and this final instruction:

`Continue from the listed next action immediately. Do not redo completed investigation. Ask the user only if blocked or if approval is required.`
