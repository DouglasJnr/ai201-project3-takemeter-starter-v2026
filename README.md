# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->
     This project gather posts from the soccer community on X(formerly Twitter). The trained classifier sorts those post into one of four labels: `match_reaction`, `player_performance`, `tactical_analysis`, or `fan_banter` that describe what the text of the post is about. The classifier also produces a confidence score, representing how sure it was about a label classification it made.



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `match_reaction`

**Definition:** An in-the-moment or immediate emotional response to events in a specific match, such as a goal, a result, or a scoreline.

**Example 1:**
> First win at Everton in 4 years 🤯🔵

**Example 2:**
> FT: Chelsea 6-2 Spurs The proverbial 'game of two halves' as the Blues smash the Lilywhites. A brace apiece from Lauren James & Ellie Carpenter, a howitzer from Sjoeke Nüsken and a stunner from Maika Hamano against her former loan club send the Blues (at least temporarily) top.


### `tactical_analysis`

**Definition:** A post that analyzes how a team plays/played and explains how or why something happened using shape, formation, pressing, build-up, or other structural football concepts.

**Example 1:**
> Credit Carrick. His system has led to the goal. Overload in midfield and Tielemans isn't tracked as he becomes free. There's a weakness on the Man United left defensive side. Far too many crosses coming in as Bruno/Mbeumo stay narrow and Dalot is isolated.

**Example 2:**
> Steve Cooper made some great points about pressure on the ball from a low block. Low block - passive pressure, you’re in trouble.


### `player performance`

**Definition:** A post that evaluates a specific player’s output, such as form, stats, or a rating, with the player as the main subject.

**Example 1:**
>🔥🇦🇷 Lionel Messi counts 23 G/A in the last 14 games with Inter Miami. Two more tonight.

**Example 2:**
>Yan Diomandé adds a golazo to his assist in Real Madrid's 4-1 win over Sevilla. Another superstar performance. Baller 💎

### `fan_banter`

**Definition:** A post whose main purpose is joking, teasing, trash-talking, or memeing about other fans, clubs, or fanbases rather than analyzing or evaluating football.

**Example 1:**
> Some of the goals that Mbappé scores… if it was a certain Barcelona forward, they demand the Ballon d’Or the next day.

**Example 2:**
> DIOMANDE HAS ARRIVED, GIVE ME BARCELONA NOW!

### The hardest boundary

**Which two labels:** `match_reaction` vs `fan_banter`

**The decision rule I used every time:** Ask who the post is aimed at. If it’s addressed to or mocking another fanbase or club, label it fan_banter. If it’s expressing your own feelings about what happened, label it match_reaction. This example is aimed at rival fans, so it’s fan_banter.

### Additional decision rules for difficult boundaries

**`tactical_analysis` vs `match_reaction`:** Ask whether the main purpose is
to explain why something happened or to express an immediate response to it.
If the post uses formations, pressing, possession, defensive shape, or
cause-and-effect reasoning to explain a team's performance, choose
`tactical_analysis`, even when it begins with an emotional reaction or refers
to a match that is still in progress. If it mainly reports a score, result, or
feeling without explaining the cause, choose `match_reaction`. For example,
the Sevilla post is `tactical_analysis` because its main evidence is the
mid-block and pressing explanation, not the opening complaint that Sevilla
threw the game away.

**`player_performance` vs `tactical_analysis`:** Ask whether the player is
being evaluated as the main subject or used to explain a wider team structure.
Choose `player_performance` when the post focuses on an individual player's
form, output, statistics, rating, or direct contribution, such as the Caicedo
example. Choose `tactical_analysis` when the post explains positioning,
movement, pressing, spacing, or the team's system, even if it names one or
more players. A player being mentioned in a tactical explanation does not
make the post `player_performance`.

**`player_performance` vs `match_reaction`:** Choose `player_performance` when
the lasting evaluation of the player's ability or contribution is the main
point. Choose `match_reaction` when the player is only part of an immediate
response to a particular match event, goal, or result. A statistic or
performance judgment supports `player_performance`; an emotional response to
what just happened supports `match_reaction`.

**`fan_banter` vs `player_performance`:** Choose `fan_banter` when the main
purpose is to mock, tease, or provoke a rival club, player, or fanbase.
Choose `player_performance` when the post seriously evaluates what a player
did, even if it uses excitement, exaggeration, or praise. The intended target
of the post takes priority over the presence of a player's name.

**`fan_banter` vs `tactical_analysis`:** Choose `tactical_analysis` when the
post's claim depends on football evidence or an explanation of how the team
played. Choose `fan_banter` when the football reference is mainly a vehicle
for ridicule, rivalry, or a meme and no real tactical explanation is offered.

