# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the official repository for **"Mathematical Foundations of Reinforcement Learning"** (Springer, 2025) by S. Zhao (Westlake University). It contains:

- Book chapters as individual PDFs and a combined `Book-all-in-one.pdf`
- Lecture slides in two variants: `slidesForMyLectureVideos/` (matching video lectures) and `slidesContinuouslyUpdated/` (latest revisions)
- A grid-world environment implementation (Python and MATLAB) for developing and testing RL algorithms

The book's 10 chapters cover: basic concepts, Bellman equation, Bellman optimality equation, value/policy iteration, Monte Carlo learning, stochastic approximation/SGD, TD learning, value function approximation, policy gradient methods, and actor-critic methods.

## Running the Grid World Code

### Python

```bash
# Requirements: Python 3.7-3.11, numpy, matplotlib
pip install numpy matplotlib

# Run the example
cd "Code for grid world/python_version/examples"
python example_grid_world.py
```

### MATLAB

Requires MATLAB R2020a+ (uses `exportgraphics()`). Open and run `Code for grid world/matlab_version/main_example.m`.

## Code Architecture

All code lives under `Code for grid world/`.

**Python structure:**
- `python_version/src/grid_world.py` — Core `GridWorld` class implementing `reset()`, `step(action)`, `render()`, `add_policy()`, `add_state_values()`
- `python_version/examples/arguments.py` — Centralized configuration via `argparse` (grid size, start/target/forbidden states, rewards, animation settings)
- `python_version/examples/example_grid_world.py` — Usage example demonstrating environment setup, policy, and visualization

**Key conventions:**
- The `GridWorld` class defaults are bound to `args` from `arguments.py` at import time
- Actions are tuples: `(0,1)` down, `(1,0)` right, `(0,-1)` up, `(-1,0)` left, `(0,0)` stay
- Policies are state-action probability matrices (rows=states, columns=5 actions, rows sum to 1)
- Coordinates use (column, row) format — `(0,0)` is top-left
- No test suite, linting, or CI/CD exists — this is educational code meant for manual verification

## Knowledge Base (`knowledge_base/`)

A 13-file, ~7,900-line structured reference distilled from the textbook. Small enough to search directly with `Read`/`Grep` — no MCP server or vector DB needed.

### File Map

| File | Content |
|------|---------|
| `00-overview.md` | Reading order, dependency diagram, chapter summary table, navigation tips |
| `01-basic-concepts.md` | Ch 1: States, actions, policies, rewards, returns, MDP, Markov property |
| `02-bellman-equation.md` | Ch 2: State/action values, Bellman equation (elementwise + matrix-vector), policy evaluation |
| `03-bellman-optimality-equation.md` | Ch 3: Optimal values, BOE, contraction mapping theorem, greedy policy |
| `04-value-iteration-policy-iteration.md` | Ch 4: Value iteration, policy iteration, truncated PI, GPI |
| `05-monte-carlo-methods.md` | Ch 5: MC Basic/Exploring Starts/ε-Greedy, model-free transition, exploration |
| `06-stochastic-approximation.md` | Ch 6: Robbins-Monro, Dvoretzky's theorem, SGD, step size conditions |
| `07-temporal-difference-methods.md` | Ch 7: TD learning, Sarsa, Expected Sarsa, n-step Sarsa, Q-learning |
| `08-value-function-methods.md` | Ch 8: Linear/neural FA, DQN, experience replay, target networks, LSTD |
| `09-policy-gradient-methods.md` | Ch 9: Policy gradient theorem, REINFORCE, softmax policy, metrics |
| `10-actor-critic-methods.md` | Ch 10: QAC, A2C, off-policy AC, DPG, DDPG, importance sampling |
| `11-appendix.md` | Appendices A–D: Probability, measure theory, convergence theorems, gradient descent |
| `12-cross-reference-index.md` | Master concept index, algorithm comparison table, theorem/lemma index, key equations |

### How to Query the Knowledge Base

1. **Concept/algorithm/theorem lookup** → Read `12-cross-reference-index.md` first. It has tables mapping every concept, algorithm, and theorem to its chapter(s).
2. **Deep dive on a topic** → Read the specific chapter file identified from the cross-reference.
3. **Keyword search** → Use `Grep` across `knowledge_base/*.md` for any term.
4. **Chapter dependencies** → Each chapter file has YAML frontmatter with `depends_on` and `required_by` fields.
5. **Algorithm comparison** → Section 2 of `12-cross-reference-index.md` has a full comparison table (type, model-required, convergence, on/off-policy).
6. **Key equations** → Section 4 of `12-cross-reference-index.md` lists the ~20 most important equations with LaTeX.
7. **Math prerequisites** → `11-appendix.md` covers probability, measure theory, martingales, convexity, and gradient descent foundations.

### Design Decision: No MCP Server

We evaluated building a FastMCP server for structured KB queries but decided against it because:
- The KB is small (~7,900 lines / ~45K tokens) — fits comfortably in direct `Read`/`Grep` access
- An MCP server would add process startup latency, `uv`/`fastmcp` dependencies, and a failure mode (crash, path issues)
- The returned text enters the context window either way — no token savings
- `Read`/`Grep` already provide the exact same search capability with zero overhead

## Important Notes

- Algorithm implementations are intentionally omitted (they are student homework). Only the environment is provided.
- Third-party algorithm implementations exist in Python, R, C++, and MATLAB — links are in the main `Readme.md`.
- Customization is done by editing `arguments.py` defaults, not via CLI arguments in practice.
