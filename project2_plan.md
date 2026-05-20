# Project 2 Research Plan
## Regime-Conditioned TFI with an Orthogonal LOB-Based Detector
## in ES Futures
*Gabriel Ungab — Georgia Institute of Technology*
*Plan version: May 2026*

---

## Overview and Motivation

This document is the formal research plan for Project 2, which
extends "Regime-Conditioned Trade Flow Imbalance and Adverse
Selection in ES Futures" (Ungab, 2026). Project 2 has two
complementary objectives:

1. **Methodological improvement (Phase 0):** Address all remaining
   quality weaknesses in the original paper through additional
   analysis on existing trades data and presentation revisions.
   Phase 0 is completed entirely on the existing trades dataset
   before MBO data is opened, and is committed to the original
   repository.

2. **Empirical extension (Phases 1–5):** Test whether an orthogonal
   LOB-based regime detector, constructed from MBO data, produces
   significant regime-conditioned TFI forward return predictability,
   or confirms that the in-sample stable-conditions result was
   circularity-driven. A three-way detector comparison (Detector A:
   original trades-only baseline; Detector B: hand-crafted LOB;
   Detector C: ML-based LOB) isolates the contribution of the ML
   functional form from the contribution of using orthogonal LOB
   inputs at all.

---

## Three-Way Detector Comparison and Paper Decision

The empirical extension tests three detector formulations:

- **Detector A (baseline):** Original trades-only lambda×TAR
  multiplicative composite. Already estimated; results in hand.
  Serves as the circularity-contaminated baseline.
- **Detector B (hand-crafted LOB):** Theoretically motivated
  multiplicative composite of LOB features from MBO data, with no
  aggressor-side signed flow anywhere.
- **Detector C (ML-based LOB):** L2-regularized logistic regression
  classifier trained on the same LOB feature set as Detector B.

**Final paper decision** is deferred until Phase 3 results are known.
If at least one of Detector B or Detector C produces a significant
positive result, the extension is published as a standalone second
paper. If both are null, Phase 0 improvements and Phase 3 MBO results
are integrated into a single unified revision of the original paper.
The Detector B null / Detector C positive case requires a Phase 4
design decision on whether the positive result reflects a genuine
finding or an overfitting artifact before the paper decision is made.
The paper decision is recorded as a required output of Phase 4.

---

## Phase Structure

---

### Phase 0 — Trades Data Quality Improvements
### (Completed Before MBO Data Is Opened)

Phase 0 addresses all remaining quality weaknesses from the original
paper that are executable on the existing trades dataset. The MBO
external SSD is not opened during Phase 0. All outputs are committed
to the original repository (`es-regime-tfi-trades`).

Items are ordered by recommended working sequence: simple empirical
items first (minimal new code), complex empirical items second, writing
revisions last. Presentation revisions (P0-9, P0-10, P0-11) are
drafted in Phase 0 but held for final language until after Phase 3
results are known. All writing revisions are applied directly to
PAPER.md and committed.

**Output directory:** `/results/final-improvements/` in the original
repository.

---

#### P0-1 — R² Decomposition

Clarify that R² = 0.236 is driven by the mean-reversion control.

**Procedure:**
1. Re-run the primary regression without the Rt control term.
2. Report R² with and without the control alongside each other.
3. Add clarifying sentence to Section 5.1: "Without the
   mean-reversion control, R² = [X], confirming that the regime-TFI
   interaction accounts for essentially none of the return variance."

**Required outputs:**
- Primary regression re-run without Rt control term, result committed
  to `/results/final-improvements/`
- R² with and without the control reported side by side in PAPER.md
  Section 5.1 with one clarifying sentence

---

#### P0-2 — Threshold Sensitivity

Stress-test the high-regime threshold of 0.5.

**Procedure:**
1. Re-run the detector validation regression (Equation 5) at
   thresholds of 0.4, 0.5, and 0.6.
2. Report percentage of bars classified as high-regime at each
   threshold.

**Required outputs:**
- Detector validation regression (Equation 5) re-run at thresholds
  0.4, 0.5, 0.6, results committed to `/results/final-improvements/`
- Percentage of bars classified as high-regime at each threshold
- Threshold sensitivity table added to robustness section of PAPER.md

---

#### P0-3 — Additive Combination Robustness

Report the additive RegimeScore result for transparency.

