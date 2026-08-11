# Testing

## Original end-to-end verification

The source implementation was run through the real Gmail-triggered flow with Gmail, Google Sheets, and OpenAI credentials connected. The source repository records a successful execution in `lesson-2.8/execution history.png` and includes supporting screenshots for Gmail Trigger, candidate extraction, and the full workflow.

The source workflow also passed node/workflow validation and connection checks.

## Regression test after import

1. Send a synthetic PDF resume to the connected Gmail account.
2. Confirm Gmail Trigger receives the unread message and attachment.
3. Confirm the duplicate lookup runs before PDF/AI processing.
4. Confirm PDF text extraction succeeds.
5. Confirm structured candidate extraction contains only explicitly present data.
6. Confirm vacancies load from Google Sheets.
7. Confirm matching returns structured JSON with scores, reasons, and risks.
8. Confirm a candidate row is appended to `Candidates`.
9. Confirm the workflow selects the expected review-recommendation branch.

## Duplicate path

Repeat the same candidate email after adding it to `Candidates`.

Expected: the duplicate branch stops processing before PDF extraction and AI calls.

## Safety checks

Verify that sex, birth date, race/ethnicity, religion, disability, and other protected/irrelevant characteristics are not used in the public matching schema or prompt.
