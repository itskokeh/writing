## Introduction

>*"When a $100K Web3 hackathon demanded a standout project, we bet on an
endless runner—only to discover that ‘simple’ games require complex glue. Our
solution? A browser-based game powered by Phaser, React, and Vite. This
write-up details how we chose that stack, the integration challenges we faced,
and what we learned from building an MVP in just 35 days."*

*(GIF: Glitchy early build vs. final MVP side-by-side)*

**Why This Stack?**

As hackathon constraints forced ruthless technical tradeoffs, we prioritized:

- Speed over familiarity: Vite’s HMR beat Webpack’s config hell.
- Separation of concerns: React for UI, Phaser for simulation.
- Web-native delivery: No installs, no Unity bloat—just a URL.<br>

Here’s how each piece fit (and where they fought back).

## Tech Stack Breakdown

### Why Phaser?

- **The *"I Played Games Built With This"* Factor:**

Beyond the obvious perks—built-in physics, plugin ecosystem, and lightweight
2D focus—I chose Phaser for a deeply personal reason: I’d played actual games
made with it (looking at you, Mario-esque platformers). That credibility sold
me.

But let’s be real: the docs are sparse. The official documentation felt like a
treasure hunt, and I spent more time on GitHub threads, obscure blogs, and
yes—AI chatbots—to piece together how things worked.<br>
*(Pro tip: Ask AI to “explain Phaser’s [X] like I’m a React dev.”)*

- **JavaScript or Bust:**

