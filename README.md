VLM-Gated DreamerV3 Training For Atari Gameplay

## Abstract

This project investigates the integration of Vision-Language Models (VLMs) into the DreamerV3 reinforcement learning framework to enhance performance on Atari games. Starting with static VLM interventions, the approach evolved to dynamic gating mechanisms that trigger advice based on real-time game state. Experiments began on Atari Boxing, a dense-reward environment, before extending to Montezuma's Revenge, a sparse-reward challenge. Results demonstrate modest improvements in sample efficiency for Boxing, with VLM guidance accelerating early learning. The framework uses Gemini 1.5 Flash for cost-effective advice, integrated via environment wrappers that monitor pixel differences and other triggers.

## Introduction

Reinforcement learning agents like DreamerV3 excel at learning world models from pixels, but struggle in environments with sparse rewards or complex exploration. This work augments DreamerV3 with VLM calls to provide strategic guidance, initially at fixed intervals and later dynamically via surprise detection. Boxing served as a testbed for dense rewards, while Montezuma's Revenge tested scalability to harder tasks. All experiments adhere to DreamerV3's atari100k configuration, training for up to 400K environment steps.

## Methods

DreamerV3's architecture includes a CNN encoder, recurrent state-space model (RSSM), and actor-critic heads. VLM integration occurs through a custom environment wrapper that computes pixel-based surprise metrics (e.g., MSE between frames) as a proxy for dynamics loss. When thresholds are exceeded, Gemini 1.5 Flash analyzes screenshots and context to generate action sequences, queued to override agent decisions temporarily.

For Boxing, early runs used basic surprise gates. Improvements added multi-model fallbacks, adaptive thresholds, and caps on VLM calls (e.g., 20 total). Montezuma's extensions included stagnation triggers (after 300 rewardless steps), episode-start consultations, and room novelty bonuses (+2 reward for new discoveries), with game-specific knowledge in prompts.

## Results

On Boxing, vanilla DreamerV3 achieved a mean score of 17.2 over 44 episodes at ~300K steps. VLM-gated variants improved to means of 26.5 and 18.9 across two runs (53 and 45 episodes), yielding a pooled gain of +5.8 points (Cohen's d = 0.28). Critically, VLM agents reached positive scores nearly twice as fast, with all 12 interventions occurring within the first 50K steps, biasing the world model toward productive trajectories.

Montezuma's baseline, trained for 400K steps, produced low scores as anticipated (literature reports 1852 at 200M steps). Dynamics loss tracked for gating signals. Advanced VLM setups targeted sparse rewards with triggers for stagnation and exploration, though quantitative results from these runs are not available in this repository as the notebooks were not executed.

## File Structure

- **Boxer_Baseline/**: Initial Boxing experiments with VLM gates.
  - Boxer_Baseline1.ipynb: 500K-step run with Gemini integration.
  - Boxer_Baseline2.ipynb: Dynamics-loss proxy gating.
  - training_results.json: Logs from one run (mean score 61, max 100).

- **Boxer_Improvements/**: Refined Boxing runs with safety measures.
  - vlm-boxing-improvement1.ipynb & VLM_boxing_improvement2.ipynb: Multi-model support, auto-stop at 350K steps or 5 hours.
  - Results_Analysis.txt: Improvement summary.
  - Results/combined_results.json: Baseline vs. VLM comparisons.

- **Improvements/**: Montezuma's iterations.
  - Improvement Notebook 1: Multi-trigger gates and room bonuses.
  - Improvement Notebook 2-4: Variations on triggers and rewards.

- **Montezuma Baseline/**: Vanilla DreamerV3 on Montezuma's (400K steps for baseline).
  - Montezuma_Baseline_400k.ipynb: Initial draft (20K steps).
  - montezuma_baseline_final.ipynb: Final version with 400K steps.

## Authors
Iqra Khurram (27100376) | Xeerak Azhar (27100310)
