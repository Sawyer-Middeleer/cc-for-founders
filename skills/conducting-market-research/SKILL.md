---
name: conducting-market-research
description: Conducts market research on a product or topic using Reddit, Twitter/X, and YouTube. Fetches product info, researches competitive landscape, searches subreddits for pain points, analyzes Twitter volume trends, and extracts YouTube comments for sentiment. Use when researching positioning, validating problems, or gathering voice-of-customer data. Trigger phrases include "market research", "find pain points", "voice of customer", "reddit research".
---

# Market Research Skill

Researches a product/topic by mining Reddit, Twitter/X, and YouTube for authentic pain points, competitive intelligence, and user language.

## Prerequisites

This skill requires MCP tools for social platform access:
- **Reddit** - to search subreddits for pain points and discussions
- **Twitter/X** - to analyze volume trends and sentiment
- **YouTube** - to search videos and extract comment sentiment

If you don't have these connected, visit [rube.app](https://rube.app/) to find and connect social media MCPs. Look for Reddit, Twitter, and YouTube integrations.

When invoked, first check what tools are available via `RUBE_SEARCH_TOOLS`. If social platform tools are missing, inform the user:

> "I need Reddit, Twitter, and YouTube MCP tools to conduct full market research. You can connect these at rube.app. I can still do web research and competitive analysis without them, but won't be able to mine social platforms for voice-of-customer data."

## Input

Either:
- **Product URL**: Fetches and analyzes the product to understand what it does
- **Topic description**: Brief description of the product/problem space

## Workflow

### Phase 1: Product Discovery

1. If URL provided, fetch with WebFetch to extract:
   - What the product does
   - Target audience
   - Core value propositions
   - Problems it claims to solve

2. Identify initial keyword themes based on:
   - Pain points the product addresses
   - Tools it might replace
   - Target audience descriptors

### Phase 2: Competitive Landscape (Web Research)

Use `WebSearch` to research the competitive landscape before social listening.

**Searches to run:**
- `[product name] competitors`
- `[product name] vs alternatives`
- `[product category] market landscape 2025`
- `best [product category] tools`

**Extract from results:**
- Direct competitors (3-5 names)
- Adjacent tools in the space
- Industry terminology/jargon
- Key influencers or thought leaders

**Output:** Competitor list with positioning notes

| Competitor | Positioning | Differentiator |
|------------|-------------|----------------|
| [name] | [how they position] | [key difference] |

This informs Twitter and Reddit searches with:
- Competitor names to monitor
- Industry keywords
- Influencer accounts to check

### Phase 3: Twitter/X Search & Volume Trends

Use `RUBE_SEARCH_TOOLS` then `RUBE_MULTI_EXECUTE_TOOL` with Twitter tools.

**Step 1: Volume trends** (use `TWITTER_RECENT_SEARCH_COUNTS` or `TWITTER_FULL_ARCHIVE_SEARCH_COUNTS`)

Check mention volume for:
- Product name
- Each competitor name
- Category keywords

```
query: "[keyword]"
granularity: "day"
start_time: [7-30 days ago]
```

**Step 2: Keyword + competitor search** (use `TWITTER_RECENT_SEARCH`)

Run parallel searches:
```
query: "[product] OR [competitor1] OR [competitor2] -is:retweet"
query: "[pain point keyword] -is:retweet"
query: "from:[industry influencer]"
tweet_fields: ["created_at", "public_metrics", "text"]
expansions: ["author_id"]
max_results: 50
```

**Step 3: Extract insights**

