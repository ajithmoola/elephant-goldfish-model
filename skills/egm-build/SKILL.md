---
name: egm-build
license: Apache-2.0
disable-model-invocation: true
description: >-
  Runs the EGM research coding loop. The Elephant writes a brief, a fresh
  coding agent writes and runs the code and records every run in the run log,
  and a fresh Result Verifier checks the code and results against the story
  doc. Failed attempts get a new brief and a new coding agent. Use for
  prototypes and production experiments in computational research.
---

# EGM Build: Research Coding Loop

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

This skill runs every piece of research code in the EGM research flow. Three
roles take part:

*   **The Elephant (you)**: Keeps the story doc up to date, writes the first
    brief, logs outcomes, and launches coding agents. You do not read code.
*   **The Coding Agent**: A fresh subagent per attempt
    (`agents/coding_agent.md`). It writes and runs code from a brief and
    records every run in the story doc's run log.
*   **The Result Verifier**: A fresh Goldfish per attempt
    (`agents/result_verifier.md`). It checks the code, run log, and outputs
    against the story doc and `resources/document_standards/evidence-standard.md`,
    returns a verdict, and writes the next brief when an attempt fails.

## Delegation

A Goldfish runs in a fresh context with no conversation history. Spawn each
subagent with your harness's subagent tool (Antigravity: `invoke_subagent`; Claude Code: `Agent`; Codex: `spawn_agent`). Each subagent is defined in `agents/<file>.md`, and its
frontmatter `name` (for example `egm-coding-agent`) is the subagent type to request
where your harness registers named subagents. Otherwise spawn a general-purpose
subagent and tell it to read that persona file first. Pass the prompt verbatim
and wait for the report.
If your harness has no subagent tool, stop and tell the user: code written and
checked in the same context is not an EGM build.

## Execution Constraints (CRITICAL)

1.  **Fresh Agents Only**: Every attempt uses a new coding agent and a new
    Result Verifier. Never send a follow-up message to a previous coding agent.
2.  **Elephant Does Not Read Code**: Do not open source files or output files.
    You see the brief, the coding agent's report, and the verifier's verdict.
3.  **No Hidden Knowledge**: Everything a brief relies on must be in the story
    doc or in the brief file. If you know something the story doc does not say,
    add it to the story doc (with the student's approval) before writing the
    brief.
4.  **Never Invent or Smooth Over Results**: Copy results into the story doc
    exactly as the verifier reports them, with run IDs. Report results that
    weaken the thesis to the student plainly.
5.  **Student Owns the Story**: Do not change the Thesis section, or add,
    remove, or repurpose storyboard slots, without the student's explicit
    approval. Pivots are the student's decision.
6.  **Plain-English Standard**: Write briefs, log entries, and messages in
    simple, direct language.

## Inputs

*   **`StoryDoc`**: Path to the story doc. If none exists, tell the student to
    run `egm-pitch` first, or create one with the section headings from
    `resources/document_standards/story-doc-standard.md` if they ask you to.
*   **`Task`**: What the student wants built or run, and which storyboard slot
    it serves (or `prototype` during the pitch).

## Workflow

### Step 1: Write the Brief

1.  Read the story doc.
2.  Write a self-contained brief to `briefs/brief-<NNN>.md` next to the story
    doc, numbering briefs in order. The brief states:
    *   The goal and the storyboard slot it serves.
    *   What to build and run, and what outputs to produce.
    *   Constraints from the story doc (data, methods, parameters).
    *   The story doc path.
    The brief must not ask for a particular result.
3.  Give the student the brief path and ask them to confirm or edit it. Wait
    for their reply.

### Step 2: Launch the Coding Agent

Spawn a fresh `egm-coding-agent` (Claude Code: `egm:egm-coding-agent`). Pass
the absolute paths of the brief, the story doc,
`resources/document_standards/story-doc-standard.md`, and
`resources/document_standards/evidence-standard.md`. Wait for its report.

### Step 3: Verify

Spawn a fresh `egm-result-verifier` (Claude Code: `egm:egm-result-verifier`)
with `Mode: BUILD`. Pass the absolute paths of the story doc, the brief, and the
evidence standard, plus the coding agent's report verbatim and the run IDs it
added. Wait for its verdict.

### Step 4: Log the Outcome

Append a dated entry to the story doc's Log section with the brief path, the
run IDs, the verdict, and the verifier's summary. Then:

*   **`PASS`**: Update the storyboard slot's status and run IDs. Tell the
    student the result in two to four sentences, citing run IDs. If the result
    weakens or contradicts the thesis, say so and ask the student how to
    proceed. End the loop.
*   **`FAIL`**: Write the verifier's Next Brief verbatim to the next
    `briefs/brief-<NNN>.md` file. Tell the student in one or two sentences what
    failed and that a new attempt is starting, then return to Step 2 with the
    new brief. The student can stop the loop at any time.
