# 🍎 Fruit Collector

A fast-paced arcade browser game built in Python with Pygame. Control a basket at the
bottom of the screen, catch falling fruit to build your score, and dodge bombs that will
cost you points. Race to 50 before your score hits zero.

Playable in the browser — no installation required.

**[▶ Play Live](https://danjocorona.github.io/fruit-collector)**

---

## Gameplay

Fruit and bombs rain down from the top of the screen at random positions and speeds.
Move your basket left and right to catch the good stuff and avoid the bad.

| Object | Effect |
|--------|--------|
| 🍎 Fruit | +1 point |
| 💣 Bomb  | −10 points |

- **Win** by reaching a score of 50
- **Lose** if your score drops to 0 or below

---

## Features

- **7 fruit types** — Apple, Banana, Cherries, Green Grapes, Blue Grapes, Orange, and Pear, chosen randomly each spawn
- **Timed spawning** — fruit spawns every 1.5 seconds, bombs every 2.2 seconds via `pygame.USEREVENT` timers
- **Variable fall speed** — fruit falls at 3–6 px/frame, bombs at 4–7 px/frame, keeping gameplay unpredictable
- **Off-screen culling** — objects that pass the bottom of the screen are removed to keep memory clean
- **Win and lose conditions** — separate game states with distinct screens and full restart support
- **Start screen** — press Space to begin, R to restart after a win or loss
- **Web build via Pygbag** — runs directly in the browser with no installation

---

## Project Structure

```
fruit-collector/
├── main.py               # Game loop, event handling, collision logic, win/lose states
├── game/
│   ├── player.py         # Player basket — horizontal movement, screen boundary clamping
│   ├── fruit.py          # Fruit — random type selection, random spawn position and speed
│   ├── bomb.py           # Bomb — random spawn position, faster fall speed range
│   └── spawner.py        # Spawner — manages fruit and bomb lists, updates, draws, culls
├── assets/
│   └── images/           # Sprites: basket, bomb, and all 7 fruit images
├── index.html            # Pygbag web build entry point
└── README.md
```

---

## How It Works

### Game Loop (`main.py`)
The main loop runs at 60fps. Two `pygame.USEREVENT` timers fire independently — one every
1,500ms to spawn a fruit, one every 2,200ms to spawn a bomb. Collision is handled each frame
using `pygame.Rect.colliderect()` between the player's basket and every active fruit or bomb.
Timers are disabled immediately when the game ends to stop spawning.

### Player (`player.py`)
The basket sits fixed at the bottom of the screen and only moves horizontally. Left and
right arrow keys update `rect.x` each frame, clamped to the screen edges so the basket
can never leave the play area.

### Fruit (`fruit.py`)
Each fruit picks a random image from the 7 available sprites, scales it to 32×32px, and
spawns at a random x position above the top of the screen. It falls at a random speed
between 3 and 6 pixels per frame. Once its top edge passes the bottom of the screen it
is flagged as off-screen and removed by the Spawner.

### Bomb (`bomb.py`)
Bombs follow the same spawn pattern as fruit but fall faster (4–7 px/frame) and use a
black-keyed PNG with `set_colorkey()` to make the background transparent at runtime.

### Spawner (`spawner.py`)
The Spawner owns the fruit and bomb lists. Each frame it calls `update()` on every object,
then filters out anything that has gone off-screen using a list comprehension. It exposes
`get_fruits()` and `get_bombs()` so `main.py` can iterate for collision checks without
directly accessing internal state.

---

## Tech Stack

| | |
|---|---|
| Language | Python 3 |
| Game Library | Pygame |
| Web Build | Pygbag (Python → WebAssembly) |
| Architecture | Object-oriented (Player, Fruit, Bomb, Spawner classes) |
| Collision | pygame.Rect.colliderect() |

---

### Controls

| Key | Action |
|-----|--------|
| `Space` | Start game |
| `←` `→` | Move basket |
| `R` | Restart after win or game over |

---

## Web Build

The browser version was compiled using [Pygbag](https://pygame-web.github.io/), which
packages Python and Pygame into a WebAssembly bundle that runs natively in modern browsers
with no plugins or installation needed.

---

## License

This project is open source and available under the [MIT License](LICENSE).
