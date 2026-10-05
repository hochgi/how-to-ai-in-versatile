# Speaker notes — How to AI in Versatile

Slide numbers match the live deck. Fine print that used to sit on the slides
lives here so the projected deck stays big and readable.

---

## Part one · the harness

### 1 · Title
Two halves. One — the harness: what it is, why it exists, and what ours
actually looks like. Two — the bill: where the money goes, and the one habit
that moves it.

### 2 · Five things that are no longer arguable
Read the emphasis, not the whole quote. Three of the five lines are the same
word: **more** people coding (i), **a lot more** applications (iii) — and (ii)
is why that scales at all. The colour does the argument: red is the line that
costs us, green are the two that pay.

On (ii): only the first half is red — *writing and reading code is not that
important*. The clause after the comma is Ng hedging, and it is greyed out on
purpose. Do not read it aloud unless someone challenges the claim.

Two consequences worth pulling apart on the next two slides: what all that
"more" costs us, and what judgment and cheap refactoring buy us.

Cite: Andrew Ng, 2026 — x.com/AndrewYNg/status/2043742105852621052

### 3 · What "more" costs us
This is the cost of the whole first half of the quote, not of (ii) alone: more
people coding, a lot more applications, and less of it read by anyone. Volume
goes up; attention per line goes down.

The safeguard is gone — not weakened — because nobody is reading the diff.
The failure human review used to catch was never “bad syntax”; it was code
that does something other than what was meant. This talk is an attempt to
rebuild that safeguard out of something other than human attention.

Cite: Lahiri, Microsoft Research, Mar 2026 — arXiv:2603.17150

### 4 · What judgment and cheap refactoring buy us
The two green lines, named rather than numbered: (iv) is judgment — deciding
what to build — and (v) is cheap refactoring.

On (iv): the spec is the scarce artifact because it carries *intent* rather
than machinery — and intent is the one kind of thing a human can still review
efficiently at volume. Do not mention phases or phase 1 here; the loop has not
been introduced yet. This slide is the reason it will exist.

(v) is Ng’s upside only: cheap refactoring makes a complexity budget
enforceable, which is what lets us insist the repo stays simple and clean.
Do not bring Pocock in here — he is the next slide.

### 5 · Slop looks right
This is the motivation, before we define a harness.

Slop is the failure mode of (ii): the model emits something that *looks*
like production code. Clean names, confident comments, it compiles, it may
even go green. The behaviour is wrong. And because nobody is reading the
diff, it ships.

Pocock ran the specs-to-code loop himself and watched each pass get worse —
“I would just end up with garbage.” His punchline, at 3:55 of the AI Engineer
Europe talk: code is not cheap; **bad code is the most expensive it’s ever
been**, because a hard-to-change repo cannot take the bounty AI offers. AI
in a good codebase does really, really well. In a sloppy one it amplifies
the slop.

That is why the next slide exists. If slop is expensive, we need a loop
around the model that makes “looks right” lose to “is right.”

Cite: Matt Pocock, “It Ain’t Broke: Why Software Fundamentals Matter More
Than Ever”, AI Engineer Europe, Apr 2026 —
https://www.youtube.com/watch?v=v4F1gFy-hqg&t=235s
Official listing: https://ai.engineer/talks/software-fundamentals-for-ai-coding

### 6 · A harness. So what *is* one?
The title is a question on purpose — the three reveals answer it, and the
definition lands last, in Böckeler’s words rather than mine. Do not define the
term before the third click.

Click the stage (or →) to cycle three states. It always wraps — you can replay.
← steps backward inside the slide; → on the quote advances the deck.

1. **Not yet a harness** — prompt, CLAUDE.md, “use Opus”, one long chat,
   nobody looks at the spec or the code. **“Yet”** is doing real work here:
   under Böckeler’s definition, three clicks later, a good prompt and a
   `CLAUDE.md` *are* harness components — so “not a harness” flat would
   contradict your own quote, and somebody will catch it. “Not **yet**”
   concedes that and keeps the claim you actually want: *you don’t have one*.
   Say it as encouragement, not accusation — most of the room is on this card,
   and they should hear that they’ve started, not that they were wrong.
   Resist the urge to soften it further to “not a *good* harness”: that
   concedes these things are harnesses, and the one person who thinks their
   `CLAUDE.md` is enough will hear permission to stop listening.
2. **A harness** — phases, write permissions, artifacts as handoff, independent
   verification, kick-back paths.
3. **The quote** — Böckeler: *Agent = Model + Harness*. Linger on the equation.

Böckeler splits a harness into **guides** (feedforward — steer it before it
acts) and **sensors** (feedback — observe after, and self-correct). Our five
phases are both: 1–2 are guides, 4–5 are sensors.

And note what `Agent = Model + Harness` implies about cost: if the harness is
the part that carries the quality, **the model is the swappable half** — which
is the entire second act of this talk.

