# Hebrew Writer Skill Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single SKILL.md file that makes Claude write Hebrew content indistinguishable from a native Israeli human writer, passing all AI detection tools and fooling native speakers.

**Architecture:** A single comprehensive markdown file (SKILL.md) containing all 7 layers as prompt instructions. The skill uses Claude Code's standard skill infrastructure — no external dependencies, no API calls, no code execution. One supporting template file for voice profiles. The entire skill is prompt engineering at industrial scale.

**Tech Stack:** Markdown (SKILL.md), Claude Code skill framework, standard allowed tools (Read, Write, Edit, Grep, Glob, AskUserQuestion)

**Spec document:** `docs/superpowers/specs/2026-04-20-hebrew-writer-design.md`

---

## File Structure

| File | Responsibility |
|------|---------------|
| `SKILL.md` | The complete hebrew-writer skill — all 7 layers, argument parsing, modes, scoring, audit loop, voice matching |
| `voice-profile-template.md` | Template for saved `.hebrew-voice.md` voice profiles |

That's it. Two files. The entire skill lives in SKILL.md. This is how all top Claude Code skills work (blader/humanizer = 1 file, humanizalo = 1 file + references).

---

## Chunk 1: Skill Foundation — Frontmatter, Argument Parsing, Mode Routing

### Task 1: Write SKILL.md frontmatter and identity

**Files:**
- Create: `SKILL.md`

- [ ] **Step 1: Write the skill frontmatter**

```markdown
---
name: hebrew-writer
description: |
  Write Hebrew content indistinguishable from a native Israeli human.
  Generates, rewrites, or detects AI patterns in Hebrew text.
  7-layer system: Hebrew-first thinking, 55+ AI pattern detection,
  Israeli voice injection, linguistic precision, rhythm engineering,
  self-audit scoring (95/100 threshold), and voice cloning.
  Use when: writing Hebrew content, humanizing Hebrew text, checking
  Hebrew text for AI tells, or matching someone's Hebrew writing voice.
  Triggers: "write in Hebrew", "Hebrew content", "כתוב בעברית",
  "humanize Hebrew", "sound Israeli", "Hebrew blog", "Hebrew article",
  "rewrite in Hebrew", "detect AI Hebrew", "תכתוב לי", "shadow writer"
user-invocable: true
argument-hint: '"topic or text" [--mode generate|rewrite|detect] [--type blog|academic|social|business|email|creative|auto] [--length short|medium|long|xl|NUMBER] [--gender male|female|neutral] [--voice profile-name] [--my-voice "sample text"] [--my-voice-file path] [--learn "text" --save-as name] [--show-score]'
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---
```

- [ ] **Step 2: Write the skill identity and core philosophy**

Write the opening section that establishes WHO Claude becomes when this skill activates. This is the most important section — it sets the tone for everything. Content:

- "You are a native Israeli Hebrew writer" identity
- The Hebrew Mind principle — think in Hebrew, never translate from English
- The dugri philosophy — direct, opinionated, human
- The core rule: "LLMs regress to the statistical mean. Israelis are weird, specific, and direct. Write like an Israeli."
- The fundamental AI tell: text that emerges from nowhere, addressed to no one, with no stake in its claims
- **Concrete countermeasures for the fundamental tell:** (1) Always establish a writer's position — who is writing this and why do they care? (2) Always write as if to a specific audience — not "readers" but "you, the person reading this right now." (3) Always have stakes — the writer should want something, believe something, or be trying to convince of something. (4) Include situated details — references to time, place, personal experience that ground the text in a real context. (5) Show the writer's thinking process — visible reasoning, course corrections, discoveries mid-paragraph.

- [ ] **Step 3: Verify frontmatter is valid YAML**

Read back the file and confirm the frontmatter parses correctly (no special characters breaking YAML, description is properly indented).

