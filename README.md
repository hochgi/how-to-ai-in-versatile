# How to AI in Versatile

A ~25-minute R&D all-hands talk on **harness engineering** — what separates an
agent that ships from an agent that produces slop, and what that costs.

### ▶ [View the deck](https://hochgi.github.io/how-to-ai-in-versatile/)

Navigate with `←` / `→` or space; `f` for fullscreen. Several slides are
interactive — the per-phase walkthrough, the staged definition, the per-tool
model reveal — so click through rather than skimming.

**[Speaker notes](SPEAKER_NOTES.md)** carry the argument behind each slide. They
are the fine print that used to sit on the slides themselves; slide numbers
match the deck. If you only read one thing here, read those.

## The argument, in two acts

**One — the harness.** An agent is `Model + Harness`. The harness is everything
except the model, and it's the half that carries the quality. Ours is a
five-phase loop — spec → red tests → green code → read-only review → read-only
mutation verification — built on three principles:

1. **The leash** — write permissions per phase; nobody touches what they
   shouldn't.
2. **Nobody grades their own homework** — verification runs in a different agent
   with no write access.
3. **Swiss-cheese review** — overlapping imperfect layers, because no single
   reviewer catches everything.

**Two — the bill.** Where the money actually goes, why the harness is what makes
the model swappable, and the one habit that moves the number: route cheap phases
to cheap models. The expensive model is for the phase that needs judgement, not
for reformatting JSON.

Then an appendix for what didn't fit — the decision ladder, complementary flows,
and sources.

## Notes on provenance

Everything on the slides is real. The code, tests and Stryker mutation report
come from a production service at `main`; the git-log receipts are from a real
feature branch. The cost figures are actual invoices, not projections.

## Running it locally

The deck is a single self-contained HTML file — no build step, no dependencies,
no network calls.

```bash
python3 -m http.server 8806
```

Then open <http://localhost:8806>. Opening `index.html` directly from the
filesystem works too.

## Files

| | |
|---|---|
| `index.html` | the deck — 28 slides, self-contained |
| `SPEAKER_NOTES.md` | the reasoning behind each slide |
| `loop.mmd` | Mermaid source for the five-phase loop diagram |
| `litellm-model-menu.md` | working notes behind the cost half's model/alias tables |

---

Talk and deck by [Gilad Hoch](https://github.com/hochgi).
