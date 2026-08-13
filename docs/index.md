# Project Documentation


## Phase 4. Technical Modification

### What Changed

I made three targeted modifications to enhance the investigation:

1. **Neutral Baseline Selection**: Used `BASELINES[3]` instead of the first (Adelie) baseline. This broader baseline (bill_length=45, bill_depth=16, flipper_length=205, body_mass=4000) helps avoid analysis bias toward one species.

2. **Feature Pairing in 2D Grid**: Changed the prediction grid from (bill_length × flipper_length) to (bill_length × body_mass). The 1D sweeps revealed that flipper_length contributes less to predictions, while body_mass warranted investigation alongside the dominant bill_length feature.

3. **3D Visualization Addition**: Added a 3D scatter plot holding body_mass fixed at 4300g, sweeping bill_length, flipper_length, and bill_depth simultaneously. This reveals decision boundaries in higher-dimensional space more clearly than 1D or 2D alone.

### Why These Changes

The modifications align with findings from systematic feature probing—bill_length emerged as the dominant predictor, so testing it against body_mass (a correlated biological feature) and visualizing in 3D provided a richer picture of decision boundaries.

### Verification

- All cells executed successfully, generating expected plots and data frames
- The 3D plot exported to an interactive HTML file (`fixed_body_mass_3d_graph.html`)
- Boundary patterns were consistent across multiple runs (after API "wake-up")

## Phase 5. Custom Project

### Basis and API

**Model**: ML Penguin Predictor
**Deployed at**: https://ml-penguin-predictor.onrender.com/predict
**Task**: Multi-class classification predicting penguin species (Adelie, Chinstrap, or Gentoo)
**Inputs**: Four biometric features (bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g)
**Data Source**: Palmer Penguins dataset

I kept the original API to maintain consistency with the example, allowing direct comparison of investigation techniques.

### Investigation Approach

I used a layered probing strategy:

1. **Baseline Confirmation** (Section 2): Verified the API responds correctly to known penguin measurements from each species.
2. **1D Feature Sweeps** (Section 3): Systematically varied each feature individually while holding others constant to identify which features drive predictions.
3. **2D Decision Grid** (Section 4): Created a heatmap visualizing predictions across a 2D feature space (bill_length × body_mass).
4. **3D Visualization** (Section 4+): Extended the grid to three dimensions, revealing decision boundaries with body_mass fixed.
5. **Edge Case Testing** (Section 5): Probed robustness by submitting invalid, extreme, and missing inputs.

### Findings: Feature Sensitivity

**bill_length_mm is dominant:**
- Predictions shift from Adelie → Gentoo → Chinstrap around 45mm
- Clear decision boundary observable in 1D sweep chart
- [See Figure 1: bill_length sensitivity sweep]

**flipper_length_mm is weak:**
- Sweep across 160–280mm shows only Adelie predictions
- Does not act as a primary differentiator
- Suggests model relies on other features for species distinction
- [See Figure 2: flipper_length sensitivity sweep]

**body_mass contributes minimally:**
- 2D grid (bill_length × body_mass) shows boundaries driven almost entirely by bill_length
- Horizontal species regions indicate body_mass does not influence transitions
- [See Figure 3: 2D prediction grid (bill_length vs body_mass)]

**3D view clarifies the surface:**
- First complete visualization showing all three species
- With body_mass fixed at 4300g, all three species appear when combining bill_length, flipper_length, and bill_depth
- Indicates these three features together contain the decision information that bill_length alone cannot capture
- [Interactive 3D plot: [fixed_body_mass_3d_graph.html](../images/fixed_body_mass_3d_graph.html) — rotate and zoom to explore decision boundaries]

### Findings: Edge Cases

**Robust handling:**
- Missing features: API correctly rejects with error (expected behavior)

**Gaps in validation:**
- Extreme values (bill_length = 1.0 or 999.0): Returns a prediction without warning
- Negative values (bill_length = -10): Returns a prediction
- Zero values (body_mass = 0): Returns a prediction

These edge cases should trigger validation errors or at minimum be flagged as out-of-range. Real penguin measurements fall within narrow biological bounds (e.g., bill_length ≈ 32–59mm).

### Summary

**Model confidence and fragility:**

The model is **confident in bill_length decisions**: It reliably distinguishes species along this dimension with a clear 45mm transition zone. However, this also reveals a potential fragility—the model may be overreliant on this single feature.

The model **struggles to leverage additional features**: flipper_length alone does not differentiate species when other features are held constant. Only the 3D view (combining bill_length, flipper_length, and bill_depth) captures all three species, suggesting decision boundaries are not axis-aligned.

**Robustness issues:**

The API lacks input validation. Physically impossible measurements (negative dimensions, zero mass, or extreme bills) are accepted and generate predictions. For deployment, stricter bounds checking is essential.

**Recommended improvements:**

1. Validate inputs against biological ranges: bill_length ∈ [30, 60], flipper_length ∈ [170, 230], body_mass ∈ [2700, 6300]
2. Return confidence scores alongside predictions
3. Log or flag out-of-range inputs for monitoring
4. Consider retraining or feature engineering to reduce bill_length dominance

**Applicability:**

This investigation technique (1D sweeps → 2D grids → 3D visualization → edge cases) scales well for low-dimensional models. For models with 10+ inputs, dimension reduction (PCA, SHAP) becomes necessary. The approach is particularly useful for deployed models where source code and training data are unavailable—a common scenario in practice.
