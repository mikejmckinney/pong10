# Domain Rules: Leaderboards & Persistence

These rules govern how scores are stored and retrieved. A persistent leaderboard enhances replay value but must be implemented with efficiency and security in mind.

## Database Selection

- **Use Firestore (Firebase v9)**: Choose the Firestore NoSQL document database instead of a SQL database. Firestore stores documents in collections, which suits the simple `{ name: string, score: number }` structure of a leaderboard and integrates easily with web clients. The v9 modular SDK must be used for tree-shakeable imports (e.g. `import { getFirestore, collection, addDoc, getDocs, query, orderBy, limit } from 'firebase/firestore'`). The analysis in the technical specification highlights that Firestore is lightweight and avoids the overhead of real-time replication mechanisms required by SQL databases.
- **Avoid Supabase or heavy SQL**: A PostgreSQL solution with real-time triggers (e.g. Supabase) is unnecessary for a simple high-score table and adds complexity such as schema migrations and row-level security.

## Schema and Collection

- Create a Firestore collection called `scores` where each document contains a `name` (string), `score` (number), and a `timestamp` (server timestamp) for sorting ties.
- Use server timestamps (`serverTimestamp()`) instead of client clocks to avoid tampering. Import `serverTimestamp` from `firebase/firestore`.

## Writing Scores

- After each match ends on the server, call an asynchronous function `recordScore(name: string, score: number)` that uses `addDoc(collection(db, 'scores'), { name, score, createdAt: serverTimestamp() })`. In prototypes we accept client-submitted names and scores, but note that a production system should validate scores server-side.
- Never write scores from the client directly. Route all score submissions through the server so that the server can enforce fair play and attach the correct timestamp.
- Handle errors gracefully: wrap Firestore calls in `try…catch` and log failures without crashing the game.

## Reading the Leaderboard

- To display the leaderboard, fetch the top 10 scores ordered by `score` descending using a query: `query(collection(db, 'scores'), orderBy('score', 'desc'), limit(10))` and then await `getDocs`.
- Cache results client-side for the session to avoid repeated reads. Refresh the leaderboard after a new score is recorded.
- Display player names alongside their scores. Consider anonymizing or truncating names if they are too long (e.g. max 8 characters) to fit the UI.

## Security Considerations

- For the scope of this project, client-side submission is accepted to simplify the prototype. However, note in comments that a production deployment should include validation (e.g. check that a player’s score was legitimately achieved) and authentication (e.g. Firebase Auth).
- Configure Firestore security rules (not implemented in code) to allow read access for everyone but restrict write access to the server. This prevents anonymous clients from injecting fake scores.

## Integration with UI

- The client should provide a “Submit Score” screen after a match ends. The player enters their name via an input field; the client sends this name and the final score to the server via Socket.io.
- In the lobby or main menu, include a “Leaderboard” scene or modal that displays the top scores. Use the synthwave typography and colours defined in `domain_ui.md`.

By following these rules, the leaderboard remains simple, secure, and consistent with the technical specification’s guidance on using Firestore and limiting queries to a manageable size.
