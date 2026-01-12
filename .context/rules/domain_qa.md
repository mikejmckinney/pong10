# Domain Rules: Quality Assurance & Testing

Reliable, self‑healing code is essential when an AI agent is in charge of development. These rules prescribe a layered testing strategy and continuous integration (CI) setup to ensure the game functions correctly across phases.

## Testing Layers

Follow the testing pyramid: many unit tests at the base, a moderate number of integration tests in the middle, and a few end‑to‑end tests at the top【684764494693227†L110-L204】. Each type of test serves a distinct purpose:

### Unit Tests

- Test individual functions, classes, or modules in isolation. For example, verify that the scoring logic increments the correct player’s score or that the paddle’s velocity caps are respected【684764494693227†L148-L156】.
- Use Jest as the test runner. Configure it with `ts-jest` or `babel-jest` for TypeScript support. Use `expect` assertions to check return values.
- Mock external dependencies (e.g. Socket.io clients or Firebase calls) using Jest’s mocking utilities so unit tests run fast and deterministically.

### Integration Tests

- Test interactions between modules, such as the server’s networking layer communicating with the game logic or the client retrieving data from the leaderboard API【684764494693227†L158-L169】.
- Use Jest with Node’s `http` or Socket.io test clients to spin up a server and connect a simulated client. Assert that messages are transmitted correctly and state updates occur as expected.
- When testing database interactions, use Firebase’s emulators or a mock Firestore instance to avoid hitting production.

### End‑to‑End (E2E) Tests

- Simulate real user flows: launching the game, joining a match, playing a round, submitting a score, and viewing the leaderboard【684764494693227†L177-L194】.
- Use Playwright or Cypress for browser automation. These frameworks can control multiple browser contexts to simulate two players. Keep E2E tests few and focused on high‑value scenarios to balance coverage with execution time.

## Headless Phaser Testing

- Phaser normally requires a DOM and a Canvas element. In CI, run Phaser in headless mode by using the `Phaser.HEADLESS` renderer and providing a canvas via the `canvas` package or `node‑canvas`. Use jsdom to mock the DOM environment【795071169652698†L469-L487】.
- Write tests that instantiate scenes, update them for a number of frames, and assert state changes. For example, spawn a ball with a known velocity, call `scene.update()` 60 times (1 second), and assert that its position has advanced accordingly【795071169652698†L481-L487】.

## Self‑Healing CI Pipeline

- Configure a GitHub Actions workflow (`.github/workflows/ci-tests.yml`) to run on every push. The pipeline should install dependencies, lint the code, run the full test suite, and build the client/server【795071169652698†L491-L503】.
- If tests fail, capture the error logs. According to the agent workflow, the AI must read the logs, fix the underlying issue, and only then mark the task as complete in `.context/state/active_task.md`【795071169652698†L491-L503】. This closed feedback loop helps maintain code quality.

## Test‑Driven Development (TDD)

- For critical systems such as the physics engine and networking protocol, consider writing tests before implementing the code. This helps clarify requirements and ensures the agent builds only what is necessary.
- Even if not strictly practising TDD, always accompany new features with appropriate tests at the relevant layer.

## Coverage & Reporting

- Configure Jest to generate code coverage reports. Aim for >80 % coverage across the core game logic. Low coverage hotspots are candidates for additional tests.
- Integrate coverage reporting into the CI pipeline so regressions are visible.

By adhering to these QA rules, the AI agent can detect regressions early, gain confidence in its code, and self‑correct when tests fail. This layered strategy aligns with industry best practices and the guidance from the project organization document【684764494693227†L110-L204】.
