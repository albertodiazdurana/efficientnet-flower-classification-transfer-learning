# Feedback: CUDA Setup Pitfalls — Pip-Based GPU Enablement

**Session:** 4 (2026-04-04)
**Project:** Advanced-DS-and-AI-Portfolio-Projects
**Category:** Environment setup, documentation gap
**Severity:** Medium, blocks GPU training until resolved

## Problem

The Session 3 checkpoint listed "Install CUDA 12.5 + cuDNN 9 for local GPU
before training" as a deferred item. This framing suggests a system-level CUDA
toolkit installation, but the actual solution for modern TensorFlow (2.20+) is
pip-based: NVIDIA provides all CUDA runtime libraries as Python packages.

The resolution required four distinct fixes, none of which were documented or
anticipated:

1. **pip install with `--only-binary`**: Without this flag, pip downloads
   source distributions for NVIDIA packages and hangs at metadata preparation.
   The packages are ~500MB+ each, source builds are not viable.

2. **nvJitLink case mismatch**: The `nvidia-nvjitlink-cu12` package installs
   `libnvJitLink.so.12` (camelCase), but TensorFlow looks for
   `libnvjitlink.so.12` (lowercase). Requires a symlink to fix.

3. **VS Code kernel.json env ignored**: The Jupyter kernel spec supports an
   `env` field to set environment variables at process start. VS Code's
   Jupyter extension does not honor this field, so `LD_LIBRARY_PATH` cannot
   be injected via the standard mechanism.

4. **LD_LIBRARY_PATH from os.environ ineffective**: Setting `LD_LIBRARY_PATH`
   via `os.environ` inside Python does not affect the dynamic linker, which
   caches search paths at process start. The fix is to preload all NVIDIA
   `.so` files via `ctypes.CDLL` with `RTLD_GLOBAL` before importing
   TensorFlow.

## Root Cause

No centralized GPU setup documentation existed across projects. The
`computer_vision` project had a working setup captured only in
`requirements_project.txt` (version pins), with no documentation of the
pitfalls encountered during setup. The knowledge was implicit in the working
state, not explicit in a guide.

## Resolution

1. Created `~/gpu-setup.md` — a central, project-agnostic guide covering the
   full setup path including all four pitfalls and their fixes.
2. Updated notebook Cell 1 with ctypes preload (Colab-compatible, no-op when
   NVIDIA packages are absent).

## Recommendation for DSM

Environment setup tasks ("install CUDA", "configure GPU") should be treated as
research-first items, not assumed-simple tasks. When a deferred item involves
system-level tooling:

1. Check existing projects for prior art before starting
2. Document the resolution centrally (not just in the project that solved it)
3. Frame the checkpoint item with the actual mechanism ("pip install NVIDIA
   CUDA packages") rather than the conceptual goal ("install CUDA")
