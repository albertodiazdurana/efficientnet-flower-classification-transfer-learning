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

**Takeaway for the blog:** The gap between 24% and 89% isn't incremental. It's the difference between a useless model and a deployable one, and transfer learning got us there with 10 images per class.

### [2026-04-06] Sprint 1 Complete: 91.90% with TTA and Checkpoint Safety

Re-ran the full training pipeline with proper weight checkpointing. Phase 4b reached 92.16% val, Phase 4c degraded to 88.97% again, but this time the automatic rollback restored 4b weights instantly. Test accuracy: 91.01%, crossing the 90% experiment gate.

**Augmentation finding:** A controlled experiment showed data augmentation *hurts* at the head-only stage (-4.90pp). With a frozen backbone and only 10 epochs, augmentation adds noise the model can't adapt to. It starts helping once backbone layers are unfrozen and can actually learn from the variety.

**TTA for free accuracy:** Test-time augmentation (5 views: original, h-flip, ±90° rotation, center crop) pushed 91.01% to 91.90% with zero retraining. Just 1.1 minutes of extra inference, no new training needed.

**Checkpoint pattern:** Saving weights + training histories as JSON after each phase made the notebook re-runnable in seconds instead of 15+ minutes. I'll be doing this in every notebook from now on.
