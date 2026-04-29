# Hebrew Writer Skill Design Spec (הכותב העברי)

**Date:** 2026-04-20
**Status:** Approved
**Approach:** Full Shadow Writer System (Approach C)

---

## 1. Overview

A Claude Code skill that writes Hebrew content indistinguishable from a native Israeli human writer — passing all AI detection tools (GPTZero, Originality.ai, ZeroGPT, Copyleaks, Turnitin) AND fooling native Hebrew-speaking Israelis.

**Target audience for generated content:** Native Israeli Hebrew speakers (the hardest judges).
**Default voice:** Casual/colloquial Israeli Hebrew — how Israelis actually talk and write.
**Content scope:** Universal — blog, academic, social media, business, creative. Like a human writer who can do it all.
**Skill size:** Comprehensive — every technique packed in, no length constraints.
**Known scope limitation:** Optimized for Israeli Hebrew speakers. Diaspora, religious, and heritage Hebrew contexts may require manual --type adjustments.

---

## 2. Architecture

### 2.1 Seven Core Layers

| Layer | Name | Purpose |
|-------|------|---------|
| 1 | **Hebrew Mind** | Forces Claude to think, reason, and compose in Hebrew from the start — not translate from English |
| 2 | **Anti-Detection Engine** | 45+ AI patterns mapped to Hebrew equivalents, plus 10 Hebrew-specific tells |
| 3 | **Israeli Voice Injection** | Dugri directness, slang, discourse markers, cultural authenticity |
| 4 | **Linguistic Precision** | Root morphology, binyanim, gender, register, spelling variation |
| 5 | **Rhythm & Statistics** | Burstiness injection, perplexity variation, Zipf's law conformity, sentence length variance |
| 6 | **Self-Audit Loop** | 4-iteration autonomous loop, 8-dimension scoring, 95/100 threshold |
| 7 | **Voice Matching** | Default Israeli voice OR clone any person's voice from samples with persistent profiles |

### 2.2 Three Operating Modes

- **Generate** — From scratch. Give it a topic/brief, it produces the entire piece.
- **Rewrite** — Transform existing content into human-sounding Hebrew. Input can be in Hebrew (polish/humanize), English (translate + humanize), or transliterated Hebrew (convert + humanize). The skill first extracts core meaning, then generates fresh Hebrew using all layers — never word-for-word translation.
- **Detect** — Scan-only report. No changes. Output format: (1) Overall human-likeness score (0-100), (2) List of detected AI patterns with location and severity, (3) Per-dimension scores on the 8 Hebrew scoring dimensions, (4) Specific recommendations for manual fixes.

### 2.3 Auto-Adaptive Content Types