**Priority rule when several labels seem possible:** classify by the post's
main purpose, not by its loudest phrase. Use this order of questions:
`fan_banter` if the purpose is mockery; otherwise `tactical_analysis` if the
post explains how or why; otherwise `player_performance` if one player's
output is being evaluated; otherwise use `match_reaction` for an immediate
response.

<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** X (Twitter) soccer forum

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I cold labelled the first 24 and then pre-labelled with AI and corrected the remaining 126 posts. I ultimately gathered 150 posts instead of 200 due to time constraints manually gathering the posts.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| match_reaction | 40 | 26.7% |
| player_performance | 33 | 22% |
| tactical_analysis | 53 | 35.3% |
| fan_banter| 24  | 16% |
| **Total** | 150 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* One could argue that Arsenal secured an important victory at a difficult venue, but the reality is that they made life difficult for themselves at the City Ground by dominating possession with a very conservative approach and a very slow tempo, lacking the courage to play inside Forest’s defensive block and in front of their back line.
>
> *Could have been:* tactical_analysis or match_recation
>
> *I chose, because:* tactical_analysis it's a post match reaction but focuses on the explanation of why Arsenal's approach made the match more difficult.

**2.**
> *The post:* Sevilla threw this game away. Who using midblock for Real Madrid? They allowed Madrid start their attacking phase from the middle, entirely letting them cruise past the first phase. Now you are two goals down you want to press them in their box & you’ve always had this ability. So why didn’t you just start this way?
>
> *Could have been:* tactical_analysis or match_reaction
>
> *I chose, because:* match_reaction because while it captures some tactical analysis, the post is mainly a reaction to what is happening in the match

**3.**
> *The post:* Caicedo is the reason we're not leaking goals or struggling in the midfield anymore. We are unbeaten in every game he's played for us this season. The BEST defensive midfielder in the league.
>
> *Could have been:* player_performance or match_reaction
>
> *I chose, because:* player_performance because it shows two areas the player has managed to improve the team which indirectly states his performance is improving the team's overall performance

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** distilbert-base-uncased

**Settings:** <!-- epochs, learning rate, batch size, seed --> EPOCHS = 3
LEARNING_RATE = 2e-5
BATCH_SIZE = 16
SEED = 42

**Anything I changed from the defaults, and why:** None, I was short on time and wanted to get my work submitted so I coudn't fine-tune like I would have wanted to.

**Split sizes:** train 104  ·  val 23  ·  test 23

                    train  val  test
label                               
match_reaction         28    6     6
tactical_analysis      37    8     8
player_performance     23    5     5
fan_banter             16    4     4

[note] 'match_reaction' has only 6 examples in the test split.
[note] 'player_performance' has only 5 examples in the test split.
[note] 'fan_banter' has only 4 examples in the test split.

<!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked Claude to evaluate my criteria 
- *What came back:* It produced more concise criterion
- *What I changed:* I tweaked the language and numbers to maintain what I would still understand

**Moment 2**

- *What I asked for:* I used Claude to verify the formatting of my `labels.csv` file as I was getting errors in my notebook
- *What came back:* a clear breakdown of all the formatting issues and where exactly they were
- *What I changed:* corrected all the errors

**Pre-labelling disclosure:**

**Moment 3**

- *What I asked for:* I asked Claude to evaluate my decision rules that I added for labeling 
- *What came back:* It produced more rigid rules for classifying posts without applying them loosely.
- *What I changed:* I tweaked the rules slightly to maintain what I believe differentiates posts from each other especially on X where context is usually key.

**Moment 1**

- *What I asked for:* I asked Claude to help me brainstorm improvements to my model 
- *What came back:* It produced varying levels of improvement methods
- *What I changed:* I opted for the simplest improvement method due to restraints such as time, and opted to relabel and see if there would be any improvements.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.348 | 0.391 | +0.043 |
| Macro F1 | 0.264 | 0.205 | -0.059 |
| F1 — `fan_banter` | 0.000 | 0.000 | +0.000 |
| F1 — `match_reaction` | 0.400 | 0.286 | -0.114 |
| F1 — `player_performance` | 0.435 | 0.000 | -0.435 |
| F1 — `tactical_analysis` | 0.222 | 0.533 | +0.311 |


