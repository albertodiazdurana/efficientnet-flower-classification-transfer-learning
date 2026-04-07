# Project Retrospective: efficientnet-flower-classification-transfer-learning

**Date:** 2026-04-07
**Duration:** 2026-04-03 to 2026-04-07 (5 calendar days)
**Sessions:** 11
**Project type:** spoke, hybrid (DSM 1.0 + DSM 4.0)

## Project summary

Built a 102-class flower species classifier on the Oxford Flowers 102 dataset using EfficientNetB0 transfer learning with progressive 3-phase fine-tuning (head-only, top 30% unfreeze, full fine-tune with auto-rollback). Delivered as a single Colab-compatible notebook telling the full story from EDA through evaluation. Final result 91.90% test accuracy with 5-view test-time augmentation (gate was 90%), versus 24.56% for a from-scratch baseline CNN. Single notebook runs end-to-end with weight and history caching so reviewers skip retraining on repeat runs.

## DSM effectiveness across the project

- **Tracks used:** DSM 1.0 (notebook collaboration, project-type hybrid) and DSM 4.0 (spoke governance, session protocol). Both were load-bearing.
- **Overall methodology score:** high. The session transcript protocol, pre-generation brief, and what/why/how rule all caught real failures during the project. The sprint plan's boundary checklist prevented a premature closure in Session 8.
- **Biggest DSM win:** the what/why/how universal rule, born inside this project in Session 3, prevented an entire class of structural-check failures for the rest of the sprint.
- **Biggest DSM cost:** per-session protocol overhead at session start and end. For short operational sessions (9, 10), the checklist-to-work ratio was high.

## Phase-level assessment

| Phase | What worked | What was missing |
|-------|------------|-----------------|
| Research/Grounding | Sprint plan up front. Hardware constraints documented in CLAUDE.md before Phase 4. | Runtime verification: CLAUDE.md said "use preprocess_input" but Keras 3 made it a no-op. Not caught until Session 3. |
| Planning | Sprint 1 boundary checklist was a clear success criterion. Phase-by-phase notebook plan matched execution. | Branch strategy: sessions 1-7 reused a single branch because /dsm-light-wrap-up didn't force merge-to-main. |
| Implementation | Progressive unfreezing with auto-rollback was the right architecture. TTA gave a clean +0.9pp for free. Weight+history caching saved enormous iteration time. | 4c full fine-tune never helped on this dataset (1734 images too few). Could have been predicted from capacity math. |
| Communication | README, blog journal, decision log all updated at sprint close. Portfolio + DSM Central notified via inbox. | Session 8 attempted cross-repo write without approval. Process gap, not content gap. |

## Cross-project transferability

**Generalizable patterns:**
- What/why/how before any generation (already promoted to universal rule)
- Weight+history JSON caching for any multi-phase notebook pipeline with expensive training cells
- Auto-rollback on validation degradation (save previous phase weights before next phase training)
- TTA batched across the full test set, not per-image
- Sprint boundary checklist as the closure gate, not the experiment gate

**Project-specific, should NOT be generalized:**
- "Feed [0, 255] to EfficientNet" is a Keras 3-specific note; other libraries and other backbones have different expectations
- "No vertical flips" is domain-specific to flowers, not a general augmentation rule
- The 3-phase progressive unfreeze schedule (head → top 30% → full) was calibrated to ~1700 training images; larger datasets can afford the full unfreeze, smaller ones cannot
- The 92% val → 91% test gap is specific to Oxford Flowers 102; val-test gap varies by dataset

## Recommendations for DSM

Captured in detail in `2026-04-07_project-staa.md`. Summary:
1. Add what/why/how universal rule to the base spoke CLAUDE.md template
2. Add cross-repo write line to spoke Command Execution policy
3. Mandate branch merge-to-main in `/dsm-light-wrap-up`
4. Add "skill self-reference" rule: read the skill before claiming its behavior
5. `/dsm-align` should prime `.claude/reasoning-lessons.md` as part of scaffold setup

## Outcome

Portfolio deliverable complete. Reviewer experience: one notebook, Colab-compatible, end-to-end under ~10 minutes, 91.90% test accuracy, full narrative from EDA through confused-pair analysis. Archived as-is. Sprint 2 will not take place.
