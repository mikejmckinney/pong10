# Domain Rules: UI & Graphics

These rules govern all client‑side rendering and input.

## Aesthetic Guidelines

- **Style:** retro‑futuristic synthwave.
- **Colors:**
  - Background: `#090D40` (deep void blue).
  - Grid lines: `#2b2b2b` (dark grey perspective lines).
  - Player 1 paddle: `#FF005C` (neon pink) with a bloom PostFX (strength 2.0).
  - Player 2 paddle: `#00C4FF` (cyber cyan) with a bloom PostFX (strength 2.0).
  - Ball: `#FFFFFF` (white) with a particle trail coloured according to the last paddle that touched it.
- **Typography:** Use the Google Font **“Press Start 2P”** for all HUD elements. Load it via the WebFontLoader plugin.

## Mobile Responsiveness

- Use `Phaser.Scale.FIT` for the scale mode and `autoCenter: Phaser.Scale.CENTER_BOTH`.
- Never use hard‑coded pixel coordinates for positioning. Always use relative positioning based on `this.cameras.main.width` and `height`, e.g. `this.cameras.main.width * 0.5` to centre objects.
- Use flex spacing for UI overlays (pause menu, scoreboard) to maintain layout across aspect ratios.

## Input Scheme

- Detect whether the device is mobile via `this.sys.game.device.os.desktop`.
- **Mobile:** Divide the canvas into two horizontal invisible zones: touching the top half moves the paddle up; touching the bottom half moves it down. Use `this.input.addPointer(2)` to enable multi‑touch and prevent locking.
- **Desktop:** Map the arrow keys (`UP`, `DOWN`) for paddle movement and allow `P` to pause.

## Effects

- Apply a CRT scanline shader when WebGL is available, and fall back to plain Canvas on devices without WebGL.
- Use PostFX pipelines for bloom/glow on paddles and ball.
- Add juice such as screen shake on scoring and particle explosions for power‑ups in later phases.
