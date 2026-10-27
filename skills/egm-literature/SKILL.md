---
name: egm-literature
license: Apache-2.0
disable-model-invocation: true
description: >-
  EGM research flow literature step. Finds and summarizes related work with the
  graduate student, writes the story doc's Literature section, and positions
  the thesis against it. Goldfish reviewers check that every citation is real
  and that the thesis is new relative to the cited work.
---

# EGM Literature: Related Work

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

This skill fills the Literature section of the story doc. It runs after
`egm-pitch` and can be rerun at any time as the thesis changes.

## Execution Constraints (CRITICAL)

1.  **Never Invent Citations**: Cite only sources you found with a search or
    fetch tool in this session, or that the student supplied. Never write a
    citation from memory. Record the URL or identifier for each source.
2.  **No Hidden Knowledge**: Everything you learn about prior work goes into
    the Literature section.
3.  **Student Owns the Thesis**: If prior work undercuts the thesis, tell the
    student plainly and let them decide what changes.
4.  **Plain-English Standard**: Write in simple, direct language.

## Workflow

### Step 1: Gather Sources

1.  Read the story doc.
2.  Ask the student for papers they already know are relevant.
3.  Search for the closest prior work to the thesis and its core method. If
    you have no search or fetch tool, tell the student and work only from the
    sources they supply.

### Step 2: Write the Literature Section

For each source, write an entry under the story doc standard
(`resources/document_standards/story-doc-standard.md`): the source details,
the URL or identifier, and one or two sentences on how it relates to the
thesis. Then add a short paragraph stating how the thesis differs from the
closest work. Give the student the path and revise until they approve it.

If the literature changes the thesis, propose the change to the student and
record their decision in the Log.

### Step 3: Goldfish Review

Read `${CLAUDE_PLUGIN_ROOT}/skills/egm-review-loop/SKILL.md` and follow it in
this thread with:

*   `TargetFile`: the story doc.
*   `SemanticTarget`: the Thesis and Literature sections.
*   `Reviewers`: `literature_checker`, `research_critic`.
*   `Standards`: `evidence-standard` for `literature_checker`;
    `story-doc-standard` for `research_critic`.
*   `GoldfishContext`: none beyond files the story doc references.

The questions for reviewers: does every citation point to a real source that
says what it is cited for, and is the thesis new relative to the cited work?

### Step 4: Hand-off

Tell the student the literature step is complete and the next step is
`egm-story`.