**What I predicted before I looked:** The trained model will have a higher macro F1 socre than the baseline model.
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:** The baseline model matched the trained model, the trained model had a slightly better overall accuracy, but lower macro F1 score.
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Achieve at least 70% accuracy for each of the three required random seeds. | ≥0.70 accuracy | 0.391 | 0.348 | 0.348 | MISSED |
| 2. Every label will achieve an F1 score of at least 0.60 | ≥0.60 F1 for every label: match 0.286, tactical 0.533, player 0.000, banter 0.000 | match 0.286; tactical 0.533; player 0.000; banter 0.000 | match 0.000; tactical 0.516; player 0.000; banter 0.000 | match 0.000; tactical 0.533; player 0.000; banter 0.000 | MISSED |
| 3. largest label contains <= 2.5 as many examples as the smallest label in the manually labeled dataset. | ≤2.5:1 | 2.21:1 | 2.21:1 | 2.21:1 | MET |
| 4. shared sample of at least 40 posts independently labeled by another person, the classifier should agree with them at least 65% of the time  | ≥65% agreement on at least 40 posts | Not available | Not available | Not available | NOT TESTED |
| 5. The most-confident third of predictions will be correct at least 85% of the time for each seed | ≥85% accuracy for the most-confident third | 0.714 | 1.000 | 0.714 | MISSED |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | match_reaction | tactical_analysis | player_performance | fan_banter |
|---|---|---|---| --- |
| **match_reaction** | 1 | 5 | 0 | 0 |
| **tactical_analysis** | 0 | 8 | 0 | 0 |
| **player_performance** | 0 | 5 | 0 | 0 |
| **fan_banter**| 0 | 4 | 0 | 0 |


**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
 5 real `match_reaction` posts were called `tactical_analysis`, but no real `tactical_analysis` posts were called `match_reaction`. The same is true for 5 real `player_performance` posts being called `tactical_analysis`. This is evidence of the model collapsing onto one class.


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall Accuracy | ≥0.70  | MISSED | Accuracy was 0.391, 0.348, and 0.348, so none of the seeds reached 0.70. |
| 2 | Every label F1 | ≥0.60 | MISSED | `player_performance` and `fan_banter` had F1 = 0.000 for every seed; `match_reaction` reached only 0.286 once and `tactical_analysis` reached 0.533 at best. |
| 3 | Largest label is no more than 2.5 times the smallest | Largest/smallest ≤2.5:1 | MET | The largest class had 53 posts and the smallest had 24, giving 53/24 = 2.21:1. |
| 4 | Agreement on a shared sample of 30 posts | ≥ 0.65 | MET | Agreement was 25/30 = 83.3% |
| 5 | The most-confident third of predictions | ≥85% | MISSED | The most-confident third was correct 71.4% for seed 42, 100.0% for seed 7, and 71.4% for seed 2024; the target was not met for every seed. |

**Diagnoses**

### Criterion 1 - Accuracy >= 70%
The task is too difficult for the current model and datsset size, with the model collapsing toward `tactical_analysis`. I know because the accuracy was 0.391, 0.348, and 0.348 across the three seeds. The confusion matrix shows that most `match_reaction`, `player_performance`, and `fan_banter` posts were predicted as `tactical_analysis`. The same low performance across all three seeds means this isn't primarily random split instability.

### Criterion 2 - Every label F1 >= 0.60
There are two related causes: insufficient examples for the weaker labels and difficult or incosistently applied label boundaries.
`player_performance` and `fan_banter` had F1 = 0.00 for every seed. The test set contained only 5 `player_performance` examples and 4 `fan_banter` examples, so those label scores are based on very few examples. The model consistently collapsed onto the `technical_analysis` label.

### Criterion 5 - The most-confident third of predictions will be correct at least 85% of the time for each seed
The model's confidence is not reliable enough, which points to a calibration problem associated with the model's weak class separation rather than a test-set split problem.
Section 6 reports accuracy for the most-confident third as 71.4% for seed 42, 100.0% for seed 7, and 71.4% for seed 2024. Because the target had to hold for every seed, the criterion was missed in two of the three runs. The model was still collapsing toward `tactical_analysis`, and the most-confident predictions were not consistently correct. The variation also shows that confidence is unstable on this small test set.


<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** __25_ / 30 = _83__%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:* "Carbon plates aren't the whole story. My times dropped 2% in the shoes and another 2% the following year in the same pair, when the only thing that changed was going from 35 to 50 miles a week."
>
> *Staff said / I said:* analysis | hot_take
>
> *My call, and why:* hot_take, because I incorrectly interpreted the post which lead me to apply the rule loosely even though it covered it
>
> *Which pile:* (a) the rule covered it and I applied it loosely

**2.**
> *The post:* "Ran the same loop in 4C and in 24C within a fortnight, same effort by heart rate. 90 seconds slower in the heat over six miles. People keep saying heat costs you 'a bit' and it's more than that."
>
> *Staff said / I said:* analysis | reaction
>
> *My call, and why:* reaction, I applied the rule loosely because I did not think the weather was used as the reason for a slower run, it was a post about the author's experience.
>
> *Which pile:* (a) the rule covered it and I applied it loosely


