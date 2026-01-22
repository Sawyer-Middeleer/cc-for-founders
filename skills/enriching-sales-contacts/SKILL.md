---
name: enriching-sales-contacts
description: Enriches sales contacts from a Google Sheet containing LinkedIn URLs. Uses contact enrichment MCPs for contact data (name, title, email, company) and web search for research notes. Processes contacts in parallel for efficiency. Trigger phrases include "enrich contacts", "fill out this sheet", "enrich this spreadsheet", "sales enrichment".
---

# Enriching Sales Contacts from Google Sheets

Use this skill when enriching a Google Sheet of LinkedIn URLs with contact data and research notes.

## Prerequisites

This skill requires the **sales-contact-enricher** subagent and MCP tools for:
1. **Contact enrichment** - to pull profile data from LinkedIn URLs
2. **Google Sheets** - to read/write spreadsheet data

If you don't have enrichment tools connected, visit [rube.app](https://rube.app/) to find and connect MCPs like Fullenrich, Apollo, Clearbit, or similar services.

## Expected Sheet Structure

| Column A | Column B | Column C | Column D | Column E | Column F | Column G | Column H |
|----------|----------|----------|----------|----------|----------|----------|----------|
| LinkedIn URL | First Name | Last Name | Title | Email | Company | Company URL | Research Notes |

- Row 1: Headers
- Row 2+: LinkedIn URLs in column A, other columns to be filled

## Workflow

### 1. Read the Sheet

```
Tool: GOOGLESHEETS_VALUES_GET
Arguments:
  spreadsheet_id: <extracted_from_url>
  range: "A1:H100"
```

Extract:
- LinkedIn URLs from column A (skip row 1 header)
- Note which rows need enrichment

### 2. Enrich Contacts in Parallel (with Real-Time Sheet Updates)

Launch **sales-contact-enricher** subagents in parallel for all contacts. Each subagent writes directly to its row as soon as enrichment completes.

```python
# Conceptual - launch via Task tool with subagent_type="sales-contact-enricher"
for row_index, linkedin_url in enumerate(urls, start=2):  # start=2 because row 1 is header
    Task(
        subagent_type="sales-contact-enricher",
        prompt=f"""Enrich this contact and write to sheet:
- linkedin_url: {linkedin_url}
- spreadsheet_id: {spreadsheet_id}
- row_number: {row_index}

Return JSON with enrichment results."""
    )
```

**Parallelization rules:**
- Launch up to 10 subagents concurrently
- Each subagent writes its own row immediately after enrichment
- User sees real-time updates in the sheet as each contact is processed
- Each returns JSON confirming: first_name, last_name, title, email, company, company_url, research_notes, sheet_updated, row_number

### 3. Summary

After all subagents complete, summarize results:
- How many contacts were successfully enriched
- How many had errors
- Any rows that need manual attention

## Enrichment API Details

Different enrichment MCPs have different schemas. Here's an example using Fullenrich:

**Starting enrichment:**
```json
{
  "name": "Batch enrichment",
  "datas": [
    {
      "linkedin_url": "https://www.linkedin.com/in/username/",
      "enrich_fields": ["contact.emails", "contact.phones"]
    }
  ]
}
```

**Key fields from response:**
- `contact.profile.firstname` - First name
- `contact.profile.lastname` - Last name
- `contact.profile.position.title` - Job title
- `contact.most_probable_email` - Best email
- `contact.profile.position.company.name` - Company name
- `contact.profile.position.company.domain` - Company domain

**Processing time:** ~20-30 seconds per batch

Other enrichment tools (Apollo, Clearbit, etc.) will have different field names but similar data. The sales-contact-enricher subagent handles mapping these fields appropriately.

## Research Notes Guidelines

Web search pattern: `{company_name} recent news 2025 2026`

Write 1 sentence that is:
- Specific (mention actual events, announcements, funding, partnerships)
- Focused on recent news and developments
- Not generic company descriptions

**Good example:** "Raised $50M Series B in January 2026 led by Sequoia to expand their AI-powered sales platform into Europe."

**Bad example:** "A growing company in the sales tech space."

## Error Handling

- **Enrichment timeout**: Retry once, then mark as "enrichment_failed"
- **No email found**: Leave email blank, continue with other fields
- **Web search fails**: Leave research_notes blank
- **Sheet write fails**: Report which rows failed, provide data for manual entry
