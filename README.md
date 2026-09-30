# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->
**By Ran Fukazawa ー Campus Life ー**

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

The Unofficial Guide answers plain-English questions about campus life using
the informal, student-written knowledge that never makes it onto the official
university website. I used the `campus_life` corpus — 88 short documents, one
topic per file, covering things like course workloads and exam formats, dining
hall wait times and hours, dorm noise levels and laundry setups, and campus
policies like add/drop deadlines and the housing lottery. Someone can ask
something like "How noisy is Innisfree Hall?" or "How often does the campus
shuttle run on weekdays?" and get back a short, sourced answer drawn from the
matching document — or a clear refusal if the question falls outside what the
corpus covers.

## Chunking Strategy

**Chunk size:** 600 characters
**Overlap:** 100 characters

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

I checked the length of all 88 documents in my corpus before picking a number,
rather than guessing. The longest document (`housing_old_brewhouse.txt`) is
554 characters, and I confirmed no document mixes two separate, unrelated
topics — each one is a single self-contained thought (one dorm's noise, one
course's workload). So I set `CHUNK_SIZE` to 600, comfortably above my actual
maximum, meaning no document in my corpus is ever split. Overlap has no real
effect here for the same reason — there's never a second chunk for it to
bridge — so 100 is a valid but functionally inert setting given my data.

I replaced the starter's `fallback_split` body in `split_documents` with
paragraph-aware merging logic (combining paragraphs up to the size limit,
falling back to a raw character cut only if a single paragraph alone exceeds
it), even knowing it wouldn't visibly change my output — I wanted the function
to behave correctly if this corpus ever grows to include a longer document,
rather than relying on a coincidence of my current document lengths.

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_biol_160.txt#0` — produced by: `chunker.py::split_documents`

```
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.
```

**Chunk 3** — source: `course_hist_118_workload.txt#0` — produced by: `chunker.py::split_documents`

```
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** — source: `dining_pellew_dining_hall_followup.txt#0 ` — produced by: `chunker.py::split_documents`

```
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_innisfree_hall.txt#0 ` — produced by: `chunker.py::split_documents`

```
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:**

If I drop a course after the second week, will it show up on my transcript?

**Answer:**

```
Yes, dropping a course after the second week shows as a W on your transcript.

Source: admin_add_drop_deadline.txt
```

```
```

**My relevance cutoff:**

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->
My relevance cutoff: 0.6

I measured 5 in-corpus and 5 out-of-scope questions. In-corpus best distances 
ranged 0.195–0.425; out-of-scope ranged 0.825–0.934 — a clean gap with no overlap. 
I set the cutoff at 0.6, roughly centered in that gap, which also matches the 
starter's default. My closest in-corpus question (the shuttle, at 0.425) was 
noticeably nearer the boundary than the rest, likely because its phrasing ("how 
often," "weekdays") is more generic than my other four, more topic-specific questions.

| Question | In corpus? | Best distance |
|---|---|---|
| If I drop a course after the second week, will it show up on my transcript? | yes | 0.254 |
| How many hours a week should I expect to spend on MATH 220 Linear Algebra? | yes | 0.195 |
| What are the wait times like at The Ridgeway Café around lunch? | yes | 0.201 |
| How noisy is Innisfree Hall? | yes | 0.272 |
| How often does the campus shuttle run on weekdays? | yes | 0.425 |
| What is the capital of Mongolia? | no | 0.825 |
| How do I change the oil in a diesel engine? | no | 0.934 |
| Who won the 1994 World Cup? | no | 0.886 |
| What is the recommended dosage of ibuprofen for a headache? | no | 0.844 |
| How do I write a for loop in Rust? | no | 0.896 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.**
I asked Claude to help me write the chunking logic for split_documents in Milestone 3. It gave me a paragraph-based merging function that keeps combining paragraphs until adding the next one would exceed CHUNK_SIZE, falling back to a raw character cut only if a single paragraph alone is too long. I kept the logic mostly as given, but I had to decide what CHUNK_SIZE itself should be — I checked all 88 documents' lengths myself first (longest was 554 characters) and set it to 600, since Claude wouldn't pick that number for me.

**2.**
I asked Claude to help me figure out my acceptance criteria for Milestone 2, criteria 4 and 5. It refused to write the criteria outright — the assignment explicitly says not to have AI write these — but it pushed back and asked me questions instead, like whether a criterion I was drafting could actually fail given my data. That's how I realized my first idea for criterion 4 (about chunks splitting correctly) couldn't ever fail, since none of my 88 documents are long enough to trigger a split in the first place. I ended up writing criterion 4 around my CHUNK_SIZE choice being defensible against my longest document instead, and picked criterion 5 (source correctness on my dining hall original/followup pairs) myself once I understood what would make a criterion meaningful.

**3.**
In unit 2, I asked Claude to help me read my before/after run logs and
figure out what actually changed after I lowered CHUNK_SIZE for the
chunking experiment. I hadn't noticed on my own that my out-of-scope
distances had all dropped too (not just my in-corpus ones) — Claude
pointed out that pattern by comparing the two log files side by side,
which changed my "Did it help?" writeup from a simple "yes" to something
that also names the tradeoff. I also asked it to help me write the
Milestone 3 diagnosis and confirmed myself, by rereading `chunker.py`,
that the paragraph-overflow fallback and CHUNK_OVERLAP really were dead
code before accepting that as a real finding rather than taking the
grader feedback at face value.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. CHUNK_SIZE (600) exceeds longest document (554 chars) | 600 ≥ 554 | ✓ | ✓ | ✓ | MET |
| 5. Correct source attribution on original/followup pairs | 4 of 5 | 5/5 (single pass) | 5/5 (single pass) | 5/5 (single pass) | MET |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->
If I drop a course after the second week, will it show up on my transcript?
  run 1: pass  (best distance 0.254)
  run 2: pass  (best distance 0.254)
  run 3: pass  (best distance 0.254)

How many hours a week should I expect to spend on MATH 220 Linear Algebra?
  run 1: pass  (best distance 0.195)
  run 2: pass  (best distance 0.195)
  run 3: pass  (best distance 0.195)

What are the wait times like at The Ridgeway Café around lunch?
  run 1: pass  (best distance 0.201)
  run 2: pass  (best distance 0.201)
  run 3: pass  (best distance 0.201)

How noisy is Innisfree Hall?
  run 1: pass  (best distance 0.272)
  run 2: pass  (best distance 0.272)
  run 3: pass  (best distance 0.272)

How often does the campus shuttle run on weekdays?
  run 1: pass  (best distance 0.425)
  run 2: pass  (best distance 0.425)
  run 3: pass  (best distance 0.425)

Out-of-scope questions (the gate should refuse these):
  refused  (best distance 0.825)  What is the capital of Mongolia?
  refused  (best distance 0.934)  How do I change the oil in a diesel engine?
  refused  (best distance 0.886)  Who won the 1994 World Cup?
  refused  (best distance 0.844)  What is the recommended dosage of ibuprofen for a headache?
  refused  (best distance 0.896)  How do I write a for loop in Rust?
  -> gate refused 5 of 5

Wrote results/run_2026-09-29_2031_before.md
15 model calls this session, 9809 tokens (9096 in, 713 out)

Commit this file. It's the evidence the run actually happened.

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer (4 of 5) | MET | All 3 runs came back 5/5, which clears the 4-of-5 bar with room to spare — no run dropped below target. |
| 2 | Every answer names a source (5 of 5) | MET | All 3 runs were 5/5 exactly, matching the target with no slack — every answer cited a file. |
| 3 | Gate stops out-of-corpus questions (5 of 5) | MET | Single deterministic pass refused all 5 out-of-scope questions; distances (0.825–0.934) were nowhere near the 0.6 cutoff, so this wasn't a close call. |
| 4 | CHUNK_SIZE (600) exceeds longest document (554 chars) | MET | Static fact, not a per-run measurement: 600 ≥ 554 holds by construction and doesn't change between runs. |
| 5 | Correct source attribution on original/followup pairs (4 of 5) | MET | Tested all 5 available pairs (not just 4) and got 5/5 correct citations in a single pass — exceeds target, though I only ran it once rather than three times. |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->
I missed nothing — all five criteria came back MET in the Run Log above.

That said, I don't think this means the system is excellent so much as that
a couple of my targets didn't get a genuinely hard test:

- **Criterion 3** (gate stops out-of-corpus questions) was never close. My
  out-of-scope distances (0.825–0.934) sat far above my 0.6 cutoff, with no
  question anywhere near the boundary. This tells me the gate works on
  obviously-unrelated questions, but I have no evidence about borderline
  ones — a question that's tangentially related to my corpus but still not
  answerable might behave very differently, and I haven't tested that case
  at all.
- **Criterion 4** (CHUNK_SIZE ≥ longest document) is true by construction —
  I set 600 specifically because I already knew my longest document was 554.
  It was never at risk of failing, since I picked the target after seeing
  the number it needed to beat. It's a legitimate criterion (it documents a
  real, deliberate design decision), but it isn't really a *test* of
  anything uncertain.

If I tightened one, it would be **criterion 3**. Instead of "the gate
refuses obviously out-of-scope questions," I'd rewrite it to specifically
target borderline cases — e.g., a question about a topic adjacent to my
corpus but not actually covered (like "what does the university's
diversity policy say," which sounds plausible for a campus_life corpus but
isn't in my 88 documents) — and set the target lower, like 3 of 5, since I
genuinely don't know how the gate performs there.

Separately, my Unit 1 feedback caught a real gap that isn't reflected in any
criterion: `split_documents`'s paragraph-overflow fallback and
`CHUNK_OVERLAP` are both untested and, on inspection, don't actually work as
documented — my README claimed a long paragraph would get a character-level
fallback split, but the code just emits it as one oversized chunk, and
overlap is never read. This didn't cause a miss because no document in my
corpus is long enough to trigger that path, but it's a real, unverified
claim I made about my own code, not something my criteria caught.

## The Improvement

**What I changed:**

I lowered `CHUNK_SIZE` from 600 to 300 (and `CHUNK_OVERLAP` from 100 to 50),
and fixed `split_documents` so it actually reads `CHUNK_OVERLAP` and falls
back to a real character-level cut for an oversized paragraph — neither of
which the original implementation did, despite my Milestone 3 write-up
claiming otherwise.

**Why I picked it:**

My Milestone 3 diagnosis (from grader feedback) found that `CHUNK_OVERLAP`
was dead code and the paragraph-overflow fallback was never exercised or
verified, because nothing in my corpus was long enough to trigger either
path. Lowering `CHUNK_SIZE` forces real splitting, which both tests the
previously-unverified code and directly follows the milestone's own
"second chunking strategy" option.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

### Run Log — After

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. CHUNK_SIZE (300) exceeds longest document (554 chars) | 600 ≥ 554 | ✗ | ✗ | ✗ | **MISSED** (see note) |
| 5. Correct source attribution on original/followup pairs | 4 of 5 | 5/5 (single pass) | 5/5 (single pass) | 5/5 (single pass) | MET |

> **Criterion 4 note:** my original criterion literally names 600 as the
> chosen size, tied to the 554-character maximum. Lowering `CHUNK_SIZE` to
> 300 for this experiment technically breaks that specific criterion as
> written — 300 is well below 554, so documents now do split. I'm treating
> this as an expected, deliberate tradeoff of testing a second chunking
> strategy, not a real regression: the original criterion was about
> defending a *no-split* design choice, and this experiment intentionally
> abandons that choice to test the alternative. See "Did it help?" below.
>
> **Criterion 5:** retested after the chunking change — still 5/5 correct across all 5 dining-hall pairs, no mismatches. Distances shifted (mostly down, e.g. Ridgeway 0.201 → 0.183, Pellew 0.311 → 0.171), consistent with the same pattern seen elsewhere: smaller chunks generally pull tighter, more targeted matches. Kestrel Commons is the one exception worth noting — its distance actually went up slightly (0.342 → 0.381) and its retrieved-sources list shrank to only 3 candidates instead of 5, though the correct pair was still on top.

**Did it help?**

Yes, on the specific thing my diagnosis pointed at — but with a real,
unanticipated cost.

My weakest question before the change (the campus shuttle, at distance
0.425 — the closest of my five to the 0.6 cutoff) improved to 0.1825, and
its retrieved chunks stopped pulling in unrelated material (a stats
workload doc, a dining hall doc) that had been crowding its top-5 before.
Ridgeway Café improved similarly (0.2012 → 0.1828). Both are consistent
with the diagnosis: smaller, more targeted chunks reduce dilution from
loosely-related content.

But my out-of-scope distances also dropped across the board — e.g. the
diesel oil question went from 0.934 to 0.850, ibuprofen from 0.844 to
0.787. No criterion actually flipped to MISSED on this axis (my narrowest
gap is still 0.2715 to 0.787, comfortably either side of 0.6), but the
gap between in-corpus and out-of-corpus distances visibly narrowed.
Smaller chunks appear to make everything look somewhat more "relevant" to
somewhat more questions, not just the ones I wanted to help. That's the
real finding: this fix helped exactly where I diagnosed a problem, but it
wasn't free, and a harder out-of-scope question or a slightly higher
threshold could have exposed that cost more clearly than my current test
questions do.

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

**Criterion 4** (CHUNK_SIZE ≥ longest document) is the one criterion that
came out MISSED after my improvement — and by design, not by accident. My
original target named 600 specifically because it sat above my longest
document (554 characters). Testing a second chunking strategy required
dropping `CHUNK_SIZE` to 300, which breaks that specific target on paper.

I'm not fixing this, because fixing it would mean reverting the actual
improvement (the smaller chunks that measurably helped my shuttle and
Ridgeway Café questions in the "Did it help?" section above). The two
things — "criterion 4 as originally written" and "a smaller, split-testing
chunk size" — are mutually exclusive by construction, and I picked the
latter deliberately once I saw what it would cost. If I kept going, the
next step would be finding a `CHUNK_SIZE` that both triggers real splitting
on a few documents *and* stays defensible under some other stated
criterion (e.g. "no chunk falls below X characters" instead of "no
document ever splits") — but that's a new criterion I haven't written or
tested, not a fix to the old one, and I ran out of time to do both
properly in this unit.

Separately, **criterion 3's margin narrowed** (out-of-scope distances
dropped, e.g. diesel oil 0.934 → 0.850) without actually failing. I
haven't tested a genuinely borderline out-of-scope question — one that's
topically adjacent to campus_life but not actually covered — so I don't
know how much further that gap could narrow before criterion 3 would
start failing for real. I flagged this as a diagnosis in Milestone 3 but
didn't build the harder test question to check it, for the same reason:
time.

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
I'd rewrite **criterion 4**. As written, it was true by construction — I
picked 600 specifically because I already knew 554 was the max, so it was
never at real risk of failing on its own terms. A better version would
have separated the two things I actually cared about: (1) "no chunk falls
below N characters" (a real completeness/fragment risk, independent of
whatever CHUNK_SIZE ends up being) and (2) "CHUNK_SIZE is documented and
justified against the corpus's actual document lengths" (a config sanity
check, not a pass/fail test). Splitting these would have let me change
CHUNK_SIZE for the Milestone 4 experiment without automatically breaking
the criterion — right now, "chunking got better" and "criterion 4 got
worse" are recording the same event from two sides, which makes the
run log table look like a regression when it isn't really one.

I'd also tighten **criterion 3** to include at least one deliberately
borderline out-of-scope question, rather than five confidently unrelated
ones. My current five (Mongolia, diesel engines, the World Cup, ibuprofen,
Rust) are all obviously outside campus_life, and the gate refusing them
tells me less than I originally thought — a topic that's plausible for a
campus corpus but still genuinely uncovered would be a much harder and
more informative test.
