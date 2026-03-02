---
name: alt-text-generator
description: Generates accessible alt text descriptions for images for use in web/HTML development. Use this skill whenever a user uploads an image and wants alt text, aria-label copy, or accessibility descriptions. Trigger when the user says anything like "write alt text", "describe this image for accessibility", "what should the alt attribute be", "generate image description for screen readers", or uploads an image in a web dev context. Also trigger when the user is building HTML/components and needs image accessibility attributes filled in.
---

# Alt Text Generator

Generate accurate, accessible `alt` attribute text for web images following WCAG 2.1 best practices.

## Output Format

- **Length**: 2–3 sentences (moderate detail)
- **Tone**: Neutral, descriptive, objective — no "image of" or "picture of" prefix
- **Use case**: Web/HTML development — output should be ready to paste into `alt="..."` directly

Always output the final alt text inside a code block like:

```
alt="Your alt text here."
```

If there are multiple reasonable interpretations, briefly explain your reasoning before the code block.

---

## Image Type Detection & Handling

Before writing alt text, identify which image type applies:

### 1. Standard Images (photos, illustrations, icons)

- Describe the **subject**, **action**, and **context** in 2–3 sentences
- Focus on information relevant to the surrounding web page context if provided
- Do not mention visual style ("beautiful", "stunning") — stick to facts
- Example: `alt="A golden retriever puppy sitting on a green lawn, looking up at the camera with its tongue out. The dog wears a red collar with a small tag."`

### 2. Charts & Graphs

- Identify the **chart type** (bar, line, pie, etc.)
- Describe the **key data trend or takeaway** — not every data point
- Mention axis labels, units, and approximate values if visible
- Example: `alt="A line chart showing monthly website traffic from January to December 2023. Traffic peaks in July at approximately 120,000 visits, with a secondary peak in November. Overall trend is upward compared to the prior year."`

### 3. Screenshots (UI / App / Web)

- Identify the **application or interface** if recognizable
- Describe the **key UI elements and their state** visible on screen
- Include any important text visible in the screenshot
- Example: `alt="A GitHub pull request page showing a merged PR titled 'Fix login bug'. The PR has 3 commits, 2 reviewers approved, and shows a green 'Merged' badge. The diff view shows changes to auth.js."`

### 4. Decorative Images

Decorative images add no informational value (purely aesthetic backgrounds, dividers, abstract textures).

Output:

```
alt=""
```

And briefly explain: "This appears decorative — an empty alt attribute tells screen readers to skip it."

---

## Decision Checklist

Ask yourself before writing:

1. Is this decorative? → Output `alt=""`
2. Is this a chart/graph? → Lead with chart type + key insight
3. Is this a screenshot? → Identify app + describe key UI elements + any visible text
4. Otherwise → Describe subject, action, context in 2–3 sentences

---

## WCAG Quick Rules

- Never start with "Image of", "Photo of", "Picture of"
- Keep under ~150 characters when possible; longer is OK for complex images
- Don't repeat info already in the surrounding caption or page text (if user shares context)
- For linked images, describe the **destination or action**, not just the image content
