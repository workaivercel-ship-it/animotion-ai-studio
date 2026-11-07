# ANIMOTION AI STUDIO — V1 Foundation

A bilingual (English/Arabic RTL) React + TypeScript foundation for the ANIMOTION AI STUDIO product.

## Locked product decisions
- General-purpose educational/storytelling content across niches.
- Hybrid visual production: deterministic 2D + AI images + AI video providers.
- Gemini TTS as the default voice provider.
- English and Arabic interface with RTL/LTR support.

## Included in this V1 foundation
- Premium dark production workspace UI.
- Dashboard, project list, project creation modal.
- Project settings: content niche, narration language, 16:9 / 9:16.
- Project cards, search, delete, workflow stage overview.
- Local persistence for immediate preview and testing.
- Firebase Auth / Firestore / Storage initialization hooks.
- Bilingual interface and RTL layout.

## Important implementation boundary
This is the V1 foundation, not the complete production engine. AI generation, Gemini TTS, cloud persistence, storyboard generation, rendering and MP4 export require credentials and subsequent modules. The workspace stage advance control is explicitly prototype navigation; it does not claim that production work has been completed.

## Run locally
1. Install Node.js 20 or newer.
2. Copy `.env.example` to `.env` and add Firebase web app credentials (optional for local demo).
3. Run `npm install`.
4. Run `npm run dev`.
5. Run `npm run build` to validate the production bundle.

Without Firebase configuration, projects are saved in browser localStorage. With Firebase configured, project records are stored under `users/{uid}/projects`.

## Firebase setup
1. Create a Firebase project.
2. Add a Web App and copy its configuration into `.env`.
3. Enable Google Authentication.
4. Create a Firestore database and Storage bucket.
5. Add security rules that scope all project data to the authenticated user's UID.

Example Firestore rules (review and adapt before production):

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/projects/{projectId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## Next engineering milestones
- V1.1: Firebase authentication completion, Firestore rules, project versioning.
- V1.2: Gemini API server endpoint and niche / idea / script generation.
- V1.3: Gemini TTS, audio upload and analysis.
- V1.4: storyboard schema and editable scene cards.
- V1.5: SVG animation preview and timeline.
- V1.6: separate rendering worker (Cloud Run + FFmpeg) and verified MP4 export.

## Security
Never place Gemini or other provider API keys in browser code. Use server-side environment secrets and authenticated backend endpoints. Never trust client-provided user IDs for authorization.