**Procedure:**
1. Construct additive RegimeScore:
   `[logistic(z_lambda) + logistic(z_TAR)] / 2`, normalized to [0,1].
2. Re-run the primary regression (Equation 6) using the additive
   RegimeScore.
3. Frame as: "the multiplicative formulation was adopted on
   theoretical grounds; the additive result is reported here for
   transparency."

**Required outputs:**
- Additive RegimeScore primary regression result committed to
  `/results/final-improvements/`
- One additional row in robustness section of PAPER.md

---

#### P0-4 — Lambda and TAR Window Sensitivity

Parameter sensitivity analysis for both rolling window lengths, run
together in the same script pass.

**Procedure:**
1. Re-run the full primary regression at lambda window lengths of
   15, 30, and 60 bars (TAR fixed at 5 bars), and at TAR window
   lengths of 3, 5, and 10 bars (lambda fixed at 30 bars).
2. Also re-run the stable-conditions analysis at each lambda window
   length.
3. If results are sensitive to TAR window, provide additional
   theoretical grounding citing Dufour and Engle (2000).
4. Report β̂₃ and p-value for all combinations in a single combined
   sensitivity table.

**Required outputs:**
- All lambda and TAR sensitivity regression results committed to
  `/results/final-improvements/`
- Stable-conditions analysis re-run at each lambda window length,
  results committed
- Combined sensitivity table added to robustness section of PAPER.md

---

#### P0-5 — Expanded Announcement Exclusion Set

Justify the announcement exclusion set relative to other high-impact
events.

**Procedure:**
1. Expanded pre-specified set: FOMC, CPI, NFP, PPI, advance GDP,
   retail sales (six highest-impact releases for ES futures per
   Andersen et al., 2007). Fed speaker events excluded — unscheduled
   in content, context-dependent impact.
2. Re-run primary regression with expanded 30-minute
   post-announcement exclusion.
3. Report change in N, β̂₃, and p-value versus original exclusion.

**Required outputs:**
- Expanded exclusion set regression result committed to
  `/results/final-improvements/`
- Change in N, β̂₃, and p-value vs. original exclusion reported
- Announcement sensitivity row added to robustness section of PAPER.md

---

#### P0-6 — Pre-Announcement Window Characterization

Descriptive characterization of the retained pre-announcement windows.

**Procedure:**
1. Identify the 30-minute pre-announcement windows for all 6 FOMC
   events in the in-sample period.
2. Compute mean RegimeScore and mean |TFI| in these windows vs.
   full-sample means.
3. Report as a descriptive footnote only. No inferential claim.

**Required outputs:**
- Mean RegimeScore and mean |TFI| for 6 FOMC pre-announcement windows
  vs. full-sample means committed to `/results/final-improvements/`
- Descriptive footnote added to PAPER.md Section 4.3 (no table, no
  inferential claim)

---

#### P0-7 — Formal Bias Simulation

Permutation simulation confirming upward bias direction on existing
trades data. Requires new simulation code — complete after P0-1
through P0-6.

**Procedure:**
1. Generate 1,000 synthetic datasets under H0 by permuting forward
   returns (Rt+1) while preserving the joint distribution of TFIt
   and RegimeScoret.
2. Apply the full primary regression pipeline (Equation 6) to each
   permuted dataset. Record β̂₃ for each permutation.
3. Compare observed β̂₃ = 0.000371 to the null distribution.
   If observed value falls above the null median, upward bias
   direction is confirmed.
4. Run the same simulation on the stable-conditions subsample
   (N = 18,355) to characterize whether β̂₃ = 0.001016 is similarly
   explained by bias alone.
5. Report: full null distribution histogram, observed value's
   percentile, simulation-based p-value.

**Required outputs:**
- 1,000-permutation simulation results for the full in-sample dataset
  committed to `/results/final-improvements/`
- 1,000-permutation simulation results for the stable-conditions
  subsample (N = 18,355) committed
- Null distribution histogram, observed β̂₃ percentile, and
  simulation-based p-value for both runs
- Simulation section added to PAPER.md replacing "plausibility
  argument" language of the original

---

#### P0-8 — Market Maker Implications Reframing

Writing revision for Section 7.

**Revision:** The sentence presenting the stable-conditions gradient
as a directional insight must be rewritten so that all three
disqualifying caveats (post-hoc threshold, no OOS replication,
circular stability metric) appear before any directional framing. The
revised framing: "the stable-conditions gradient motivates a specific
future investigation, but the three caveats documented above preclude
any current directional application."

