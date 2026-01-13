# Development Roadmap

This roadmap breaks the project into logical phases. Each phase builds upon the previous one and introduces new domains. Before starting a phase, the agent should read the relevant domain rules and update `active_task.md` accordingly.

## Phase 1 – Core Loop (Single-Player Foundation)

**Goal:** Build a local, single-player Pong game without networking.

**Context Files:** `domain_ui.md` (visuals, input), `domain_audio.md` (optional early sound).

**Tasks:**
1. **Set up the development environment.** Initialize a Vite or Webpack project with TypeScript support. Install Phaser 3 via npm or CDN.
2. **Create the main scene.** Implement `BootScene` to preload assets (fonts, shaders, audio sprite) and `GameScene` to contain the game loop.
3. **Implement core entities.** Use Phaser’s Arcade Physics to create `Paddle` and `Ball` classes. Apply the synthwave colours and bloom effects defined in `domain_ui.md`.
4. **Add input handling.** For mobile, divide the screen into invisible touch zones; for desktop, map arrow keys.
5. **Implement scoring logic.** Display scores using the `Press Start 2P` font. Reset the ball after a point is scored.
6. **Ensure responsiveness.** Use `Phaser.Scale.FIT` and relative positioning to scale to different aspect ratios.
7. **Validate.** Run the game locally and ensure the ball bounces off paddles and walls correctly, paddles move smoothly, and the game scales on mobile screens.

## Phase 2 – Networking Plumbing

**Goal:** Establish real-time communication between two clients and a server.

**Context Files:** `domain_net.md` (networking), `domain_ui.md` (input remains relevant).

**Tasks:**
1. **Create the server.** Implement a Node.js (TypeScript) server with Socket.io. Start with a basic HTTP and WebSocket server.
2. **Define game state schema.** Add TypeScript interfaces (`Player`, `GameState`) in the `/shared` folder so both client and server agree on the structure.
3. **Implement room logic.** Use Socket.io rooms to match two players. Emit events when players join or leave.
4. **Echo test.** Have the client send a message to the server and log it back to ensure the connection works.
5. **Sync initial positions.** Send the initial paddle and ball positions from the server to the clients on join.
6. **Validate.** Open two browser windows locally; both should connect to the same room and see each other’s presence.

## Phase 3 – Authoritative Physics Integration

**Goal:** Migrate game logic to the server and implement prediction/interpolation.

**Context Files:** `domain_net.md` (authoritative server & client prediction), `domain_ui.md` (rendering).

**Tasks:**
1. **Server simulation loop.** Run a fixed-rate loop on the server (e.g. 60 Hz) that updates the game state: apply velocities, handle collisions, update scores, and broadcast state deltas.
2. **Input messages.** Clients send input intents (`UP`, `DOWN`, `STOP`) instead of absolute positions.
3. **Client prediction.** On receiving local input, immediately move the paddle; when authoritative updates arrive, reconcile differences as described in `domain_net.md`.
4. **Interpolation.** Buffer remote updates and interpolate positions to smooth out motion.
5. **Power-ups (optional).** Introduce power-up logic on the server: spawning, collision handling, and timed effects.
6. **Validate under latency.** Use Chrome DevTools to simulate network lag and ensure the game remains playable.

## Phase 4 – Polish, Audio & Deployment

**Goal:** Achieve production readiness, add audio and visual juice, integrate persistence, and deploy.

**Context Files:** `domain_audio.md`, `domain_leaderboard.md`, `domain_qa.md`.

**Tasks:**
1. **Audio integration.** Implement the audio sprite and mobile audio unlock pattern.
2. **Visual effects.** Add screen shake on scoring, particle explosions for power-ups, and CRT shaders.
3. **Leaderboard integration.** Hook up Firestore: record scores server-side at the end of a match, retrieve the top 10, and display them in a dedicated scene or modal.
4. **Testing & CI.** Expand test coverage, particularly integration and E2E tests; configure headless Phaser testing and ensure CI passes.
5. **Deployment.** Dockerize the server and configure the deployment pipeline to a PaaS (e.g. Railway). Use GitHub Pages or similar to host the client and ensure WebSocket endpoints are reachable.
6. **Documentation.** Update `AI_REPO_GUIDE.md` and architecture diagrams to reflect the final system. Ensure `.context/state/active_task.md` marks the project as complete.

By following this phased roadmap, the AI agent can build the project incrementally, validate each stage, and produce a polished final game. Each phase corresponds to a discrete milestone, enabling clear progress tracking and targeted rule loading.
