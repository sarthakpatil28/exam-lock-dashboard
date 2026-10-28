# EXAMLOCK --- Zero-Trust Time-Lock Exam Distribution

> **Hackathon prototype. Simulation only.** This repository currently
> demonstrates a static dashboard that reads sample JSONL event files.
> It does not, by itself, implement cryptographic key release,
> encryption enforcement, print-node authentication, or tamper-proof
> audit storage.

## Project overview

EXAMLOCK explores a controlled exam-distribution workflow:

1.  Prepare an exam bundle.
2.  Record a three-share authorization ceremony.
3.  Wait for the authorized release time.
4.  Display a release QR and record print-node verification.
5.  Record printing, exceptions, and recovery.
6.  Compare a suspected leak with documented paper-variant records as an
    investigative aid.

The goal is to make distribution events easier to observe and
investigate while reducing premature access opportunities.

## Current prototype features

-   Static, responsive dashboard hosted with GitHub Pages.
-   Normal workflow scenario loaded from `sample_events.jsonl`.
-   Failure-and-recovery scenario loaded from `failure_events.jsonl`.
-   Event timeline with timestamps, stages, status, centre identifier,
    and details.
-   Summary cards for bundle, release, print workflow, and event count.
-   Illustrative forensic-trace panel.
-   Bilingual 40-question mock paper created for demonstration use.

## Repository files

``` text
exam-lock-dashboard/
├── index.html
├── sample_events.jsonl
├── failure_events.jsonl
├── README.md
└── documents/
    └── EXAMLOCK_Mock_Bilingual_40Q_Paper.docx
```

The `documents/` folder is optional unless you choose to keep the mock
paper in this repository. Avoid publishing the answer key alongside any
real exam material.

## Run the dashboard locally

Because the dashboard uses `fetch()` to load JSONL files, serve the
folder over HTTP rather than opening `index.html` directly from the
filesystem.

### Windows PowerShell

``` powershell
cd D:\Exam-Lock
py -m http.server 8000
```

Then open:

-   Dashboard: http://localhost:8000/
-   Normal sample log: http://localhost:8000/sample_events.jsonl
-   Failure sample log: http://localhost:8000/failure_events.jsonl

Stop the local server with `Ctrl+C`.

If `py` is unavailable, try `python -m http.server 8000`.

## Publish with GitHub Pages

1.  Commit and push the files to the repository's `main` branch.
2.  Open repository **Settings → Pages**.
3.  Select **Deploy from a branch**, choose `main` and `/(root)`, then
    save.
4.  Wait for the Pages deployment to finish.
5.  Test the deployed site:
    https://sarthakpatil28.github.io/exam-lock-dashboard/

The two `.jsonl` files must be in the same published root as
`index.html` because the dashboard requests them using relative paths.

## Event log format

Each line in a `.jsonl` file is one independent JSON object. Required
fields are:

-   `time`: ISO-like timestamp string used for ordering and display.
-   `stage`: workflow stage such as `bundle_created`, `release`,
    `printing`, `jam`, or `complete`.
-   `status`: typically `ok`, `warning`, or `error`.
-   `details`: human-readable explanation.
-   `center_id`: optional centre identifier.

Example:

``` json
{"time":"2026-10-28T09:00:00","stage":"bundle_created","center_id":"CTR-042","status":"ok","details":"Demo bundle created"}
```

Keep each event on a single line. Do not put commas between separate
lines. Use fictional data in this public demo.

## Pre-demo testing checklist

-   [ ] Open the live dashboard and hard-refresh with `Ctrl+F5`.
-   [ ] Click **Run Demo Scenario**.
-   [ ] Confirm the timeline loads events from `sample_events.jsonl`.
-   [ ] Confirm the normal scenario shows release and completion events.
-   [ ] Click **Failure & Recovery**.
-   [ ] Confirm the unauthorized-node event appears as an error.
-   [ ] Confirm the printer-jam event appears as a warning.
-   [ ] Confirm the resume and completion events appear.
-   [ ] Click **Reset**, then confirm the dashboard returns to its
    initial state.
-   [ ] Open each JSONL URL directly to confirm it is published.
-   [ ] Check the browser console if a fetch or parsing error occurs.
-   [ ] Make sure all displayed records are described as simulated.
-   [ ] Rehearse the 3-minute demo and prepare a backup screen
    recording.

## Troubleshooting

### A scenario shows HTTP 404

Check that the matching JSONL file is committed to `main`, is in the
repository root, and has the exact lowercase filename.

### The page loads but the timeline does not

Use a hard refresh (`Ctrl+F5`). Open the JSONL URL directly. Confirm
each non-empty line is valid JSON and includes `time`, `stage`, and
`status`.

### The dashboard works locally but not on GitHub Pages

Make sure all three files are published in the same folder and use
relative paths. Check the repository's **Actions** tab for the Pages
build status.

## Security, privacy, and limitations

-   Sample events are static, public, and editable; they are not a
    secure audit ledger.
-   A `key_ceremony` event does not prove that threshold cryptography or
    secure key custody exists.
-   A `node_scanned` event does not prove that a print node was
    cryptographically authenticated.
-   A release event in the log does not enforce a time lock.
-   The progress bar is illustrative and inferred from event types; it
    is not a measurement of physical print output.
-   A paper-variant match may help narrow a source batch but does not
    independently identify the person who leaked a paper or prove
    misconduct.
-   Do not commit real exam papers, answer keys, encryption keys,
    credentials, candidate names, seat assignments, or other
    confidential data to a public repository.

## Roadmap

1.  Define a documented threat model and trust boundaries.
2.  Implement and test actual bundle encryption and key management.
3.  Enforce release policy server-side and validate time sources.
4.  Implement cryptographic print-node identity and signed release
    tokens.
5.  Store audit records in a tamper-evident backend.
6.  Design privacy-aware paper variants and evaluate trace reliability.
7.  Add automated tests, access controls, and operational recovery
    procedures.

## Demo narration outline

1.  Explain the problem and proposed workflow.
2.  Run the normal scenario and follow the event timeline.
3.  Run the failure-and-recovery scenario.
4.  Explain the fictional variant register and its limitations.
5.  Close by separating implemented prototype features from future
    security engineering.

------------------------------------------------------------------------

**Project:** EXAMLOCK\
**Dashboard:** https://sarthakpatil28.github.io/exam-lock-dashboard/\
**Status:** Hackathon prototype with simulated event data