- [ ] **Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: hebrew-writer skill foundation — frontmatter and identity"
```

### Task 2: Argument parsing and mode routing

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the argument parser section**

Content should cover:
- Parse $ARGUMENTS for all flags: --mode, --type, --length, --gender, --voice, --my-voice, --my-voice-file, --my-voice-files, --learn, --save-as, --show-score
- Default values for each
- Text extraction (everything not a flag)
- Error handling: no text and no --file = prompt user
- --learn requires --save-as (validate)

- [ ] **Step 2: Write mode routing logic**

Three paths based on --mode:
- generate: Go to "Generation Pipeline" section
- rewrite: Go to "Rewrite Pipeline" section (extract meaning first, then generate)
- detect: Go to "Detection Report" section (scan only, output report)

- [ ] **Step 3: Write content type detection logic**

Rules for auto-detecting content type when --type is "auto":
- Contains academic keywords (מחקר, תזה, ביבליוגרפיה) → academic
- Contains social markers (hashtag, emoji, very short) → social
- Contains email markers (שלום, בברכה, לכבוד) → email
- Brief/topic is business-related → business
- Default → blog

Include the content type ↔ voice layer interaction rules from the spec.

- [ ] **Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: argument parsing and mode routing"
```

---

## Chunk 2: Layer 1 (Hebrew Mind) + Layer 4 (Linguistic Precision)

These two layers are combined because they're both about HOW to write correct, natural Hebrew at the mechanical level.

### Task 3: Hebrew Mind instructions

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the Hebrew Mind section**

Implement all 7 core principles from spec Section 3.2:

1. Hebrew-first composition instruction (concrete prompting: "plan in Hebrew, choose Hebrew synonyms directly")
2. SVO with flexible emphasis — examples of natural topicalization
3. Pro-drop rules table (past/future drop, present keep, emphasis keep)
4. Nominal sentences — examples and when to use them
5. Morphological thinking — root family examples
6. Hebrew sentence economics — compactness advantage
7. Sentence-initial particles — ו, אז, אבל, גם, כי as natural openers

- [ ] **Step 2: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 1 — Hebrew Mind instructions"
```

### Task 4: Linguistic Precision rules

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the gender system section**

Content:
- Conjugation rules (verbs, adjectives, numbers agree with gender)
- --gender flag behavior (controls writer's voice gender)
- The counterintuitive numeral rule (feminine nouns take masculine-form numerals)
- Strategic imperfection: ~1 gender error per 800-1000 words, with specific examples (צומת as feminine, etc.)

- [ ] **Step 2: Write the spelling variation section**

Content:
- Ktiv male rules and when to break them
- Accepted variant pairs: מזרן/מזרון, צוהריים/צהריים, and more
- Academy 2017 rules: acknowledge but don't follow uniformly
- Instruction: vary spelling within same document (~1-2 inconsistencies per 500 words)

- [ ] **Step 3: Write the construct state vs. של section**

The full table from the spec:
- Formal → סמיכות
- Casual → של
- Frozen expressions → always construct
- Natural mixing → both in same text

- [ ] **Step 4: Write the binyanim awareness section**

Rules for each binyan:
- Pa'al for basic, Pi'el for intensive, Hitpa'el for reflexive
- Nif'al middle voice nuance
- Avoid overusing passive (Pu'al, Huf'al)

- [ ] **Step 5: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 4 — linguistic precision (gender, spelling, binyanim)"
```

---

## Chunk 3: Layer 2 (Anti-Detection Engine)

The largest single section. Contains all pattern definitions and fixes.

### Task 5: Hebrew AI vocabulary blacklist and content patterns

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the Hebrew AI vocabulary blacklist**

Full list of Hebrew words that are AI tells:
- 15+ blacklisted words with their plain Hebrew replacements
- Instruction: if you catch yourself using these, stop and rephrase

- [ ] **Step 2: Write content pattern detection rules**

Map each English AI pattern to its Hebrew equivalent:
- P1: Hebrew significance inflation (מהווה אבן דרך, מסמל את, etc.) with before/after Hebrew examples
- P2: Hebrew copula avoidance (משמש כ, מהווה, עומד בבסיס) with fixes
- P3: Hebrew superficial -ing equivalents (תוך הדגשת, המשקף את) with fixes
- P4: Hebrew promotional language with fixes
- P5: Hebrew vague attributions with fixes
- P6: Hebrew formulaic challenges sections with fixes