Cite: Birgitta Böckeler, “Harness engineering for coding agent users”, 2 Apr 2026
— martinfowler.com/articles/harness-engineering.html

### 7 · Forward on artifacts, back on findings
Walk the blue arrows first (what each phase hands forward), then the red
kick-backs (findings route to the phase that caused them). The dashed box is
read-only: reviewer and verifier may not write production code.

The count moved into the breadcrumb (`the loop · /spec-to-ship · five
phases`) so the headline fits one line and the diagram gets the rest of the
slide. Say "five phases" out loud anyway — it is the first time the number
lands.

### 8 · The same five phases, on one real ticket
`RD-24010 / AJ-18` — sign the Slack approval button so a forged click cannot
approve a tool call.

**Phase 0** is the only human input: `/spec-to-ship RD-24010`. The command
file (`.claude/commands/spec-to-ship.md`) is the entry point — it tells the
orchestrator to run the five phases for `$ARGUMENTS`, delegate each phase to
its sub-agent, not do a phase's work itself, and route findings back to the
phase that caused them. Do not linger on the file; the next clicks *are*
what it does.

Click a phase, or press **0–5**. Phases **2** and **5** have a second click
in them (the assertion; walking the two survivors). Everything on this slide
is real: spec, tests, implementation and the Stryker report from
`vnatures/van-damme-slack-app` at the AJ-18 commit.

### 9 · It's just some markdown and JSON files
**Three clicks.** Let the bare tree sit for a beat first — the point of the
first state is that there is no product here, no platform, no service: it is
markdown and JSON in the repo, and the comments are worth reading out loud.

**Click 1 — `spec-to-ship.md`.** The command is the only thing you type. Say
the line that is lit: *all five phases are delegated sub-agents.* Each `##
Phase` heading names an agent and a skill, and that is the whole wiring — no
framework, no graph, no runtime.

**Click 2 — `reviewer.md`, and this is the one to slow down on.** A sub-agent
is not "the same model with a different system prompt". Point at `tools:` and
read the whole line out: `Read, Grep, Glob, Bash`. Then say what is *missing*
— no `Edit`, no `Write`. **The reviewer physically cannot fix what it finds,
so it has to write it down.** That is principle 2 (slide 11) already enforced
two slides early, and enforced by a line in a file rather than by anyone's
discipline. The coder's file is the same shape with `Edit, Write` added; the
verifier's is the same as the reviewer's.

If someone asks how that is stopped from rotting: `.harness/models.json` is
the single source of truth for the model column, and `check-agent-skills.sh`
fails the build if an agent's frontmatter disagrees with it.

Do not get hung up on the file count. Different repos, and different tools,
carry adaptations. What is stable is the *shape*: one command, five phase
agents, shared skills, and a model manifest.

**Why the same harness is defined three times** (`.claude/`, `.cursor/`,
`.opencode/`): each tool reads a different on-disk convention. The agents and
skills are the same contract; Cursor also gets `complexity-budget.mdc`,
OpenCode points `skills.paths` at the Cursor skills rather than duplicating
them. The per-phase write-permission table and the per-tool model column live
in the cost half — we do not replay that table here. If someone asks “who may
write what”, the answer is: spec-author → specs; test-author → tests and
stubs; coder → prod code; reviewer and verifier → nothing.

### 10 · Constrain the agents — don't ask them nicely
The leash is made of things that are not us. Karpathy: “how you keep the AI
on the leash.” Compiler, linter + budget, a test that was red first, a
surviving mutant, the bot panel, every suppression audited.

### 11 · Nobody grades their own homework
We had it collapsed: phase 3 wrote the code *and* triaged its own mutation
survivors — which is how you get a 100% score that measures nothing. A
suppression is legitimate only when the invariant that makes the mutant
equivalent is named and proved. Every disable is audited: apply the mutant
by hand, run the suite, restore.

### 12 · Swiss-cheese review
Five slabs, holes in different places, reading left to right in the order slop
meets them. The harness is the front door — leftmost, nearest the audience.
Anything through it still has to pass Baz, Bugbot, Copilot and VanDamme.

Walk the pink line, don’t explain it: it enters at the left, goes *through* a
hole in the harness, *through* a hole in Baz — those two happened to line up,
and both are ringed in pink — and then hits solid cheese at Bugbot. That is the
whole argument in one gesture. The two aligned holes are the point: any single
gate would have let this through.

Hover a slice and the other four drop back, so you can talk about one gate on
its own. The stack never moves, so the slop path doesn’t jump.

1. **The harness** — the whole `/spec-to-ship` loop. First slice, not a bot.
2. **Baz** — every PR, automatic.
3. **Cursor Bugbot** — `@cursor review`.
4. **Copilot** — request review.
5. **@VanDamme** — @-mention, a full agent session.

