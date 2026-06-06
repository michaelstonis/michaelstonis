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

## Seasonal Theme Reference

Apply the following theme whenever the season has changed since the last README update. The typing SVG URL contains a `color=` query parameter and the header line ends with an emoji — both must be updated together.

| Season      | Typing SVG color | Header emoji | Section emoji (🛠️→) | Section emoji (🔭→) | Section emoji (⌨→) |
|-------------|-----------------|--------------|----------------------|----------------------|----------------------|
| Winter      | `A8D8EA`        | ❄️           | 🧊                   | 🌨️                  | 📖                  |
| Spring      | `78C27E`        | 🌱           | 🌿                   | 🌸                   | ✏️                  |
| Summer      | `FFA500`        | ☀️           | 🔥                   | 🏖️                  | 📝                  |
| Fall/Autumn | `D2691E`        | 🍂           | 🛠️                   | 🔭                   | ⌨                  |

**Typing SVG color rule**: Replace the hex value in `color=XXXXXX` within the `readme-typing-svg.herokuapp.com` URL with the season's value (no `#` prefix — the URL already omits it).

**Header emoji rule**: The first line of `README.md` ends with an emoji. Replace it with the season's header emoji.

**Section emoji rule**: The `### 🛠️ Tech Stack & Tools`, `### 🔭 Current Projects & Highlights`, and `### ⌨ Blog` headings each carry an emoji. Replace them according to the table above for the current season.

Only apply theme changes when the season differs from what is currently in the README (detect by checking the current `color=` value or header emoji). If the season matches, skip theme updates.

## Task

Research recent activity and produce an updated `README.md` file. Follow these steps:

### Step 1: Read the current README

Read the current contents of `README.md` from the repository root.

### Step 2: Determine the current season and whether a theme change is needed

Run `date` in bash to get the current month and derive the season. Compare the current `color=` value in the typing SVG URL against the expected value for this season. If they differ, a seasonal theme update is required.

### Step 3: Gather GitHub activity

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

### Step 4: Check for new blog posts

Use the `web_fetch` tool to visit **https://ston.is** and extract:
- The titles and URLs of the most recent 3–5 blog posts
- Publication dates if visible
- Topics covered

### Step 5: Determine what has changed

Assess the following:
- **Season changed** → apply the seasonal theme (color, header emoji, section emojis) per the Seasonal Theme Reference above
- **New repos created** in the last 14 days → mention them in Current Projects
- **Repos with significant recent activity** (pushed within 7 days) → mention or update highlights
- **New blog posts** since the last README update → update the Blog section to reference recent posts
- **Active focus areas** (inferred from repo languages, topics, and recent pushes) → update the Tech Stack badges and intro paragraph if the focus has shifted

### Step 6: Update README.md

Apply targeted updates to `README.md`. Keep Michael's voice — enthusiastic, professional, and developer-focused. Make the smallest changes that reflect reality:

1. **Seasonal theme**: If the season has changed, update the typing SVG `color=` value, the header emoji, and the three section emojis as defined in the Seasonal Theme Reference. Do not change any other part of the typing SVG URL.

2. **Header paragraph**: Refresh if the current season or active work focus has shifted meaningfully. Keep it concise.

3. **Tech Stack & Tools**: Add or remove badges only if there is clear evidence of a technology shift from the GitHub activity (e.g., a new language or framework appearing prominently in recent repos). Do not remove established core technologies (.NET, C#, MAUI, Azure).

4. **Current Projects & Highlights**: Update the bullet list to reflect:
   - Any newly created public repos (add them)
   - Any existing projects with notable recent updates (refresh descriptions)
   - Remove or de-emphasize projects with no activity in 90+ days if a newer project replaces them

5. **Blog section**: If new blog posts are found, mention the most recent one or two by title and link. Change "I occasionally write" to reflect actual recent cadence if posts are frequent.

6. **No-op**: If nothing significant has changed (no new repos, no new blog posts, no seasonal theme change, no tech focus change), call `noop` with a brief explanation and do not create a PR.

## Output Format

- Write the complete updated `README.md` content as a git patch targeting the file `README.md` in the repository root.
- The PR title should summarize the most significant change (e.g., "Summer theme + new blog post on MVVM").
- Keep all existing badge image URLs and social links intact unless they are factually wrong.
- Use GitHub-flavored markdown.
- Preserve the overall structure: header, typing SVG, intro paragraph, tech stack, current projects, blog, social links.

## Safe Outputs

- Use `create-pull-request` to submit the updated README as a PR for review.
- Use `noop` with a short explanation when no meaningful update is warranted.
