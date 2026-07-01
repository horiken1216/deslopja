# deslop

A Claude skill that detects and removes formulaic AI-writing patterns — "slop" — from prose in any language, so text reads as if a deliberate human wrote it.

日本語版は [README_ja.md](README_ja.md) にあります。

## What it does

AI-generated prose converges on recognizable tics: meta-commentary that announces what a sentence is about to do, dead metaphors, mechanical "not X but Y" scaffolding, hedging chains, monotonous rhythm. The underlying failure modes are the same across languages; only their surface forms differ. `deslop` defines one language-independent detection and rewriting process, backed by per-language pattern catalogs.

It works in two modes:

- **Detect & fix** (default) — measure the slop rate, report it, rewrite with the priority *delete > compress > replace*, then re-measure and show the before/after diff.
- **Detect only** — when you just want a score ("how AI-like is this?"), it reports the measurement and stops.

## The slop rate

```
slop rate = sentences containing at least one detected pattern / total sentences × 100 (%)
```

| Slop rate | Verdict |
| --------- | ------- |
| < 10%     | Low — likely reads as human; spot-fix only |
| 10–25%    | Moderate — noticeable AI flavor; targeted rewrite |
| 25–50%    | High — clearly AI-flavored; full rewrite pass |
| > 50%     | Severe — formulaic throughout; consider redrafting |

This is an LLM judgment against a finite catalog, not a deterministic detector. Counts are reproducible enough to compare before/after on the same text, but they don't prove whether a text was written by AI — human writers can score high, and edited AI text can score low.

## The eight universal slop categories

1. **Typographic and punctuation tics** — overused dashes, scare quotes, decorative bold, emoji garnish.
2. **Meta-commentary and self-narration** — "it is important to note", "in conclusion", rhetorical self-answers.
3. **Stock phrases and dead metaphors** — game-changers, "landscape/era/wave" imagery, manufactured pull-quotes.
4. **Structural templates** — firstly/secondly, the rule-of-three reflex, negative listing, summary endings.
5. **False agency and disembodied narration** — data "tells us", cultures "shift", the floating lecturer voice.
6. **Rhythm monotony** — every sentence the same length and ending (also its inverse: relentless staccato).
7. **Hedging and false balance** — "may potentially", every claim softened; or lazy "always/never" extremes.
8. **Content dilution** — saying the same thing twice, generic landings, vague "the implications are significant".

`deslop` takes a **frequency-based** stance, not an absolutist one: a striking image used once is style; the same stock image in every draft is slop. It preserves original metaphors, intentional repetition, and the author's register — deslopping changes style, never the argument.

## Files

- `SKILL.md` — the skill definition (workflow, measurement, categories, verification).
- `references/english.md` — English pattern catalog.
- `references/japanese.md` — Japanese pattern catalog (translationese, over-explicit connectives, formulaic openers, 終助詞 balance, and more).

Other languages are handled by applying the eight universal categories directly.

## Usage

Install as a Claude skill and invoke it by asking to remove AI-sounding style, humanize or proofread a draft, or score text for slop — in any language ("make this sound less AI", "自然な文章に", "slop率を出して").

## Acknowledgements

The category framework extends ideas from [stop-slop](https://github.com/hardikpandya/stop-slop) (MIT) — in particular the false-agency, negative-listing, narrator-from-a-distance, and pull-quote concepts, and the five-dimension scoring rubric — generalizing them across languages and replacing absolute bans with frequency-based judgment.

## License

[MIT](LICENSE)
