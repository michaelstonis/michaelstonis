---
emoji: 📝
description: Weekly update to README.md based on recent GitHub activity, new blog posts, and seasonal/project changes — delivered as a PR.
on:
  schedule: weekly
permissions:
  contents: read
  issues: read
  pull-requests: read
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
  web-fetch: {}
network:
  allowed:
    - defaults
    - github
    - "ston.is"
safe-outputs:
  create-pull-request:
    title-prefix: "[weekly-update] "
    labels: [automated, readme]
    draft: false
    allowed-files:
      - "README.md"
---

# Weekly README Update

## Context

You are updating the `README.md` profile page for **michaelstonis** (Michael Stonis), a Mobile Solution Architect and Microsoft MVP based in the Chicago area. He is the President of Eight-Bot and the author of *Enterprise Application Patterns for .NET MAUI*. His blog is at https://ston.is and his GitHub username is `michaelstonis`.

The current date is available via `date` in bash. Use this to determine the current season (Northern Hemisphere):
- **Winter**: December, January, February
- **Spring**: March, April, May
- **Summer**: June, July, August
- **Fall/Autumn**: September, October, November

## Task

Research recent activity and produce an updated `README.md` file. Follow these steps:

### Step 1: Read the current README

Read the current contents of `README.md` from the repository root.

### Step 2: Gather GitHub activity

Use `gh` commands to gather information about **michaelstonis**:

1. **Recent public repository activity** — list public repos recently pushed to (last 7 days):
   ```
   gh api /users/michaelstonis/repos --jq '[.[] | select(.private == false) | {name, description, pushed_at, html_url, language, topics}] | sort_by(.pushed_at) | reverse | .[0:10]'
   ```

2. **New public repositories** — check for repos created in the last 14 days:
   ```
   gh api /users/michaelstonis/repos --jq '[.[] | select(.private == false) | {name, description, html_url, created_at, language}] | sort_by(.created_at) | reverse | .[0:5]'
   ```

3. **Organization repos at TheEightBot** — check for recent activity in TheEightBot org repos:
   ```
   gh api /orgs/TheEightBot/repos --jq '[.[] | select(.private == false) | {name, description, pushed_at, html_url, language}] | sort_by(.pushed_at) | reverse | .[0:5]'
   ```

### Step 3: Check for new blog posts

Use the `web_fetch` tool to visit **https://ston.is** and extract:
- The titles and URLs of the most recent 3–5 blog posts
- Publication dates if visible
- Topics covered

### Step 4: Determine what has changed

Assess the following:
- **New repos created** in the last 14 days → mention them in Current Projects
- **Repos with significant recent activity** (pushed within 7 days) → mention or update highlights
- **New blog posts** since the last README update → update the Blog section to reference recent posts
- **Current season** → adjust tone and any seasonal references in the intro paragraph
- **Active focus areas** (inferred from repo languages, topics, and recent pushes) → update the Tech Stack badges and intro paragraph if the focus has shifted

### Step 5: Update README.md

Apply targeted updates to `README.md`. Keep Michael's voice — enthusiastic, professional, and developer-focused. Make the smallest changes that reflect reality:

1. **Header paragraph**: Refresh if the current season or active work focus has shifted meaningfully. Keep it concise.

2. **Tech Stack & Tools**: Add or remove badges only if there is clear evidence of a technology shift from the GitHub activity (e.g., a new language or framework appearing prominently in recent repos). Do not remove established core technologies (.NET, C#, MAUI, Azure).

3. **Current Projects & Highlights**: Update the bullet list to reflect:
   - Any newly created public repos (add them)
   - Any existing projects with notable recent updates (refresh descriptions)
   - Remove or de-emphasize projects with no activity in 90+ days if a newer project replaces them

4. **Blog section**: If new blog posts are found, mention the most recent one or two by title and link. Change "I occasionally write" to reflect actual recent cadence if posts are frequent.

5. **No-op**: If nothing significant has changed (no new repos, no new blog posts, no seasonal shift, no tech focus change), call `noop` with a brief explanation and do not create a PR.

## Output Format

- Write the complete updated `README.md` content as a git patch targeting the file `README.md` in the repository root.
- The PR title should summarize the most significant change (e.g., "Add MauiNativePdfView, new blog post on MVVM").
- Keep all existing badge image URLs and social links intact unless they are factually wrong.
- Use GitHub-flavored markdown.
- Preserve the overall structure: header, typing SVG, intro paragraph, tech stack, current projects, blog, social links.

## Safe Outputs

- Use `create-pull-request` to submit the updated README as a PR for review.
- Use `noop` with a short explanation when no meaningful update is warranted.