**3.**
> *The post:* "Went back through the club results going back to 2012. Every single year the number of people who enter the spring marathon and don't start it sits between 18 and 23 percent. It's not a bad-luck year, it's the base rate."
>
> *Staff said / I said:* analysis | hot_take
>
> *My call, and why:* hot_take because I did not see this year's rate and so did not think I could truly classify this post as analysis if the numbers weren't fully supporting the claim.
>
> *Which pile:* (c) the rule is ambiguous here and my reading is defensible 


**4.**
> *The post:* "Three of us ran the same session on the same day, different shoes, within 10 seconds of each other over 5 miles. I'm not saying shoes do nothing, but whatever they do was smaller than the noise between three people."
>
> *Staff said / I said:* analysis | reaction
>
> *My call, and why:* The post is about the author and their experience, their claim about the shoes did not appear load bearing to me for me to classify the post as analysis, and so reaction was more fitting for me
>
> *Which pile:* (c) the rule is ambiguous here and my reading is defensible 


**5.**
> *The post:* "Times are down about 3% since the shoe rules changed, so obviously the shoes are the whole story and the sport has a problem."

>
> *Staff said / I said:* hot_take | analysis
>
> *My call, and why:* Claim is that times are down due to shoes because they started slowing down at the time shoe rules were changed so that would be the reason for the change in times. The number is the argument in thi scenario according to the decision rules.
>
> *Which pile:* (c) the rule is ambiguous here and my reading is defensible 

**What the pattern in my disagreements tells me:** My disagreements were either a consistency issue with loose application of the rules, or they were a debatable scenario that was either debatable according to the decision rules or the rule was too ambiguous to solidify one choice.



---

## The Improvement

**What I changed:** I added new boundary rules for my labels and relabeled the posts in my `labels.csv` file according to the new rules.

**Which diagnosis pointed at it:** Diagnosis 2 pointed at this as the model classifications were collapsing onto the tactical analysis label.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥0.70 | 0.348 | 0.348 | 0.348 | MISSED |
| 2. Every label F1  | ≥0.60| match 0.000; tactical 0.516; player 0.000; banter 0.000 | match 0.000; tactical 0.516; player 0.000; banter 0.000 | match 0.000; tactical 0.516; player 0.000; banter 0.000 | MISSED |
| 3. largest label to smallest label ratio| ≤2.5:1 | 2.29:1 (55/24) | 2.29:1 (55/24) | 2.29:1 (55/24) | MET |
| 4. shared sample agreement | ≥65% | 83.3% (25/30) | 83.3% (25/30) | 83.3% (25/30) | MET |
| 5. The most-confident third of predictions | ≥85% | 0.714 | 0.857 | 0.714 | MISSED |

**Did it help, and how do I know:**

My improvement didn't help, it made the model slightly worse, and no criterion changed its verdict. The diagnosis I sought to improve did not improve. The model continues to collapse onto the `tactical_analysis` label for everything. The overall accuracy is therefore a reflection of the share of tactical posts. The relabel made the imbalance slightly worse.
<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->

---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->
### Criterion 1 - Accuracy >= 70%
**What I'd do:** Change the training settings rather than the labels. I would raise `EPOCHS` and `LEARNING_RATE` then check that the training loss drops well below 1.386 (the loss of a model guessing blindly between four labels). My 83% agreement with the staff labels suggests my labelling is not the main problem, so the next fix should target the model.

**Why I stopped:** I was short on time, and every three-seed run on my laptop took several minutes per seed. I chose to finish the write-up rather than start a hyperparameter search I couldn't complete properly.

### Criterion 2 - Every label F1 >= 0.60
**What I'd do:** Collect more posts for the thin labels, aiming for the 200 the brief asks for, with the extra posts going mostly to `fan_banter` and `match_reaction`. With only 4–6 test examples per label, one prediction changes a label's F1 a lot. I would also try class weights in the loss so the model is penalised more for always guessing `tactical_analysis`.

**Why I stopped:** Gathering posts by hand from X was the slowest part of the project, which is why I stopped at 150. Class weights would mean changing the notebook's training code, which I didn't have time to do and test.

### Criterion 5 - Most-confident third >= 85%
**What I'd do:** Nothing directly. While the model predicts one label for every post, its confidence just reflects how "tactical-sounding" a post is, so this criterion can't be fixed on its own. Once criteria 1 and 2 improve, I would re-check it. A third of 23 test posts is only 7 predictions, so one error moves the score by 14 points.

**Why I stopped:** It depends on fixing the model collapse first.



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->
I meant my labels to capture the purpose of a post: reacting, evaluating a player, explaining tactics, or mocking rivals. The model learned none of those purposes and instead predicts `tactical_analysis` for every post, which the after confusion matrix shows.



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