I’ll admit it: I’m a JavaScript dev at heart. The thought of learning both
game dev and a new language (looking at you, C#/GDScript) sounded like
hackathon suicide. So I evaluated JS-based options:

| Library | Verdict |
|---------|---------|
| `Three.js` | Overkill for 2D (it’s a 3D powerhouse). |
| `PixiJS` |  Great for rendering, but I’d need to wire physics/state myself. |
| `Phaser` |  Winner. It abstracts the boring stuff (like gravity) with APIs (view examples below)|

**Examples**<br>
**Set Gravity `Phaser` Edition:**

```javascript
player.body.setGravityY(1000);
```

**`Javascript` 'I Have to Do Everything' Edition:**

```javascript
// Gravity Logic
function applyGravity() {
  if (player.isGrounded) return; // Already on ground

  // Apply gravity (with terminal velocity cap)
  player.velocityY = Math.min(player.velocityY + GRAVITY, TERMINAL_VELOCITY);
  player.y += player.velocityY;

  // Ground collision
  if (player.y + player.height >= GROUND_Y) {
    player.y = GROUND_Y - player.height;
    player.isGrounded = true;
    player.velocityY = 0;
  }
}

// 3. Game Loop
function gameLoop() {
  applyGravity();
  // ... (render player, handle input, etc.)
  requestAnimationFrame(gameLoop);
}
gameLoop();
```

>“Phaser isn’t just a library—it’s a hackathon time machine. You trade some
control for all your sanity.”

- **Batteries Included:**

Phaser **defaults to canvas** and handles the game loop, asset loading, and
collision detection out of the box. For a hackathon, this was clutch—I could
focus on game design instead of reinventing wheels.

*(Optional: Add a comparison snippet of “vanilla JS” vs. Phaser for a feature
like sprite movement.)*

**Key Takeaways**<br>

1. **Phaser’s trade-off**: Less control, more speed. Perfect for hackathons;
   less ideal for hyper-customized engines.

2. **Docs are rough**, but the community (and AI) fills the gaps.

3. **Staying in JavaScript** kept me sane. No regrets.

### React + Phaser: An Unlikely Duo

#### The DOM Split: A Clever Hack

To avoid React’s virtual DOM diffing and Phaser’s canvas fighting for control,
we leveraged the humble `<div>`:

1. **Two Containers**:

- `#root`: React’s mount point (for onboarding, rules, and post-game stats).

- `#game-container`: Phaser’s canvas home.

**The Toggle Trick**:

```javascript
// Toggle visibility on game start (React code)
const gameContainer = document.getElementById('game-container');
    gameContainer.style.display = 'block';

    // Load the Phaser game
    const { default: initGame } = await import('../../phaser/main.js');
    initGame();

    const reactRoot = document.getElementById('root');
    if (reactRoot) {
      reactRoot.style.display = 'none';
    }

  // Toggle visibility on game end (Phaser Code)
  function quitGame () {
  const game = document.getElementById('game-container');
  const root = document.getElementById('root');

  game.style.display = 'none';
  root.style.display = 'block';
}
```

```md
React UI (visible)    ->  Phaser Canvas (visible)  
[Root: block]         ->  [Root: none]  
[Game: none]          ->  [Game: block]  
```

**Why this works**:

- **Performance**: Phaser owns the canvas 100% during gameplay—no React state updates or re-renders.<br>
- **Simplicity**: No need to bridge frameworks mid-game (though we’ll explore exceptions below).<br>
- **Phaser’s Built-in UI**: Good Enough
Since our game’s UI was lightweight (score counters, timers), we used Phaser’s native text and image APIs:

```javascript
// Example: In-game UI with Phaser  
this.scoreText = this.add.text(10, 10, "Score: 0", { font: "16px Arial" });
```

**Pros**:

- No unnecessary React ↔ Phaser communication overhead.<br>
- Pixel-perfect positioning within the canvas.

**When React Did Shine**

- **Pre-game**: Rules, inspiration, and settings (complex UI easier in JSX).
- **Post-game**: Sharing scores/analytics (React’s state management).

> **_The Secret_**: Crossing the Streams
*"For character selection, we’re experimenting with controlling Phaser from React—like passing a selected skin via localStorage or even real-time updates through a shared state manager (Redux/Zustand). Early tests suggest it’s possible... but at what cost to performance? Stay tuned for Part 2!"*

### Vite: The Unsung Hero

Vite’s zero-config magic worked perfectly for React—but combining it with
Phaser required some creative engineering. Unlike Webpack (where we’d drown
in loader configurations), Vite’s simplicity let us focus on the game, not
tooling. Here’s how we tamed the beast:

**The Config That Made It Possible**

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  optimizeDeps: {
    include: ['react', 'react-dom'] // Pre-bundle React for speed
  },
  build: {
    emptyOutDir: true, // Avoid stale files during rebuilds
    rollupOptions: {
      // Multi-page setup: React for UI, Phaser for game core
      input: {
        main: path.resolve(__dirname, './index.html'), // React entry
        game: path.resolve(__dirname, './src/phaser/main.js') // Phaser entry
      },
      output: {
        // Keep builds separated to avoid collisions
        entryFileNames: '[name]/[name]-[hash].js',
        assetFileNames: '[name]/assets/[name]-[hash].[ext]'
      }
    },
    sourcemap: false // Faster builds (sacrificed debugging for speed)
  }
});
```

**Key Wins**

- **No Phaser Plugin? No Problem**: Vite’s Rollup underpinnings let us
  manually configure multi-entry builds.

- **Instant Feedback**: HMR kept our React UI updates snappy, while Phaser’s
  canvas reloaded cleanly.

- **No Webpack Nightmares**: Compared to past projects, we spent minutes on
  config—not hours.

**Gotcha We Hit**

Phaser’s global Phaser object conflicted with Vite’s ESM approach. Our fix:

```javascript
// In index.html
<script>
  window.Phaser = await import('phaser'); // Shim for Phaser's UMD
</script>
```

**Lesson Learned**:

>"Vite’s ‘batteries-included’ approach shines—until you need to weld on new
batteries. A few tweaks unlocked the best of both worlds."

**Reflections on Version Control and Development Practices**:
Working on this project was a turning point in my understanding of version
control and development workflow. Until now, I didn’t fully grasp the value
of the “commit often” mantra—but during development, it finally clicked. I
encountered a situation where a new change broke previously working code,
and I hadn’t committed recently enough. That experience drove home how
frequent commits serve as checkpoints, making recovery and debugging much
easier.

I also learned the importance of branching, especially when introducing new
features. Early in the project, I attempted to integrate a sign-in feature
using React without switching to a new branch. The implementation turned out
to be more complex than expected, and I had to make several tweaks to
various parts of the codebase. Unfortunately, this led to a situation where
the game logic became tangled, and the project became unstable—despite
having commits. The lack of a separate feature branch made reverting or
isolating the changes much harder.

Looking back, I realize how much I figured out during this project:

I developed better discipline with Git—committing regularly, and
appreciating the safety it provides.

I learned the critical role of branching, especially for experimental or
complex features.

I gained a deeper appreciation for architectural planning, though I’ll reflect more on specific architectural lessons in the coming days.

If I were to start over, improved branching strategy would be at the top of my list. While I think I did well with commit frequency, there's still room for better structural decisions and planning ahead.
