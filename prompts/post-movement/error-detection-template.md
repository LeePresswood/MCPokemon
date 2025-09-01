## Error Detection and Backtracking Template

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
The LLM plans a sequence of moves to exit the building and return to the previous known correct position (outside the Player's House).

**Reassessment**:  
After backtracking, the LLM updates its internal state and confidence in the new plan. It proceeds with a revised sequence of moves to reach Oak's Lab.