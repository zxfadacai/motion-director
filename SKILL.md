---
name: motion-director
description: Create code-driven motion films from a creative brief, including showreels, product films, launch stories, and explainers. Use when the user wants a reproducible HTML, Remotion, or GSAP-based motion piece with storyboarding, sound, rendering, review, and delivery artifacts.
---

# Motion Director

Use the complete production prompt in
[references/motion_prompt_zh_v2.md](references/motion_prompt_zh_v2.md) as the
authoritative workflow. Read it in full before planning or producing anything.

## Baseline Behavior

- Execute the reference as written. Do not improve, rewrite, shorten, or replace
  its workflow during baseline testing.
- Preserve the user's supplied brief values. For blank fields, use the
  reference's delegation rule and record each choice in `brief.md`.
- Honor the reference's hard constraints, mode rules, `◆` checkpoints, review
  loop, quality bar, and deliverables.
- If the environment cannot satisfy a required capability, stop and state the
  limitation. Do not silently substitute a different workflow or claim an
  unperformed check passed.
- Keep the film, source, metadata, review records, and QA results together in one
  project directory so runs can be compared.
