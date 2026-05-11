# ARCHITECTURE.md

Written by team-lead before spawning teammates. This is the shared blueprint —
teammates read it to understand what they are building and how their module fits.
Update it when the structure changes; do not let it drift from the actual code.

## Module Structure

- src/types/__init__.py: Core type definitions (Ball, Paddle, Position, Velocity, Score, GameState)
- src/config/__init__.py: Game constants (screen dimensions, paddle size, ball speed, winning score)
- src/service/__init__.py: GameEngine class - ball movement, collision detection, scoring logic
- src/ui/__init__.py: CLI render loop, input handling for paddle movement, game display
- src/runtime/__init__.py: Main entry point, wiring components together

## Interfaces

### types/__init__.py
- `Position(x: float, y: float)` - ball/paddle coordinates
- `Velocity(dx: float, dy: float)` - ball movement vector
- `Paddle(y: float, height: float, width: float = 1.0)` - vertical paddle representation
- `Score(left: int = 0, right: int = 0)` - player scores
- `GameState(ball: Ball, left_paddle: Paddle, right_paddle: Paddle, score: Score, status: str)`
- `Ball(position: Position, velocity: Velocity, radius: float = 0.5)`

### config/__init__.py
- `SCREEN_WIDTH: float = 80`
- `SCREEN_HEIGHT: float = 24`
- `PADDLE_HEIGHT: float = 5`
- `PADDLE_WIDTH: float = 1`
- `BALL_RADIUS: float = 0.5`
- `BALL_SPEED: float = 1.0`
- `PADDLE_SPEED: float = 1.0`
- `WINNING_SCORE: int = 5`

### service/__init__.py
- `class GameEngine`
  - `move_left_paddle(direction: str) -> None` - up/down
  - `move_right_paddle(direction: str) -> None` - up/down
  - `update() -> None` - advance game state (call each frame)
  - `get_state() -> GameState` - current game state
  - `is_game_over() -> bool`
  - `reset_ball() -> None`

### ui/__init__.py
- `class GameRenderer`
  - `render(state: GameState) -> str` - produce ASCII art for terminal
  - `get_input() -> str | None` - non-blocking input for paddle control
- `class InputHandler`
  - `handle_input(key: str, state: GameState) -> GameState` - process keypresses

### runtime/__init__.py
- `main() -> None` - entry point, game loop

## Shared Data Structures

All data structures are in types/__init__.py:
- Game runs on a coordinate system (0,0) at top-left
- Y increases downward, X increases rightward
- Paddles are vertical, centered at their Y position
- Ball is circular with radius 0.5

## External Dependencies

- Standard library only (no external packages required)
- Uses curses for terminal handling (built into Python)
