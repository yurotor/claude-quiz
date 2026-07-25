# Question Bank Spec

Every question is a JSON object in an array. One file per branch under `questions/`.

## Schema

```json
{
  "id": "3.6-L2-01",
  "topic": "3.6",
  "topicName": "Context lifecycle & compaction",
  "difficulty": 2,
  "question": "…?",
  "options": ["A…", "B…", "C…", "D…"],
  "correctIndex": 2,
  "explanation": "…"
}
```

- `id`: `<topic>-L<difficulty>-<seq>`, seq two digits, unique within the file.
- `difficulty`: 1–4 per the levels below.
- `options`: exactly 4, one correct. Distractors must be plausible and of the same
  kind as the answer (all commands, all numbers, all mechanisms — never one obvious joke option).
- `correctIndex`: 0–3. Spread roughly evenly across the file — never let one index dominate.
- `explanation`: a mini-lesson, ~80–160 words. Must (a) explain WHY the correct answer is
  right at mechanism level, not just restate it, (b) briefly say why the tempting
  distractor(s) are wrong, and (c) where natural, connect to the keystone mental model
  (the stateless loop / the resend) or the Bedrock-vs-Anthropic framing. It is shown after
  every answer, right or wrong, so write it to teach, not to congratulate.

## Difficulty levels

| L | Name | Targets someone who… |
|---|---|---|
| 1 | Beginner | barely/never used Claude Code; tests orientation & vocabulary |
| 2 | Practitioner | uses it daily; tests product-surface fluency (commands, files, modes) |
| 3 | Mechanic | tests the machinery: loop, context assembly, caching, cost reasoning |
| 4 | Expert | tests internals: protocol details, cache economics, provider deltas, model architecture |

A good L3/L4 format: "A colleague claims X — what's wrong with that?" (misconception-busting).
Use it for a meaningful share of L3/L4 questions, but not all.

## Distractor calibration by level

Distractor difficulty must scale with the question's level — the higher the level, the
harder the wrong answers are to distinguish from the right one:

- **L1**: distractors may be plainly wrong to anyone with exposure, but never jokes.
- **L2**: distractors are things a casual user might genuinely believe (confusing two
  commands, misremembering a default).
- **L3**: distractors are near-misses — statements that are true of an ADJACENT mechanism,
  or correct-sounding causal stories with one wrong link. A daily user should hesitate.
- **L4**: distractors are expert traps — each one either (a) true in a different provider /
  configuration / era, (b) correct except for one load-bearing detail (wrong tier, wrong
  direction, wrong multiplier), or (c) a real mechanism attached to the wrong layer. An
  expert should need to actually reason, not pattern-match. The explanation must then
  disambiguate each trap explicitly.

At L3/L4, never make the correct answer identifiable by surface features (longest option,
most hedged option, only option with a number). Match the options in length, tone, and
specificity.

## Framing rules

- Point of view: the quiz-taker usually runs Claude Code against **AWS Bedrock** and only
  sometimes against the Anthropic API. When a topic differs between providers, frame the
  question or explanation from that POV.
- General examples only — never reference a specific person's machine, paths beyond the
  standard ones (`~/.claude/`, `CLAUDE.md`, `settings.json`), or private projects.
- Claude Code + Anthropic Claude models are the reference harness/models. Local-LLM tooling
  (Ollama etc.) is out of scope; general model concepts (parameters, quantization, MoE) are in scope.
- Facts must be durable: prefer mechanisms and invariants over version-specific trivia
  (exact prices, exact model names of the week). If a number is used (e.g. cache-write = 1.25×
  base input for 5-minute TTL), it must be a stable, documented one.
- Don't invent features. If unsure a detail is real, ask about the mechanism instead.