Triage **every** comment: fix it, or reply with your reasoning. Never leave
one silently unresolved. High noise, higher recall — a hole in one reviewer
is exactly what another catches.

Reason’s model: defenses are slices; accidents happen only when holes
momentarily align. We stack reviewers so they don’t.

---

## Part two · the bill

Part two is mainly *what* and *how* — the *why* is obvious. Ten slides, then
the appendix. Everything the slides used to say in small type is below.

### 13 · Part two · The bill
The act break. Two sentences, then move on — this slide exists so the audience
knows the subject changed, the same way part one opened.

Say: part one was how we keep the agents honest; this half is what it costs. And
flag the asymmetry — part two is mostly *what* and *how*, because the *why* is
just a number, and it is the very next slide.

### 14 · The bill, in real numbers
**Open on the chart, not the concept.** Three panels, one arc: what it cost,
what it produced, what ran it. The headline is the whole slide — *same engineer,
9.5x less money, 2.2x the work* — so say it and then let people read.

The numbers (June -> July -> August 2026, one seat, mine):

| | Jun | Jul | Aug |
|---|---|---|---|
| Cost (as billed) | **$2,006.01** | $645.91 | **$210.45** |
| Tokens, all tools | 2.01B | 1.69B | **4.49B** |
| Cursor: sonnet+opus+fable | 92% | 30% | **1%** |
| Cursor: grok+composer | 6% | 46% | **99%** |
| **$ per billion tokens** | **$999** | $383 | **$47** |

That last row is the real finding: **21x better cost-per-token.** The bars show
9.5x on spend, but we did more than twice the work — so per unit of work it is
21x. If you only remember one number from part two, that is the one.

The story behind the three months, in one line each:
- **June** — we were not paying attention. Cost crept up exponentially and the
  company hit a record **>$30K** on AI in one month. I never tool-hopped, so
  everything went to Cursor *including* everything past my quota. My Claude
  Code on-demand in June was **$0** — I never even reached the CC cap.
- **July** — started noticing. Began experimenting with tool-hopping, so both
  seat quotas got used. On-demand fell from $1,941 to $387.
- **August** — upgraded to a premium Cursor seat ($120 vs $40) and moved the
  grind onto grok. **Cursor on-demand hit exactly $0.** The seat upgrade paid
  for itself roughly sixteen times over in the first month.

Panel 3 is the mechanism, and it is the same point as the next slide's warn
card: June's $1,941 of on-demand was **93.8% Anthropic-through-Cursor**. That
one habit was essentially the entire bill. grok + composer going 6% -> 99% is
what replaced it.

**Where each number comes from — keep these straight if challenged:**
- **Dollars: finance only** (Orly) — the actual billed amounts, shown to the
  cent on the slide so they can be checked against the invoice:
  June $40.00 + $1,941.01 (Cursor) + $25.00 + $0.00 (Claude) = **$2,006.01**;
  July $40.00 + $387.17 + $25.00 + $193.74 = **$645.91**;
  August $120.00 (premium seat) + $0.00 + $25.00 + $65.45 = **$210.45**.
  Never the tools' own numbers.
- **Tokens and model mix: local telemetry** — `~/ai-usage/usage.json`, produced
  by the collector in `dev/ai-dashboard` (`npx github:vnatures/ai-dashboard`,
  which merges into the existing file so history survives log rotation).
- Model mix is Cursor-only, because the Cursor export is the only source with a
  per-request Model column.

**Why no per-tool dollar figures from telemetry:** the collector's `costUsd` is
a notional list-price estimate and it **double-counts everything the seat
already paid for.** For August it says Claude Code cost $1,847 where finance
says $90 — overstated **20x**. That is not a bug in the collector; it is the
reason this slide's dollars come from finance. It is also the cleanest possible
proof of the point: if you form an opinion about spend from the tools' own
dashboards, you will be wrong by an order of magnitude.

**Caveats, if pushed:** the Cursor CSV does not reconcile exactly with finance
on-demand ($2,051 vs $1,941 for June; $427 vs $387 for July) — the boundary is
probably not the 1st of the month, and excluding "User API Key" rows accounts
for only $3. So the CSV is used for *volume and mix*, never dollars. Also worth
saying out loud: this is **one engineer's seat**, not the company's bill.

**Say whose numbers these are, out loud, first.** This is my own seat and my
own usage — not a composite, not an average, not an illustration. The billed
figures came from Finance; the token counts came off my machine. That is the
whole reason the slide carries any weight, and it is also the reason to be
precise about the caveats further down.

**Four clicks — do not rush them.** One panel at a time, full width, with the
full breakdown under each chart so every number is legible without hovering
anything. Click (or →) advances; ← steps back.

1. **What it cost** — the billed dollars, fully detailed. Land the June bar
   before you say anything else: that single orange block is $1,941 of
   on-demand.
2. **What it produced** — cost shrinks to a recap chip top-right and tokens take
   the stage. The point to make out loud: **August did more work than June.**
