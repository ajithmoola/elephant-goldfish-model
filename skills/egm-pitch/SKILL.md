---
name: egm-pitch
license: Apache-2.0
disable-model-invocation: true
description: >-
  First step of the EGM research flow. Sharpens a graduate student's rough idea
  into a thesis through open-ended questions, runs a quick prototype through
  egm-build to check for a real effect, and starts the story doc. Goldfish
  reviewers check that a stranger can say what the thesis is and why it
  matters.
---

# EGM Pitch: Thesis and Prototype

When running in Codex or ChatGPT Work, first read [Codex integration](../../docs/codex.md).
In Claude Code or Cowork, read `${CLAUDE_PLUGIN_ROOT}/docs/claude.md` first.

## Overview

The research flow treats a paper or thesis as a story with the thesis as its
spine. This skill produces the first version of that spine. It creates the
story doc (`story_doc.md` in the working directory), which holds everything the
tool knows about the project from here on. Later steps are `egm-literature`,
`egm-story`, `egm-build`, and `egm-screen`.

## Execution Constraints (CRITICAL)

1.  **No Hidden Knowledge**: Everything agreed with the student goes into the
    story doc. Do not rely on chat history in later steps.
2.  **Student Owns the Thesis**: Propose wording; the student decides.
3.  **Default to Stress-Testing**: Find the weakest points of the idea before
    affirming anything. Do not flatter.
4.  **Open-Ended Questions**: Ask plain-text questions, one or two per turn.
    Do not offer pre-written multiple-choice answers.
5.  **Never Invent Results**: Prototype results come only from `egm-build`
    and are cited by run ID.
6.  **Plain-English Standard**: Write in simple, direct language.

## Workflow

### Step 1: Create the Story Doc

If `story_doc.md` does not exist, create it with the section headings from
`resources/document_standards/story-doc-standard.md`: Thesis, Literature,
Storyboard, Log, Run Log. If it exists, read it and continue from its contents.

### Step 2: Sharpen the Thesis

Question the student until these are clear, using the questions in
`resources/document_standards/heilmeier-standard.md` as a guide:

*   What is the claim, in a sentence or two?
*   What result would show it is wrong?
*   Why does it matter, and to whom?
*   What is the core method idea?

Write the working thesis to the Thesis section and give the student the path.
Revise until they approve it.

### Step 3: Prototype

When the student wants to check the idea with code, read
`${CLAUDE_PLUGIN_ROOT}/skills/egm-build/SKILL.md` and follow it in this thread
with `Task: prototype`. After each result, revisit the thesis with the student:
the prototype and the thesis shape each other. Record each change in the Log.

### Step 4: Goldfish Review

When the student says the thesis is ready, read
`${CLAUDE_PLUGIN_ROOT}/skills/egm-review-loop/SKILL.md` and follow it in this
thread with:

*   `TargetFile`: the story doc.
*   `SemanticTarget`: the Thesis section.
*   `Reviewers`: `comprehension`, `research_critic`.
*   `Standards`: `heilmeier-standard` for both.
*   `GoldfishContext`: none beyond files the story doc references.

The question for reviewers: can a stranger say what the thesis is and why it
matters, from the Thesis section alone?

### Step 5: Hand-off

Tell the student the pitch is complete and the next step is `egm-literature`.
