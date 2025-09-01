# Prompts Directory

This directory contains static text assets for the MCPokemon project's LLM game loop prompts.

## Purpose

The prompts directory serves as a central repository for:

- **Narration Templates**: Structured prompt templates for the 5-stage narration process
- **Game State Templates**: Templates for analyzing and tracking game state
- **Decision Making Templates**: Templates for planning and executing moves
- **Error Handling Templates**: Templates for post-movement analysis and error correction

## Organization

Prompts are organized into logical subdirectories based on the 5-stage narration process and game loop phases:

### Core Narration Stages
- **perception/**: Templates for describing what is visible (bounding boxes, objects, sprites)
- **knowledge-injection/**: Templates for overlaying canonical knowledge
- **task-framing/**: Templates for defining current subgoals
- **decision-path/**: Templates for step-by-step planned moves with timing
- **hallucination-risks/**: Templates for explicit uncertainty notes

### Game Loop Phases
- **game-state/**: Templates for determining current game state before moves
- **post-movement/**: Templates for analyzing results after executing moves

## Usage

These prompt templates support the MCPokemon MCP server's LLM reasoning process, providing structured formats for:

1. **Debugging and Analysis**: Consistent narration style for observing reasoning
2. **Knowledge Grounding**: Templates for injecting canonical Pokémon knowledge
3. **Error Detection**: Structured approaches for identifying and correcting mistakes
4. **State Tracking**: Templates for maintaining game state continuity

The templates follow the debugging-focused narration style described in the main README, serving as tools for observing when and where reasoning diverges from ground truth.