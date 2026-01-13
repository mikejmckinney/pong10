# Domain Rules: Audio & Sound Effects

## Audio Sprites

- Combine all sound effects (paddle hit, score, power-up, countdown, win/lose) into a single audio file and generate a JSON cue sheet with start/end offsets.
- Load the audio sprite in Phaser using the `this.load.audioSprite` method.
- When playing a sound, reference the cue name rather than loading individual `.mp3` files. This reduces HTTP requests and mobile latency.

## Unlocking Audio on Mobile

- Mobile browsers block audio playback until the user interacts with the page. To unlock audio, play a silent buffer from the audio sprite when the user first taps (e.g. when pressing “Start Game”).
- Only after unlocking should actual sounds be played. If audio is attempted before unlocking, catch and ignore the exception.

## Mixing & Levels

- Keep sound effects subtle so they complement the retro aesthetic. Use short, 8-bit inspired bleeps and whooshes.
- Provide a global mute toggle in the UI. Respect the OS volume.

## Music (optional)

- If you implement background music, loop a synthwave track. Fade the music in on the title screen and out when the game ends. Do not overpower sound effects.
