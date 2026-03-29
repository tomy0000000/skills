---
name: hn-digest
description: >
  Digests Hacker News posts into a structured summary. Given a Hacker News URL
  (news.ycombinator.com/item?id=...), fetches the linked article and comment
  threads, then produces a concise summary of the article followed by an
  organized breakdown of community reactions (agree / disagree / different
  voices). Use this skill whenever the user shares a Hacker News link, asks to
  summarize an HN discussion, says things like "what are people saying about
  this HN post", "digest this HN thread", "summarize the comments on this",
  or pastes any news.ycombinator.com URL. Also trigger when the user references
  "Hacker News", "HN thread", or "HN comments" alongside a link or post ID.
  This skill is for link-type posts (posts that point to an external article);
  for text-only posts like "Ask HN" or discussion threads without an external
  link, skip this skill and summarize conversationally instead.
---

# HN Digest

You turn a Hacker News post into a structured, opinionated digest: a tight
summary of the original article plus an organised map of community sentiment
from the comments.

## When this skill applies

The user gives you a Hacker News URL (e.g. `https://news.ycombinator.com/item?id=12345`)
or asks you to summarize / digest an HN discussion. This skill targets
**link-type posts** — posts that point to an external article or blog post.

If the post is a text-only "Ask HN" or "Show HN" with no external link, don't
use this skill; just respond conversationally.

## Workflow

### Step 1 — Fetch the HN comments page

Use `web_fetch` on the Hacker News URL to get the discussion page. Extract:

- **Post title**
- **External article URL** (the link the post points to)
- **Point count and comment count** (if visible)
- **Comment threads** — capture top-level comments and notable replies (replies
  that add substantial new information, strong counterarguments, or get
  significant engagement). Skip low-signal replies like "+1", "this", or
  single-emoji responses.

### Step 2 — Fetch the linked article

Use `web_fetch` on the external article URL. Read the full content. If the
fetch fails or returns a paywall, note this and work with whatever content is
available from the HN discussion itself (commenters often summarize or quote
the article).

### Step 3 — Write the digest

Produce the output in this structure:

---

## 📰 [Post Title]

**Source:** [domain of the article]
**HN:** [link to the HN discussion] · [X points] · [Y comments]

### Key Takeaways

A 100–200 word bullet-point summary of the original article. Each bullet
should capture one distinct claim or insight from the article. Focus on:

- The core thesis or announcement
- The most important supporting evidence or arguments
- Why it matters (so what?)

Aim for 3–6 bullets. Write in your own words — distill, don't parrot. If the
article is technical, make it accessible without dumbing it down.

### Community Debate

Organize the agree and disagree comments into a **markdown table** grouped by
topic. Each row represents one debated question or claim from the thread.
This lets the reader compare opposing views at a glance.

Table format:

| Topic                   | 🟢 Agree                                               | 🔴 Disagree                                   |
| ----------------------- | ------------------------------------------------------ | --------------------------------------------- |
| **[Short topic label]** | [Paraphrased position] (~N comments) [1](url) [2](url) | [Paraphrased position] (~N comments) [1](url) |

Guidelines for the table:

- Identify 3–7 topics where commenters are clearly engaging the same question
  from opposite sides. The topic label should be a short, neutral phrase
  (e.g., "Learning curve", "Applicability beyond safety-critical systems").
- Each cell should be 1–3 sentences — tight enough to scan, detailed enough
  to be useful.
- **Show the weight of each position.** After the paraphrased text, include
  a rough comment count in parentheses like `(~5 comments)` indicating how
  many substantive comments express this view. Then list numbered markdown
  links to the most representative comments (up to 3). This lets the reader
  instantly distinguish a lone contrarian from a broad consensus.
  Example: `Formal specs catch bugs earlier (~6 comments) [1](url) [2](url) [3](url)`
- If a topic has comments on one side but not the other, leave that cell
  with "—" (em dash). This is fine and expected.
- Row order: put the most actively debated topics first.
- The total number of filled Agree vs Disagree cells should roughly reflect
  the actual proportions of the thread. If the thread skews 70% supportive,
  the table should have more filled Agree cells than Disagree cells.

### 🔵 Different Voices

Bullet points for comments that take the conversation in a genuinely different
direction — tangential insights, related experiences, adjacent topics, or
meta-observations that don't fit the debate table. These are often the most
interesting part of HN threads. Each bullet should:

- Describe the alternative angle or tangent
- Explain why it's noteworthy or what it adds to the discussion
- End with a markdown link to the HN comment

---

## Guidelines

**Proportional representation.** The number of bullets in each section (Agree,
Disagree, Different Voices) should roughly reflect the actual balance of the
comment thread. If 70% of substantive comments agree and only a handful
disagree, don't force equal bullet counts — let the digest reflect reality.
Typical range is 2–7 bullets per section, but skew with the thread.

**Every bullet gets a link.** End each comment bullet with a markdown link to
the specific HN comment being summarised. This lets the reader click through
to the full thread if a point is interesting. Format:
`[link](https://news.ycombinator.com/item?id=XXXXX)`

**Attribution without over-quoting.** Paraphrase comments in your own words.
You can reference a commenter's claimed background if relevant ("one commenter
who works in chip manufacturing noted...") but don't use HN usernames.

**Notable replies matter.** If a top-level comment sparks a substantive
back-and-forth, fold the key takeaway from that thread into the bullet rather
than listing each reply separately.

**Signal over noise.** HN threads have a lot of low-signal comments. Your job
is editorial — surface the insights that someone skimming the thread would
miss, not catalog every hot take.

**Tone.** Be direct and slightly opinionated in your editorial voice. You're
writing a digest for a busy person, not a neutral Wikipedia summary.
