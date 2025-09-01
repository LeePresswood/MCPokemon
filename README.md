# MCPokemon

> Play Pokémon via MCP!

This project is an experimental **Model Context Protocol (MCP) server** designed to let an LLM play **Pokémon Red** autonomously.

The goal is to simulate long-term planning, short-term control, and dynamic reasoning while balancing prompt size, memory, and contextual knowledge.

---

## Project Goals

-   Simulate the Pokémon Red game loop through **LLM-based reasoning + emulator input**.
-   Explore how **prompt context size** and **temporal granularity** affect planning (e.g., 25-second foresight with 100 steps vs. shorter loops).
-   Test **narration/debugging style** that reveals _why_ the AI chooses its moves.
-   Experiment with **knowledge grounding** using Bulbapedia or other canonical resources.
-   Provide a reproducible testbed for LLM **agent-style gameplay**.

---

## Why Pokémon Red

Pokémon Red is an ideal choice for this project because it can be effectively represented as a series of state transitions. In the overworld, each move corresponds to one of four possible directions, allowing the player to navigate toward a desired location. Similarly, in Pokémon battles, the player interacts with a series of menu options to work toward the desired end state of defeating a trainer. Wild Pokémon battles follow the same structure, with the additional option of catching the Pokémon. This structure makes the game a natural fit for modeling as a finite state machine, where discrete states and transitions can be explicitly defined and chosen.

Another advantage of Pokémon Red is the absence of time-based events, which are common in action games. This allows the game to progress at a pace dictated by the AI, without the need to account for real-time constraints. The turn-based nature of battles and the step-based overworld movement ensure that the AI can deliberate as long as necessary to make optimal decisions. This combination of structured gameplay and lack of time pressure makes Pokémon Red an excellent testbed for exploring LLM-driven reasoning and decision-making.

Another compelling example of Pokémon Red's suitability for state-based modeling is the success of **[Twitch Plays Pokémon](https://en.wikipedia.org/wiki/Twitch_Plays_Pok%C3%A9mon)**. This social experiment demonstrated that even with chaotic, crowd-sourced inputs, the game could still progress through a series of discrete states. The collective actions of thousands of players effectively navigated the overworld, completed battles, and achieved milestones, all within the constraints of the game's state machine. This reinforces the idea that Pokémon Red's mechanics are inherently robust and well-suited for structured, AI-driven decision-making.

---

## Phased Milestones

### Phase 1: Foundations

-   Collect starter Pokémon.
-   Catch a wild Pokémon.
-   Defeat first trainer.
-   Evolve first Pokémon.
-   Win first Gym Badge.

### Phase 2: Mobility + Utilities

-   Learn and use HM Cut.
-   Learn and use HM Flash.
-   Unlock Fly (optional but highly recommended).
-   Begin long-distance exploration.

### Phase 3: Team Rocket & Midgame

-   Defeat Team Rocket in Celadon.
-   Defeat Team Rocket in Saffron.
-   Progression toward Surf unlock.

### Phase 4: Legendary Access

-   Learn HM Surf → Catch/defeat Zapdos.
-   Learn HM Strength → Catch/defeat Articuno.
-   Retrieve Cinnabar Gym key.
-   Catch/defeat Moltres.

### Phase 5: Endgame

-   Defeat 8th Gym.
-   Reach Indigo Plateau and heal.
-   Defeat Elite Four + Rival → Become Champion.
-   Catch/defeat Mewtwo.

---

## Technical Considerations

### Context Size & Planning

-   **Short Decision Window** → LLM inputs up to 10 state transitions in a row to be played at 0.25s intervals.

    -   Responsive, low hallucination risk.
    -   Shorter contexts ensure the LLM focuses on immediate tasks, reducing the chance of logical errors.
    -   Longer action windows increase the risk that the LLM will fail to understand how the series of actions impacted the new state of the game.
    -   Planning just a few moves ahead allows the LLM to make progress without spamming too many requests for analysis.
    -   Useful for tasks requiring precise, step-by-step execution, such as navigating tight spaces, puzzle rooms (such as the teleport puzzle in the Saffron City gym), and moving through the game's menus.

### Memory & Persistence

-   **Game State Tracking** → Use structured tracking for inventory, team, badges, etc.

    -   Keep logs and save checkpoints for future game loops to rely upon, such as current Pokemon team, current number of badges, or current city location.
        -   Checkpoints allow the game to be resumed from specific states, reducing the need to restart from scratch.
        -   This allows the AI to make decisions based on the current game state without relying on assumptions.
    -   The LLM does not have persistent memory, so explicit tracking ensures continuity across sessions.
        -   Consider that fresh sessions prevent the accumulation of hallucinations or errors over time.
        -   Long prompts may retain more context but risk introducing inconsistencies if the LLM misremembers details.

### Knowledge Injection

