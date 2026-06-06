# NIFTY Implied Volatility Surface Reconstruction

## Overview

This project focuses on reconstructing missing implied volatility (IV) values for NIFTY options across multiple strikes and timestamps.

The competition objective was to predict missing IV values as accurately as possible while maintaining a clean, reproducible, and financially sensible methodology.

Rather than treating each missing value independently, the project approaches the problem as a volatility surface reconstruction task, leveraging the smoothness of implied volatility across neighboring strikes.

---

## Final Result

**Best Public Leaderboard Score:** `0.0000423397`

**Final Selected Method:**
Strike-wise Linear Interpolation with Extrapolation

**Final Reproducible Notebook:**
`final_submission_results/final_linear_submission.ipynb`

**Final Submission File:**
`final_submission_results/submission_best_linear_fit.csv`

---

## Repository Structure

```text
IV_Surface_Reconstruction/

├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_baselines.ipynb
│   ├── 03_modeling.ipynb
│   ├── ...
│   └── experiment notebooks
│
├── final_submission_results/
│   ├── final_linear_submission.ipynb
│   └── submission_best_linear_fit.csv
│
├── outputs/
├── data/
├── src/
├── requirements.txt
└── README.md
```

---

## Experiment Summary

| Method | Validation MSE | Kaggle Public Score | Status |
|----------|----------:|----------:|----------|
| Mean Fill | 0.023462 | Not Submitted | Rejected |
| Linear Interpolation | 0.000090 | **0.0000423397** | ✅ Final Model |
| Quadratic Interpolation | 0.000590 | Not Submitted | Rejected |
| PCHIP Interpolation | 0.000637 | Not Submitted | Rejected |
| Moneyness-Based Interpolation | ~0.000090 | Not Submitted | No Improvement |
| ATM-Aware Interpolation | 0.000092 | Not Submitted | Rejected |
| Gap-Aware Interpolation | 0.000625 | Not Submitted | Rejected |
| Gap + ATM Interpolation | 0.000092 | Not Submitted | Rejected |
| Surface Interpolation | Worse than Linear | Not Submitted | Rejected |
| Time Interpolation | Worse than Linear | Not Submitted | Rejected |
| LightGBM | 0.001283 | Not Submitted | Rejected |
| Neighbor Feature LightGBM | 0.000555 | Not Submitted | Rejected |
| Variance Space Interpolation | ~0.000090 | Not Submitted | Identical to Linear |
| Edge-Time Hybrid | Sandbox: 0.014568 | 0.0044056987 | Failed on Kaggle |
| Expanded Edge-Time Hybrid | Sandbox: 0.015353 | Not Submitted | Worse than Edge-Time |
| Edge Clamp | Sandbox: 0.017838 | Not Submitted | Promising but Unsubmitted |

---

## Dataset Understanding

The dataset consists of:

* NIFTY underlying prices
* Multiple Call Option (CE) implied volatilities
* Multiple Put Option (PE) implied volatilities
* Missing IV observations distributed across the surface

Key observations:

* Missing values were spread relatively uniformly across strikes.
* Most missing regions were isolated or short gaps.
* The volatility surface exhibited strong local smoothness across neighboring strikes.
* Call and Put surfaces behaved similarly in validation.

These observations motivated interpolation-based approaches before moving to machine learning models.

---

## Validation Strategy

To evaluate candidate methods, a masking-based validation framework was developed.

### Procedure

1. Select known IV observations.
2. Randomly mask a subset of values.
3. Reconstruct the masked values.
4. Compare predictions against true values using Mean Squared Error (MSE).
5. Repeat across multiple random seeds.

This allowed every method to be evaluated under the same conditions before generating Kaggle submissions.

The workflow intentionally avoided look-ahead bias and ensured fair comparison between methods.

---

# Experiment Journey

The project followed an iterative validation-driven approach.

Every new method was tested against the baseline before being considered for submission.

---

## Baseline: Mean Imputation

### Idea

Replace missing values with average IV.

### Result

Very poor reconstruction quality.

### Conclusion

Volatility surfaces contain local structure that cannot be captured by global averages.

---

## Strike-wise Linear Interpolation

### Idea

For each timestamp:

* Sort strikes
* Interpolate missing values across neighboring strikes
* Use extrapolation for edge strikes

### Validation

Consistently achieved the lowest reconstruction error.

### Kaggle Result

**0.0000423397**

### Conclusion

Strong local smoothness across strikes made linear interpolation surprisingly effective.

This ultimately became the final selected model.

---

## Quadratic Interpolation

### Idea

Replace linear interpolation with quadratic curves.

### Result

Higher validation error than linear.

### Observation

Additional curvature introduced instability and overfitting.

### Conclusion

Rejected.

---