Process in `RUBE_REMOTE_WORKBENCH`:
- Compare volume trends (who's talked about more?)
- Identify high-engagement tweets about pain points
- Note sentiment patterns (complaints vs praise)
- Extract specific language people use

**Output:**

| Term | 7-day Volume | Trend | Notes |
|------|--------------|-------|-------|
| [product] | [count] | up/down/flat | |
| [competitor] | [count] | up/down/flat | |

**Notable tweets:**
> "[tweet text]" — [@handle](url) | [likes] likes

### Phase 4: YouTube Search & Comment Analysis

Use `RUBE_SEARCH_TOOLS` then `RUBE_MULTI_EXECUTE_TOOL` with YouTube tools.

**Step 1: Find relevant videos** (use `YOUTUBE_SEARCH_YOU_TUBE`)

Search for:
- `[product name] review`
- `[product name] tutorial`
- `[product name] vs [competitor]`
- `[problem/pain point] solution`
- `best [product category] tools`

```
q: "[search term]"
type: "video"
maxResults: 10
```

**Step 2: Pull comments** (use `YOUTUBE_LIST_COMMENT_THREADS`)

For top 3-5 most relevant videos:
```
videoId: "[video_id]"
maxResults: 50
order: "relevance"
```

**Step 3: Extract insights**

Process in `RUBE_REMOTE_WORKBENCH`:
- Complaints about product/competitors
- Feature requests ("I wish it could...")
- Praise (what people love)
- Questions (gaps in understanding)
- Comparisons to alternatives

**Output:**

| Video | Channel | Views | Comment Themes |
|-------|---------|-------|----------------|
| [title](url) | [channel] | [n] | [key themes] |

**Notable comments:**
> "[comment text]" — [video title](url)

### Phase 5: Subreddit Search

Use `RUBE_SEARCH_TOOLS` then `RUBE_MULTI_EXECUTE_TOOL` with `REDDIT_SEARCH_ACROSS_SUBREDDITS`.

Run 3-5 parallel searches with pain-focused queries:
- `[problem] frustrating annoying`
- `[competitor tool] hate limitations`
- `[workflow] tedious repetitive`
- `[audience] struggling with`

Search parameters:
```
limit: 20-25 per query
sort: relevance
```

Relevant subreddit categories to consider:
- Product-specific: r/Slack, r/Notion, r/ChatGPT, etc.
- Audience-specific: r/startups, r/productivity, r/sysadmin, etc.
- Problem-specific: r/automation, r/remotework, etc.

### Phase 6: Quote Extraction

Process results in `RUBE_REMOTE_WORKBENCH`:

1. **Deduplicate** posts by ID
2. **Filter** for relevance:
   - Exclude off-topic subreddits
   - Require minimum engagement (3+ comments or 5+ score)
   - Match pain point keywords
3. **Extract** verbatim quotes preserving:
   - Exact wording (human voice)
   - Subreddit source
   - Post URL for citation
4. **Rank** by authenticity signals:
   - Specific actions described (e.g., "cmd+c, cmd+tab")
   - Named tools (Slack, ChatGPT, Notion)
   - Frustrated/honest tone
   - Questions people ask themselves

### Phase 7: Output Generation

Synthesize all findings into a **cohesive narrative report**. Do NOT list data source-by-source. Instead, weave insights from Twitter, YouTube, Reddit, and web research together to tell a unified story about the market.

## Output Format

The report should read as a single analytical document, not a data dump. Structure:

```markdown
# [Product/Topic] Market Research

## What is [Topic]?
Brief definition and context (1-2 sentences). Establish why this matters.

## Market Opportunity
Synthesize the core tension or gap in the market. Draw from ALL sources:
- What's broken about current solutions? (Reddit complaints + Twitter sentiment)
- What are people searching for? (YouTube content themes)
- Where are incumbents vulnerable? (Web research + user frustration)

## Competitive Landscape
| Player | Position | Vulnerability |
|--------|----------|---------------|
| [name] | [position] | [weakness revealed by research] |

Weave in Twitter volume data as supporting evidence (e.g., "Twitter volume reflects this: [category] generates X tweets/week while [specific tool] has minimal organic discussion").

## Core Pain Points
Organize by theme, NOT by source. Each pain point should:
- State the problem clearly
- Include severity (High/Med/Low)
- Cite evidence from multiple sources where possible
- Include 1-2 verbatim quotes inline

Example:
**1. [Pain Point Name] (High Severity)**
[Explanation of the problem]. Users describe it as "[verbatim quote]" ([source](url)). This is echoed in YouTube comments requesting [theme] and Twitter discussions about [related topic].

## Voice of Customer
Synthesize the language patterns across sources:
- Common phrases and terminology
- Specific tools/actions mentioned
- Emotional tone (frustration, confusion, hope)
- What to AVOID in messaging (marketing-speak that triggers skepticism)

## Where to Find This Audience
| Community | Why It Matters |
|-----------|----------------|
| [subreddit/channel/hashtag] | [engagement level + what makes it valuable] |

## Strategic Implications
3-5 actionable takeaways that synthesize all findings:
1. [Positioning recommendation based on competitive gaps]
2. [Messaging recommendation based on voice of customer]
3. [Channel recommendation based on audience location]
```

## Synthesis Principles

**DO:**
- Lead with insights, cite sources as evidence
- Connect patterns across platforms (e.g., "Reddit users complain about X, which explains why YouTube videos about Y get high engagement")
- Use tables sparingly for structured data (competitors, communities)
- Embed quotes naturally within analysis
- End with actionable strategic implications

**DON'T:**
- List "Twitter findings" then "Reddit findings" then "YouTube findings"
- Include raw data without interpretation
- Repeat the same insight multiple times from different sources
- Use section headers like "Twitter/X Volume" or "YouTube Insights" as standalone sections

## Quality Criteria

**Good quotes have:**
- Specific actions (`cmd+c`, `cmd+tab`, `switching between 5 apps`)
- Named tools (Slack, ChatGPT, Notion, Gmail)
- Frustrated/honest tone
- Real scenarios (`searching for @dave in Slack`)

**Good synthesis:**
- Connects dots across sources (Reddit complaint → YouTube search trend → Twitter volume)
- Identifies patterns, not just lists data points
- Leads with the "so what" before the evidence
- Uses quotes as proof points within narrative, not as standalone sections

**Avoid:**
- Generic complaints without specifics
- Marketing-speak or polished language
- Posts that are actually ads/promotions
- Low-engagement content (likely not resonant)
- Source-by-source reporting ("Here's what Twitter said... Here's what Reddit said...")
- Data dumps without interpretation

## Example Queries by Product Type

**Team collaboration tool:**
- `team knowledge lost slack threads`
- `copy paste ChatGPT workflow frustrating`
- `wiki documentation nobody reads`

**Productivity app:**
- `too many apps context switching`
- `overwhelmed tools notifications`
- `brain full before day starts`

**Developer tool:**
- `debugging workflow tedious`
- `switching between IDE terminal browser`
- `documentation outdated stale`