The skill auto-detects content type from context (topic keywords, length cues, explicit --type flag) and adjusts register, vocabulary, structure:
- Blog/article (שיווק תוכן)
- Academic/professional (כתיבה אקדמית)
- Social media (רשתות חברתיות)
- Business communications (תקשורת עסקית)
- Email/correspondence (דוא"ל) — Israeli email style: very direct, minimal greetings, often terse
- Creative writing (כתיבה יצירתית)

**Content type vs. voice interaction:** When --type overrides the default casual voice:
- Academic: Layer 3 (Israeli Voice) shifts to formal register — dugri directness preserved but slang removed, discourse markers replaced with formal linking words, cultural references kept subtle
- Business/Email: Layer 3 moderates — תכל'ס and דוגרי survive (they cross registers), but יאללה/סבבה/אחי are dropped
- Social: Layer 3 at maximum — full slang, חחחחח, emoji, fragments
- Creative: Layer 3 adapts to the story's voice — not the writer's default

---

## 3. Layer 1: Hebrew Mind

### 3.1 Hebrew-First Composition

All internal reasoning about word choice, sentence structure, and argumentation happens in Hebrew logic. No English intermediary. Concrete implementation: the skill instructs Claude to "plan your response structure, choose your arguments, and select your vocabulary entirely in Hebrew. Do not formulate ideas in English and translate. When considering word choices, think of Hebrew synonyms directly, not English words to translate."

### 3.2 Core Principles

1. **SVO with flexible emphasis** — Default Modern Hebrew word order, but naturally front elements for topicalization like native speakers do. Example: "את זה אני לא מבין" instead of rigid "אני לא מבין את זה."

2. **Pro-drop awareness** — Drop subject pronouns when verb conjugation carries the information:
   - Past (1st/2nd person): Drop. כתבתי not אני כתבתי
   - Future: Drop. אכתוב not אני אכתוב
   - Present: Keep. אני כותב (present tense needs pronoun)
   - Emphasis/contrast: Keep. אני כתבתי את זה, לא הוא

3. **Nominal sentences** — Use verbless sentences naturally. "הספר מעניין" (the book is interesting), "הבעיה ברורה" (the problem is clear). AI forces verbs everywhere; Hebrew doesn't need them.

4. **Morphological thinking** — Exploit root-family connections. When writing about כתיבה (writing), naturally associate מכתב (letter), כתבה (article), כתב (reporter). Think in שורשים.

5. **Hebrew sentence economics** — Hebrew words pack more information than English. A single word like שנפגשנו = "that we met" (3 English words). Leverage this compactness for natural rhythm.

6. **Register autopilot** — Default to casual Israeli register. Auto-detect when context demands formal/academic and shift accordingly.

7. **Sentence-initial particles** — Naturally start sentences with ו (ve), אז (az), אבל (aval), גם (gam), כי (ki). Textbooks discourage this; real Israelis do it constantly.

---

## 4. Layer 2: Anti-Detection Engine

### 4.1 Category A: Universal AI Tells (Mapped to Hebrew)

40+ patterns from Wikipedia's "Signs of AI Writing," research papers, and existing humanizer skills, translated to Hebrew manifestations:

**Hebrew AI Vocabulary Blacklist:**
- מגוון (diverse), מרתק (fascinating), חיוני (vital), מהותי (essential), ייחודי (unique), רב-ממדי (multifaceted), מקיף (comprehensive), חדשני (innovative), פורץ דרך (groundbreaking), חסר תקדים (unprecedented), מתקדם (advanced), משמעותי (significant — when used for inflation), מרכזי (central/pivotal), בולט (prominent), רלוונטי (relevant — overused)
- Replace with plain Hebrew or delete entirely.

**Hebrew Copula Avoidance:**
- AI writes "משמש כ..." (serves as), "מהווה" (constitutes), "עומד בבסיס" (stands at the base of)
- Fix: Use nominal sentences or simple הוא/היא/זה

**Hebrew Significance Inflation:**
- "מהווה אבן דרך משמעותית" (constitutes a significant milestone)
- "מסמל את" (symbolizes the)
- "משקף מגמה רחבה יותר" (reflects a broader trend)
- Fix: State what the thing IS or DOES. Cut the commentary.

**Hebrew Formulaic Transitions:**
- Overuse of: בנוסף (in addition), יתר על כן (furthermore), לסיכום (in conclusion), כמו כן (likewise), אי לכך (therefore — formal)
- Fix: Use natural connectors: אז, גם, חוץ מזה, ובכלל, and sentence-initial particles

**Formatting Tells:**
- Em dash (—): Zero tolerance. Replace with commas, periods, parentheses.
- Rule of three: Don't force triads. Two is underrated. Four is fine.
- Bold overuse: Strip mechanical emphasis.
- Title Case headings: Use sentence case.

**Hedging Pile-ups:**
- AI Hebrew: ייתכן שאולי אפשר לטעון ש... (stacked qualifiers)
- Fix: Pick ONE hedge or state it directly.

**Superficial -ing Equivalents in Hebrew:**
- AI uses תוך הדגשת (while emphasizing), תוך שימת דגש על (while placing emphasis on), המשקף את (reflecting the)
- Fix: Delete or promote to own sentence with source.

**Negative Parallelisms:**
- "זה לא רק X, זה גם Y" / "לא מדובר ב-X אלא ב-Y"
- Fix: State Y directly. Once per piece maximum.

### 4.2 Category B: Hebrew-Specific AI Tells

10 patterns unique to AI-generated Hebrew that no English humanizer covers:

1. **Over-formality syndrome** — Using לפיכך, בשל כך, כאשר where אז, בגלל, כש- are natural
2. **Missing dugri energy** — Too polite, too hedged, too balanced. Israelis are assertive.
3. **Sanitized vocabulary** — Missing slang: סבבה, יאללה, תכל'ס, אחי, סתם, באסה
4. **Too-perfect grammar** — No gender mistakes with irregular nouns, no spelling inconsistencies
5. **Over-consistent spelling** — Using one ktiv male variant uniformly when natives mix them
6. **Missing pro-drop** — Including unnecessary pronouns (אני, הוא, היא) everywhere
7. **Wrong construct vs. של** — Using סמיכות in casual contexts or של in formal ones
8. **Missing discourse markers** — No כאילו, יעני, בעצם, נו in informal writing
9. **Register-deaf connectors** — Using formal linking words in casual content
10. **Absent cultural texture** — No IDF references, no Israeli humor, no cultural markers

### 4.3 Category C: Statistical Fingerprint Elimination

Targets classifier-based detection (GPTZero, Originality.ai, Turnitin):

- **Uniform sentence length** — AI clusters at 15-20 words. Enforce variance (3-40+).
- **Low burstiness** — Inject deliberate complexity variation between sentences.
- **Predictable token distribution** — Increase perplexity through unexpected word choices.
- **N-gram uniformity** — Break repetitive n-gram patterns.
- **Zipf's law deviation** — Ensure word frequency distribution matches human baselines.

---

## 5. Layer 3: Israeli Voice Injection

### 5.1 The Dugri Principle (עקרון הדוגריות)

The core personality engine:

- **Say it straight.** Don't dance around the point. If something is bad, say it's bad.
- **Have a take.** Neutral, balanced writing is the biggest AI tell in Hebrew. Israelis have opinions.
- **Warmth through directness.** Israeli directness isn't cold — it's intimate. It says "I respect you enough to be honest."

### 5.2 Slang & Loanword System

Natural sprinkling, context-aware:

| Element | When to use | When NOT to use |
|---------|-------------|-----------------|
| סבבה, יאללה, אחי | Casual blog, social media | Academic papers |
| תכל'ס, דוגרי | Semi-formal too — these cross registers | Legal/medical |
| English loanwords (קונטנט, סטארטאפ, פידבק) | Tech, business, lifestyle | Literary, academic |
| Arabic-origin (יאללה, סחתיין, חלאס) | Casual, adds flavor | Formal contexts |
| IDF slang (חפ"ש, מילואים, תותחן) | Cultural references | When audience isn't Israeli |

### 5.3 Discourse Marker Injection

Natural filler that signals a human brain at work:

- כאילו (ke'ilu) — hedge, filler, self-correction marker
- יעני (ya'ani) — "I mean," clarification
- בעצם (be'etsem) — "actually," reframing
- נו (nu) — Yiddish-origin urging
- אז (az) — natural transition
- נכון? (nakhon?) — confirmation-seeking tag

Rule: Research shows ~12% of Hebrew speech tokens are discourse/filler words (~8% social words + ~3% discourse markers). In casual writing, aim for ~3-5% discourse markers (lower than speech but present). In semi-formal writing, ~1-2%. In academic, near zero — use formal linking words instead.

### 5.4 Humor & Cultural Texture

- Self-deprecating humor (very Israeli)
- Specific cultural references (not generic)
- The חחחחח convention for written laughter
- Sarcasm and irony — Israelis love it, AI avoids it
- References to shared experiences (army service, weather complaints, bureaucracy frustration)

### 5.5 Emotional Authenticity

- Mixed feelings expressed openly: "אני לא יודע מה לחשוב על זה"
- Strong opinions followed by doubt: "זה מעולה. או שלא. אני צריך לחשוב על זה עוד פעם"
- Mood bleeds into writing naturally — fatigue, excitement, frustration

---

## 6. Layer 4: Linguistic Precision

### 6.1 Gender System

- Correct verb/adjective/number agreement with subject gender
- Audience gender detection and appropriate form usage
- Counterintuitive numeral rule (feminine nouns take masculine-form numerals)
- Strategic imperfection — occasionally make native-speaker gender mistakes: treating צומת (masculine) as feminine, hesitating on גרב. Calibration: ~1 gender imperfection per 800-1000 words, ~1-2 spelling inconsistencies per 500 words. Too many = non-native writer. Too few = AI. This range matches observed native speaker error rates.

### 6.2 Spelling Variation (כתיב מלא)

- Alternate between accepted variants: מזרן/מזרון, צוהריים/צהריים
- Don't apply Academy 2017 rules uniformly — most Israelis ignore them
- Mix ktiv male conventions naturally
- Occasional spelling "drift" within the same document

### 6.3 Construct State vs. של

| Context | Use | Example |
|---------|-----|---------|
| Formal/literary | סמיכות (construct) | בית הספר, שר החינוך |
| Casual/informal | של (analytic) | הבית של סבתא, החבר של דני |
| Frozen expressions | Always construct | בית ספר, בית חולים |
| Natural mixing | Both in same text | Natives do this constantly |

### 6.4 Binyanim Awareness

- Pa'al for basic actions (כתב, אכל, הלך)
- Pi'el for intensive/causative naturally (דיבר, סיפר, ניקה)
- Hitpa'el for reflexive naturally (התלבש, התרחץ, הסתדר)
- Nif'al middle voice — the nuance AI misses. "הדלת נפתחה" (opened by itself) vs. passive with agent
- Avoid overusing passive binyanim (Pu'al, Huf'al) — AI defaults to passive too much

### 6.5 Sentence-Initial Particles

Native speakers start sentences with ו, אז, אבל, גם, כי. Textbooks discourage it. The skill embraces it.

---

## 7. Layer 5: Rhythm & Statistical Anti-Detection

### 7.1 Burstiness Engineering

Aspirational target: burstiness coefficient ~0.61 (human average) vs AI's ~0.38. Note: Claude cannot compute this numerically during generation. Instead, these concrete rules approximate the target:

- **The 3-40 rule:** Sentence lengths must range from 3 words to 40+ words within any paragraph
- **Never 3+ consecutive sentences of similar length**
- **At least one fragment per 500 words** (one-word or two-word sentence)
- **At least one sentence exceeding 30 words per 500 words**
- Rhythm patterns: Long -> Short -> Medium -> Very Short -> Long

### 7.2 Perplexity Injection

- Unexpected word choices — instead of the most probable next word, occasionally choose the third or fourth most natural option
- Hebrew synonym variance — rotate informal phrasings instead of always using the "dictionary" word
- Domain-crossing vocabulary — cooking metaphors in tech writing, military analogies in business (very Israeli)
- Consciously deviate from the "obvious" word choice ~20-30% of the time

### 7.3 Vocabulary Diversity (Zipf's Law Inspired)

Note: Zipf's law conformity is an emergent statistical property that cannot be directly controlled word-by-word. These concrete rules approximate human-like word frequency distributions:

- Include words that appear only once in the text (hapax legomena) — reach for the unusual word when it fits
- Use wider vocabulary range including rare/uncommon Hebrew words rather than recycling common ones
- Vary function word frequencies (את, של, על, עם) rather than uniform rates
- Avoid the "safe word" trap — don't always pick the most common synonym

### 7.4 N-gram Pattern Breaking

- No repeated sentence openers across paragraphs
- Avoid repeating the same connector within 300 words (soft rule — Hebrew has a smaller casual connector inventory, so repeating אז or אבל is acceptable when alternatives sound forced)
- Alternate clause structures: simple, compound, complex, fragments
- Vary paragraph openings: verb, question, number, quote, one-word reaction, mid-thought

### 7.5 Paragraph Length Variance

- One-sentence paragraphs allowed and encouraged occasionally
- Range from 2-3 sentences to 6-7 sentences
- Never more than 2 consecutive paragraphs of similar length

---

## 8. Layer 6: Self-Audit Loop

### 8.1 Process

```
Write -> Detect -> Score -> 95+? -> Output
                              | No
                       Rewrite -> Detect -> Score -> 95+? -> Output
                                                      | No
                                               Rewrite -> Detect -> Score -> 95+? -> Output
                                                                              | No
                                                                    Emergency Rewrite (full rebuild)
                                                                              -> Score -> Output best version
```

### 8.2 Design Principles

- **Integrated checklist approach** — The self-audit loop is implemented as an internal mental checklist within a single generation pass. Claude writes the initial draft, then mentally reviews it against all 8 dimensions, identifies weaknesses, and revises inline before outputting. This is NOT a multi-turn external loop — it is a structured internal revision process within one response.
- **4-phase internal revision** — Phase 1: Generate draft. Phase 2: Score against 8 dimensions. Phase 3: Identify dimensions below 9.5 and rewrite those sections. Phase 4: Final scan for surviving AI patterns. If the mental score still feels below threshold, Phase 5: Emergency approach — set aside the draft, extract only the core meaning as bullet points, and rewrite from scratch.
- **Fully autonomous** — no user prompts mid-process, no "would you like me to try again?"
- **Minimum dimension score: 8/10** — no single dimension can score below 8, even if weighted total hits 95. Note: mathematically, achieving 95/100 weighted requires most dimensions at 9-10 since all-8s yields only 80 and all-9s yields only 90.

### 8.3 Self-Detection Scan

Each iteration scans for:
- Surviving AI patterns from the 45+ pattern list
- Statistical uniformity (sentence length, paragraph length)
- Register mismatches
- Missing human texture (slang, discourse markers, opinions, emotion)
- Any word from the Hebrew AI vocabulary blacklist
- Gender/spelling that's too consistent

### 8.4 Eight Hebrew-Specific Scoring Dimensions

| Dimension | Question | Weight |
|-----------|----------|--------|
| דוגריות (Directness) | Does it get to the point? No dancing around? | 13% |
| קצב (Rhythm) | Is sentence length varied? Does burstiness feel natural? | 14% |
| אמינות (Authenticity) | Could you picture a specific Israeli writing this? | 14% |
| טקסטורה (Texture) | Does it have slang, cultural references, discourse markers? | 11% |
| נשמה (Soul) | Does it have opinions, emotions, a human behind it? | 12% |
| צפיפות (Density) | Is every sentence earning its place? Nothing cuttable? | 8% |
| רישום (Register) | Does formality level match the content type? | 8% |
| אנטי-זיהוי (Anti-Detection) | Would a statistical detector flag this? Sentence length variance, word choice predictability, n-gram patterns? | 20% |

### 8.5 Scoring Thresholds

- **95+**: Pass. Output the text.
- **Below 95**: Targeted rewrite focusing on weakest dimensions.
- **Iteration 4**: Emergency full rebuild from scratch.
- **Final output**: Always the highest-scoring version produced.

---

## 9. Layer 7: Voice Matching & Personalization

### 9.1 Two Operating Modes

**Mode A: Default Voice** — Built-in Israeli casual voice (dugri, opinionated, slang-sprinkled). The out-of-box experience.

**Mode B: Voice Clone** — Analyzes a writing sample and replicates that specific person's Hebrew voice.

### 9.2 Voice Analysis Engine

Extracts 10 features from writing samples:

| Feature | What it measures |
|---------|-----------------|
| אורך משפטים | Average and range of sentence lengths |
| רמת פורמליות | Register position on formal-casual spectrum |
| מילות מילוי | Which discourse markers they favor |
| מבנה טיעון | How they build arguments |
| פיסוק | Punctuation habits |
| אוצר מילים | Vocabulary level and preferences |
| אנגלית | English loanword frequency |
| הומור | Humor style |
| חוות דעת | How they express opinions |
| פתיחות | How they open paragraphs/sections |

### 9.3 Sample Input Methods

| Method | Command | Best for |
|--------|---------|----------|
| Inline | `--my-voice "paste 200+ words"` | Quick one-off |
| File | `--my-voice-file path/to/writing.md` | Reusable |
| Multi-sample | `--my-voice-files path/to/folder/` | Highest accuracy |

### 9.4 Accuracy Tiers

| Sample size | Level | Captures |
|-------------|-------|----------|
| 200-500 words | Basic | Sentence length, formality, basic vocabulary. Expectations: rough approximation, not a perfect clone. |
| 500-1500 words | Strong | + Humor, argument structure, discourse markers, punctuation. Solid voice match. |
| 1500+ words | Full Clone | + Emotional patterns, opinion style, cultural reference density, paragraph rhythm. High-fidelity match. |
| Multiple texts | Maximum | + Cross-document consistency, topic-dependent variation, full range. Professional ghostwriter level. |

### 9.5 Persistent Voice Profiles

Voice profiles saved as `.hebrew-voice.md` files in the project's `.claude/voices/` directory (or `~/.claude/voices/` for global profiles):

```
/hebrew-writer --learn "text or path" --save-as "profile-name"
```

Creates `.claude/voices/profile-name.hebrew-voice.md`. Reusable:

```
/hebrew-writer "topic" --voice profile-name
```

Multiple profiles supported — your voice, a friend's voice, a client's voice.

### 9.6 Voice Profile Contents

Internal profile structure (generated automatically):

```
VOICE PROFILE:
- Sentences: pattern description
- Register: formality level and habits
- Markers: preferred discourse markers and frequency
- Arguments: argumentation structure
- Punctuation: habits and preferences
- English: code-switching ratio
- Humor: style description
- Opinions: expression pattern
- Mistakes: characteristic errors (authenticity markers)
- Cultural: reference density and type
```

### 9.7 The Ghostwriter Rule

Match their voice, not yours. If the sample person writes in short fragments, don't produce flowing paragraphs. If they never use סבבה, don't inject it. If they overuse אז, overuse it too.

---

## 10. Command Interface

### 10.1 Primary Commands

```
/hebrew-writer "topic or brief"                    # Generate from scratch (default voice)
/hebrew-writer "text to transform" --mode rewrite  # Rewrite existing content
/hebrew-writer "text to scan" --mode detect        # Scan only, report patterns
/hebrew-writer "topic" --voice danny               # Use saved voice profile
/hebrew-writer --learn "sample text" --save-as danny  # Create voice profile
/hebrew-writer "topic" --my-voice "inline sample"  # One-off voice matching
/hebrew-writer "topic" --type academic             # Force content type
/hebrew-writer "topic" --show-score                # Show scoring breakdown in output
```

### 10.2 Arguments

| Argument | Values | Default |
|----------|--------|---------|
| --mode | generate, rewrite, detect | generate |
| --voice | profile name | default (Israeli casual) |
| --my-voice | inline text sample | none |
| --my-voice-file | file path | none |
| --my-voice-files | folder path | none |
| --learn | text or file path (with --save-as) | none |
| --save-as | profile name | none |
| --type | blog, academic, social, business, email, creative, auto | auto |
| --show-score | flag | off |
| --length | short (200-400 words), medium (500-800), long (1000-1500), xl (2000+), or specific number | medium |
| --gender | male, female, neutral | auto-detect from context |

**Gender parameter clarification:** `--gender` controls the gender of the WRITER's voice (first-person conjugations, self-references). It does NOT control the audience's gender. Examples: `--gender female` means the writer is female, so verbs conjugate accordingly (אני חושבת not אני חושב). When addressing a specific audience, the skill infers audience gender from context or uses masculine default per Israeli convention.

---

## 11. Research Foundation

### 11.1 AI Detection Tools Analyzed

- GPTZero: 7-layer system, perplexity + burstiness + sentence-level HMM classifier
- Originality.ai: Custom ELECTRA architecture, 160GB training data, BERT-based discriminator
- Turnitin: Sentence-level BERT classifier (AIW-2), 0.33% false positive rate
- Copyleaks: Three-layer system (AI Phrases + AI Source Match + AI Logic)
- ZeroGPT: DeepAnalyse multi-stage analysis
- Sapling: Dual approach (whole-text + per-sentence perplexity)

### 11.2 Academic Research Incorporated

- DetectGPT (Mitchell et al., ICML 2023): Log probability curvature
- Fast-DetectGPT (Bao et al., ICLR 2024): Conditional probability curvature
- Binoculars (Hans et al., ICML 2024): Dual-LLM perplexity ratio
- Kirchenbauer et al. (ICML 2023): Watermarking via green/red token lists
- Liang et al. (2023): Detector bias against non-native writers
- RAID Benchmark (ACL 2024): Cross-domain detection evaluation
- NeurIPS 2024 DeTeCtive: Multi-level contrastive learning
- Pangram Labs: Frequency analysis (phrases appearing 294,000x more in AI text)

### 11.3 Existing Skills Analyzed

- blader/humanizer (14,629 stars): 29 patterns, two-pass audit, voice calibration
- Humanizer-zh (6,328 stars): Chinese localization proving framework adaptability
- Aboudjem/humanizer-skill: 37 patterns, 5 voice profiles, burstiness/perplexity basis
- Humanizalo (Hainrixz): 40 patterns, self-audit loop, 6-dimension scoring
- jpeggdev/humanize-writing: 8-pass editing process
- stop-slop, anti-ai-slop-writing, unslop: Additional pattern lists

### 11.4 Hebrew-Specific Research

- Dicta project (dicta-il): State-of-the-art Hebrew NLP
- Hebrew tokenization: ~5.81 tokens/word (4x English cost)
- Hebrew training data: 0.5% of internet content
- 10 telltale signs of AI-generated Hebrew identified
- 6 categories of native speaker mistakes (authenticity markers)
- Hebrew discourse markers: ~12% of speech tokens
- focusai-labs/hebrew-lint: Academy of Hebrew Language writing standards

---

## 12. Key Differentiators

What makes this skill unique vs. all existing humanizers:

1. **First Hebrew-native writing skill** — No existing skill targets Hebrew specifically
2. **Hebrew-first thinking** — Not a translation layer; thinks in Hebrew from the ground up
3. **Hebrew-specific AI tell detection** — 10 patterns no English humanizer covers
4. **Cultural authenticity** — Israeli dugri culture, slang, humor, references
5. **Linguistic depth** — Binyanim, gender, spelling variation, pro-drop, construct state
6. **Scientific anti-detection** — Targets the actual math behind every major detector
7. **95/100 autonomous quality gate** — Highest threshold of any existing skill
8. **Persistent voice profiles** — Learn a person's voice once, write as them forever
9. **Universal content types** — Same skill handles casual to academic
10. **Research depth** — Built on analysis of 50+ repos, 10+ academic papers, 6+ detection tools

---

## 13. Validation Plan

### 13.1 Success Criteria

The skill is considered successful when:
1. Generated text scores below 20% AI probability on GPTZero, Originality.ai, ZeroGPT, Copyleaks, and Turnitin
2. Native Israeli Hebrew speakers cannot distinguish skill output from human writing in blind tests
3. The skill works autonomously across all content types without user intervention

### 13.2 Testing Protocol

1. **Detection tool testing:** Generate 10+ pieces across all content types, submit each to all 5 major detection tools, record scores
2. **Native speaker test:** Share outputs with native Israeli Hebrew speakers without disclosure, ask them to rate naturalness 1-10
3. **Regression testing:** After any skill modification, re-run detection tool tests to ensure no regression
4. **Edge case testing:** Test with very short (tweet-length) and very long (2000+ word) content, academic register, social media register, and voice-cloned output

### 13.3 Detection Tool Volatility

Detection tools update their models regularly. The skill's anti-detection strategies target fundamental statistical properties (burstiness, perplexity, vocabulary diversity) rather than tool-specific exploits. This makes the skill more resilient to detector updates. However, periodic re-testing is recommended.

### 13.4 Known Limitations

- Voice cloning at 200-word tier is approximate, not precise
- Highly technical/domain-specific content may require manual review
- The skill optimizes for Israeli Hebrew; diaspora/religious Hebrew contexts may need --type adjustments
- Self-scoring is Claude assessing its own output — not externally validated. Real-world testing against detection tools is the true measure
