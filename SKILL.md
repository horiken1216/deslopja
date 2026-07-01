---
name: deslop
description: Detect and remove formulaic AI-writing patterns ("slop") from prose in any language, making text read as if written by a deliberate human author. Use this skill whenever the user asks to remove AI-sounding style, make writing feel natural or human, polish/edit/proofread an article, reduce "slop", or improve writing quality — in Japanese ("slop感を消したい", "AIっぽさを消す", "自然な文章に", "校正して"), English ("make this sound less AI", "humanize this", "deslop"), or any other language. Also use it as a slop DETECTOR when the user asks to measure or judge how AI-like a text is ("slop率を出して", "slop判定して", "AIっぽさを採点して", "score this for AI patterns", "how much slop is in this") — in that case report the score without rewriting. Also use it as a self-check before delivering any long-form article, column, blog post, or business document that Claude itself has drafted, even if the user did not explicitly ask. When replying in Japanese, also run a lightweight self-check against the turn-taking and dialogue patterns in references/japanese.md (translationese, over-explicit connectives, formulaic acknowledgement openers, restating the user before answering, acknowledge→answer→offer boilerplate, missing or overused 終助詞) — not only for drafted documents but for conversational responses.
---

# deslop: Removing formulaic AI-writing patterns

AI-generated prose converges on recognizable patterns that signal "a machine wrote this."
The underlying failure modes are universal across languages; only their surface forms differ.
This skill defines a language-independent detection/rewriting process, with per-language
pattern catalogs in `references/`.

## Modes

- **Detect & fix** (default): measure first, report the slop score, then rewrite and re-measure
- **Detect only**: when the user asks to judge/score/measure ("slop率は？", "判定だけ",
  "how AI-like is this?"), report the measurement and stop. Offer to fix, but don't rewrite
  unasked

## Workflow

1. **Identify the language(s)** of the text, then read the matching reference file:
   - Japanese → `references/japanese.md`
   - English → `references/english.md`
   - Other languages → apply the universal categories below directly, instantiating them
     from your knowledge of that language's AI-writing clichés
2. **Detect**: list every occurrence that matches a category, grouped by category
3. **Measure and report the slop score** (see Measurement below) BEFORE touching the text.
   In detect-only mode, stop here
4. **Build a protection list**: identify expressions unique to this text — original metaphors,
   deliberate repetitions (e.g., a title echoed in the closing), the author's idiosyncratic
   phrasings. These are off-limits. Slop is a problem of *formula and frequency*, not of
   strong expression per se; a striking image used once is style, the same stock image seen
   in every AI draft is slop
5. **Rewrite** with the priority **delete > compress > replace**. The goal is subtraction.
   Never add ornamentation to "fix" plainness
6. **Verify**: re-measure the rewritten text, run the rhythm check and the five-dimension
   self-score below
7. **Report the diff**: before/after slop scores, what was changed and why, grouped by
   category, with counts

## Measurement

The primary metric is the **slop rate**:

```
slop rate = sentences containing at least one detected pattern / total sentences × 100 (%)
```

Count sentences by terminal punctuation (。．.!?), excluding headings. A sentence with
multiple patterns still counts once for the rate; also report **total pattern count** and
**density** (patterns per sentence) so heavy clustering is visible.

Severity bands:

| Slop rate | Verdict |
| --------- | ------- |
| < 10%     | Low — likely reads as human; spot-fix only |
| 10–25%    | Moderate — noticeable AI flavor; targeted rewrite |
| 25–50%    | High — clearly AI-flavored; full rewrite pass |
| > 50%     | Severe — formulaic throughout; consider redrafting from the argument up |

Report format (always show before rewriting; show before→after when fixing):

```
Slop率: 34% (12/35文)  判定: High
パターン総数: 19件 (密度 0.54/文)
内訳: 記号3 / メタ実況4 / 紋切り6 / 構造型2 / 偽主語1 / リズム2 / ヘッジ1 / 希薄化0
```

Caveats to state when reporting: this is an LLM judgment against a finite catalog, not a
deterministic detector — counts are reproducible enough to compare before/after on the same
text, but not calibrated across texts or against external AI-detection tools. List the
detected instances so the user can audit the count. Never claim the score proves a text
was or was not written by AI; it measures stylistic formula, and human writers can score
high while edited AI text scores low.

## Universal slop categories

These eight failure modes appear in every language. The reference files map each to
concrete surface patterns.

