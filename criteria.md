# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**

I expect at least one of my five questions to be harder than the rest (e.g. a 
topic covered by only one or two documents), so I'm not requiring a perfect 5 
of 5.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**

Every retrieved chunk always carries its own source filename, and generate.py 
has access to that metadata for every chunk it's given — there's no path where 
an answer is produced without a source attached, so I expect this one to hold 
at 5 of 5 with no exceptions.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**
<!-- What did your distances look like when you set the cutoff in Milestone 4?
     Was there a clean gap, or did the two groups overlap? -->

---

## 4. Something about your chunks

My chosen CHUNK_SIZE is 600 characters, which is longer than the longest document 
in my corpus (554 characters, housing_old_brewhouse.txt), confirmed by checking 
the length of all 88 documents rather than a sample.

**Why this target:**

No document in my corpus mixes two topics, and the longest is only 554 characters, 
so a chunk size at or above that should never split a document mid-thought.

---

## 5. Your choice

For at least 4 of 5 test questions about a dining hall that has both an original and
a followup document, the answer cites the source document that actually matches the 
hall asked about — not a mismatched or unrelated pair.

**Why this target:**

My corpus has several dining halls with near-duplicate original + followup docs 
(same wait times, same details, reworded). I want to know my system pulls the right 
hall's source, not just a plausible-looking one, since these pairs are similar enough 
that retrieval could get confused between them.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
