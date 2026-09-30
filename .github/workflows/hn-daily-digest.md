---
name: HN Daily Digest

on:
  schedule: daily on weekdays
  workflow_dispatch:

permissions:
  contents: read

engine:
  id: copilot
  model: gpt-4.1

network:
  allowed:
    - defaults
    - hacker-news.firebaseio.com

tools:
  bash: true

steps:
  - name: Fetch HN top stories
    run: |
      mkdir -p /tmp/gh-aw/agent
      for id in $(curl -s https://hacker-news.firebaseio.com/v0/topstories.json | jq -r '.[:30][]'); do
        curl -s "https://hacker-news.firebaseio.com/v0/item/$id.json"
      done | jq -s '[.[] | select(.score > 100) | {title, url, score, comments: .descendants}]' > /tmp/gh-aw/agent/hn.json

safe-outputs:
  create-issue:
    max: 1
---

# HN Daily Digest

Read the file /tmp/gh-aw/agent/hn.json.

It contains the top Hacker News stories with score above 100, including:
- title
- url
- score
- comments

Keep only stories about:
- software engineering
- cloud infrastructure
- AI/ML
- developer tooling
- distributed systems

Only include stories that are useful today for large-company developers.

Create one GitHub issue titled:

"HN Digest - <today's date>"

Format the results as a Markdown table with the columns:

| Title | URL | Score | Comments | Why it matters |

For "Why it matters", write one sentence explaining why the story is relevant to enterprise developers.

If no story qualifies, say so in the issue.