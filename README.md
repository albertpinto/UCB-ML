#  Will the Customer Accept the Coupon?

**UC Berkeley ML/AI — Practical Application Module 5**

This project explores a survey-based dataset to understand what kinds of drivers accept location-based coupons — and what kinds do not. The goal is to give a marketing team a clear, plain-English picture of who is worth targeting and when.

📓 **Notebook:** [`prompt.ipynb`](prompt.ipynb)

---

## The Question

Imagine driving through town and a coupon for a nearby restaurant, bar, or coffee house appears on your phone. Would you accept it? Drive over right away, save it for later, or ignore it entirely?

The data behind this project comes from a survey on Amazon Mechanical Turk (originally published in the UCI Machine Learning Repository). Respondents were given different driving scenarios — destination, weather, time of day, who was in the car — and asked whether they would accept the coupon. About **12,684 scenarios** were collected across five coupon types: cheap restaurants, expensive restaurants, bars, coffee houses, and carry-out.

---

## How the Data Was Prepared

A few quick clean-up steps before any analysis:

- **Dropped the `car` column** — 99% of values were missing.
- **Dropped `toCoupon_GEQ5min`** — every value was the same (1), so it carried no information.
- **Filled small gaps in dining-frequency columns** (`Bar`, `CoffeeHouse`, `CarryAway`, `RestaurantLessThan20`, `Restaurant20To50`) with an explicit `"unknown"` category. These columns had under 2% missing values, but rather than throw out ~600 rows, the missing entries were kept as their own category so the analysis preserved statistical power.

---

## Headline Number

**Across all coupon types, 56.8% of scenarios resulted in acceptance.** That's the baseline against which every more interesting comparison gets measured.

---

## What We Learned About Bar Coupons

Bar coupons had the **lowest overall acceptance rate** of any coupon type — about **41%**. But that average hides a huge split:

| Driver Type | Acceptance Rate |
|---|---|
| Goes to a bar more than 3 times a month | **~76%** |
| Goes to a bar 3 or fewer times a month | **~37%** |
| Frequent bar-goer **and** over 25 | **~70%** |
| Frequent bar-goer, no kids in car, non-farming occupation | **~71%** |
| Combined "good profile" (see notebook q6) | **~71%** |
| Everyone else | **~30%** |

The pattern is consistent and clear: **age, occupation, and passenger profile matter, but only because they correlate with the one thing that actually drives acceptance — whether the person already goes to bars.**

---

## What We Learned About Coffee House Coupons (Independent Investigation)

Coffee house was chosen for the independent investigation because it has the largest sample size of any coupon type and the most interesting interaction effects.

The same pattern showed up, even more cleanly:

| Driver Type | Acceptance Rate |
|---|---|
| Visits a coffee house more than 3 times a month | **~68%** |
| Visits a coffee house 3 or fewer times a month | **~45%** |
| Combined profile (frequent + non-kid + not widowed, OR under 30, OR low-income frequent diner) | **62.3%** |
| Everyone else | **35.8%** |

A **26-point gap** between drivers who match the combined profile and those who don't.

---

## Hypothesis

> **Coffee house coupon acceptance is driven primarily by the driver's existing coffee house visit frequency.** Drivers who already visit at least once a month are substantially more likely to accept, and situational factors — non-kid passengers, being under 30, mid-day timing — further lift acceptance above the ~50% baseline.

The same one-line takeaway applies to bar coupons, and probably to every other coupon type in the dataset:

> **People accept coupons for places they already like to go.**

---

## Recommendations

1. **Target by existing behavior, not demographics.** Visit frequency is a stronger predictor than age, income, or marital status. Marketing budgets should prioritize known frequent visitors.
2. **Don't waste impressions on infrequent or never-visitors** — their acceptance rates hover near 30%, well below the dataset baseline.
3. **Avoid sending coupons when kids are in the car.** Across both bar and coffee house coupons, "Kid(s)" as passenger consistently dragged acceptance down.
4. **Mid-day delivery beats early-morning or late-night** for coffee house coupons specifically (worth deeper analysis).

---

## Next Steps

- Replicate the analysis on the remaining three coupon types (cheap restaurants, expensive restaurants, carry-out) to confirm the frequency-dominates pattern holds universally.
- Move from descriptive statistics to a predictive model (logistic regression or a tree-based classifier) and quantify the lift from each feature.
- Test interactions between **expiration window** (2h vs. 1d) and **time of day** — early evidence suggests 1-day coupons accept far better, but this hasn't been broken out by context.

---

## Repository Structure

```
.
├── README.md          ← this file
├── prompt.ipynb       ← analysis notebook
└── data/
    └── coupons.csv    ← UCI ML Repository dataset
```

---

## Tools Used

Python · pandas · NumPy · Matplotlib · Seaborn · Jupyter
