# Active Task

**Phase:** 1 – Core Loop

**Status:** Not Started

**Task Description:**

You are beginning Phase 1 of the development roadmap. Your goal is to produce a working, local Pong game without networking. Follow these steps:

1. **Set up the project:** Initialize a TypeScript project using Vite or Webpack. Install Phaser 3 via npm or include it via a CDN in your HTML. Commit the initial scaffold (`package.json`, bundler config, `index.html`).
2. **Implement basic scenes:** Create a `BootScene` that loads the `Press Start 2P` font, shaders, and any stub assets; and a `GameScene` that will contain the core game.
3. **Create game objects:** Implement `Paddle` and `Ball` classes using Phaser’s Arcade Physics. Respect the visual rules from `domain_ui.md` (synthwave colours, bloom effects, CRT scanlines).
4. **Handle input:** Implement split-screen touch controls for mobile and arrow keys for desktop as described in `domain_ui.md`.
5. **Add scoring and UI:** Draw score text using relative positioning. Make sure the ball resets after a point.
6. **Validate:** Run the game locally in a browser. The ball should bounce off walls and paddles, paddles should respond to input, and the game should scale across aspect ratios.

**Next Step:** After completing the above tasks and committing the working single-player game, update this file with a summary of what you did, any remaining TODOs, and mark yourself ready to start Phase 2 (Networking Plumbing).
