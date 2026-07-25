# Lesson Spec

One lesson per leaf topic of the topic tree. Lessons are shown when a quiz-taker clicks
"Learn more about this topic" — no questions, pure teaching. Depth target: as deep as the
source courses (learn-harness / learn-local-llm) — mechanism-level, not marketing-level.

## File format

One JSON object per branch file, keyed by topic id:

```json
{
  "3.6": {
    "title": "Context lifecycle & compaction",
    "intro": "One or two sentences framing why this topic exists and what mental model it hangs off.",
    "sections": [
      { "heading": "Why compaction must exist", "body": "…\n\n…" },
      { "heading": "What is kept, what is lost", "body": "…" }
    ],
    "resources": [
      { "label": "Claude Code docs — Manage costs", "url": "https://docs.claude.com/…" }
    ]
  }
}
```

- `title`: matches the topic name from the tree (may be polished for display).
- `intro`: 1–3 sentences, sets the frame.
- `sections`: 3–6 per lesson, each covering one subtopic from the tree (use the tree's
  sub-bullets as the section plan where they exist). `body` is plain text; paragraphs
  separated by `\n\n`; inline code marked with `backticks`. No other markup, no HTML.
  Each body: roughly 80–200 words. Total lesson: ~400–800 words.
- `resources`: 0–3 links to OFFICIAL sources only (docs.claude.com, docs.anthropic.com /
  platform docs, docs.aws.amazon.com for Bedrock, anthropic.com engineering posts).
  Only include links you are confident exist — verify with WebFetch if unsure.
  Omit the array or leave empty when nothing fits naturally.

## Content rules

- Same POV as the quiz: reader primarily uses Claude Code on AWS Bedrock, sometimes the
  Anthropic API. Flag provider deltas inline where they matter.
- Teach mechanisms and invariants, not version trivia. Stable documented numbers are fine
  (cache write 1.25× base input at 5-minute TTL, cache read ~0.1×).
- Connect to the keystone where natural: the stateless loop / the full resend each turn.
- Assume the reader just answered a quiz question on this topic — possibly wrongly. The
  lesson should leave them able to explain the topic to a colleague, not just recognize it.
- General examples only; standard paths (`~/.claude/`, `CLAUDE.md`, `settings.json`) are fine.
- Don't invent features. When unsure of a specific, describe the mechanism instead.
