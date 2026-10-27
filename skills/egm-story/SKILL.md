---
name: egm-story
license: Apache-2.0
disable-model-invocation: true
description: >-
  EGM research flow script and storyboard step. Writes the detailed thesis and
  plans every method and result figure in the story doc, built on the
  prototype's preliminary methods and results. Rerun during production to
  update the storyboard as methods and results change. Goldfish reviewers
  check that a reader would understand the paper from the figure plan alone.
---

# EGM Story: Script and Storyboard

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

This skill writes the detailed thesis (the script) and the figure plan (the
storyboard) in the story doc. The storyboard is a living section: during
production, the student alternates between `egm-build` (improve methods, get
results) and this skill (update the storyboard to match).

## Execution Constraints (CRITICAL)

1.  **Student Owns the Story**: Propose changes to the thesis and storyboard;
    the student approves them. Pivots are the student's decision. Record each
    approved change in the Log.
2.  **Never Invent Results**: A storyboard slot cites results only by run ID
    from the Run Log. A slot with no run yet has status `planned`.
3.  **No Hidden Knowledge**: Everything agreed with the student goes into the
    story doc.
4.  **Default to Stress-Testing**: Point out slots that do not serve the thesis
    and parts of the thesis with no slot. Do not flatter.
5.  **Plain-English Standard**: Write in simple, direct language.

## Workflow

### Step 1: Write the Script

Read the story doc. With the student, expand the Thesis section: the full
claim, why it matters, the method in outline, and what the reader should
believe by the end. Give the student the path and revise until they approve.

### Step 2: Write or Update the Storyboard

With the student, write one slot per planned figure, method figures and result
figures alike, under `resources/document_standards/story-doc-standard.md`.
Start from the prototype's preliminary methods and results, citing run IDs.
Order the slots the way a reader will meet them.

On later runs of this skill, update slots to match the latest Run Log and Log
entries: change status, cite new run IDs, and propose adding, removing, or
changing slots where methods or results have changed.

### Step 3: Goldfish Review

Read `${CLAUDE_PLUGIN_ROOT}/skills/egm-review-loop/SKILL.md` and follow it in
this thread with:

*   `TargetFile`: the story doc.
*   `SemanticTarget`: the Thesis and Storyboard sections.
*   `Reviewers`: `research_critic`, `comprehension`, `style`.
*   `Standards`: `story-doc-standard` for all three.
*   `GoldfishContext`: none beyond files the story doc references.

The question for reviewers: from the thesis and figure plan alone, would a
reader understand the paper and learn what each figure promises?

### Step 4: Hand-off

Tell the student which slots are `planned` or `in progress`, and that
`egm-build` is the next step for each.
