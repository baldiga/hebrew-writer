---
name: [profile-name]
created: [date]
sample-size: [word count]
accuracy-tier: [basic|strong|full-clone|maximum]
gender: [male|female|neutral]
primary-content-type: [blog|social|business|mixed]
version: 3
---

## 42-Feature Voice Profile: [name]

### Category 1: Sentence Architecture

| # | Feature | Value |
|---|---------|-------|
| 1 | Average sentence length | [X words] |
| 2 | Sentence length std deviation | [low/medium/high] ([min]-[max] range) |
| 3 | Shortest typical sentence | [X words] |
| 4 | Longest typical sentence | [X words] |
| 5 | Fragment frequency | [X%] of sentences under 5 words |
| 6 | Question frequency | [X] per 500 words |
| 7 | Exclamation frequency | [X] per 500 words |
| 8 | Ellipsis frequency | [never/occasional/heavy] — every [X] words |

### Category 2: Vocabulary Profile

| # | Feature | Value |
|---|---------|-------|
| 9 | Formality level | [X/10] |
| 10 | Hebrew-English ratio | [X%] English words |
| 11 | Slang density | [list of used slang with frequency: heavy/moderate/light/never] |
| 12 | Preferred intensifiers | [ranked list: e.g., ממש >> מאוד > ביותר] |
| 13 | Characteristic words | [5-10 words this person overuses] |
| 14 | Avoided words | [words this person never uses] |
| 15 | Vocabulary breadth | [narrow/moderate/broad] |
| 16 | Technical jargon density | [none/light/moderate/heavy] ([domain]) |

### Category 3: Discourse & Flow

| # | Feature | Value |
|---|---------|-------|
| 17 | Preferred discourse markers | [ranked list: e.g., נו >> בעצם > כאילו] |
| 18 | Discourse marker frequency | [X%] of tokens |
| 19 | Preferred connectors | [list: e.g., אז, אבל, כי] |
| 20 | Self-correction frequency | [never/rare/occasional/frequent] — every [X] words |
| 21 | Parenthetical/aside frequency | [X] per 500 words |
| 22 | Paragraph length pattern | [short (2-3 sent) / medium (3-5) / long (5+) / mixed] |

### Category 4: Argumentation Style

| # | Feature | Value |
|---|---------|-------|
| 23 | Opening style | [claim-first / story-first / question-first / context-first] |
| 24 | Opinion strength | [bold / moderate / hedged] |
| 25 | Evidence style | [anecdotal / data-driven / mixed] |
| 26 | Pushback handling | [confrontational / acknowledging / avoiding] |
| 27 | Conclusion style | [abrupt / reflective / call-to-action / summary / none] |

### Category 5: Emotional Register

| # | Feature | Value |
|---|---------|-------|
| 28 | Humor style | [sarcastic / self-deprecating / dry / warm / absent] |
| 29 | Humor frequency | [rare / occasional / frequent] |
| 30 | Emotional temperature | [cool / warm / hot] |
| 31 | Vulnerability level | [exposed / guarded / absent] |
| 32 | Irony usage | [heavy / light / none] |

### Category 6: Punctuation & Formatting

| # | Feature | Value |
|---|---------|-------|
| 33 | Comma density | [light / moderate / heavy] |
| 34 | Period preference | [many short sentences / fewer long / mixed] |
| 35 | Ellipsis usage | [never / occasional / heavy] |
| 36 | Formatting style | [minimal / moderate bold / heavy formatting] |

### Category 7: Hebrew-Specific Patterns

| # | Feature | Value |
|---|---------|-------|
| 37 | Construct state vs. של | [X/Y ratio] |
| 38 | Pro-drop consistency | [always / sometimes / never] drops pronouns |
| 39 | Binyan preferences | [dominant patterns] |
| 40 | Gender handling | [standard / inclusive / mixed] |
| 41 | Sentence-initial particles | [list with frequency] |
| 42 | Cultural reference density | [sparse / moderate / rich] ([categories]) |

---

## Signature Passages

5-10 verbatim excerpts (15-40 words each) that best capture this writer's distinctive voice. Used as few-shot style anchors during generation.

### SP1: [label — e.g., "rhythm + fragment"]
> "[verbatim passage from sample]"
— demonstrates: [what this passage shows about the writer's style]

### SP2: [label]
> "[verbatim passage]"
— demonstrates: [description]

### SP3: [label]
> "[verbatim passage]"
— demonstrates: [description]

### SP4: [label]
> "[verbatim passage]"
— demonstrates: [description]

### SP5: [label]
> "[verbatim passage]"
— demonstrates: [description]

[Add SP6-SP10 as needed for Full Clone and Maximum tiers]

---

## Fusion Rules

### Overrides (user style wins over skill defaults)
- Sentence length target: [user's avg] instead of 10-12
- Slang selection: [user's specific slang] instead of full menu
- Discourse markers: [user's ranked list] at [user's frequency]
- English ratio: [user's %] instead of 5-8%
- Opinion strength: [user's level]
- Humor style: [user's type]
- Opener patterns: [user's natural openers]
- Construct/של ratio: [user's ratio]

### Safety net (skill always wins)
- AI vocabulary blacklist (16 words) — always enforced
- Em dash ban — always enforced
- Big No-Nos (macro copy, slide structure, punchlines) — always enforced
- Anti-detection minimums (burstiness, perplexity, n-gram) — always enforced
- Self-audit 95/100 threshold — always enforced

### Soul layer additions needed
Based on what this user's writing naturally includes vs. lacks:
- [x] or [ ] Proper nouns — user naturally includes / needs injection
- [x] or [ ] דווקא — user naturally uses / needs injection
- [x] or [ ] Memory/anecdote — user naturally includes / needs injection
- [x] or [ ] Self-correction — user naturally does / needs injection
- [x] or [ ] Stakes/vulnerability — user naturally shows / needs injection
- [x] or [ ] Unusual numbers — user naturally uses / needs injection

---

## Generation Notes

[Any special instructions or observations about this voice that don't fit the structured categories above. E.g., "This writer always ends posts with a question" or "Uses חחח but never לול" or "References army service in almost every piece"]
