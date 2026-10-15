# PokerBot

PokerBot is an experimental poker-playing agent that combines a language model with structured memory systems to play Texas Hold'em. The project uses Anthropic's Claude models to reason about game states, keeps short-term and long-term memory for each session, and includes a Streamlit-based UI for simulating hands against multiple opponent profiles.

## Overview

This repository explores the idea of a poker agent with working memory and adaptive play. Instead of relying only on raw hand logic, the bot uses:

- a short-term memory buffer for the current hand
- a medium-term memory log for the active game
- a long-term memory system for persistent opponent profiles and self-reflection
- a judge agent that reviews the previous hand and records strategic observations
- a simulation environment built on PokerKit

The end result is a research prototype that behaves more like an LLM-powered poker strategist than a traditional rules-only engine.

## Key Features

- Claude-based decision making for Texas Hold'em actions
- Game-state JSON passed to the model at each decision point
- Action tool calling with `fold`, `check`, `call`, and `raise`
- Memory digest injected into the prompt to improve context continuity
- Opponent archetypes:
  - pocket pair aggressor
  - punter / loose aggressive player
  - tight passive player
- Judge model that evaluates opponent tendencies and stores notes
- Streamlit dashboard for observing stack sizes, board state, reasoning, judge reports, and token usage

## Repository Structure

```text
PokerBot/
├── README.md
├── .gitignore
├── PokerKit Environment.ipynb
├── full_integration_final.ipynb
├── multi_game_runner_JSON.ipynb
└── src/
    ├── Equity_calc.py
    ├── app2.py
    ├── evals.py
    ├── longterm.py
    ├── main script.py
    ├── mediumterm.py
    └── shortterm.py
```

## Core Components

### `src/app2.py`

This is the main app and the most complete entry point. It runs a Streamlit interface that simulates a poker table, tracks player stacks, shows board state and community cards, and has the full system of:

- Claude acting as the Hero
- judge analysis after each hand
- medium-term memory trends
- opponent strategy models

Run it with:

```bash
streamlit run src/app2.py
```

### `src/main script.py`

A lightweight experiment showing Claude making poker decisions from JSON game state. This script demonstrates tool usage for a poker action and is useful for quick testing and prototyping.

### `src/shortterm.py`

Implements a short-term memory buffer scoped to a single hand. It logs the state of each street, stores reasoning, and returns the hand record once complete.

### `src/mediumterm.py`

Tracks memory across an entire game. It maintains:

- raw hand log
- trend observations
- aggregate stats for players
- session metadata

This is the table-level memory layer used during a game.

### `src/longterm.py`

Provides a persistent long-term memory system. It stores opponent profiles as text files with tags, summaries, and observations, and keeps a JSON index for efficient lookups. This is meant to accumulate strategic knowledge across sessions.

### `src/evals.py`

Reads a simulation results file and prints summary statistics, including:

- total games
- total hands played
- win rate
- action frequencies
- opponent aggression summaries
- a basic matplotlib chart of decision distribution

### `src/Equity_calc.py`

Custom equity calculation utilities or investigation work related to evaluating poker hand strength. This file appears to support deeper analysis but is not the main app entry point.

## Notebooks

The repository also includes several Jupyter notebooks:

- `PokerKit Environment.ipynb` — exploratory environment work
- `full_integration_final.ipynb` — likely the combined integration notebook
- `multi_game_runner_JSON.ipynb` — multi-game / JSON simulation experimentation

These notebooks are useful for understanding the project evolution and for experimentation before moving logic into scripts.

## Setup

### Prerequisites

- Python 3.10+
- An Anthropic API key for Claude access
- A working virtual environment is recommended




## How It Works

The project uses a layered memory stack:

1. The game state is converted into JSON for the language model.
2. The model decides an action (`fold`, `check`, `call`, `raise`) using a tool call.
3. The hand is logged in short-term memory.
4. After the hand ends, the medium-term memory stores raw actions, reasoning, and trend observations.
5. The judge agent evaluates the non-Hero players and records insights.
6. Long-term memory persists these insights so future sessions can use them.

This creates an agent that is not only evaluating the current hand, but also learning and adapting across games.

## Example Usage

### Run the main interactive app

```bash
streamlit run src/app2.py
```

### Quick direct call test

```bash
python "src/main script.py"
```

### Analyze simulation output

```bash
python src/evals.py
```

## Notes

- This is an experimental research project rather than a polished production poker bot.
- Performance depends heavily on the Anthropic model, the quality of the prompts, and the simulation environment.
- The repo includes multiple prototypes and notebooks, so the architecture may be more exploratory than fully standardized.
- The project is best used as a learning and experimentation platform for LLM-powered decision-making in poker.

## Future Improvements

Potential next steps include:

- tighter hand-equity calculations
- more realistic opponent modeling
- persistent session history across runs
- cleaner module boundaries and refactoring
- stronger validation and testing of action logic
- caching or batching of LLM calls

