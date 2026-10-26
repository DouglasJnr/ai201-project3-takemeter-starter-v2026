# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.The classifier will achieve at least 70% accuracy for each of the three required random seeds.

<!-- Your criterion. It must name a number. -->



**Why this target:** With four labels, random guessing would be about 25% accurate. A 70% target requires the model to distinguish meaningful differences between match reactions, tactical analysis, player performance, and fan banter rather than relying only on common words.



---

## 2. Every label will achieve an F1 score of at least 0.60.

<!-- Your criterion. -->



**Why this target:** The labels have overlapping language, especially match_reaction and fan_banter, so overall accuracy could hide poor performance on one category. Each label should be useful on its own, while allowing some variation because smaller labels will have less stable scores.



---

## 3. The largest label will contain no more than 2.5 times as many examples as the smallest label in the manually labeled dataset.

<!-- Your criterion. -->



**Why this target:** A heavily overrepresented label could make the classifier appear accurate while underperforming on less common categories such as `tactical_analysis` or `fan_banter`



---

## 4. On a shared sample of at least 40 posts independently labeled by another person, the classifier should agree with them at least 65% of the time

<!-- Your criterion. -->


**Why this target:** Some posts could fit more than one label, especially match_reaction and fan_banter. A 65% agreement rate would show that the classifier usually makes similar decisions to another human.

> **Revised in Unit 6:** On a shared sample of at least 30 posts independently labeled by another person, the classifier should agree with them at least 65% of the time
>
> **Why revised:**  The sample `staff_labels` have 30 posts instead of 40, so using 30 was more relaistic to get an actual verdict than simply 'missing' because of a broken criterion.

---

## 5. Predictions assigned a confidence of at least 0.80 will be correct at least 85% of the time

<!-- Your criterion. -->


**Why this target:** High-confidence predictions should be dependable enough to use with minimal manual review. This is particularly important for posts that contain both emotional language and football analysis, where the model should avoid presenting an uncertain classification as reliable.

> **Revised in Unit 6:** The most-confident third of predictions will be correct at least 85% of the time for each seed.
>
> **Why revised:** section 6 did not report accuracy for predictions with confidence >= 0.80. It reports accuracy for the most-confident third instead which meant the original criterion cannot be evaluated from the available results. The revised criterion allows me to obtain a verdict


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
