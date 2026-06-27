---
topic: What Data represents vs What it hides
domain: Foundations
difficulty: Beginner
confidence: "9"
status: Complete
url:
---
## Core (Explanation)

Data is a **compressed story** about reality, not reality itself. It picks a few aspects of the world, turns them into symbols (numbers, categories, text), and throws the rest away.

**Analogy (strong one):**

Think of data as a city map.
-  The map shows roads, names, landmarks: that’s what the data represents. It hides smells, noise, people’s moods, and temporary roadblocks: that’s what the data does **not** capture. If you navigate only by the map, you can move around, but you will still be surprised by things the map never showed you.

---

## Why This Exists 

The core problem: we cannot store or compute on the full complexity of reality, so we need a selective representation. Data solves this by giving a manageable, structured snapshot we can store, query, and analyze.

If this concept did not exist (if we treated data as “the full truth”):

- Models would be over-trusted on domains that are underrepresented, biased, or missing entirely in the data (e.g., medical models trained mostly on one demographic).

---
## Formal Definition

Data is a structured representation of selected aspects of some target system or phenomenon, created through a measurement or encoding process, for a particular purpose. It **represents** what was measured and **hides** what was ignored, unmeasured, or distorted.

**Key assumptions (often implicit):**  
- The measured variables are relevant to the question (“adequate for purpose”).
- The recording process is reasonably accurate (limited measurement error).
- The sample is representative of the population or scenario you care about (no strong representation bias).
- Missing data is limited or behaves “nicely” (e.g., random enough not to systematically skew conclusions).

**When those assumptions fail:**
- Variables: Important causal factors are omitted → models latch onto proxies and spurious correlations.
- Measurement: Systematic measurement error (e.g., faulty sensors) leads to biased estimates even with huge data.
- Representation: Underrepresented groups or regions make models brittle and unfair off-distribution.
- Missingness: Non-random missing data quietly distorts parameter estimates, significance, and generalization.
---
## How It Works Internally (Step-by-Step)

World → Measurement → Encoding → Dataset → Interpretation.

Each step is **both**:
- A necessary abstraction to make the problem tractable.
- A potential source of “what the data hides” (omissions, distortions, biases).
---
## Tiny Example

Scenario: You want to predict if a user will churn from a subscription app.

**World:**  

- True drivers: price sensitivity, satisfaction, competing apps, internet quality, personal finances, support quality.  

**Your data design:**
- You log only: **`sessions_per_week`**, **`avg_session_length`**, **`has_turned_off_notifications`**, **`country`**.
- Churn label: 1 if user left within 30 days.  

**What data represents:**
- Engagement patterns via sessions and time spent.
- Some coarse context via **`country`**.

**What it hides:**
* Whether users left due to high price, bad support, or competitor offers (no variables about pricing changes, tickets, or campaigns).
- Within-country inequality (rural vs urban, income levels) since “country” is too coarse.  

**Walkthrough:**
1. You train a classifier on this dataset.
2. It learns “low sessions_per_week → churn”.
3. You conclude “just increase engagement” and launch gamification features.
4. But many users actually churned due to price and bad support; engagement did not solve that.
5. Your model “works” on historical logs but fails to reduce churn in production because **the key causal factors were never represented in the data.**
---
## Failure Modes & Pitfalls

**Common misunderstandings:**
- “More data = better truth”: Large biased datasets can make wrong conclusions look statistically confident.
- “If it’s in the dataset, it’s objective”: Data reflects collection choices, social context, and incentives; it is not neutral.  

**Silent failure cases:**
- Underrepresented segments: Model performs well on average but fails for small groups not well captured in training data.
- Hidden confounders: Omitted variables cause spurious correlations that models happily exploit.
- Non-random missingness: People who skip surveys may systematically differ from those who answer, biasing conclusions.  

**When this gives misleading results:**
- When you use proxy variables (zip code as proxy for income, click as proxy for satisfaction) without acknowledging what they hide.
- When you push models to new domains assuming the existing data schema captures the new context (e.g., hospital A model used in hospital B with different patient mix and protocol).
---
## How to Know If It’s Working

**Signals of success:**
- Out-of-sample and out-of-domain checks: performance degrades gracefully, not catastrophically, when applied to slightly different populations or time periods.
- Error analysis by subgroup: no hidden pockets of very high error for specific demographics, regions, or edge cases.
- Domain-expert sanity checks: experts say “these variables and patterns roughly match how the real system behaves.”

**Symptoms when it’s broken:**
- Model accuracy is high in cross-validation, but business or real-world outcomes barely improve or even get worse.
- Performance collapses on new cohorts, time windows, or geographies → indicates data represent only a narrow slice of reality.
- Surprising, unstable feature importances when you re-sample or re-train → often suggests hidden confounders or data artifacts. 
---

## One Mental Model to Remember

Use the **“satellite photo vs being on the ground”** model:
- Data is a satellite photo: excellent for seeing structure, trends, and big roads.
- Reality is being on the street: you notice construction, people’s behavior, noise, and temporary events.

---
## Additional Notes
1. https://sustainability-directory.com/fundamentals/data-limitations-fundamentals/
2. https://issues.org/limits-of-data-nguyen/
3. https://www.pragmaticinstitute.com/resources/articles/data/5-common-bias-affecting-your-data-analysis/