---
name: egm-screen
license: Apache-2.0
disable-model-invocation: true
description: >-
  Final step of the EGM research flow. Fresh Goldfish readers screen the
  graduate student's draft paper or thesis: does the story land, does it match
  the thesis, and does every claim and citation trace to the run log or a real
  source. Writes a screening report; never edits the draft.
---

# EGM Screen: Draft Screening

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

The student writes the paper or thesis. This skill screens their draft against
the story doc with fresh reviewers and writes the findings to
`screening_report.md` in the working directory.

## Delegation

A Goldfish runs in a fresh context with no conversation history. Spawn each
subagent with your harness's subagent tool (Antigravity: `invoke_subagent`; Claude Code: `Agent`; Codex: `spawn_agent`). Each subagent is defined in `agents/<file>.md`, and its
frontmatter `name` (for example `egm-comprehension`) is the subagent type to request
where your harness registers named subagents. Otherwise spawn a general-purpose
subagent and tell it to read that persona file first. Pass the prompt verbatim
and wait for the report.
If your harness has no subagent tool, stop and tell the user: a review in the same context is not a Goldfish review.

## Execution Constraints (CRITICAL)

1.  **Never Edit the Draft**: The student writes the paper. Report findings
    only.
2.  **Never Smooth Over Findings**: Report every evidence finding at the
    severity the reviewer gave it.
3.  **Plain-English Standard**: Write the report in simple, direct language.

## Inputs

*   **`Draft`**: Path to the student's draft (any text format the reviewers
    can read, such as LaTeX or Markdown).
*   **`StoryDoc`**: Path to the story doc.

## Workflow

### Step 1: Dispatch Reviewers

Spawn these reviewers in parallel, passing absolute paths:

*   **`comprehension`** (`agents/comprehension.md`) with `Mode: REVIEW` on the
    draft only, against `resources/document_standards/re-implementation-standard.md`.
    Also ask it to state, in three sentences, what a reader takes away from the
    draft. Do not give it the story doc.
*   **`research_critic`** (`agents/research_critic.md`) on the draft and the
    story doc, against `resources/document_standards/story-doc-standard.md`.
    Ask whether the draft's story matches the thesis and storyboard.
*   **`result_verifier`** (`agents/result_verifier.md`) with `Mode: SCREEN` on
    the draft and the story doc, against
    `resources/document_standards/evidence-standard.md`.
*   **`literature_checker`** (`agents/literature_checker.md`) on the draft's
    citations, against `resources/document_standards/evidence-standard.md`.

### Step 2: Write the Report

Write `screening_report.md` with:

1.  **Reader Takeaway**: The comprehension reviewer's three sentences, next to
    the thesis from the story doc.
2.  **Evidence Findings**: Result verifier and literature checker findings,
    Blockers first.
3.  **Story Findings**: Research critic and comprehension findings, Blockers
    first.

### Step 3: Notify the Student

Give the student the report path and the number of findings by severity.
