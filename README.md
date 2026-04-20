<div align="center">

# הכותב העברי
### Hebrew Writer — Claude Code Skill

**Write Hebrew that passes every AI detector and fools native Israelis.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/claude-code)
[![Lines](https://img.shields.io/badge/Skill%20Size-2%2C081%20lines-green)]()
[![Hebrew](https://img.shields.io/badge/Language-Hebrew%20%F0%9F%87%AE%F0%9F%87%B1-white)]()

<br>

*The first and most comprehensive Hebrew-native writing skill for Claude Code.*
*Built on analysis of 50+ repos, 10+ academic papers, 6 detection tools, and 500K real Israeli comments.*

</div>

---

## What This Does

You give it a topic. It writes Hebrew that sounds like a native Israeli sat down and typed it.

Not translated English. Not formal textbook Hebrew. Not "AI Hebrew" that every Israeli spots in two seconds. **Real, casual, dugri Israeli Hebrew** — the kind you'd read on Geektime, in a LinkedIn post from a Tel Aviv founder, or in a WhatsApp message from your team lead.

It passes GPTZero. It passes Originality.ai. It passes ZeroGPT, Copyleaks, and Turnitin. And more importantly — it passes the test that actually matters: a native Israeli reading it and not thinking twice.

## How It Works — 7 Layers

| Layer | What It Does |
|-------|-------------|
| **Hebrew Mind** | Forces Claude to think and compose in Hebrew from scratch — not translate from English |
| **Anti-Detection Engine** | 55+ AI patterns mapped to Hebrew, including 16-word blacklist, 6 content patterns, and 10 Hebrew-specific tells no English humanizer covers |
| **Israeli Voice** | Dugri directness, slang system (Arabic/Yiddish/English loanwords), discourse markers (כאילו, יעני, תכל'ס), humor, cultural texture |
| **Linguistic Precision** | Gender system with strategic imperfection, spelling variation, construct state vs. של, binyanim awareness, pro-drop |
| **Rhythm Engineering** | Data-driven sentence lengths (avg 8.9 words, based on 500K real comments), burstiness, perplexity injection, ellipsis, n-gram breaking |
| **Self-Audit Loop** | 8-dimension scoring (95/100 threshold), autonomous quality gate, no user intervention needed |
| **Voice Matching** | Clone any person's Hebrew writing voice from samples, save persistent profiles |

## The Big No-Nos

Five advanced AI patterns that survive every other humanizer — we catch them all:

1. **Macro Feeling Copy** — "ויש לזה מחיר אמיתי:" is a drum roll, not content. We delete it.
2. **Presentation Slide Structure** — Parallel stacked points pretending to be prose. We weave them into the argument.
3. **LinkedIn Punchline Syndrome** — "הפתרון הכי חכם הוא הפתרון הכי פשוט" belongs on a sunset poster, not in writing.
4. **Disconnected Temperature** — Flat emotional energy throughout. Real writers care more about some parts.
5. **"Not X, It's Y" Addiction** — The most common AI essay structure. We break it.

## Quick Start

### Install (Global — works everywhere)

```bash
mkdir -p ~/.claude/skills/hebrew-writer
curl -sL https://raw.githubusercontent.com/baldiga/hebrew-writer/main/SKILL.md \
  -o ~/.claude/skills/hebrew-writer/SKILL.md
curl -sL https://raw.githubusercontent.com/baldiga/hebrew-writer/main/voice-profile-template.md \
  -o ~/.claude/skills/hebrew-writer/voice-profile-template.md
```

### Install (Project-scoped)

```bash
mkdir -p .claude/skills/hebrew-writer
curl -sL https://raw.githubusercontent.com/baldiga/hebrew-writer/main/SKILL.md \
  -o .claude/skills/hebrew-writer/SKILL.md
curl -sL https://raw.githubusercontent.com/baldiga/hebrew-writer/main/voice-profile-template.md \
  -o .claude/skills/hebrew-writer/voice-profile-template.md
```

### Use It

```bash
# Generate from scratch
/hebrew-writer "למה סטארטאפים ישראלים מצליחים"

# Rewrite AI-sounding Hebrew
/hebrew-writer "הטכנולוגיה המתקדמת מהווה גורם מכריע..." --mode rewrite

# Scan text for AI tells
/hebrew-writer "paste your text here" --mode detect

# Clone someone's voice
/hebrew-writer --learn "paste their writing" --save-as avi
/hebrew-writer "topic" --voice avi

# Control length, type, gender
/hebrew-writer "topic" --length long --type academic --gender female
```

## Modes

| Mode | What It Does |
|------|-------------|
| `generate` | Write from scratch. Give it a topic, get a complete piece. |
| `rewrite` | Transform existing content (Hebrew, English, or mixed) into natural Israeli Hebrew. |
| `detect` | Scan text and report AI patterns found. No changes — diagnosis only. |

## Content Types

Auto-detected or manually set with `--type`:

| Type | Register | Example |
|------|----------|---------|
| `blog` | Casual Israeli (3/10 formality) | Blog posts, LinkedIn, articles |
| `academic` | Formal with Israeli backbone (8/10) | Papers, research, academic writing |
| `social` | Maximum casual (1/10) | Twitter, Facebook, Instagram |
| `business` | Professional but Israeli (6/10) | Reports, proposals |
| `email` | Direct and terse (4/10) | Israeli business emails |
| `creative` | Adapts to story voice | Fiction, poetry, narrative |

## Voice Cloning

Feed it a sample of someone's writing. It learns their patterns and writes as them.

```bash
# Learn from inline text (minimum 200 words)
/hebrew-writer --learn "paste 200+ words of their writing here" --save-as danny

# Learn from a file
/hebrew-writer --learn --my-voice-file path/to/writing.md --save-as danny

# Learn from multiple files (highest accuracy)
/hebrew-writer --learn --my-voice-files path/to/folder/ --save-as danny

# Use saved voice
/hebrew-writer "topic" --voice danny
```

**Accuracy tiers:** 200-500 words = basic match | 500-1500 = strong | 1500+ = full clone | multiple files = maximum

## Self-Audit Scoring

Every piece is internally scored on 8 dimensions before output:

| Dimension | Weight | What It Measures |
|-----------|--------|-----------------|
| דוגריות (Directness) | 13% | Does it take a position? |
| קצב (Rhythm) | 14% | Sentence length variation |
| אמינות (Authenticity) | 14% | Could an Israeli have written this? |
| טקסטורה (Texture) | 11% | Slang, register shifts, word surprises |
| נשמה (Soul) | 12% | Emotions, opinions, visible thinking |
| צפיפות (Density) | 8% | Every sentence earns its place |
| רישום (Register) | 8% | Formality matches content type |
| אנטי-זיהוי (Anti-Detection) | 20% | Statistical fingerprint clean |

**Threshold: 95/100.** No dimension below 8/10. Fully autonomous — no mid-process prompts.

Add `--show-score` to see the breakdown.

## Research Foundation

This skill wasn't built on vibes. It's backed by:

- **6 AI detection tools analyzed** — GPTZero, Originality.ai, Turnitin, Copyleaks, ZeroGPT, Sapling
- **10+ academic papers** — DetectGPT (ICML 2023), Binoculars (ICML 2024), RAID Benchmark (ACL 2024), and more
- **50+ GitHub repos reviewed** — blader/humanizer, humanizalo, stop-slop, anti-ai-slop-writing, unslop
- **500,000 real Israeli comments analyzed** — HeBERT UGC dataset from Tel Aviv University
- **Real Geektime articles analyzed** — for authentic Israeli tech writing patterns
- **Hebrew linguistics research** — Academy of Hebrew Language standards, discourse marker studies, morphological analysis

## What Makes This Different

| Feature | Other Humanizers | Hebrew Writer |
|---------|-----------------|---------------|
| Language | English only | Hebrew-native from the ground up |
| Hebrew-specific tells | None | 10 patterns no English skill covers |
| Data-driven | Based on theory | Based on 500K real Israeli comments |
| Cultural authenticity | N/A | Israeli dugri culture, slang, humor |
| Sentence length | Theory-based (15-20 words) | Data-driven (avg 8.9 words) |
| Voice cloning | Some | Full system with persistent profiles |
| Quality threshold | 42/60 or 80/100 | 95/100 with 8-dimension scoring |
| Advanced patterns | Vocabulary + formatting | Big No-Nos: macro copy, slide structure, punchlines, temperature, X-vs-Y |

## Compatibility

Works with Claude Code, Cursor, VS Code, Codex CLI, Gemini CLI, Windsurf, Continue.dev, and OpenClaw.

## Files

| File | Lines | What It Is |
|------|-------|-----------|
| `SKILL.md` | 2,081 | The complete skill — all 7 layers |
| `voice-profile-template.md` | 118 | Template for saved voice profiles |

## License

MIT — use it, share it, improve it.

---

<div align="center">

**Built with obsessive attention to what makes Hebrew sound human.**

</div>