Each pattern needs: trigger words in Hebrew, what's happening, the fix, before/after example IN HEBREW.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 2 — Hebrew AI vocabulary blacklist and content patterns"
```

### Task 6: Language and style anti-patterns

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write language and style pattern rules**

- Formulaic transitions in Hebrew (בנוסף, יתר על כן → אז, גם, חוץ מזה)
- Em dash ban (zero tolerance, same as English)
- Rule of three in Hebrew
- Negative parallelisms in Hebrew (זה לא רק X, זה גם Y)
- Synonym cycling in Hebrew
- Bold/formatting overuse
- Hedging pile-ups in Hebrew
- Title case in Hebrew headings

- [ ] **Step 2: Write the statistical fingerprint rules**

Aspirational targets translated to concrete instructions:
- Sentence length variance (3-40 word range)
- Burstiness approximation rules
- Token predictability reduction
- N-gram pattern breaking
- Vocabulary diversity instructions

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 2 — language and style anti-patterns, statistical rules"
```

### Task 6b: Hebrew-specific AI tells

**Files:**
- Modify: `SKILL.md`

**Note:** Cross-reference the vocabulary blacklist from Task 5 when writing these tells — ensure no contradictions with Layer 3's slang/vocabulary sections coming in Task 7.

- [ ] **Step 1: Write the 10 Hebrew-specific AI tells**

The unique patterns from spec Section 4.2:
1. Over-formality syndrome
2. Missing dugri energy
3. Sanitized vocabulary
4. Too-perfect grammar
5. Over-consistent spelling
6. Missing pro-drop
7. Wrong construct vs. של
8. Missing discourse markers
9. Register-deaf connectors
10. Absent cultural texture

Each with: what to look for, why it happens, how to fix it, Hebrew example.

Reference the research document `hebrew_writing_research.md` Section 4 for the detailed analysis of each tell. Also reference the spec's Section 11 (Research Foundation) for detector-specific architecture details to inform anti-detection rules.

- [ ] **Step 2: Verify all Hebrew examples are linguistically accurate**

Re-read all Hebrew text written in Tasks 5, 6, and 6b. Cross-reference with `hebrew_writing_research.md` for accuracy.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 2 — 10 Hebrew-specific AI tells"
```

---

## Chunk 4: Layer 3 (Israeli Voice) + Layer 5 (Rhythm)

### Task 7: Israeli Voice Injection

**Files:**
- Modify: `SKILL.md`

**Note:** Cross-reference Layer 2's blacklist (Tasks 5-6b) when writing vocabulary/slang sections. Ensure slang words encouraged here are NOT on the AI blacklist, and that Layer 3's vocabulary doesn't contradict Layer 2's pattern avoidance rules.

- [ ] **Step 1: Write the Dugri Principle section**

The personality engine:
- Say it straight — no dancing, no hedging
- Have a take — neutral writing is the biggest AI tell
- Warmth through directness
- Examples of dugri vs. AI-style writing in Hebrew

- [ ] **Step 2: Write the slang and loanword system**

The full table from the spec:
- Register-appropriate slang rules
- When to use Arabic-origin, Yiddish-origin, English loanwords
- When NOT to use them
- Specific word lists with context rules

- [ ] **Step 3: Write the discourse marker injection rules**

Full list of markers with usage contexts:
- כאילו, יעני, בעצם, נו, אז, נכון?
- Frequency rules: ~3-5% casual, ~1-2% semi-formal, near zero academic
- Examples of natural insertion points

- [ ] **Step 4: Write the humor and cultural texture section**

- Self-deprecating humor patterns
- חחחחח convention
- Sarcasm and irony techniques
- Cultural reference categories (army, weather, bureaucracy, food, holidays)
- Examples in Hebrew

- [ ] **Step 5: Write the emotional authenticity section**

- Mixed feelings expression patterns
- Opinion + doubt patterns
- Mood bleeding into writing
- Before/after examples showing soulless vs. alive Hebrew text

- [ ] **Step 6: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 3 — Israeli voice injection (dugri, slang, humor, soul)"
```

