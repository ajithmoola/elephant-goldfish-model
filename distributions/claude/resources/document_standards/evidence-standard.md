# Evidence Standard

This standard defines the bar every result, claim, and citation must meet. It
applies to the story doc, to coding agent reports, and to the student's draft
paper or thesis.

## Core Principle

The tool never invents or smooths over results. A claim passes only if a fresh
reader can trace it to a real output in the run log, or to a real source in the
Literature section, and that output or source says what the claim says.

## Requirements

*   **Traceable Results**: Every reported number, figure, or qualitative result
    cites a run log entry. The cited entry lists output files that exist and
    contain that result.
*   **Complete Run Log**: Every run is recorded, including failed and abandoned
    runs. A result from a run that is not in the run log is not evidence.
*   **Faithful Reporting**: Reported results match the output files exactly.
    No rounding beyond what is stated, no dropped data points, no relabeling,
    no edited outputs.
*   **No Cherry-Picking**: When several runs address the same question, the
    claim either covers all of them or states which runs were excluded and why.
    Excluding runs because their results were worse is not a valid reason.
*   **Claim Matches Evidence**: The claim says no more than the evidence shows.
    A result on one dataset, parameter range, or setting is not claimed for
    others.
*   **Real Citations**: Every citation names a source that exists and can be
    found from the details given. The source says what it is cited for.
*   **Stated Gaps**: When evidence for a claim is missing or incomplete, the
    document says so instead of filling the gap.

## Violations

Reviewers must flag each of these as a Blocker:

*   A result, number, or figure with no run log entry behind it.
*   A result that differs from the output files it cites.
*   A citation that cannot be found or does not say what it is cited for.
*   Runs left out of the run log, or excluded from a claim without a stated
    reason.
*   Output files, data, or logs altered after a run to change its result.
