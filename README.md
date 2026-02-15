# 🛡️ PromptShield v2 – AI Detection & Encoding Tool

> ⚠️ **Experimental Prototype**
> Heuristic-only detection has inherent accuracy limits (~55–65%). Use as one lens among many — not as definitive proof of AI use.

**[Live Demo](https://adrianspeyer.github.io/Prompt-Shield/)** · Built with [Speyer UI](https://github.com/adrianspeyer/speyer-ui)

---

## How It Works: Two Layers of Detection

PromptShield uses a two-layer approach designed for educators:

**Layer 1 — Encode & Trap (high confidence).** Before sharing an assignment, the teacher pastes their instructions into PromptShield's Encode mode. The tool wraps the text in invisible zero-width character watermarks and injects a subtle "bait phrase" that reads like a natural instruction. The teacher gives this encoded version to students. If a student copies those instructions directly into ChatGPT, Claude, or any AI and pastes back the response, the watermark and bait phrase will likely survive in the output. When the teacher runs the submission through Detect mode, those proprietary signals light up immediately. This isn't guessing — it's a trap that either triggers or doesn't.

**Layer 2 — Standalone Heuristic Detection (directional).** For students who are less obvious — they rephrase, use their own prompt, or only paste the AI's answer — PromptShield falls back to 23 heuristic rules that check for statistical, vocabulary, structural, and stylistic patterns common in AI-generated text. This layer is transparent: every signal, its weight, and whether it fired are visible. It's honest about its ~55–65% accuracy ceiling and is designed to help teachers decide whether to investigate further with commercial tools (GPTZero, Turnitin, Copyleaks) or have a conversation with the student — not to serve as a verdict on its own.

The two layers work together: Layer 1 catches the lazy copy-paste. Layer 2 catches patterns that suggest AI involvement even when the original instructions weren't used.

---

## What Changed in v2

PromptShield v2 is a ground-up rewrite of the detection engine based on 2025–2026 AI detection research. The original 20-rule engine was written in mid-2025; this version adds the signals that research has since identified as the strongest discriminators.

**New detection signals:**
- **Burstiness** and **Sentence Uniformity** — the two strongest statistical discriminators per current research. AI text has remarkably uniform sentence lengths; human writing is spiky.
- **AI Vocabulary Fingerprinting** — 28 words AI overuses (delve, harness, tapestry, realm, etc.) now tracked.
- **Model Attribution** — detects phrases specific to ChatGPT, Claude, and Gemini.
- **Participial Clause Overuse** — ", [verb]-ing" patterns at 2–5× human rates.
- **"From X to Y" Constructions** — AI-favored range patterns.
- **Contrastive Focus** — "While X, Y" / "Not only X, but also" overuse.
- **Perfect Grammar Detection** — zero contractions + zero typos is now an AI signal.
- **Balanced Perspectives** — artificial balance in every argument (safety-tuning artifact).
- **Human Tells** — typos, colloquial language, and inconsistent spacing now *reduce* the AI score.

**Removed/replaced:**
- FewContractions (reversed by model tuning — AI now uses contractions)
- RhetoricalQuestions (too many false positives on formal writing)
- EllipsisUsage (unreliable)
- NumericDensity, SentimentUniformity, EmojiUsage, ComplexPunctuation (low signal)

**Other changes:**
- UI rebuilt with Speyer UI System (SUI) v2.0.7 — light/dark mode, WCAG 2.1 AA accessible
- Confidence indicator warns when text is too short for reliable analysis
- Signals organized into 6 categories instead of a flat list
- Code split into `index.html` + `engine.js` for maintainability

---

## 🚀 Quick Start

### ✍️ Encode an Assignment

1. Switch to **Encode** mode.
2. Paste your assignment instructions.
3. Click **Encode** — this will:
   - Wrap your text in a randomized zero-width watermark.
   - Inject one "bait" phrase at the first paragraph break.
4. Click **Copy to Clipboard** and share the encoded instructions with students.

### 🔍 Detect AI Use

1. Switch to **Detect** mode.
2. Paste the student's essay or text.
3. Adjust **Strictness** (0.5×–2×) to scale sensitivity.
4. Click **Analyze** to get:
   - **AI Match Score** (0–10) with color-coded ring
   - **Confidence indicator** (low/moderate/higher based on word count)
   - **Model attribution** (if ChatGPT/Claude/Gemini patterns detected)
   - **Signal breakdown** across 6 categories
   - **Factor bar chart**
5. (Optional) **Export JSON** for record-keeping.

---

## 🚨 Detection Signals (v2)

### 🔒 Proprietary Signals

| Signal | Weight | Description |
| --- | --- | --- |
| **Watermark** | 5.0 | Invisible zero-width markers (ZWSP, ZWNJ, etc.) from encoded instructions |
| **Bait Phrase** | 3.0 | One of the encoded trap phrases detected in submission |

### 📊 Statistical Analysis

| Signal | Weight | Description |
| --- | --- | --- |
| **Low Burstiness** | 4.0 | Coefficient of variation of sentence length < 0.35 (AI typically < 0.3, humans > 0.5) |
| **Sentence Uniformity** | 3.5 | Standard deviation of sentence length < 5 words |
| **Low Lexical Diversity** | 2.5 | Unique/total word ratio < 0.45 |
| **Trigram Repetition** | 2.0 | Same 3-word sequence appears 3+ times |

### 📖 AI Vocabulary

| Signal | Weight | Description |
| --- | --- | --- |
| **AI Vocabulary Words** | 3.0 | 3+ words from a 28-word AI-favored list (delve, harness, tapestry, realm, etc.) |
| **Model Fingerprint** | 2.0 | Phrases associated with specific models (ChatGPT, Claude, Gemini) |

### 🏗 Structure & Patterns

| Signal | Weight | Description |
| --- | --- | --- |
| **Participial Clauses** | 3.0 | ", [verb]-ing" pattern in >25% of sentences |
| **Repetitive Structures** | 3.0 | Formulaic essay markers (One major reason, In conclusion, etc.) |
| **"From X to Y"** | 2.5 | AI-favored range construction used 3+ times |
| **Contrastive Focus** | 2.5 | "While X, Y" / "Not only X, but also" patterns ≥ 3 |
| **Excessive Transitions** | 2.0 | 5+ formal transition words (Furthermore, Moreover, etc.) |
| **Uniform Paragraphs** | 2.0 | All paragraphs within ±15 words of average |

### 🎨 Style & Tone

| Signal | Weight | Description |
| --- | --- | --- |
| **Hedge Phrases** | 2.0 | 3+ cautious qualifiers (it's important to note, arguably, etc.) |
| **Em-Dash Overuse** | 2.0 | 3+ em-dashes; >15% of sentences contain one |
| **Cliché Phrases** | 2.0 | 2+ stock phrases (at the end of the day, going forward, etc.) |
| **Balanced Perspectives** | 2.0 | Counter-balancers (however, on the other hand) in >15% of sentences |
| **Perfect Grammar** | 1.5 | 300+ words with near-zero contractions or typos |
| **No Personal Voice** | 1.5 | Fewer than 3 personal pronouns |

### ✋ Human Tells (reduce score)

| Signal | Weight | Description |
| --- | --- | --- |
| **Inconsistent Spacing** | −2.0 | Mixed punctuation spacing — a human tell |
| **Typos** | −1.5 | Common misspellings found |
| **Colloquial Language** | −1.5 | Informal expressions or slang (kinda, gonna, tbh, etc.) |

---

## ⚙️ Configuration

All rule weights live in the `WEIGHTS` object in `engine.js`:

```javascript
const WEIGHTS = {
  // Proprietary
  Watermark: 5, BaitPhrase: 3,
  // Statistical
  LowBurstiness: 4, SentenceUniformity: 3.5, LowLexicalDiversity: 2.5, NgramRepetition: 2,
  // Vocabulary
  AIVocabulary: 3, ModelFingerprint: 2,
  // Structural
  ParticipalClauses: 3, FromXtoY: 2.5, ContrastiveFocus: 2.5, RepetitiveStructures: 3,
  ExcessiveTransitions: 2, UniformParagraphs: 2,
  // Stylistic
  HedgePhrases: 2, EmDashOveruse: 2, NoPersonalVoice: 1.5, ClichePhrases: 2,
  PerfectGrammar: 1.5, BalancedPerspectives: 2,
  // Human tells (inverted — subtracted)
  PunctuationSpacing: 2, Typos: 1.5, ColloquialLanguage: 1.5
};
```

- **Adjust any weight** to make a rule more or less impactful.
- The **Strictness** slider (0.5×–2×) multiplies all weights at runtime.
- **Human tell** rules are inverted — when they fire, they *subtract* from the AI score.

---

## 📊 Confidence Levels

| Word Count | Confidence | Meaning |
| --- | --- | --- |
| < 100 | Low | Heuristic analysis unreliable on short samples |
| 100–300 | Moderate | Results are directional, not definitive |
| 300+ | Higher | Enough signal for meaningful heuristic analysis |

---

## ⚖️ Honest Limitations

- **Heuristic-only detection tops out around 55–65% accuracy.** Commercial tools like GPTZero and Copyleaks use transformer-based classifiers trained on millions of examples — a fundamentally different approach.
- **PromptShield will never match those tools on raw accuracy.** That's not what it's for.
- **What it does offer is transparency.** Every signal, its weight, and whether it fired are visible. No black boxes.
- **Use alongside manual review.** This tool is one input, not a verdict.
- **AI models evolve.** These heuristics will need periodic updates as models change.

---

## 📁 File Structure

```
Prompt-Shield/
├── index.html       ← UI (Speyer UI from CDN)
├── engine.js        ← Detection engine (23 rules, 6 categories)
├── README.md
└── License.md
```

---

## 🤝 Academic Integrity

PromptShield is designed to support honest learning and transparent conversations, not to enforce punitive actions on its own. Use it as one lens among many when evaluating student work.

---

## About

Created by [Adrian Speyer](https://github.com/adrianspeyer). Made in Canada with love 🇨🇦

Built with [Speyer UI System](https://github.com/adrianspeyer/speyer-ui) · Licensed under [EnCL](License.md)