### Task 8: Rhythm and statistical anti-detection

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the burstiness engineering rules**

Concrete rules (not mathematical targets):
- The 3-40 rule for sentence length range
- Never 3+ consecutive similar-length sentences
- Fragment requirement per 500 words
- Long sentence requirement per 500 words
- Rhythm pattern examples: Long→Short→Medium→VeryShort→Long

- [ ] **Step 2: Write the perplexity injection rules**

Concrete word-choice instructions:
- Avoid the "obvious" word ~20-30% of the time
- Hebrew synonym variance examples
- Domain-crossing vocabulary encouragement
- Specific examples showing predictable vs. surprising word choices in Hebrew

- [ ] **Step 3: Write the vocabulary diversity rules**

- Hapax legomena instruction
- Vocabulary breadth instruction
- Function word variance
- The "safe word" trap warning

- [ ] **Step 4: Write the n-gram and paragraph rules**

- No repeated sentence openers
- Connector variety (soft 300-word rule)
- Clause structure mixing checklist
- Opening variety checklist (verb, question, number, quote, reaction, mid-thought)
- Paragraph length variance rules

- [ ] **Step 5: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 5 — rhythm engineering and statistical anti-detection"
```

---

## Chunk 5: Layer 6 (Self-Audit Loop) + Layer 7 (Voice Matching)

### Task 9: Self-Audit Loop

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the self-audit loop process**

The integrated mental checklist approach:
- Phase 1: Generate draft using all layers
- Phase 2: Score against 8 dimensions (internal assessment)
- Phase 3: Identify dimensions below 9.5, rewrite those sections
- Phase 4: Final scan for surviving AI patterns
- Phase 5 (emergency): Extract meaning as bullets, rewrite from scratch

Instruction: This is all done internally within a single response. Not multi-turn.

- [ ] **Step 2: Write the 8-dimension scoring rubric**

Full rubric with updated weights from spec revision:
- דוגריות (Directness) — 13%
- קצב (Rhythm) — 14%
- אמינות (Authenticity) — 14%
- טקסטורה (Texture) — 11%
- נשמה (Soul) — 12%
- צפיפות (Density) — 8%
- רישום (Register) — 8%
- אנטי-זיהוי (Anti-Detection) — 20%

Each dimension: what 10/10 looks like, what 8/10 looks like, what below 8 looks like.

- [ ] **Step 3: Write the quality gate rules**

- Threshold: 95/100 weighted total
- Minimum: 8/10 on every dimension
- Math note: all-8s = 80, all-9s = 90, need most at 9-10
- Autonomous: no user prompts, just deliver the best result
- If --show-score: include the breakdown in output

- [ ] **Step 4: Write the quick-check checklist**

A fast pre-output checklist:
- No Hebrew AI blacklist words?
- No em dashes?
- No 3+ consecutive similar-length sentences?
- No formal connectors in casual text?
- Has discourse markers (if casual)?
- Has opinions/emotions?
- Gender consistent (or strategically imperfect)?
- Spelling has natural variation?

- [ ] **Step 5: Commit**

```bash
git add SKILL.md
git commit -m "feat: Layer 6 — self-audit loop with 8-dimension scoring"
```

### Task 10: Voice Matching and Personalization

**Files:**
- Modify: `SKILL.md`
- Create: `voice-profile-template.md`

- [ ] **Step 1: Write Mode A (Default Voice) section**

Brief section: when no voice sample is provided, use the built-in Israeli casual voice defined by Layers 1-5. This is the out-of-box experience.

- [ ] **Step 2: Write Mode B (Voice Clone) section**

The voice analysis engine:
- 10 features to extract from samples (sentence length, formality, discourse markers, argument structure, punctuation, vocabulary, English ratio, humor, opinions, openings)
- How to analyze each feature
- How to generate the internal voice profile
- Accuracy tier logic: calculate sample word count, assign tier (Basic: 200-500, Strong: 500-1500, Full Clone: 1500+, Maximum: multiple texts), and adjust which features are extracted based on tier. Basic tier: only extract sentence length, formality, basic vocabulary. Strong tier: add humor, argument structure, discourse markers. Full Clone+: extract all 10 features.

- [ ] **Step 3: Write the voice profile file handling**

- --learn + --save-as workflow: read sample, analyze, generate profile, save to .claude/voices/
- --voice workflow: load profile — check `.claude/voices/` (project-local) first, then `~/.claude/voices/` (global). Local takes precedence.
- --my-voice / --my-voice-file: one-off analysis without saving
- --my-voice-files: multi-file analysis for maximum accuracy

Error handling:
- Sample under 200 words: warn user "Sample too short for reliable voice matching. Minimum 200 words recommended. Proceeding with basic approximation."
- .claude/voices/ directory doesn't exist: create it automatically when saving
- --voice profile not found: list available profiles and ask user to choose or provide a sample
- --my-voice-file path doesn't exist: inform user and ask for correct path

- [ ] **Step 4: Create the voice profile template**

Write `voice-profile-template.md`:

```markdown
---
name: [profile-name]
created: [date]
sample-size: [word count]
accuracy-tier: [basic|strong|full-clone|maximum]
---

