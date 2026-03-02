---
name: emoji-recommender
description: >
  Recommends the 5 best emoji for any description, phrase, concept, mood, or situation — with clear reasoning for each pick. Use this skill whenever the user asks for emoji suggestions, says things like "what emoji should I use for X", "give me emojis that mean Y", "which emoji fits Z", or wants help picking emojis for a caption, bio, message, or vibe. Trigger this even if the user just casually tosses out a phrase and seems to want emoji without explicitly saying so.
---

# Emoji Recommender

Given a description, phrase, concept, mood, or situation, surface the 5 emoji that best represent it — with a punchy explanation for each.

## Core Task

1. Read the user's input (could be a word, phrase, feeling, scenario, aesthetic, etc.)
2. Think broadly: consider literal meaning, metaphorical associations, cultural usage, emotional tone, and how people actually use the emoji in the wild
3. Return exactly **5 emoji candidates**, ranked from best fit to honorable mention

## Output Format

Present results like this:

**[emoji] — [1-sentence reason why it fits]**

Keep reasoning concise and human — like you're texting a friend, not writing a dictionary entry. Mention both the literal and vibe-based reasons when relevant.

End with a brief note on which one you'd actually pick if you had to choose just one, and why.

## Quality Criteria

- **Diversity**: Don't cluster on too-similar emoji. Cover different angles (literal, emotional, symbolic, aesthetic).
- **Relevance**: Prioritize emoji people actually reach for in this context, not just technically accurate ones.
- **Insight**: The explanations should make the user go "oh yeah, that's exactly it" — not just describe what the emoji looks like.
- **Cultural awareness**: Consider how the emoji is actually used on social media / in messaging, not just its official name.

## Examples

**Input**: "feeling nostalgic"

- 🌅 — Sunsets = looking back, the bittersweet beauty of something passing
- 📼 — VHS tape vibes, literal throwback aesthetic
- 🥹 — That face you make when something hits different emotionally
- 🍂 — Autumn as a universal metaphor for time passing
- 💭 — Lost in thought, deep in memories

_Best pick: 🥹 — it captures the emotional complexity of nostalgia better than anything else_

---

**Input**: "chaotic coworker energy"

- 🤡 — The classic "this person is a clown" shorthand
- 🌀 — Pure chaos swirl
- 😵 — What you feel after interacting with them
- 🎪 — It's literally a circus
- 💥 — Everything they touch somehow explodes

_Best pick: 🎪 — because it implies you're not just dealing with one clown, it's a whole show_
