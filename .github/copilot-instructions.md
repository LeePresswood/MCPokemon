# MCPokemon - MCP Server for Autonomous Pokémon Red Gameplay

This is an experimental Model Context Protocol (MCP) server project designed to let an LLM play Pokémon Red autonomously. The project is currently in the concept and design phase with detailed documentation but no implementation yet.

**Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.**

## Current Project State

**IMPORTANT**: This project is currently in the design/concept phase. There is NO source code, build system, dependencies, or runnable application yet. The repository contains only:
- README.md (comprehensive project design and technical specifications)
- LICENSE (Unlicense - public domain)
- .gitignore (Node.js style, suggesting future Node.js/TypeScript implementation)
- package-lock.json (empty, no dependencies - suggests npm was initialized but no packages installed)

## Working Effectively

### Understanding the Project
- **Start by reading README.md thoroughly** - it contains the complete project vision, technical architecture, and implementation strategy
- The README.md covers:
  - Project goals and rationale for choosing Pokémon Red
  - 5-phase milestone roadmap (from starter Pokémon to defeating Mewtwo)
  - Technical considerations (context size, memory, knowledge injection, image recognition)
  - 5-stage narration framework for debugging LLM reasoning
  - Post-movement analysis and error correction strategies

### Key Concepts to Understand
- **State-based gameplay**: Pokémon Red is modeled as finite state transitions suitable for LLM decision-making
- **5-stage narration**: Perception → Knowledge Injection → Task Framing → Decision Path → Hallucination Risks
- **Short decision windows**: Plan 10 moves at 0.25s intervals to minimize hallucination risk
- **Knowledge grounding**: Use Bulbapedia and canonical sources to correct LLM assumptions
- **Error detection and backtracking**: Compare game states before/after moves to detect and correct mistakes

## Development Guidelines (For Future Implementation)

### When Code is Added
- Based on .gitignore, this will likely be a Node.js/TypeScript project
- Expect dependencies like emulator libraries, image processing tools, and MCP framework
- Follow the technical architecture outlined in README.md sections on "Context Size & Planning" and "Memory & Persistence"

### Architecture Expectations
- **MCP Server**: Will implement Model Context Protocol for LLM integration
- **Game Emulator Integration**: Interface with Pokémon Red emulator for input/output
- **Image Recognition**: Process game screenshots to extract state information  
- **State Tracking**: Persistent memory for inventory, team, badges, location
- **Knowledge Base**: Integration with Pokémon knowledge sources (potentially Bulbapedia)

### Validation Requirements
- **When implementation begins**: Always test the complete gameplay loop described in README.md
- **State transition validation**: Ensure moves produce expected game state changes
- **Error handling**: Implement the post-movement analysis framework from README.md
- **Knowledge accuracy**: Validate against canonical Pokémon Red sources

## Common Tasks

### Current State Tasks
```bash
# View project documentation
cat README.md

# Check repository structure  
ls -la
find . -type f -name "*.md" -o -name "*.json" -o -name "*.js" -o -name "*.py" -o -name "*.ts"

# Understand project scope
grep -n "Phase [1-5]" README.md
grep -n "Technical Considerations" README.md
```

### Future Development Tasks (When Code Exists)
```bash
# Expected build process (not yet implemented)
npm install          # Install dependencies - currently fails: no package.json
npm run build        # Build the MCP server
npm run test         # Run test suite
npm run dev          # Development mode

# Available development tools:
node --version       # Node.js v20.19.4 available
python --version     # Python 3.12.3 available
```

### Repository Contents Reference
```bash
# Current repository structure:
.
├── .git/
├── .github/
│   └── copilot-instructions.md
├── .gitignore       # Node.js style gitignore
├── LICENSE          # Unlicense (public domain)
├── package-lock.json # Empty lockfile (88 bytes, no dependencies)
└── README.md        # Complete project specification (11,575 bytes)
```

## Important Notes

### Current Limitations
- **No runnable code**: Project is in design phase only
- **No build/test system**: Nothing to compile or execute yet  
- **No dependencies**: Empty package-lock.json, no package.json, no actual dependencies
- **No CI/CD**: No GitHub Actions or automated testing

### Validated Commands (Working)
```bash
# These commands have been tested and work correctly:
cat README.md                    # ✓ Shows complete project documentation
ls -la                          # ✓ Lists repository contents
find . -name "*.md"             # ✓ Finds documentation files
grep -n "Phase [1-5]" README.md # ✓ Shows project milestones
grep -n "Technical Considerations" README.md # ✓ Shows architecture section
node --version                  # ✓ Shows Node.js v20.19.4
python --version                # ✓ Shows Python 3.12.3
```

### Commands That Appropriately Fail
```bash
# These commands fail as expected (no implementation yet):
npm install                     # ✗ Error: no package.json file
npm run build                   # ✗ No package.json with scripts
npm run test                    # ✗ No test infrastructure
```

### Future Development Priorities
1. **MCP Server Framework**: Implement basic MCP protocol handling
2. **Emulator Interface**: Connect to Pokémon Red emulator (likely VBA or similar)
3. **State Recognition**: Image processing to extract game state from screenshots
4. **Decision Engine**: Implement the 5-stage narration framework
5. **Knowledge Integration**: Connect to canonical Pokémon knowledge sources

### Debugging and Validation Strategy
- Follow the 5-stage narration pattern outlined in README.md for all LLM decision loops
- Implement post-movement analysis to detect reasoning errors
- Use structured state tracking to maintain game progress across sessions
- Ground decisions in canonical knowledge sources to reduce hallucinations

## References

- **Primary Documentation**: README.md contains the complete technical specification
- **Inspiration**: Twitch Plays Pokémon as proof-of-concept for crowd-sourced discrete state progression
- **Knowledge Source**: Bulbapedia for canonical Pokémon Red information
- **Target Game**: Pokémon Red (Game Boy, 1996) - chosen for state-based nature and lack of time pressure

**Remember**: This project is currently conceptual. When contributing code, ensure it aligns with the comprehensive design framework already established in README.md.