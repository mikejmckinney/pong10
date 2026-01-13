# Pong10 🎮

A retro-style multiplayer Pong game optimized for mobile browsers with real-time networking, power-ups, and synthwave aesthetics.

## 🌟 Features

- **Real-time Multiplayer** – Battle friends online with authoritative server-side physics
- **Mobile-First Design** – Touch-friendly controls with responsive layout
- **Retro Synthwave Style** – Neon colors, bloom effects, and CRT shaders
- **Power-Up System** – Dynamic gameplay with various power-ups
- **Persistent Leaderboard** – Firebase Firestore integration for high scores
- **Optimized Networking** – Client prediction and interpolation for smooth gameplay

## 🚀 Tech Stack

- **Frontend:** TypeScript, Phaser 3, Vite/Webpack
- **Backend:** Node.js, Socket.io
- **Database:** Firebase Firestore (v9 modular SDK)
- **Testing:** Jest with jsdom and node-canvas

## 📁 Project Structure

```
├── client/          # Browser-based game client (Phaser 3)
├── server/          # Node.js authoritative game server
├── shared/          # Shared TypeScript types and constants
├── assets/          # Sprites, fonts, audio, and shaders
├── tests/           # Unit, integration, and E2E tests
├── .context/        # Agent memory and development roadmap
└── .github/         # CI/CD and AI tool configurations
```

## 🎯 Quick Start

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn

### Installation

```bash
npm install
```

### Running the Client

```bash
cd client
npm run dev
```

The game will be available at `http://localhost:5173`

### Running the Server

```bash
cd server
npm start
```

### Testing Multiplayer

Open `http://localhost:5173` in two browser windows to test local multiplayer functionality.

## 🧪 Testing

```bash
# Run all tests
npm test

# Integration tests
npm run test:integration

# End-to-end tests
npm run test:e2e
```

## 🎨 Development

This project follows a phased development approach:

1. **Phase 1** – Core Loop (Single-player foundation)
2. **Phase 2** – Networking Plumbing (Real-time communication)
3. **Phase 3** – Authoritative Physics (Server-side game logic)
4. **Phase 4** – Polish & Deployment (Audio, visual effects, production ready)

See [roadmap.md](.context/roadmap.md) for detailed phase descriptions.

## 📚 Documentation

- **[AI_REPO_GUIDE.md](AI_REPO_GUIDE.md)** – Comprehensive repository guide for AI agents
- **[AGENTS.md](AGENTS.md)** – Agent guidelines and maintenance instructions
- **[Development Roadmap](.context/roadmap.md)** – Phased development plan
- **Domain Rules:**
  - [UI Domain](.context/rules/domain_ui.md) – Visual design and input handling
  - [Audio Domain](.context/rules/domain_audio.md) – Sound implementation
  - [Network Domain](.context/rules/domain_net.md) – Networking architecture
  - [Leaderboard Domain](.context/rules/domain_leaderboard.md) – Persistence layer
  - [QA Domain](.context/rules/domain_qa.md) – Testing strategies

## 🎮 Controls

### Desktop
- **Arrow Keys** – Move paddle up/down

### Mobile
- **Left/Right Touch Zones** – Tap left or right side of screen to control respective paddle

## 🔧 Configuration

### Firebase Setup

1. Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable Firestore Database
3. Add your Firebase configuration to environment variables (never commit credentials)

### Environment Variables

```bash
# Server
FIREBASE_API_KEY=your_api_key
FIREBASE_PROJECT_ID=your_project_id
# ... additional Firebase config
```

## 🚀 Deployment

The game can be deployed to any Platform-as-a-Service provider (e.g., Render, Railway, Heroku):

1. Build the client: `cd client && npm run build`
2. The Node.js server can serve static client assets and handle WebSocket connections
3. Ensure environment variables are configured in your PaaS dashboard

## 🤝 Contributing

This project uses AI-assisted development workflows. Before contributing:

1. Read [AGENTS.md](AGENTS.md) for agent guidelines
2. Review [AI_REPO_GUIDE.md](AI_REPO_GUIDE.md) for repository conventions
3. Check the current phase in [active_task.md](.context/state/active_task.md)

## 📋 Conventions

- **Responsive Layout:** Use `Phaser.Scale.FIT` and relative positioning
- **Networking:** Server is authoritative; clients send input commands
- **Testing:** All features require unit tests; networking changes need integration tests
- **Code Style:** Follow TypeScript best practices and existing patterns

## 🐛 Troubleshooting

### Game doesn't scale correctly
Ensure Phaser config includes:
```typescript
scale: {
  mode: Phaser.Scale.FIT,
  autoCenter: Phaser.Scale.CENTER_BOTH
}
```

### Mobile audio is silent
Audio requires user interaction to unlock. Call a silent sound on first tap.

### CI tests fail in headless mode
Use `type: Phaser.HEADLESS` and provide canvas via node-canvas.

## 📄 License

This project is available for educational and personal use.

## 🎯 Current Status

**Phase 1: Not Started**

The project is currently in the planning phase. Check [active_task.md](.context/state/active_task.md) for current development status.

---

Built with ❤️ using Phaser 3 and Socket.io