## Cubic / Spline-Based Interpolation

### Idea

Use smoother higher-order curves.

### Result

Failed to outperform linear interpolation.

### Conclusion

Added complexity without improving reconstruction accuracy.

Rejected.

---

## PCHIP Interpolation

### Idea

Shape-preserving cubic interpolation.

### Result

Consistently worse than linear interpolation.

### Observation

Even though the surface is smooth, local linear relationships dominated.

### Conclusion

Rejected.

---

## Moneyness-Based Interpolation

### Idea

Use strike relative to underlying price rather than absolute strike.

### Result

Produced nearly identical behavior to standard linear interpolation.

### Conclusion

No measurable advantage.

Rejected.

---

## ATM-Aware Interpolation

### Idea

Identify the strike closest to the underlying price and interpolate separately around ATM.

### Result

Validation performance nearly identical to linear interpolation.

### Conclusion

Did not justify additional complexity.

Rejected.

---

## Gap-Aware Interpolation

### Idea

Treat small and large missing gaps differently.

* Gap size = 1 → Linear interpolation
* Gap size > 1 → Neighbor-based averaging

### Result

Validation performance worsened.

### Conclusion

Standard linear interpolation already handled gaps effectively.

Rejected.

---

## Surface Interpolation

### Idea

Treat IV as a 2D surface:

(time, strike) → IV

### Result

Significantly worse than strike-wise interpolation.

### Observation

The time dimension introduced noise rather than useful information.

### Conclusion

Rejected.

---

## Time Interpolation

### Idea

Interpolate IV through time for each strike.

### Result

Underperformed compared to strike interpolation.

### Conclusion

Temporal information alone was insufficient.

Rejected.

---

## Edge-Time Hybrid

### Idea

For extreme strikes only:

* Use neighboring timestamps
* Replace strike interpolation

### Sandbox Result

Looked promising.

### Kaggle Result

Performance collapsed.

### Conclusion

Validation environment and leaderboard behavior diverged.

Rejected.

---

## Expanded Edge-Time Hybrid

### Idea

Apply Edge-Time logic to more edge strikes.

### Result

Worse than the original Edge-Time experiment.

### Conclusion

Rejected.

---

## Edge Clamp

### Idea

Replace extrapolation with edge clamping.

### Observation

Produced reasonable validation behavior.

### Status

Promising but not sufficiently validated before submission deadline.

Not selected.

---

## Variance Space Interpolation

### Idea

Convert IV to variance.

Interpolate variance.

Convert back to IV.

### Result

Practically identical to linear interpolation.

Difference was negligible.

### Conclusion

No meaningful improvement.

Rejected.

---

## LightGBM

### Features

* Strike
* Option type
* Underlying price
* Moneyness
* Additional engineered features

### Validation

Much worse than interpolation.

### Observation

Machine learning failed to exploit additional information effectively.

### Conclusion

Rejected.

---

## Neighbor Feature LightGBM

### Features

* Left IV
* Right IV
* Neighbor mean
* Neighbor differences
* Moneyness
* Strike features

### Result

Improved over basic LightGBM but still failed to beat linear interpolation.

### Conclusion

Rejected.

---

## Error Analysis

Extensive error analysis was performed.

### Findings

Largest reconstruction errors repeatedly occurred at extreme strikes:

* 23800 PE
* 23900 PE
* 26400 CE
* 26500 CE

These contracts require extrapolation rather than interpolation, making them inherently harder to reconstruct.

Despite this, alternative edge-handling techniques failed to consistently outperform the baseline.

---

# Why Linear Interpolation Won

The final conclusion of the project was that implied volatility surfaces in this dataset are dominated by local strike smoothness.

Most sophisticated approaches attempted to model:

* temporal structure
* higher-order curvature
* machine learning relationships
* volatility surface dynamics

However, validation repeatedly showed that these additions introduced more noise than signal.

The simplest model consistently generalized best.

---

# Final Method

For each timestamp:

1. Sort strikes in ascending order.
2. Interpolate missing CE values across strikes.
3. Interpolate missing PE values across strikes.
4. Use linear extrapolation for edge strikes.
5. Apply a final sanity check to prevent non-positive IV values.
6. Generate the submission file.

---

# Reproducibility

The final submission can be reproduced exactly by running:

```text
final_submission_results/final_linear_submission.ipynb
```

This notebook generates:

```text
final_submission_results/submission_best_linear_fit.csv
```

which corresponds to the submitted Kaggle solution.

---

# Key Takeaways

* Simpler models can outperform more sophisticated alternatives.
* Validation-driven experimentation is critical.
* Implied volatility surfaces exhibit strong local smoothness.
* Machine learning does not automatically outperform financial intuition.
* Strike-wise interpolation proved to be the most robust solution for this dataset.

---