-   **Canonical Knowledge Sources** → Use of Bulbapedia or similar resources to ground knowledge.

    -   The LLM has partial memory of Pokémon Red maps but often makes logical errors (e.g., misplacing Oak’s lab based on a misunderstanding of an image of Pallet Town).
        -   A well-placed call to [the Bulbapedia page on Pallet Town's geography](https://bulbapedia.bulbagarden.net/wiki/Pallet_Town#Geography) would give the LLM more direct context in recent memory.
    -   Pre-loading canonical knowledge from athoritative sources could ensure decisions are based on accurate information.
    -   Ideally unnecessary to call out to other sites or APIs, but it is an option.
        -   Parsing and summarizing large knowledge sources prevent overwhelming the LLM with irrelevant details.

### Image Recognition

-   **Image Recognition MCP** → Use an image recognition module to extract game elements from screenshots.
    -   Identify the player's character, surrounding buildings, trees, cave entrances, and other key map features.
    -   Provides structured data (e.g., bounding boxes, object types) that can be fed into the LLM for reasoning.
    -   Reduces reliance on the LLM's ability to interpret raw images, which can be error-prone.
    -   Ensures consistent and accurate perception of the game state, even in visually complex areas.

---

## Narration Style for Game Loops

To debug reasoning and identify hallucinations, each loop should follow a **5-stage narration**:

1. **Perception** → What is visible (bounding boxes, objects, sprites).
2. **Knowledge Injection** → Overlay canonical knowledge (e.g., “Oak’s Lab is SE, not NE”).
3. **Task Framing** → Current subgoal (e.g., “Reach Oak’s Lab to get starter”).
4. **Decision Path** → Step-by-step planned moves (with timing).
5. **Hallucination Risks** → Explicit notes on uncertain assumptions.

This narration style is **not flavor text**. It is a **debugging tool** for observing when/where reasoning diverges from ground truth.

### Determining Current Game State

Before executing a planned sequence of moves, the LLM performs a game state analysis to determine the current position in the overworld or in the game's menus. This involves identifying the player's current city location, number of badges, party composition in terms of Pokemon species, types, levels, and movesets, and upcoming short and long-term goals.

#### Example: Moving from Player's House to Professor Oak's Lab

Here’s an example of how the LLM narrates its reasoning and actions to achieve a short-term goal ("Reach Oak's Lab") while contributing to a long-term goal ("Begin the Pokémon journey"):

---

**Perception**:  
I can see the Player's House at the top of the map and Professor Oak's Lab at the southeast corner of Pallet Town. The map positions the houses at the top and the lab at the bottom-right.

**Knowledge Injection**:  
To reach Oak's Lab, I know that the correct path is to head south from the Player's House to the main path, then east toward the lab entrance.

**Task Framing**:  
My short-term goal is to move from the Player's House to Oak's Lab. This contributes to my long-term goal of starting the Pokémon journey by interacting with Professor Oak.

**Decision Path**:  
To achieve this, I’ll plan a sequence of 10 moves spaced at 0.25-second intervals:

-   Move south to exit the Player's House and reach the main path.
-   Move east along the path to reach the lab entrance.
-   Once at the entrance, press "A" to interact with the door and enter the lab.

**Hallucination Risks**:  
I assume that the lab is directly southeast of the Player's House and that no obstacles block the path. If this assumption is incorrect, I may need to adjust my plan after observing the updated game state.

---

This example demonstrates how the LLM combines perception, knowledge, and planning to achieve its objectives while narrating its reasoning for debugging and analysis.

### Post-Movement Analysis

After executing a planned sequence of moves, the LLM performs a post-movement analysis to determine if progress was made toward the short-term or long-term goals. This involves comparing the previous game state (State 1) with the new game state (State 2) and identifying any discrepancies or errors. If necessary, the LLM can decide to backtrack partially or fully to correct mistakes.

#### Example: Error Detection and Backtracking

Here’s an example of how the LLM handles a situation where it mistakenly enters the wrong building while attempting to reach Oak's Lab:

---

**Perception (State 1)**:  
The player is outside the Player's House, facing south. The goal is to move southeast to Oak's Lab.

**Action**:  
The LLM executes a sequence of moves south and east, intending to reach the lab entrance.

**Perception (State 2)**:  
The player is now inside a building. The surroundings do not match the expected interior of Oak's Lab.

**Error Detection**:  
The LLM recognizes that the current state does not align with the expected outcome. The building's interior and dialog indicate that the player entered the wrong house.

**Hypothesis**:  
The LLM hypothesizes that it miscalculated the path or overshot the intended destination.

**Backtracking**:

-   The LLM plans a sequence of moves to exit the building and return to the previous known correct position (outside the Player's House).
-   It reassesses the map and recalculates the path to Oak's Lab, ensuring the correct building is targeted this time.

**Reassessment**:  
After backtracking, the LLM updates its internal state and confidence in the new plan. It proceeds with a revised sequence of moves to reach Oak's Lab.

---

This structured approach ensures that errors are detected and corrected efficiently, minimizing wasted actions and improving the LLM's ability to achieve its goals. The process includes:

1. **Perception**: Capturing and comparing game states before and after movement.
2. **Error Detection**: Identifying inconsistencies between expected and actual outcomes.
3. **Backtracking**: Reversing incorrect actions to return to a known correct state.
4. **Reassessment**: Updating the plan and retrying with improved accuracy.

This post-movement analysis is critical for maintaining progress and avoiding compounding errors during gameplay.