3. **What ran it** — tokens join cost in the corner, and the model mix takes the
   stage. Panel 3 is the mechanism.
4. **The punchline** — all three summaries are now parked in the corner and the
   slide is just the number: **$999 → $47 per billion tokens.** Land the last
   line deliberately: *nothing about how I work changed; I changed which model
   does the grind.* That is the sentence the whole half is built to earn.

The corner chips are deliberate — the earlier panels don't vanish, they park, so
by the last click all three sets of numbers are visible at once and nobody has
to remember a figure you already took off screen.

Each chart also carries its own breakdown table and its own provenance line, so
if anyone asks "what was the Claude seat in July?" the answer is already on the
slide. Different panels, different sources — that is intentional, and the line
under each table says which.

**On the labels:** the buckets are "sonnet + opus + fable" and
"grok + composer", named rather than branded, because that is literally what the
export contains — June was opus-4-8 and sonnet-5, July added fable-5, August is
opus-5. Folding composer in with grok also moves the numbers slightly (June's
non-Anthropic share is 6%, not 2.5%), so the percentages here are not the ones
from an earlier draft.

Point people at the `ai-usage` dashboard v-drop to pull their own breakdown
before forming an opinion about their own spend.

### 15 · Two meters
This slide has one job: pick up the thread from the punchline you just
delivered. You said *"I changed which model does the grind."* This is why that
was even possible — **not all tokens are priced alike**, and the tiers behave
in opposite ways.

Seat quota is free at the margin until it isn't, and then it is the *most*
expensive thing we buy anywhere. Metered is small-but-always. The parallel on
the slide is the thing to say out loud: **one has a silent wall, the other has
no wall at all.** Neither one stops you working — that is exactly the problem.
Running out silently moves you onto the priciest tokens unless you deliberately
move yourself onto the cheapest.

The warn card is the one concrete "stop doing this" in the talk, so land it
slowly — the slide only carries the rule, the reasoning is yours to say: a
Cursor seat is **two pools**, not one. First-party (grok, composer) is
generous; third-party is thin; and overflow runs *one way only* — grok past its
limit eats third-party quota, third-party past its limit goes straight to cash.
So running Anthropic models *inside Cursor* spends the pool that is also grok's
safety net. Do that work in Claude Code, or switch the Cursor model to grok.
Either is fine. Both beat the status quo.

### 16 · Cost mitigation — now what?
The title asks the question the previous slide set up: the wall is silent, and
you just hit it. Three moves; only one of them is expensive. The cards run in
**order of effort**, so read them left to right and let the ladder do the work.
Do **not** explain the gateway here — that is the next slide.

**Card 1 — keep going.** The expensive one, and the one everybody picks,
because it is the only option that requires no decision at all.

**Card 2 — flip.** Lead with this, because it costs the audience nothing to
adopt: one command, the session survives, and you are metered instead of at
list price. Slide 18 has the two commands. This is the realistic answer for
"I am mid-task and I do not want to think about it."

