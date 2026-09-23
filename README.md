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
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

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

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

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

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
