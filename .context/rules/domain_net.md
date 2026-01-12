# Domain Rules: Networking & Game State

These rules govern the server architecture and client‑server interactions.

## Architecture

- **Pattern:** **Authoritative server** – the server is the single source of truth for game state; clients render and predict but never authoritatively change state.
- **Protocol:** WebSockets using Socket.io v4. Use JSON for messages. Do not rely on HTTP polling.

## Server Logic

- Implement the server in Node.js using TypeScript.
- Maintain an in‑memory game state object for each match. Define `Player` and `GameState` TypeScript classes or interfaces in `/shared` so client and server agree on the shape.
- Run a fixed‑tick simulation loop at 60 FPS using `setInterval()` or a game loop library. Each tick should:
  1. Apply velocity to paddles and the ball based on current inputs.
  2. Detect and handle collisions.
  3. Update scores and power‑up timers.
  4. Broadcast the minimal state delta to clients via Socket.io `emit`.
- Clients must send **intent messages** (`{input: 'UP' | 'DOWN' | 'STOP'}`) rather than positions. Do not send full coordinates.

## Client Logic

- On input, immediately move the local paddle for responsiveness (client‑side prediction) and send the intent to the server.
- When receiving state updates, reconcile the predicted position with the authoritative value. If the difference exceeds 5 pixels, snap to the server position; otherwise continue.
- For remote entities (opponent paddle and ball), buffer updates and interpolate between the last two states using `Phaser.Math.Linear` to smooth motion.

## Rooms & Matchmaking

- Use Socket.io rooms to isolate matches. A player joins or creates a room; if the room has one player, it waits; if two, the game starts. Additional players are rejected or queued.
- On disconnection, end the match, declare the remaining player the winner, and persist the score.

## Security & Fairness

- Do not trust client messages. Validate inputs and enforce movement speed caps.
- Prevent cheating by never accepting client‑side position updates.
