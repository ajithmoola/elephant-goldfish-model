# Story Doc Standard

This standard defines the quality bar for the story doc: the single document
that holds everything the research tool knows about a graduate student's
computational research project.

## Core Principle

A story doc passes if a fresh reader (or fresh AI session) with no conversation
history can read it alone and understand the thesis, how it relates to prior
work, what each planned figure must show, what has been tried, and what every
run produced. Nothing the tool relies on may exist only in an agent's memory.

## Required Sections

The story doc must contain these sections, in this order:

*   **Thesis**: The core claim in a few sentences, why it matters, and what the
    reader should believe by the end of the paper or thesis.
*   **Literature**: The related work the thesis builds on or differs from. Each
    entry names a real source (authors, title, venue or URL, year) and says in
    one or two sentences how it relates to the thesis.
*   **Storyboard**: One slot per planned figure, covering both method figures
    and result figures. Each slot states:
    *   What the figure must show.
    *   What the reader should learn from it.
    *   Status: `planned`, `preliminary`, `in progress`, or `final`.
    *   The current method or result behind it, with run log IDs for any
        result.
*   **Log**: Dated entries recording what was tried, what happened, what
    changed, and why. Pivots record that the student made the decision.
*   **Run Log**: One entry per run, including failed and abandoned runs. See
    Run Log Entries below.

## Run Log Entries

Each run log entry must state:

*   **Run ID**: A unique, stable identifier (for example `R-0042`).
*   **Date**.
*   **Purpose**: Which brief or storyboard slot the run serves.
*   **Code**: The commit hash, or the files and their state if uncommitted.
*   **Command**: The exact command that was run.
*   **Inputs**: Data files, parameters, and random seeds.
*   **Outputs**: Paths to output files (data, logs, figures).
*   **Outcome**: `completed`, `failed`, or `abandoned`, with the error or
    reason if not completed.

Run log entries are append-only. An entry is never edited or deleted after it
is written, except to add a note that a later run superseded it.

## Formatting Rules

*   Write in plain, direct language. Define every term that a reader outside
    the student's subfield would not know, or link to a definition.
*   Every result mentioned outside the run log must cite at least one run ID.
*   Every claim about prior work must cite an entry in the Literature section.
