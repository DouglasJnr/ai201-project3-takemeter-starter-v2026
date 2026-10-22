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
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

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

**Agreement rate:** ___ / 30 = ___%

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
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



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