**Required output:** Revised paragraph committed to PAPER.md Section 7
(draft; final language after Phase 3 results are known)

---

#### P0-9 — Net-Return Magnitude Framing

Writing revision for Section 7.

**Revision:** The calculation using β̂₃ = 0.000371 must be explicitly
labeled as an upper bound on the noise-level magnitude associated with
a null result, not a plausible signal estimate. The calculation using
β̂₃ = 0.001016 retains its existing caveats.

**Required output:** Revised paragraph committed to PAPER.md Section 7
(draft; final language after Phase 3 results are known)

---

#### P0-10 — Efficiency Interpretation Logic Chain

Writing revision for the Abstract and Section 5.1.

**Revision:** The logical chain must be stated explicitly: "The
contemporaneous validation establishes that the regime detector
identifies elevated within-bar price impact (2.278× amplification,
p < 0.001). This within-bar amplification, if genuine and not purely
circularity-driven, implies that regime-conditioned order flow
information acts on prices within the bar rather than across bars —
making the null forward-return result a consequence of within-bar
information incorporation rather than an absence of signal. The
extension's orthogonal LOB-based detector test will determine whether
this interpretation is warranted."

**Required output:** Revised Abstract and Section 5.1 committed to
PAPER.md (draft; final language locked after Phase 3 contemporaneous
result is known)

---

*Phase 0 complete. MBO data may now be opened.*

---

### Phase 1A — MBO Data Familiarization

First contact with the MBO dataset (~60GB, stored at
`X9 Pro/raw-market-data/es-futures/mbo/`). Scope is entirely
schema-level — no signal construction, no outcome variables examined.

**Required outputs before Phase 1A is considered complete:**
- MBO schema documentation: all event types (add, cancel, modify,
  fill), field definitions, and mapping to original trades feed fields
- Order-ID tracking verification: confirmation that order IDs are
  consistent across add/cancel/modify events for the same resting
  order (prerequisite for the cancellation-based label)
- L2 LOB reconstruction: validated best-bid/ask and depth-by-level
  snapshot series built from the L3 MBO event stream; this is the
  foundation for all Phase 1C feature computation
- L2 reconstruction validation: no negative depths, no crossed books,
  fill events consistent with trades feed aggressor-side
  classifications
- Sample LOB snapshot: tabular verification of the reconstructed book
  at a representative timestamp for at least one session (best
  bid/ask price, spread, depth at each level) to catch silent
  miscalibrations that pass statistical validation
- Intraday event arrival rate characterized by event type (adds,
  cancels, modifies, fills separately)
- Holiday and roll handling documented consistent with original
  paper's exclusions
- Data quality flags: timestamp gaps, anomalous sessions, and any
  sessions requiring exclusion documented

**Constraint:** No LOB-derived features computed beyond the
reconstruction validation. No forward returns constructed or examined.

---

### Phase 1B — Detector Design, ML Classifier Design,
### and Literature Review
### (Research and Critical Thinking)

The intellectual core of Project 2. This phase is not data work. Its
output is a complete, pre-specified design for all three detectors
and the ML classifier, argued from theory and literature, before any
feature is computed from MBO data.

#### Detector B — Hand-Crafted LOB Detector

Specify a theoretically motivated composite of LOB features using
MBO data. Required design document outputs:
- All LOB feature components with theoretical grounding for each
- Verification that each component is orthogonal to TFI by
  construction: no component may be derived from aggressor-side
  signed order flow in any form
- The combination method (multiplicative preferred for consistency
  with Detector A's theoretical hierarchy, but must be argued from
  the literature)
- All rolling window lengths argued from theory or literature
- The stability criterion for the stable-conditions analysis: must be
  derived entirely from LOB structure, sharing no inputs with TFI or
  any detector component. A continuous interaction (rather than a
  data-driven threshold) is preferred to avoid repeating the post-hoc
  problem. Orthogonality to the ML classifier's feature set must also
  be verified.
- Contemporaneous test specification: whether sub-bar resolution is
  included, the lag structure used, and explicit statement of
  residual confounding at each specification

#### Detector C — ML-Based LOB Detector

Specify an L2-regularized logistic regression classifier trained on
the same LOB feature set as Detector B.

