# Blog Journal

Append-only capture file for blog-worthy observations. Entries accumulate
across sessions and are extracted into materials files at project/epoch end.
Reference: DSM_0.1 Blog Artifacts (three-document pipeline).

## Entry Template

### [YYYY-MM-DD] {Title}
{Observation, story, pattern, or insight}

### [2026-04-04] Sprint 1: Transfer Learning on 10 Images Per Class

Built a flower classifier for 102 species using only ~10 labeled images per class. A from-scratch CNN managed 24.56% test accuracy, barely above random (1/102 = ~1%). EfficientNetB0 with progressive unfreezing hit 89.27%, a 3.6x improvement, trained in under 10 minutes on a consumer GPU.

**The checkpoint lesson:** Phase 4b (top 30% unfrozen) reached 93.87% val accuracy. Phase 4c (full fine-tune) degraded to 90.93%. Because I didn't save the 4b weights, I couldn't roll back. The test accuracy of 89.27% reflects 4c, not the best model. Lesson learned: always checkpoint before risky fine-tuning phases. `restore_best_weights` only restores within a single `.fit()` call, not to pre-fit state.

**Error analysis insight:** The model's top confusion pairs (petunia/tree mallow, petunia/hibiscus, rose/globe-flower) all share color palette and petal structure. This suggests the model relies heavily on color and shape, and more training data for these specific classes would have the highest ROI.

**Takeaway for the blog:** Transfer learning isn't just a performance boost, it's a paradigm shift for small datasets. The gap between 24% and 89% isn't incremental; it's the difference between a useless model and a deployable one.