### 1. Typographic and punctuation tics
Overused marks that have become AI signatures: a particular dash, excessive quotation-mark
scare quotes, decorative bullets/bold where prose would do, emoji garnish, headings built
on dashes or colons.

### 2. Meta-commentary and self-narration
Sentences that announce what the text is about to do instead of doing it: "the reason is
clear", "in conclusion", "it is important to note", rhetorical question-and-self-answer,
redundant previews. Default action: delete the announcement and start with the content.
Related principle: trust the reader. Cut softening, justification, and hand-holding that
exist only to manage the reader's reaction.

### 3. Stock phrases and dead metaphors
Expressions that appear in thousands of AI drafts: game-changer equivalents, "quietly
collapsing" premises, "landscape/era/wave" metaphors, intensifiers that survive deletion
without changing meaning. A useful heuristic: **the pull-quote test** — if a sentence
sounds engineered to be quoted (aphoristic, symmetrical, suspiciously profound), it is
probably manufactured insight; rewrite it as a plain claim.

### 4. Structural templates
Mechanical scaffolding: enumerated firstly/secondly/thirdly, the rule-of-three reflex,
repeated "not X but Y" contrast frames (allow at most two per document), **negative
listing** ("It's not A. It's not B. It's C." — a striptease through negation; just state C),
every section ending with a mini-conclusion that pre-empts the actual conclusion,
summary endings that restate the whole piece.

### 5. False agency and disembodied narration
Inanimate things performing human actions: decisions "emerge", data "tells us", cultures
"shift", conversations "move toward". AI favors these because they avoid naming an actor.
Name the human who acts, or address the reader directly. The same failure appears as a
floating lecturer voice ("people tend to...", "nobody designed this") that observes from
above instead of putting the reader in the scene. Languages differ in how much agentless
construction is natural — calibrate against the reference file, not against English norms.

### 6. Rhythm monotony
Every sentence the same length; every sentence ending the same way (language-specific:
e.g. Japanese 〜ます chains, English participial tails). Break monotony with short
declaratives, varied endings, occasional fragments where the register permits. The inverse
is also slop: relentless staccato fragments stacked for drama. Both are metronomes.

### 7. Hedging and false balance
Chains of qualifiers ("may potentially", "it seems that perhaps"), every claim immediately
softened, hedging dressed up as reassurance. Also its mirror image: lazy extremes
("always", "never", "everyone") doing vague work where a specific claim belongs.

### 8. Content dilution
Saying the same thing twice in different words, generic landings ("time will tell",
"balance is key"), long noun enumerations that should be cut to two or three
representative items, vague declaratives ("the implications are significant") that
should name the specific implication.

## What to preserve (over-deletion guard)

- Metaphors and turns of phrase that only work in this text
- Intentional repetition and structural echoes the author built in
- The author's register and orthographic habits (formality level, spelling/kanji choices)
- The argument itself: deslopping changes style, never claims or logic
- This skill takes a frequency-based stance, not an absolutist one. Some catalogs ban
  entire word classes (all adverbs, all em-dashes, all passives). Treat those as smoke
  detectors, not laws: an adverb that survives the deletion test, or a passive that
  correctly de-emphasizes an unknown actor, can stay
- If the result reads flat and voiceless, you deleted too much. "Natural" means
  *individual*, not *bland*

## Verification: five-dimension self-score

After rewriting, score the text 1–10 on each dimension. If any dimension scores 6 or
lower, do another pass focused on that dimension.

| Dimension    | Question                                                      |
| ------------ | ------------------------------------------------------------- |
| Directness   | Does it state things, or announce that it is about to?        |
| Rhythm       | Do sentence lengths and endings vary, or is it metronomic?    |
| Trust        | Does it respect the reader, or soften and hand-hold?          |
| Authenticity | Could a specific person have written this, or anyone/no one?  |
| Density      | Is anything cuttable without loss?                            |

## Reporting format

After rewriting, report concisely:
- Patterns removed or modified, by category, with counts
- Expressions deliberately preserved and why
- Any judgment calls, offered as options rather than silently decided

## Acknowledgements

The category framework here extends ideas from the open-source project
stop-slop (github.com/hardikpandya/stop-slop, MIT): in particular the false-agency,
negative-listing, narrator-from-a-distance, and pull-quote concepts, and the
five-dimension scoring rubric. This skill generalizes them across languages and
replaces absolute bans with frequency-based judgment.