**Label:** A **cancellation-based depth-erosion measure**, computable
only from MBO data via order-ID tracking. Within each 1-minute bar,
compute the net cancellation imbalance — the difference between
bid-side cancellations and ask-side cancellations, normalized by
total resting depth at bar open. A bar with systematic bid-side
cancellation asymmetry indicates market makers withdrawing liquidity
in anticipation of informed buying. This label:
- Contains no aggressor-side signed flow — orthogonal to TFI by
  construction
- Is grounded in Glosten and Milgrom (1985): market makers widen
  quotes and withdraw in response to adverse selection risk
- Is structurally leading: depth erosion fires before price impact
  becomes visible, making the label prospective rather than
  retrospective in character. Sub-bar timing analysis (whether
  cancellation asymmetry in the first fraction of a bar predicts
  price impact in the remainder) is deferred as Direction 2 in the
  paper's future research section.
- Shares a residual causal connection with TFI: the same informed
  trader driving cancellations also drives TFI, though no signed flow
  appears in the label itself. This bias is upward toward H1 and is
  documented in the paper; any null result remains a conservative
  bound on the true causal effect.
- The exact label formula (threshold definition, handling of low-
  cancellation bars, whether to use top-of-book only or multiple
  depth levels) is determined in Phase 1B and pre-registered in
  Phase 2. Hard constraint: no aggressor-side signed flow under any
  formulation.

**Model class:** L2-regularized logistic regression.
- Outputs a probability in [0,1] directly interpretable as a regime
  score (drop-in replacement for RegimeScore in Equation 6)
- Pre-registerable with one meaningful hyperparameter (regularization
  strength C), selected by time-series cross-validation (not random
  k-fold, which would introduce lookahead bias)
- Linear decision boundary is a regularization advantage given the
  training sample size
- Coefficient vector is interpretable: directly reveals which LOB
  features the data-driven classifier weighted most, enabling
  comparison with Detector B's hand-crafted component hierarchy

**Temporal holdout design:**
- Training period: May–September 2025 (~105 trading days, ~40,000
  bars after exclusions)
- Validation period for regularization parameter selection:
  October–December 2025
- Classifier frozen before any forward-return regression is run
- OOS period (2026 Jan–Mar) untouched as always

#### Literature Review Addendum

A minimum floor of four topic areas must be covered during Phase 1B
to justify the design decisions above. Specific papers are determined
during Phase 1B execution:
1. Cancellation-based and depth-erosion signals as leading adverse
   selection indicators
2. MBO data structure and its information content relative to LOB
   snapshots
3. Temporal cross-validation methodology for financial ML (to justify
   the classifier training design)
4. LOB feature predictiveness of adverse selection and price impact
   (to justify the feature set)

**Required output before Phase 1B is considered complete:** One
committed design document containing all Detector B specifications,
all Detector C specifications (label formula, model class, temporal
holdout design), and the literature review addendum. No MBO feature
computation begins until this document is committed.

---

### Phase 1C — LOB Feature and Label Characterization
### (Exploratory, Inputs Only)

With the design document from Phase 1B in hand, compute and
characterize the chosen LOB features and the cancellation-based label
from MBO data. This is exploratory analysis of inputs only — no
outcome variables examined.

**Required outputs before Phase 1C is considered complete:**
- Distribution characteristics (mean, std, skewness) and intraday
  profiles of each pre-specified LOB feature
- Autocorrelation structure of each LOB feature (informs whether
  pre-specified window lengths are appropriate)
- Cross-correlations between chosen LOB features (checks for
  redundancy warranting design document revision)
- Distribution, intraday profile, and lookahead bias verification of
  the cancellation-based label
- Verification that the label shows expected intraday patterns
  (higher cancellation asymmetry near open and around announcements,
  lower at midday)
- Class frequency of the cancellation-based label: proportion of bars
  in the positive class (required empirical input for Phase 2
  class-weighting decision)
- Revision note if any pre-specified feature or the label is not
  computable as designed — with explicit documentation of the revision
  reason committed to the repository before proceeding to Phase 2

---

**Constraint:** Forward returns are not computed or examined in this
phase.

---

### Phase 2 — Pre-Registration

All design decisions finalized and committed. Hard gate before any
regression involving forward returns is run on MBO data.

**Required output before Phase 2 is considered complete:** One
committed pre-registration document containing all of the following:
- Final LOB feature set with orthogonality verification for each
  component
- Detector B full specification: all parameters and combination
  method locked
