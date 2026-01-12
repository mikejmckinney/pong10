# Retro Pong Game – AI Repository Guide

## What This Repo Does
This project implements a retro‑style Pong game optimized for mobile browsers. It features real‑time multiplayer, touch‑friendly controls, a power‑up system, retro synthwave visuals and sound, and a persistent leaderboard. The client runs entirely in the browser using Phaser 3, while the server uses Node.js with Socket.io to synchronize game state. A Firebase Realtime Database stores leaderboards.

## Tech Stack

- **Language:** TypeScript (client & server)
- **Client Framework:** Phaser 3 with Vite or Webpack bundler
- **Server Framework:** Node.js with Socket.io for WebSocket communication
- **Persistence:** Firebase v9 Firestore (NoSQL document database) for leaderboards. Use the modular SDK (e.g. `getFirestore`, `collection`, `addDoc`, `getDocs`, `orderBy`, `limit`) to record and query high scores.
- **Build Tools:** Vite/Webpack for client bundling; npm scripts for server
- **Test Framework:** Jest with jsdom and node‑canvas for headless Phaser tests
- **Deployment:** Deploy via a Platform‑as‑a‑Service (e.g. Render, Railway). The Node server hosts API and WebSocket endpoints and can optionally serve static client assets.

## Folder Map

- `/client` — Browser‑based game client built with Phaser 3 (scenes, sprites, input, UI).
- `/server` — Node.js server handling authoritative game logic, networking via Socket.io, and leaderboard endpoints.
- `/shared` — Shared TypeScript types/constants used by both client and server (e.g. enum for input commands).
- `/assets` — Game assets such as sprites, fonts, audio sprites, and shaders.
- `/tests` — Unit, integration, and end‑to‑end tests. Mirrors the source layout.
- `/.context` — Agent memory. Contains immutable rules (`.context/rules`), the development roadmap (`.context/roadmap.md`) and the current task state (`.context/state/active_task.md`).
- `/.github` — CI configurations and prompts used by GitHub Copilot or other AI tools.

## Key Entry Points

- **Client:** `client/src/main.ts` bootstraps the Phaser game and loads the initial scene. Scenes are located in `client/src/scenes`.
- **Server:** `server/src/index.ts` creates the HTTP server and Socket.io instance, registers rooms, and starts the authoritative game loop.
- **Shared Types:** `shared/types.ts` defines interfaces for messages and game state.

## Quickstart

1. Install dependencies:
   ```bash
   npm install
   ```
2. Run the client in development mode with live reload:
   ```bash
   cd client
   npm run dev
   ```
3. Run the server:
   ```bash
   cd server
   npm start
   ```
4. Open `http://localhost:5173` in two browser windows to test local multiplayer.

## How to Test

- **Unit tests:** `npm test`. Tests are written using Jest; headless Phaser tests use jsdom and node‑canvas.
- **Integration tests:** Use `npm run test:integration` to spin up a server and client to verify networking.
- **End‑to‑End tests:** Use `npm run test:e2e` with Playwright or a similar framework to simulate full gameplay flows.

## Conventions

- **Responsive Layout:** Always use `Phaser.Scale.FIT` and relative positioning (`width * 0.5`) for UI elements. Avoid hard‑coded pixel coordinates.
- **Input Handling:** On mobile, split the screen into left/right invisible zones for paddle movement; on desktop, map arrow keys.
- **Color & Typography:** Use the synthwave palette and `Press Start 2P` font described in `.context/rules/domain_ui.md`.
- **Networking:** The server is authoritative. Clients send input commands (`UP`, `DOWN`, `STOP`); the server runs physics at 60 Hz, updates positions, and broadcasts state.
- **Leaderboards:** Use Firestore functions from the modular SDK to record scores after each match and retrieve the top 10 scores ordered by `score` descending. See `.context/rules/domain_leaderboard.md` for implementation guidance.
- **Audio:** Use an audio sprite (single sound file with cue map) and unlock audio on first user interaction.
- **Testing:** Each new feature must include unit tests. Networking and physics changes require integration tests. Keep CI green before merging.

## Where to Add Things

- **New scenes:** `client/src/scenes`.
- **New server rooms or handlers:** `server/src/rooms`.
- **Shared constants/enums:** `shared/types.ts`.
- **Power‑up logic:** Encapsulate in `client/src/powerups` and mirror server logic in `server/src/powerups`.
- **Leaderboard functions:** `server/src/leaderboard.ts` and call them from the server when matches end.

## Troubleshooting & Gotchas

- If the game doesn’t scale correctly, ensure `scale: { mode: Phaser.Scale.FIT, autoCenter: Phaser.Scale.CENTER_BOTH }`.
- Mobile audio may be silent until the first tap; call a silent sound in response to user input to unlock.
- When running tests in CI, use headless mode (`type: Phaser.HEADLESS`) and provide a canvas via node‑canvas.
- Ensure Firebase credentials are stored securely (e.g. via environment variables) and are **not** committed to the repo.

Use this guide as the canonical reference for the repository. Update it whenever commands, structure, or conventions change.
