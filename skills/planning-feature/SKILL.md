---
name: planning-feature
description: Guides founders through structured feature planning before development begins. Covers user research, problem validation, UX/UI concepts, and technical tradeoffs in plain language. Use when you have a feature idea and want to think it through properly before building. Trigger phrases include "plan a feature", "new feature", "feature planning", "think through this feature".
---

# Feature Planning

A structured approach to planning a new feature—from initial idea through validated concept ready for development. Designed for founders who want to think through a feature properly before writing code.

## When to Use This

Use this skill when you:
- Have a feature idea but haven't validated the need
- Want to explore the problem space before jumping to solutions
- Need to think through UX before involving designers
- Want to understand technical tradeoffs without being an engineer
- Are preparing to brief your team or contractors on what to build

This is **not** the same as Claude Code's plan mode, which focuses on implementation. This is the step *before* that—making sure you're building the right thing.

## Input

Describe your feature idea in plain language. Include:
- What you're thinking of building
- Who it's for
- Why you think they need it (or what problem it solves)
- Any context about your product/business

Don't worry about being precise—that's what this process helps with.

---

## Phase 1: Problem Validation

Before designing anything, make sure the problem is real and worth solving.

### Questions to Answer

1. **Who exactly has this problem?**
   - Be specific: "Marketing managers at B2B SaaS companies with 10-50 employees"
   - Not vague: "Marketers" or "People who need to collaborate"

2. **How do they solve it today?**
   - What workarounds exist?
   - What tools do they cobble together?
   - What manual processes do they endure?

3. **How painful is it really?**
   - Is this a hair-on-fire problem or a nice-to-have?
   - How often do they encounter it?
   - What does it cost them (time, money, frustration)?

4. **Why hasn't this been solved already?**
   - If competitors exist, why haven't users switched?
   - If no competitors, is it because the problem isn't big enough?

### Research Actions