## Voice Profile: [name]

### Sentence Patterns
- Average length: [X] words
- Range: [min]-[max] words
- Preferred rhythm: [description]

### Register
- Formality level: [1-10 scale, 1=very casual, 10=very formal]
- Slang usage: [none|light|moderate|heavy]
- English code-switching: [percentage estimate]

### Discourse Markers
- Primary: [list of most-used markers]
- Frequency: [light|moderate|heavy]

### Argument Structure
- Pattern: [direct-claim-first|build-up|story-first|question-driven]
- Opinion strength: [bold|moderate|hedged]

### Punctuation
- Comma usage: [light|moderate|heavy]
- Period preference: [short-sentences|long-sentences|mixed]
- Special: [any notable habits]

### Vocabulary
- Level: [casual|standard|elevated|technical]
- Preferred words: [list of characteristic words]
- Avoided words: [list of words this person never uses]

### Humor
- Style: [sarcastic|self-deprecating|dry|warm|none]
- Frequency: [rare|occasional|frequent]

### Emotional Expression
- Temperature: [cool|warm|hot]
- Uncertainty: [expressed-openly|rarely-shown|hidden]

### Characteristic Mistakes
- [Any spelling or grammar patterns unique to this person]

### Cultural References
- Density: [sparse|moderate|rich]
- Types: [which categories of references]
```

- [ ] **Step 5: Commit**

```bash
git add SKILL.md voice-profile-template.md
git commit -m "feat: Layer 7 — voice matching, profile persistence, template"
```

---

## Chunk 6: Detection Mode, Rewrite Pipeline, and Final Assembly

### Task 11: Detection mode output

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the Detect mode section**

When --mode detect:
1. Read the input text
2. Scan for all 55+ patterns (Hebrew AI vocabulary, content patterns, language patterns, Hebrew-specific tells, statistical patterns)
3. Output a structured report:
   - Overall human-likeness score (0-100)
   - Detected patterns table: Pattern ID, Pattern name, Location (quote), Severity (high/medium/low)
   - Per-dimension scores on 8 dimensions
   - Recommendations for manual fixes
4. No changes to the text — report only

- [ ] **Step 2: Commit**

```bash
git add SKILL.md
git commit -m "feat: detect mode — scan-only report format"
```

### Task 12: Rewrite pipeline

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write the Rewrite mode section**

When --mode rewrite:
1. Read the input text
2. Identify source language (Hebrew, English, transliterated Hebrew, other)
3. Extract core meaning — bullet points, not sentences. Strip all style, keep only facts/arguments/opinions
4. Generate fresh Hebrew text from the bullet points using all 7 layers
5. Run the self-audit loop
6. Output the rewritten text (optionally with score)

Key instruction: "NEVER translate word-for-word. Extract meaning, then write fresh. The output should share no sentence structure with the input."

- [ ] **Step 2: Commit**

```bash
git add SKILL.md
git commit -m "feat: rewrite mode — meaning extraction and fresh generation"
```

### Task 13: Content type register presets

**Files:**
- Modify: `SKILL.md`

**Note:** Re-read Layers 3-5 (Tasks 7-8) before writing presets. Each preset modifies those layers' behavior — verify no contradictions are introduced. Also note: the spec's argument table (Section 10.2) omits `email` from --type values but Section 2.3 includes it. We include email as a content type.

- [ ] **Step 1: Write register presets for each content type**

For each of the 6 content types, define specific adjustments to Layers 3-5:

**Blog:**
- Voice: Full casual, slang, discourse markers, opinions
- Rhythm: Short-medium paragraphs, punchy, engaging
- Formality: 3/10

**Academic:**
- Voice: Dugri preserved, slang removed, formal linking words
- Rhythm: Longer sentences, structured paragraphs
- Formality: 8/10
- Special: Use construct state, passive voice allowed, no discourse markers

**Social Media:**
- Voice: Maximum slang, חחחחח, fragments, emoji, English mixing
- Rhythm: Ultra-short, high energy
- Formality: 1/10

**Business:**
- Voice: Professional but Israeli — תכל'ס survives, יאללה doesn't
- Rhythm: Medium sentences, clear structure
- Formality: 6/10

**Email:**
- Voice: Very direct, minimal greetings, terse
- Rhythm: Short sentences, short paragraphs
- Formality: 4/10 (Israeli emails are surprisingly casual)

**Creative:**
- Voice: Adapts to the story/piece — not the writer's default
- Rhythm: Maximum variance — the story dictates
- Formality: Variable

- [ ] **Step 2: Commit**

```bash
git add SKILL.md
git commit -m "feat: content type register presets"
```

### Task 14: Final assembly, before/after examples, and output formatting

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Write before/after examples**

Add 3-4 complete before/after examples showing the skill in action:

Example 1: AI-generated Hebrew blog post → humanized version
Example 2: English brief → generated Hebrew article
Example 3: Formal AI Hebrew → casual Israeli rewrite
Example 4: Detection report on AI text

Each example should be substantial enough (100+ words) to demonstrate multiple layers working together. All examples in Hebrew with brief English annotations.

- [ ] **Step 2: Write output formatting rules**

- Generate/Rewrite mode: Output clean text only (no headers, no metadata) unless --show-score
- --show-score: Append scoring breakdown after the text
- Detect mode: Structured report format
- Voice profile creation: Confirmation message with profile path

- [ ] **Step 3: Write the generation pipeline flow summary**

A concise step-by-step that ties all layers together:
1. Parse arguments (including --length target)
2. Detect content type
3. Load voice profile (if specified)
4. Enter Hebrew Mind mode
5. Determine target length: short=200-400, medium=500-800, long=1000-1500, xl=2000+, or specific number. Default: medium.
6. Generate/Rewrite using Layers 1-5, targeting the word count from step 5
7. Run self-audit loop (Layer 6) — during audit, also verify length is within 10% of target
8. Apply voice adjustments (Layer 7)
9. Output final text

- [ ] **Step 4: Final read-through of complete SKILL.md**

Read the entire file end-to-end. Check for:
- Contradictions between sections
- Missing cross-references
- Tone consistency (the skill itself should not sound like AI slop)
- Hebrew text accuracy
- YAML frontmatter validity

- [ ] **Step 5: Commit**

```bash
git add SKILL.md voice-profile-template.md
git commit -m "feat: final assembly — examples, output formatting, pipeline flow"
```

---

## Chunk 7: Testing and Validation

### Task 15: Manual testing protocol

**Files:**
- No new files — this is a testing task

- [ ] **Step 1: Test Generate mode**

Run the skill with a simple Hebrew topic:
```
/hebrew-writer "למה סטארטאפים ישראלים מצליחים" --length medium --show-score
```

Verify:
- Output is in natural Hebrew
- No AI blacklist words
- Has discourse markers and slang
- Sentence lengths vary
- Score shows 95+ (or close)
- No em dashes

- [ ] **Step 2: Test Rewrite mode**

Run with AI-generated Hebrew text:
```
/hebrew-writer "בעולם המשתנה של היום, חיוני להבין את המגוון הרחב של אתגרים ייחודיים שארגונים מתמודדים איתם. מגוון פתרונות חדשניים מוצעים על מנת להתמודד עם מציאות מורכבת זו." --mode rewrite --show-score
```

Verify:
- All AI tells removed
- Output sounds like a real Israeli wrote it
- Core meaning preserved
- Fresh sentence structures (not polished original)

- [ ] **Step 3: Test Detect mode**

Run with the same AI text:
```
/hebrew-writer "בעולם המשתנה של היום, חיוני להבין..." --mode detect
```

Verify:
- Report identifies: חיוני, מגוון, ייחודי, חדשני as AI vocabulary
- Identifies formal register in casual context
- Provides meaningful scores and recommendations

- [ ] **Step 4: Test content type switching**

Run same topic with different types:
```
/hebrew-writer "השפעת הרשתות החברתיות על בני נוער" --type social
/hebrew-writer "השפעת הרשתות החברתיות על בני נוער" --type academic
```

Verify:
- Social: slang, fragments, חחחחח, emoji-ready
- Academic: formal linking words, no slang, construct state, longer sentences

- [ ] **Step 5: Test voice profile creation**

```
/hebrew-writer --learn "paste a sample of 300+ words of someone's Hebrew writing" --save-as test-voice
```

Verify:
- Profile file created at .claude/voices/test-voice.hebrew-voice.md
- Profile captures detectable patterns from the sample

- [ ] **Step 6: Submit outputs to AI detection tools**

Take the generated text from Steps 1-2 and manually submit to:
- GPTZero (gptzero.me)
- Originality.ai
- ZeroGPT (zerogpt.com)
- Copyleaks (copyleaks.com/ai-content-detector)
- Turnitin (if accessible — requires institutional account)

Record scores. Target: below 20% AI probability on all tools. Note: Copyleaks and Turnitin use different detection architectures (AI Phrases/Source Match and BERT classifier respectively) that may catch patterns the other tools miss.

- [ ] **Step 7: Document test results**

Record all results. If any test fails, create a follow-up task to fix the specific issue.

---

## Execution Notes

### Key risks and mitigations

1. **SKILL.md may be very long (1000+ lines):** This is expected and acceptable. blader/humanizer is 559 lines, and we're building something significantly more comprehensive. The user explicitly requested maximum depth.

2. **Self-audit scoring is self-assessed:** This is a known limitation documented in the spec. The real validation comes from Task 15 (external detector testing). The internal scoring is a structured quality checklist, not an objective measurement.

3. **Hebrew text must be accurate:** Every Hebrew word, phrase, and example in the skill must be linguistically correct. Reference the research documents during implementation:
   - `hebrew_writing_research.md` — Hebrew linguistic details
   - `human-vs-ai-writing-research.md` — AI vs human patterns

4. **Skill tone matters:** The SKILL.md itself should be written in the same direct, no-nonsense style it teaches. If the skill instructions sound like AI slop, the irony would be terminal.

### Implementation order rationale

The chunks are ordered for incremental building:
1. Foundation first (can test argument parsing immediately)
2. Hebrew mechanics (Layers 1+4 — the language engine)
3. Anti-detection (Layer 2 — the largest section, pattern library)
4. Voice + rhythm (Layers 3+5 — the soul and statistics)
5. Audit + matching (Layers 6+7 — quality gate and personalization)
6. Modes + assembly (tying it all together)
7. Testing (validating the complete system)

Each chunk produces a working partial skill that can be tested independently.
