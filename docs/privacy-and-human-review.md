# Privacy & Human Review

Resume processing involves personal data. This portfolio project is a technical demonstration, not a recommendation to automate final employment decisions.

## Data minimization

The public workflow excludes `sex` and `birth_date` from the matching data model. Matching should focus on job-relevant information such as skills, experience, role, location/work-mode constraints, and explicitly stated salary expectations.

## Human review

A model score must not be treated as the hiring decision. Use it to prioritize manual review. Before sending interview invitations, rejections, or making employment decisions, require a human reviewer to inspect the candidate data and matching reasons.

## Production controls

Define retention and deletion periods, restrict access to Gmail and candidate records, avoid storing unnecessary raw resume text, log changes to matching criteria/prompts, and test for systematic bias before using the workflow with real applicants.
