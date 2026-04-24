---
name: [profile-name]
created: [date]
sample-size: [word count]
accuracy-tier: [basic|strong|full-clone|maximum]
gender: [male|female|neutral]
primary-content-type: [blog|social|business|mixed]
version: 3.1
calibration-rounds: [0|1|2|3|4]
---

## Key Tells (Priority 1 — Top of profile)

The 3-5 most statistically unusual behaviors this writer exhibits. These are enforced absolutely during generation. Any conflict with the 42-feature table is resolved in favor of the Key Tell.

### KT1: [Short descriptive name]
- **What:** [Specific behavior, in one sentence]
- **Baseline:** [Israeli Hebrew norm for this feature]
- **This writer:** [Measured value, showing deviation]
- **Enforcement:** [Concrete rule for generation]

### KT2: [Short descriptive name]
[Same structure]

### KT3: [Short descriptive name]
[Same structure]

[KT4 and KT5 optional — only if they rise to the "most outlier" level]

---

## Differential Features (Priority 2 — From negative sample, if provided)

Features that are present in the writer's real voice but absent from their "wrong" / flat voice. These emerge from contrastive analysis of positive vs. negative samples.

### DF1: [Feature name]
- **Positive sample:** [value/pattern]
- **Negative sample:** [value/pattern]
- **Delta:** [what makes the positive "more them"]
- **Enforcement:** [how to apply]

[DF2, DF3 optional]

---

## Signature Passages (Priority 3 — Style-extreme selection)

5-10 verbatim excerpts selected for maximum stylistic outlierness — the passages most different from generic Israeli Hebrew. These serve as few-shot style anchors during generation.

### SP1: [rhythm label] + [Key Tell(s) demonstrated]
> "[verbatim passage]"
— demonstrates: [what this shows about the writer's distinctive voice]
— enforces: [KT numbers this passage anchors]

### SP2: [rhythm label] + [Key Tell(s) demonstrated]
> "[verbatim passage]"
— demonstrates: [description]
— enforces: [KT numbers]

[SP3-SP10 as needed]

---

## Behavioral Profile (Priority 4 — From --setup-deep interview, if run)

Self-reported preferences from the 10-question voice interview. Complements sample-measured features.

### Peak voice anchor
[Quote from Q1 — writer's self-identified "most them" moment]

### Anti-voice (from Q2)
[What the writer flees from, if they provided a negative anchor]

### Arousal rhythm (from Q3)
[How style shifts when emotionally engaged]

### Argument architecture (from Q4)
[claim-first / build-up / story-first / question-first]

### Disagreement pattern (from Q5)
[head-on / sideways / bypass]

### Preferred discourse markers (from Q6)
[Top 3 in order]

### Ending behavior (from Q7)
[explicit / CTA / reflective / just stop / question]

### Aside density (from Q8)
[never / rare / moderate / heavy]

### Uncertainty expression (from Q9)
[hedge openly / state and move / mix]

### Voice reference (from Q10)
[External anchor — person, show, vibe]

---

## 42-Feature Measurements (Priority 5 — Supporting scaffold)

### Category 1: Sentence Architecture (8 features)
[Same as v3 — see 42-feature extraction section of SKILL.md]

### Category 2: Vocabulary Profile (8 features)
[Same as v3]

### Category 3: Discourse & Flow (6 features)
[Same as v3]

### Category 4: Argumentation Style (5 features)
[Same as v3]

### Category 5: Emotional Register (5 features)
[Same as v3]

### Category 6: Punctuation & Formatting (4 features)
[Same as v3]

### Category 7: Hebrew-Specific Patterns (6 features)
[Same as v3]

---

## Fusion Rules (Generation-time priority order)

1. Key Tells (absolute)
2. Differential Features (near-absolute — Priority 2, directly below KT)
3. Signature Passages (active few-shot anchors)
4. Behavioral Profile (preference overrides)
5. 42-Feature measurements (baseline scaffold)
6. Skill safety net (AI blacklist, em dash ban, Big No-Nos)
7. Soul Layer requirements (proper nouns, דווקא, memory drops)

---

## Calibration History

### Round 1 ([date])
- Topic: [topic]
- User preference: [A/B/both similar/neither]
- User feedback: [quote]
- Profile changes: [list of updates]

[Additional rounds as they happen]

---

## Generation Notes

[Any special instructions, observations, or resolved conflicts that don't fit the structured categories above. E.g., "Sample measured 2% ellipsis but user said 'I use ... constantly' — trusting user, overriding sample" or "KT4 and DF1 both point to short sentences — reinforced enforcement."]
