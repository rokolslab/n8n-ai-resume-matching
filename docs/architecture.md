# Architecture

The workflow separates deterministic intake/validation steps from probabilistic AI steps.

1. Gmail Trigger receives unread emails with PDF attachments.
2. Email metadata is normalized while binary data is preserved.
3. Existing candidates are checked by sender email before AI processing.
4. New resumes are converted from PDF to text.
5. Candidate data is extracted into a fixed schema.
6. Vacancies are read from Google Sheets and aggregated.
7. An AI matching stage scores professional fit and returns structured JSON.
8. The result is persisted to Google Sheets.
9. The workflow produces a decision-support recommendation branch for human review.

The public portfolio variant intentionally removes sex and birth date from the matching data model and does not send autonomous invite/rejection emails.
