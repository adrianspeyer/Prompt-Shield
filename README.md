# 🛡️ PromptShield – Advanced AI Detection & Encoding Tool

> ⚠️ **Experimental Prototype**  
> Use responsibly as one part of a broader evaluation process. No warranties or guarantees.

---

## 🚀 Quick Start

### ✍️ Encode an Assignment
1. Switch to **Encode** mode.  
2. Paste your assignment instructions.  
3. Click **Encode** — this injects a random zero-width watermark and interleaves it across the text.  
4. Click **Copy to Clipboard** and share with students.

### 🔎 Detect AI Use
1. Switch to **Detect** mode.  
2. Paste the student’s essay or text.  
3. Adjust **Strictness** (0.5×–2×) to scale sensitivity.  
4. Click **Analyze** to get:
   - **AI Match Score** (0–10)  
   - **Likelihood** (“Moderate” / “High”)  
   - **Factor breakdown** (messages)  
   - **Bar-chart visualization**  
5. (Optional) **Export Report** as JSON for record-keeping.

---

## 🧬 Key Features

- **Randomized Zero-Width Watermark**  
  Invisible markers (ZWSP, ZWNJ, etc.) interleaved throughout text.  
- **Bait Phrases**  
  Subtle “assistive” prompts to lure AI completions.  
- **Configurable Strictness**  
  Slider multiplies all rule weights (0.5×–2×).  
- **Modular RULES & WEIGHTS**  
  Tweak any rule or weight in one place (`WEIGHTS` object).  
- **Spinner & Chunked Processing**  
  Keeps UI responsive on longer texts.  
- **Exportable JSON Report**  
  Save score, likelihood, and factor values.

---

## 🚨 Detection Signals

| Signal                   | Description                                                      |
| ------------------------ | ---------------------------------------------------------------- |
| **Watermark**            | Invisible zero-width markers                                     |
| **Few Contractions**     | Formal tone with few “don’t/aren’t/I’m…”                         |
| **Repetitive Structures**| “One major reason…”, “In conclusion…”, etc.                      |
| **Excessive Transitions**| “Moreover,” “Therefore,” “Additionally,” etc.                    |
| **Uniform Paragraphs**   | Very similar lengths across paragraphs                           |
| **No Personal Voice**    | Lack of “I/you/we/my/our”                                        |
| **Numeric Density**      | >3% of tokens are numbers or statistics                          |
| **Opinion Insertion**    | “In my opinion,” “I believe,” etc.                               |
| **Rhetorical Questions** | Absence of any “?”                                               |
| **Readability Score**    | Flesch–Kincaid between 35–60 (narrow AI band)                    |
| **Low Lexical Diversity**| Repetitive vocabulary (unique/total < 0.4)                       |
| **Passive Voice**        | Frequent “is/was/being … -ed” patterns                           |
| **N-gram Repetition**    | Same trigram appears >2 times                                    |
| **Sentiment Uniformity** | Very low variance in positive/negative tone                      |
| **Hedge Words**          | “possibly,” “arguably,” “potentially,” etc. (>3)                |
| **Emoji Usage**          | Any true pictographic emoji (faces, hands, weather symbols)     |
| **Complex Punctuation**  | >5 occurrences of `;` `:` `,`                                   |
| **Adverb Overuse**       | >10 “-ly” adverbs                                                |
| **Cliché Phrases**       | Multiple stock phrases (“at the end of the day,” etc.)           |
| **Long Sentences**       | Avg. sentence length >25 words                                   |
| **Ellipsis Usage**       | >1 occurrence of `…` or `...`                                    |
| **Title-Case Overuse**   | >5% of words in Title-Case mid-sentence                          |
| **Em-Dash Usage**        | >2 occurrences of em-dash (`—`)                                  |

---

## ⚙️ Configuration & Weight Tuning

All rule weights live in the `WEIGHTS` object at the top of the `<script>`:

```js
const WEIGHTS = {
  Watermark:            5,
  FewContractions:      2,
  RepetitiveStructures: 4.5,
  ExcessiveTransitions: 2,
  UniformParagraphs:    2,
  NoPersonalVoice:      2,
  NumericDensity:       2,
  OpinionInsertion:     1.5,
  RhetoricalQuestions:  2,
  ReadabilityScore:     1.5,
  LowLexicalDiversity:  1.5,
  PassiveVoice:         1.5,
  NgramRepetition:      1.5,
  SentimentUniformity:  1.5,
  HedgeWords:           1.5,
  EmojiUsage:           1.5,
  ComplexPunctuation:   1.5,
  AdverbOveruse:        1.5,
  ClichePhrases:        2,
  LongSentences:        1.5,
  EllipsisUsage:        1,
  TitleCaseOveruse:     1,
  EmDashUsage:          1.5
};
```

## ⚙️ Weight Tuning & Sensitivity

- **Adjust any weight** in the `WEIGHTS` object to make a rule more or less impactful on the final score.  
- The **Strictness** slider (0.5×–2×) multiplies all weights at runtime.  

**Workflow for tuning:**
1. Identify a rule that’s over- or under-triggering.  
2. **Increase** its weight to boost sensitivity, or **decrease** it to quiet false positives.  
3. Optionally adjust that rule’s test threshold in the `RULES` array for finer control.  
4. Refresh and re-test on sample essays until you reach your desired balance.  

---

## ⚖️ Interpretation

- **Score > 5/10**: Moderate–High likelihood of AI involvement  
- **Score ≤ 5/10**: Lower likelihood, more human-like  
- **Context is key**: Always combine with manual review and other evidence.

---

## ⚖️ Important Notes

- Scores above 5/10 suggest moderate to high likelihood of AI involvement.
- This tool does **not** definitively prove AI use; always consider context and additional evidence.
- Designed as a research and educational aid — not a disciplinary instrument on its own.

---

## 🤝 Academic Integrity

PromptShield is designed to support honest learning and transparent conversations, not to enforce punitive actions on its own. Use it as one lens among many when evaluating student work.
