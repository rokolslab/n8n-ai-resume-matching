# Setup

## Requirements

- n8n with Gmail, Google Sheets, Extract From File, and LangChain/OpenAI nodes
- Gmail OAuth2 credential
- Google Sheets OAuth2 credential
- OpenAI credential

## Google Sheets

Create one spreadsheet with two sheets.

### Vacancies

Recommended columns:

```text
title
description
requirements
salary
```

### Candidates

Recommended columns:

```text
sender_email
full_name
position
skills
experience
city
country
salary
matching_result
processed_at
raw_matching_json
```

The public workflow uses `REPLACE_WITH_GOOGLE_SHEET_ID`. Select your spreadsheet again in each Google Sheets node after import.

## Gmail

Connect a test Gmail account to the Gmail Trigger. The trigger filters unread mail with PDF attachments. Use synthetic/demo resumes for verification.

## OpenAI

Reconnect the extraction and matching model nodes. If the model names in the export are unavailable in your n8n/OpenAI environment, select suitable current models and re-run the regression tests.

## Human review

The public portfolio variant ends in review recommendations rather than autonomous candidate-facing email actions. If you add outbound email, place an explicit human approval step before it.
