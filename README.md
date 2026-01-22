# Claude Code for Founders

A collection of Claude Code skills and subagents built for startup founders. These plugins help with the non-code parts of building a company—market research, sales prospecting, content creation, and product planning.

## What's Included

### Skills

Skills are structured workflows you can invoke for specific tasks.

| Skill | What It Does |
|-------|--------------|
| [**planning-feature**](skills/planning-feature/SKILL.md) | Guides you through structured feature planning: problem validation, solution exploration, UX concepts, and technical tradeoffs. Designed for non-technical founders who want to think through a feature before building. |
| [**conducting-market-research**](skills/conducting-market-research/SKILL.md) | Researches a product or topic by mining Reddit, Twitter/X, and YouTube for pain points, competitive intelligence, and voice-of-customer data. Outputs a synthesized report, not a data dump. |
| [**enriching-sales-contacts**](skills/enriching-sales-contacts/SKILL.md) | Batch enriches a Google Sheet of LinkedIn URLs with contact data (name, title, email, company) and research notes. Processes contacts in parallel for speed. |

### Subagents

Subagents are specialized agents that can be spawned for focused, autonomous work.

| Subagent | What It Does |
|----------|--------------|
| [**personal-voice-writer**](subagents/personal-voice-writer.md) | Writes content in a distinctive human voice, avoiding AI slop. Works out of the box with sensible defaults, but you can customize it to match your specific writing style. |
| [**writing-research-assistant**](subagents/writing-research-assistant.md) | Helps improve drafts by finding sources, verifying facts, and locating citations. Give it a draft with notes like "need source for this claim" and it finds authoritative evidence. |
| [**sales-contact-enricher**](subagents/sales-contact-enricher.md) | Enriches a single sales contact from a LinkedIn URL and writes the data to a Google Sheet row. Designed to be spawned in parallel by the enriching-sales-contacts skill. |

---

## Installation

### Option 1: Copy Individual Plugins

Copy the skills or subagents you want into your project's `.claude/` directory:

```bash
# From your project root
mkdir -p .claude/skills .claude/agents

# Copy a skill (entire directory)
cp -r path/to/cc-for-founders/skills/planning-feature .claude/skills/

# Copy a subagent
cp path/to/cc-for-founders/subagents/personal-voice-writer.md .claude/agents/
```

### Option 2: Clone the Whole Marketplace

Clone this repo and reference plugins as needed:

```bash
git clone https://github.com/your-org/cc-for-founders.git
```

Then copy plugins into your projects as you need them.

---

## Usage

### Using Skills

Skills are automatically available when placed in `.claude/skills/`. Invoke them by describing what you want:

- "Help me plan a new feature for user onboarding"
- "Do market research on AI writing assistants"
- "Enrich this spreadsheet of LinkedIn contacts"

Or use trigger phrases mentioned in each skill's description.

### Using Subagents

Subagents in `.claude/agents/` can be spawned by Claude Code when their expertise is needed. You can also request them directly:

- "Use the personal-voice-writer to rewrite this blog post"
- "Have the research assistant find sources for my claims"

---

## Prerequisites

Some plugins require external MCP (Model Context Protocol) tools to access third-party services. If a plugin needs tools you don't have, it will tell you.

### Finding MCP Tools

Visit [rube.app](https://rube.app/) to browse and connect MCP tools for:

- **Contact enrichment**: Fullenrich, Apollo, Clearbit
- **Social platforms**: Reddit, Twitter/X, YouTube
- **Productivity**: Google Sheets, Gmail, Slack
- **And many more**

### Which Plugins Need What

| Plugin | Required MCPs |
|--------|---------------|
| enriching-sales-contacts | Contact enrichment (Fullenrich, Apollo, etc.) + Google Sheets |
| sales-contact-enricher | Contact enrichment + Google Sheets |
| conducting-market-research | Reddit, Twitter/X, YouTube (optional—falls back to web research) |
| planning-feature | None (web search only) |
| personal-voice-writer | None |
| writing-research-assistant | None (web search only) |

---

## Customization

### Personalizing the Voice Writer

The `personal-voice-writer` subagent works out of the box, but you'll get better results by customizing it to match your voice:

1. Open `subagents/personal-voice-writer.md`
2. Replace the examples in "The Difference" section with your own writing samples
3. Add your personal anti-patterns to "What to Avoid"
4. Adjust the principles to match how you actually write

### Modifying Skills

Skills are just markdown files with instructions. Edit them to:

- Change the output format
- Add or remove steps
- Adjust the focus areas
- Add company-specific context

---

## Directory Structure

```
cc-for-founders/
├── README.md
├── registry.json
├── skills/
│   ├── planning-feature/
│   │   └── SKILL.md
│   ├── conducting-market-research/
│   │   └── SKILL.md
│   └── enriching-sales-contacts/
│       └── SKILL.md
└── subagents/
    ├── personal-voice-writer.md
    ├── writing-research-assistant.md
    └── sales-contact-enricher.md
```

---

## Contributing

1. Fork this repository
2. Create your plugin in the appropriate directory
3. Add it to `registry.json`
4. Submit a pull request

See the existing plugins for examples of structure and style.

---

## License

MIT License - See [LICENSE](LICENSE) for details