**Card 3 — hop.** Cheapest of the three, and the point to make out loud is the
*order*: **burn the seats you already pay for before you touch anything
metered.** Claude Code first, then Cursor, and only then a metered agent —
OpenCode or pi — through LiteLLM. Seat capacity is already bought; spending it
costs nothing marginal, so leaving it unspent while you pay per token is the
one genuinely irrational outcome. (Callback to slide 15: hopping to Cursor
means Cursor's *own* models. Do not run Anthropic models inside Cursor.)

Hopping isn't the problem — hopping *without a handoff* is. The context dies at
every hop, guardrails change under you, and a half-finished slice waits in a
dirty tree while you re-explain what the last tool knew. Ask the tool you're in
for a handoff prompt *before* you leave: branch, spec path, phase completed,
what's green, what's next. Cheap, because the harness already wrote most of it
to disk — **the artifacts *are* the handoff.**

Punchline: decide before you're out, not after. Check quota when you pick up a
task, not when it stops responding.
### 17 · Valar Code · the wrapper
This is where slide 16's "flip" card gets its two commands, and it lands
*before* the gateway diagram on purpose: the audience just heard "flip", so
give them the command while they still want it.

The mechanism in one line: **it re-points the tool you are already in.** It
edits `~/.claude/settings.json`, swapping `ANTHROPIC_BASE_URL` and the key.
Same API, same `/model` picker, same session. (Valar calls these wrappers
"harnesses"; in this talk a harness lives in your repo, so say **wrapper**.)
`valar cursor on` and `valar claude-desktop on` cover the other tools.

**The quote is the point of the lower half of the slide.** Read it out. Tom
Amsterdam is our Valar contact, and what he is describing — only *overflow*
routes through Valar — is exactly the "seat first, then metered" rule that
slide 16 asked people to follow by hand. The interesting part is that it stops
being a discipline problem and becomes a plumbing problem, which is the thesis
of this whole half of the talk.

Be straight about the status: promised for the week of 20 Aug, chased on
31 Aug, answer was "the upcoming 2–3 days" (they had a delay on Codex support
and the GLM-5.3 release). So: **real, imminent, not here yet.** Do not build a
plan on the date. Until then the `sleep 120m` one-liner on the slide is a
genuinely fine substitute, and it is what I actually run.

If asked why not a time-based quota: Valar offered exactly that, and I said no
— automatic overflow is strictly better than asking people to watch a clock.

The "verify `valar claude status` today" line used to live on this slide too.
It is gone from here on purpose — slide 23 makes the same ask as an action,
and saying it twice bought nothing but a smaller font on both slides.

End on the catch, because it sets up the next slide: **the wrapper runs
everything at `Now`**, the most expensive tier Valar sells. It saves your
session, not your money.

### 18 · Valar’s API · same model, up to 2.9× cheaper
The turn: *"that was one way in. Here is the other, and it is the one that
actually moves the bill."* Valar also exposes an **OpenAI-compatible endpoint**,
and unlike the wrapper it lets you name a **tier**.

Land the idea before the numbers: **you are buying patience, not quality.** The
weights are identical across every row. Standard is the same model as Now; it
just may take up to a minute. That is the whole trick, and it is why this is
not a quality-versus-cost trade-off at all.

Then the ladder. Numbers are `deepseek-v4-pro`, read off **Valar's own model
page on 2 Sep 2026** — not out of our LiteLLM config, which had them wrong
(and understated output, which is where agent spend actually lands). If
anyone challenges a figure, the model page is the answer; re-read it before
you present, because these move.

- **Now** — immediate — the only tier the wrapper can reach
- **Flex** — under 5 minutes — **2.9× cheaper**, and the bottom rung we can
  actually reach

**Batch is greyed out on purpose.** It exists on Valar at 3.3×, but it is not
aliased on our LiteLLM any more — a turnaround measured in hours is no use to
a coding agent, so it was removed rather than left as a trap. The row stays on
the slide because the ladder's shape is the point and somebody always asks why
it stops at Flex. If anyone wants 3.3× for a genuinely offline job — a bulk
eval, a migration sweep — that is a direct Valar call, not the gateway. **The
headline says 2.9× because that is what a Versatile engineer can actually get.**

Note the shape while you are on it: **output is 3× input** on every rung, so
a phase that writes a lot (the coder) is the expensive one and a phase that
mostly reads (the reviewer) is cheap even before you drop its tier. Cached
input is cheaper again — about **30×** below fresh input — which is slide 15's
point arriving in a price list.

The catalogue is the other half of the pitch: this is the **best open weights
going** — GLM-5.3, Qwen-3.8-max, Kimi-K3, DeepSeek-V4 — and every family has a
ladder of its own, so the discount is not confined to one cheap model.

That catalogue is no longer aspirational: **GLM-5.3 and Qwen-3.8-max are now
aliased on our gateway** — `vn-coding` points at `valar-glm-5.3-flex` and
`vn-review` at `valar-qwen3.8-max-flex`. `vn-verify` is still on GLM-5.2, which
is a one-field change on slide 21 whenever somebody decides to move it. That is
the loop this deck is arguing for, and it has already run once since the slides
were written.

Note which rung those two sit on: **Flex**, not Standard. So the reviewer
example on this slide is not hypothetical — our review phase really does take
five minutes and really does cost about a third.

The one that lands with engineers: **a reviewer or verifier does not need to
answer in a second.** Nobody is watching it type. Those two phases alone are
half your calls in a `/spec-to-ship` run, and dropping them to Standard halves
their cost for literally no change in output.

Close on the handoff: it speaks OpenAI, so anything can sit in front of it —
**and something already does.** Next slide.

### 19 · The plumbing
**This is the map.** Valar is the one name they already have — you just showed
its wrapper — so lead with that: *"the last slide was one way to reach Valar.
Here is the whole picture, and the other way in."* Four new proper nouns
follow, which is why this is a picture and not a bulleted list. Read the arrows
left to right once, out loud, before you click anything: *"you work in
OpenCode, it points at one gateway, and the gateway decides which inference
provider actually serves the tokens."* That single sentence is the whole
architecture.

Then click the boxes. Each one swaps the frame underneath; arrow keys walk
them too, so it works from a clicker. Order does not matter — click whichever
one somebody asks about. A right-arrow past the last box moves on to the next
slide as usual.

The one label on the picture is **`opencode.json`**, and it is worth pointing
at: the connection is *config*, not a command. One `baseURL` and one key, and
OpenCode is on our plumbing. (`opencode models` lists what that gets you.)

The fan-out edges are deliberately unlabelled — say it instead of drawing it:
**LiteLLM routes on the model name.** That is the whole trick, and it is what
makes the next two slides possible. Do not use the word *alias* yet if you can
help it; it arrives with the console on slide 21, and it lands better when the
audience has already seen a name being routed.

Extras per box, if asked:

- **OpenCode** — lead with the comparison, it is the fastest way in: *it is a
  coding agent, same category as Cursor and Claude Code.* What makes it worth
  a slide is that it is not tied to one vendor's models — any inference
  provider works out of the box, which is exactly why it can point at us.
- **LiteLLM** — the framing that lands is **"we are an inference provider
  now."** Behind it we can proxy someone else, or serve a model we host
  ourselves, and nobody downstream has to care which. The real win is that
  keys and quota stop being a per-engineer problem — nobody should be pasting
  an API key into a config to get work done.
- **Bedrock** — credits we already hold. Spending them is strictly better than
  spending cash.
- **Anthropic** — **the one people are surprised by, so do not skip it.** Yes,
  we proxy Anthropic too: the same Sonnet/Opus your Claude Code seat runs on,
  reachable through the gateway, metered per token, no seat required. This is
  the bridge back to slide 16 — "hop, but hand off" and "don't hop, flip" both
  end up here.
- **Valar** — you just spent two slides on it; here it is only a box. If asked,
  the tier ladder was two slides back.

The five marks are each vendor's own — OpenCode's favicon, the AWS
architecture icon for Bedrock, the Claude mark for Anthropic, the V from
Valar's wordmark, LiteLLM's bullet train — so the row should be recognisable
to anyone who has seen the tools. Credited on the sources slide.

Close on the punchline, which is the only line that stays on screen the whole
time: the cheapest good-enough model changes every few weeks, and **you should
not have to notice.** That is what a gateway buys — not today's price, but
never having to re-learn it.

### 20 · The lever · cost-adapted models per phase
The interactive one. Click the three tool tabs — same five phases, same
artifacts, same write permissions; **only the engine changes.** That is the
point: the harness is per-tool for a real reason, not a generic abstraction.

On the OpenCode tab, click any alias — or press **R** for all five — to wipe
the curtain aside and show what `litellm.internal.stg` resolves it to right
now. Phases 4 and 5 still cannot write production code, on any tool.

One file per repo, `.harness/models.json`, is the **only** place a model may be
named. A strong-model phase bills the seat that already paid for it; a grind
phase falls through to a metered alias.

### 21 · Retargeting, in practice
The slide is **just the console** — deliberately. Say all of this; don't read
anything, because there is nothing to read.

Pencil, type a different model group, Save. **That is the whole operation.** No
deploy, no PR, no announcement — and every developer on every repo picks it up
on their next run, because their repos say `litellm/vn-coding`, not a model
name.

That is the point of the manifest. The repo commits to a **role** — "whatever
is best value for the grind loop." The gateway decides which model that role
resolves to this week. Two different decisions, two different review cycles;
the file exists so they stop being the same edit.

Recreation of the live console — real aliases, real targets, as of this talk:
`vn-spec → mantle-gpt-5.6-sol`, `vn-test → bedrock-claude-opus-4-8`,
`vn-coding → valar-glm-5.3-flex`, `vn-review → valar-qwen3.8-max-flex`,
`vn-verify → valar-glm-5.2-flex`. **Re-screenshot the console before you
present** — these change, and the whole point of the slide is that changing
them is cheap.

The blue row is mid-edit on purpose, and it is a real edit: `vn-coding` moved
from `valar-glm-5.2-flex` to `valar-glm-5.3-flex` after GLM-5.3 landed on
Valar. Worth saying out loud — *"this is not a mock; this row moved last
week, no repo changed, and nobody was told."* Note `vn-verify` is still on
5.2: two phases, two decisions, which is exactly what the manifest is for.

### 22 · Cheapen the grind. Never the judgment.
The takeaway slide of part two. Cheapen phase 3 freely: high volume,
mechanical, and a red suite catches it if the cheap model gets it wrong. Never
cheapen judgment, spec, or ideation — nothing downstream catches a bad
decision, it just gets implemented, faithfully.

And when a phase looks expensive *and* requires judgment: that's a design
smell. Split it — that's what phase 5 is.

### 23 · Do this tomorrow — first thing
Three cards, each an action someone can take tomorrow. Walk them, don't read
them.

Card 1 — say "one of our repos", not one repo. Verified on default branches:
`van-damme-slack-app` (21 harness files + `.harness/models.json`) and
`vn-server` (17 + models.json). `cycle-processing`'s harness lives on `beta`,
not `main`, so don't send anyone to its default branch.

Card 2 — **the account is the blocker, so lead with it.** New Valar workspaces
need approval, so people cannot self-serve: they need an invite from me. Say
that out loud and offer it in the room. Then `valar claude status` today, not
at the wall — the whole point is that this is set up *before* you need it.

Card 3 — the card just names the goal: the whole panel on your PRs. **The
caveat is yours to say out loud, because it does not belong on the slide.**

Say it roughly like this: Baz is automatic. The rest are not — not everyone
has a Cursor or Copilot seat, and @VanDamme has to be summoned. But a missing
seat should not cost you a reviewer, and it doesn't have to: I set up a Grok
bot that watches my team's PRs and summons the whole panel automatically, on
my behalf, for anyone on the team. Van Damme triggers do the same job. **It is
a one-time setup someone does for a team, not a per-person licence.** Offer to
do it for another team in the questions.

Two things that used to be cards and are now yours to say, because the deck
already made both points:
- **Run the loop, don't hand-drive** — the five-phase slides are the argument;
  repeating it here was telling them what they just watched.
- **Keep it on the leash** — that is the closer's job, and principle 1 already
  landed it. Still say it: "all green" is a hypothesis until you re-ran it
  yourself.

Offer to wire someone's repo in the questions.

### 24 · Closer
"A good harness makes the model interchangeable." Then questions.

---

## Appendix · optional

"How to AI" is a much bigger topic than harness + cost. These are the things
that did not fit but are worth having ready. Ordered most- to least-likely to
come up.

### 25 · The decision ladder
The most useful of the appendix slides — reach for this one first if there is
time, or if anyone asks "but when does it stop and ask me?"

The failure mode it prevents: decision fatigue. If the agent surfaces every
choice, then at 6pm you YOLO-click the recommended option a hundred times in a
row. **That is not review** — it is rubber-stamping with extra steps. So make
the agent raise only what it genuinely cannot infer.

The ladder: change nothing observable unless asked → only what was explicitly
requested (ticket, thread, doc) → a behaviour change is forced and nobody
addressed it, so ask. Never ask about mechanism. Record which rung each
decision came from.

Every *technical* call — naming, decomposition, port shape, test structure,
extraction, mutation-noise triage — is the agent's to make and record.
Approval gates feel safe; in practice they convert an autonomous pipeline into
a queue that only moves when you are at your desk.

Cite: Böckeler — a good harness directs human input to where it matters most.

### 26 · Complementary flows
`/spec-to-ship` assumes a packet that is *already* specified. At Versatile
product work happens separately, so by the time a task is approved it has a
JIRA linking PRDs and design docs — `/spec-to-ship JIRA_ID` is where engineer
work starts. What we are presenting is **a harness and one specific way to use
it**: incomplete, and engineer-work adjusted.

When there is no ticket — greenfield, a spike — something has to *make* the
packet. What I use personally is `/grill-with-docs`: it interviews me until
every branch of the design is resolved and sharpens the domain language while
it does, and out comes a packet I can hand to `/spec-to-ship`.

Install: `npx skills@latest add mattpocock/skills` — the installer lets you
pick. **The slide deliberately does not list the other skills in that repo**
(`grill-me`, `to-spec`, `to-tickets` are all in there and all worth a look) —
this slide is one personal tip, not a catalogue. Mention them only if asked.
(I could not verify a `claude plugin install` route for these, so do not
project one.)

The slide links the source — <https://www.aihero.dev/grill-with-docs>, Matt
Pocock's own write-up of the flow. Send anyone who wants the reasoning there
rather than explaining it from the stage; this is an appendix slide and the
link is the point of it.

Not all questions are "grillable" — sometimes it is grill → prototype → grill
again. Also worth saying: what we *don't* have is ticket → packets. There are
sixteen hand-written packet docs in `cycle-processing` and no flow that makes
them.

Copy the harness; then write the flows your work actually starts from.

### 27 · The cheapest seat quota
The turn: *"one last thing, and it is about the seat rather than the harness."*

**The mechanism first, because it is the whole slide.** OpenCode can
authenticate a **SuperGrok subscription directly** — `/connect`, pick xAI,
pick *SuperGrok Subscription*, device-code OAuth. No `XAI_API_KEY`, no console
account. Verified in OpenCode's own provider docs: *"Any Grok or X Premium plan
that includes Grok API access works; you do not need a separate `XAI_API_KEY`."*
Note the condition — the plan has to include Grok API access, so check the tier
before anyone buys.

**Why that matters here and nowhere else.** A Cursor or Claude Code seat is
prepaid quota you can only spend *in that tool*. When it runs dry you flip
tools and lose your context — the exact thing slide 17 is about. A Grok seat
is prepaid quota you spend **inside OpenCode**, so when it runs dry you pick a
Valar or Bedrock model in the same session and keep going. **You change a
model, not a tool.** That is the only reason this slide exists; the price is a
bonus.

**The prices.** Cursor 40/120, Claude Code 25/125, Copilot 19, Grok
10/30/100/300 — all $/user/month. I confirmed Cursor Teams standard/premium
(40/120), Claude Team standard/premium (25 annual / 125 monthly) and Copilot
Business (19) against vendor pages. Grok's ladder and the 3-months-for-1 promo
are read off `grok.com/plans`; **re-read it before you present**, because
xAI's consumer tiers and promos move constantly.

**The Copilot caveat, which you asked me to check rather than take on faith.**
It holds up, with one correction to how you framed it. A Copilot **Business**
seat includes **300 premium requests** per user per month, and overage is
**$0.04 per premium request**. It used to be blocked: every account had an
automatic **$0 premium-request budget**. GitHub made an *overage policy*
generally available in Aug 2025 whose default is **Enabled — allow charges**,
and on **18 Nov 2025** removed the account-level $0 budgets entirely. So your
claim is right: unless an admin explicitly disables the policy, you cross from
prepaid into paid without being asked. The correction: it is not literally
"very little included usage" — code completions and base-model chat are
unmetered — it is that *premium-model* use, which is exactly what an agent
does, is capped at 300 and then billed.

**If someone asks whether they should switch:** the honest answer is that this
is for people who already work in OpenCode. If you live in Cursor or Claude
Code, your seat is the right buy. This slide is for the OpenCode column of
slide 20.

### 28 · Sources
Every claim about *our* repos in this deck is checkable: `cycle-processing`
branch `beta`, and `.harness/models.json` in all three services.

---

## Cut, but keep in your pocket

**Row 2 is not a bug** (cut from the main line). `.harness/models.json` had
`test-author` pinned to `claude-opus-4-8` while every other phase said `opus`.
It looked like rot; it was deliberate. The defect was never the value — it was
that **a deliberate choice and a forgotten one look identical in a diff**, and
the only symptom either way is one phase quietly performing differently. I read
that file as rot and "fixed" it. I was wrong, and nothing in the repo could
have told me. Hence the `rationale` block, and the `check-agent-skills`
pre-push guard. Good answer if anyone asks why the manifest needs prose in it.

**One more, if pushed on mutation testing:** among the ten replaced disables,
one documented decision (reduce precedence, decision 6) had no test at all.


---

## Deck mechanics — read this before editing a slide

**Type size is one number per slide.** Every font size in the deck is written
as `N * var(--u)`, and each `<section class="slide">` carries its own
`--u:min(Xvw, Yvh)` — one number, written twice, with `Y = X × 16/9`. Raise it
to make a slide's text bigger, lower it to make room for more content. There
are no `clamp()` ceilings any more: a hard px cap is what made a 1080p
projector render the same layout with *smaller* text than a laptop, which was
the whole complaint. The `vh` half of the `min()` only bites on a window
shorter than 16:9 — a browser tab rather than a projector — where it scales the
deck down with the height instead of letting it run off the bottom.

To retune a slide after editing it, open the deck and binary-search `--u` for
the largest value where `slide.scrollHeight <= slide.clientHeight`. The ceiling
in use is **1.30vw** — past that, lines get short enough that body copy starts
to read as a headline. Slides sit a few percent under their measured maximum
on purpose, so a machine whose font metrics differ slightly does not overflow.

Two slides opt out. The walkthrough (8) sizes its artifact panel at runtime —
a two-line composer and a mutation report want very different type, so the
panel bisects its own `--up` after every render. The sources appendix (28) runs
small and in two columns, because it is a reference list nobody reads from the
back of the room.

**Boxes stack; columns are opt-in.** `.split` and `.cards` are one column by
default. Two boxes side by side means half-width lines, and a half-width line
is what caps how big the type can be. Add `.cols` only where the side-by-side
comparison *is* the message.

**The kicker is a breadcrumb, not a title.** It is grey and behind a rule so it
reads as chrome. That means every `<h2>` has to make sense with the kicker
covered up — if a headline is a fragment that only lands after reading the grey
line above it, fix the headline.

**Two levels of code emphasis.** In any `pre` the deck highlights, `«…»` in the
source marks a line quietly and `‹…›` marks it loudly (amber box, glow). Both
are stripped before the tokenizer runs, so a mark can never land inside a
syntax span, and both survive on top of the syntax colours. Use the loud one
for the *one* token on a slide that has to be findable from the back — on the
repo slide (9) the five agent files carry `«»` and `coder.md` carries `‹›`.

**Files quoted on a slide are quoted, not written.** Slides 8 and 9 show real
content from `vnatures/van-damme-slack-app` @ main — trimmed with `…`, never
reworded. `reviewer.md` really does say `model: opus`; if the manifest retargets a
phase, the slide is now stale and needs re-quoting, not editing to taste.
On slide 9 the agent card keeps only the frontmatter and the `##` headings,
bodies elided with `…` — same shape as the command card next to it. If you
find yourself adding a paragraph of body text back, don't.

**SVG text does not scale with `--u`.** Diagram labels are in user units and
scale with the viewBox, so they have their own sizes in the stylesheet
(`.fig`, `.cheese`/`.sc-*`, `.plumb`). If you resize a diagram, check its
labels separately.