If you're unsure about any answers, I can help with:
- **Web research**: Find existing solutions, competitor features, industry discussions
- **Social listening**: Search Reddit, Twitter, YouTube for people discussing this problem (requires social MCPs via [rube.app](https://rube.app/))
- **Review mining**: Find what users say in competitor reviews on G2, Capterra, App Store

### Output: Problem Statement

```markdown
## Problem Statement

**Who**: [Specific user persona]

**Situation**: [Context when the problem occurs]

**Problem**: [The specific pain point]

**Current solutions**: [How they handle it today]

**Why current solutions fail**: [Gap your feature fills]

**Evidence**: [How we know this is real - quotes, data, observations]
```

---

## Phase 2: Solution Exploration

Now that we understand the problem, explore how to solve it.

### Approach Options

For any feature, there are usually multiple ways to solve the problem:

| Approach | Description | When It Fits |
|----------|-------------|--------------|
| **Automate** | Do the task for the user | Repetitive, well-defined tasks |
| **Assist** | Help the user do it faster | Tasks requiring judgment |
| **Inform** | Give users better information | Decisions lacking context |
| **Connect** | Link people or data together | Coordination problems |
| **Simplify** | Remove unnecessary complexity | Overbuilt existing solutions |

### Questions to Answer

1. **What's the minimum version that solves the core problem?**
   - Strip away nice-to-haves
   - What's the one thing it must do well?

2. **What would a delightful version look like?**
   - If resources were unlimited
   - What would make users love it?

3. **What are the risks?**
   - Could this confuse existing users?
   - Could this be misused?
   - What happens if it fails or has bugs?

### Output: Solution Options

```markdown
## Solution Options

### Option A: [Name]
**Approach**: [Automate/Assist/Inform/etc.]
**How it works**: [Plain language description]
**Pros**: [Benefits]
**Cons**: [Drawbacks]
**Complexity**: [Low/Medium/High]

### Option B: [Name]
...

### Recommendation
[Which option and why, or hybrid approach]
```

---

## Phase 3: User Experience Design

Think through how users will actually interact with this feature.

### User Journey Mapping

Walk through the feature step by step:

1. **Entry point**: How do users discover/access this feature?
   - Menu item? Button? Automatic trigger?
   - Will new users find it? Will existing users notice it?

2. **Core interaction**: What does using the feature feel like?
   - How many steps to complete the task?
   - What decisions does the user need to make?
   - What information do they need to see?

3. **Completion**: How do users know it worked?
   - Confirmation message? Visual change?
   - What happens next?

4. **Edge cases**: What could go wrong?
   - Errors and how to communicate them
   - Empty states (no data yet)
   - Partial completion

### Interface Concepts

Describe the UI in plain language:

```markdown
## Interface Concept

### Screen/Component: [Name]

**Purpose**: [What the user accomplishes here]

**Key elements**:
- [Element 1]: [What it shows/does]
- [Element 2]: [What it shows/does]

**User actions**:
- [Action 1]: [What happens when they do it]
- [Action 2]: [What happens when they do it]

**States**:
- Default: [What it looks like normally]
- Loading: [What users see while waiting]
- Empty: [What users see with no data]
- Error: [What users see if something fails]
```

### Questions to Answer

1. **Does this feel like our product?**
   - Consistent with existing patterns?
   - Matches our brand/voice?

2. **Is it obvious how to use it?**
   - Could a new user figure it out?
   - Do we need onboarding/tooltips?

3. **What's the learning curve?**
   - Instant understanding vs. requires training?
   - Worth the complexity for the power it provides?

---

## Phase 4: Technical Considerations

Understand the technical tradeoffs without needing to be an engineer.

### Questions to Ask Your Technical Team

I'll help you formulate the right questions:

1. **Feasibility**
   - "Is this technically possible with our current stack?"
   - "What would make this significantly harder or easier?"

2. **Data requirements**
   - "What data do we need that we don't currently have?"
   - "Do we need to integrate with any external services?"

3. **Performance implications**
   - "Will this slow anything down for users?"
   - "Can this scale if we 10x our users?"

4. **Development effort**
   - "Is this a few days, a few weeks, or a few months?"
   - "What's the riskiest/most uncertain part?"

5. **Maintenance burden**
   - "Once built, how much ongoing work does this create?"
   - "What could break and how would we know?"

### Build vs. Buy vs. Partner

For many features, you have options beyond building from scratch:

| Option | When It Makes Sense |
|--------|---------------------|
| **Build** | Core to your value prop, competitive advantage, or simple enough to be fast |
| **Buy/Integrate** | Commodity functionality, well-solved problem, or faster time to market matters |
| **Partner** | Need expertise you don't have, or the vendor relationship adds value |

### Technical Tradeoff Framework

```markdown
## Technical Considerations

### Approach 1: [e.g., "Build from scratch"]
**Effort**: [Low/Medium/High]
**Risk**: [What could go wrong technically]
**Dependencies**: [What this relies on]
**Future flexibility**: [Easy/Hard to change later]

### Approach 2: [e.g., "Use third-party service"]
**Effort**: [Low/Medium/High]
**Risk**: [What could go wrong technically]
**Dependencies**: [What this relies on]
**Ongoing cost**: [Pricing model implications]

### Recommendation for technical discussion
[What to propose to your engineering team and why]
```

---

## Phase 5: Decision Framework

Bring it all together to decide whether and how to proceed.

### Go/No-Go Criteria

| Criterion | Status | Notes |
|-----------|--------|-------|
| Problem validated with evidence | | |
| Solution clearly differentiated | | |
| UX is achievable and coherent | | |
| Technical approach is feasible | | |
| Effort justified by expected impact | | |
| Aligns with product strategy | | |

### Impact Assessment

```markdown
## Expected Impact

**User value**: [How much does this help users? What metrics move?]

**Business value**: [Revenue, retention, expansion, or strategic value]

**Effort**: [Rough sizing - days/weeks/months]

**Risk**: [What's the worst case if this fails?]

**Opportunity cost**: [What are we NOT doing to do this?]
```

### Recommendation

```markdown
## Recommendation

**Decision**: [Build / Don't build / Investigate further / Simplify scope]

**Reasoning**: [2-3 sentences on why]

**Next steps**:
1. [Immediate next action]
2. [Following action]
3. [Following action]

**Open questions to resolve**:
- [Question 1]
- [Question 2]
```

---

## Output: Feature Brief

After completing all phases, I'll compile a feature brief you can share with your team:

```markdown
# Feature Brief: [Feature Name]

## Summary
[2-3 sentence description of what we're building and why]

## Problem
[Problem statement from Phase 1]

## Solution
[Recommended approach from Phase 2]

## User Experience
[Key screens/flows from Phase 3]

## Technical Approach
[Recommended approach and key considerations from Phase 4]

## Success Metrics
[How we'll know this worked]

## Scope
**In scope**: [What's included]
**Out of scope**: [What's explicitly not included]
**Future considerations**: [What we might add later]

## Open Questions
[Unresolved items that need answers]

## Next Steps
[What happens now]
```

---

## Tips for Non-Technical Founders

- **You don't need to know how it's built** to have a strong opinion on what gets built and why
- **Ask "why" a lot** when engineers say something is hard—sometimes it's essential complexity, sometimes it's solvable with a different approach
- **Scope creep is the enemy**—every "while we're at it" makes the feature later and riskier
- **Ship something small first** if you're unsure—real user feedback beats theoretical planning
- **Write things down**—this brief becomes the artifact your team aligns around
