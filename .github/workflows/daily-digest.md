---
name: Daily Digest

on:
  schedule: daily on weekdays
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

safe-outputs:
  create-issue:
    max: 1
---

# Daily Digest

Every weekday, create a GitHub issue that summarises all open issues
and pull requests in this repository.

Group them by label.

Include the total count, the title, the author, and how long each item
has been open.

Title the issue "Daily Digest – <date>".

If there are no open issues or pull requests, indicate that clearly
in the issue.