- Detector C full specification:
  - Label formula locked (exact, no aggressor-side signed flow under
    any formulation)
  - Training period (May–September 2025) and validation period
    (October–December 2025) locked
  - Regularization parameter selection procedure locked:
    time-series cross-validation
  - Class imbalance handling decision locked: class-weighted logistic
    regression used if positive class frequency from Phase 1C falls
    below a pre-specified threshold (stated explicitly here)
  - Explicit statement that classifier weights will not be modified
    after the validation period
- Stability criterion and threshold method locked (continuous
  interaction preferred)
- Naive volatility benchmark specification locked: primary regression
  re-run with realized intraday volatility dummy at a pre-specified
  volatility percentile
- Primary regression specification for all three detectors (same
  structure as Equation 6, replicated for each RegimeScore)
- Contemporaneous test specification for all three detectors
- OOS period locked (2026-01-02 through 2026-03-06)
- All pre-specified sensitivity checks listed
- Paper decision rule locked
- Explicit statement: "No regression involving forward returns will
  be run before this document is committed"

---

### Phase 3 — Formal MBO Analysis

Execute all pre-registered regressions. No specification search. No
post-hoc additions without explicit flagging as exploratory.

**Required outputs before Phase 3 is considered complete:**

For each of Detector A, B, and C:
- Detector validation: contemporaneous amplification test result with
  residual confounding explicitly characterized (analogous to Section
  4.4 of original paper)
- Primary forward-return regression (T+1): β̂₃, z-stat, p-value, N
- Horizon analysis: T+5 and T+15 results
- Stable-conditions analysis result using the pre-specified orthogonal
  stability criterion
- Subsample stability results: May–September vs. October–December
- OOS validation result: held-out 2026 period, no parameter refitting
- Lagged regime conditioning robustness result: fully predetermined
  specification
- Transaction cost analysis: required only for any result where β̂₃
  is statistically significant

Additional for Detector C only:
- Feature importance analysis: logistic regression coefficient vector
  with confidence intervals, with commentary on whether the
  data-driven weights align with Detector B's theoretical component
  hierarchy

For the pre-specified naive volatility benchmark:
- Primary regression re-run with realized intraday volatility dummy,
  result reported alongside the three-detector results

**Constraint:** Any result not in the pre-registration document is
labeled exploratory and clearly distinguished from confirmatory tests.

---

### Phase 4 — Integration, Conditional OOS Test, and Paper Decision

**Conditional permutation test for the late-OOS episode:**

The anomalous OOS significance (β̂₃ = 0.000774, p = 0.022)
concentrated in late February through early March 2026 has no
orthogonal conditioning variable explaining its origin. The
conditional permutation test provides the strongest available
characterization; Phase 3 LOB OOS results provide additional evidence
— if the LOB-based detectors replicate the significance, the episode
gains interpretive weight; if not, it is likely a circularity
artifact of the trades-only detector.

**Procedure:**
1. Segment the late-OOS period (Feb 23–Mar 6, 2026, N = 3,290 bars)
   into high-regime and low-regime cells using RegimeScore > 0.5.
2. Permute forward returns within each cell separately (preserving
   the marginal distribution of returns given regime state).
3. Run 1,000 permutations, recovering β̂₃ from each.
4. Compare observed late-OOS β̂₃ = 0.000774 to the conditional null
   distribution.
5. Compute the β̂₃/β̂₁ ratio in early vs. late OOS: if stable, the
   late-OOS result reflects proportional TFI amplification; if
   disproportionately large in the late period, it is regime-specific.

**Required outputs before Phase 4 is considered complete:**
- Conditional permutation test: null distribution, observed β̂₃ =
  0.000774 percentile, and conditional p-value
- β̂₃/β̂₁ ratio diagnostic for early OOS vs. late OOS
- Final PAPER.md with P0-8, P0-9, and P0-10 writing revisions
  finalized using Phase 3 results
- Final paper decision recorded (standalone vs. unified)
- Final paper PDF produced and committed to the appropriate repository

---

### Phase 5 — Code Polish, Paper Finalization, Repository Cleanup

Standard final phase analogous to Phase 6 of the original project.

**Required outputs before Phase 5 is considered complete:**
- All code in both repositories reproducible from raw data (trades
  data in original repository; MBO data in new repository)
- README updated in both repositories
- CLAUDE.md updated in both repositories
- Final PDF committed to the appropriate repository per the paper
  decision recorded in Phase 4

---
