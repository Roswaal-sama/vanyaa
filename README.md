# VANYAA Gauntlet

Modern VANYAA Valorant tournament platform with a public player arena, private CRM, live leaderboard, admin-controlled gauntlet flow, and optional Firebase persistence.

## Structure

- `user/` - public frontend: hero, registration, leaderboard, live match card.
- `crm/` - private admin frontend: login, player search/edit/export, sequence control, match results, continue/quit decisions, private prize tracking.
- `backend/` - Node backend: API, auth sessions, storage, Server-Sent Events, static hosting.
- `config.js` - frontend API target. Keep blank for backend-served local use, set to Render URL when deploying frontend separately on Vercel.

## Local Run

```powershell
cd C:\Users\black\Documents\Codex\2026-07-08\ad\outputs\double-or-nothing
npm install
npm start
```

Open:

- Player site: `http://127.0.0.1:4177/user/`
- Admin CRM: `http://127.0.0.1:4177/crm/`

Admin login code:

```text
VANYAA-ADMIN
```

Override it:

```powershell
$env:CRM_LOGIN_CODE="YOUR-PRIVATE-CODE"; npm start
```

## Tournament Rules

- Registration closes automatically at 51 players.
- Public players never receive prize, amount, or private admin fields.
- Admin can edit, approve/reject, remove, delete, search, and export players.
- Admin arranges seed order before the first match.
- Admin starts, pauses, resumes, restarts, or resets the tournament.
- Each match win adds `Rs 50` to the winner's private active amount.
- After every winner selection, Admin chooses `Continue` or `Quit & claim`.
- Continue pairs the active winner with the next eligible seed.
- Quit removes the winner from the run and keeps their claimed amount.
- Losing resets the loser's active amount to `Rs 0`.
- Match history and activity log record wins, forfeits, and decisions.

## Firebase

The backend works locally with JSON persistence. In production, set these env vars on Render to store state in Firestore:

```text
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_SERVICE_ACCOUNT_JSON={"type":"service_account",...}
FIREBASE_COLLECTION=vanyaa
FIREBASE_DOC=tournamentState
```

If Firebase credentials are missing, the backend falls back to `backend/data/state.json`.

## Render Backend

Use `outputs/double-or-nothing` as the Render root directory.

Build command:

```text
npm install
```

Start command:

```text
npm start
```

Recommended env vars:

```text
CRM_LOGIN_CODE=your-private-admin-code
CORS_ORIGIN=https://your-vercel-domain.vercel.app
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_SERVICE_ACCOUNT_JSON=your-service-account-json
```

`render.yaml` is included if you prefer Render Blueprint deploys.

## Vercel Frontend

Use `outputs/double-or-nothing` as the Vercel project root.

After the Render backend is live, set `config.js`:

```js
window.VANYAA_API_BASE = "https://your-render-service.onrender.com";
```

Then deploy the static frontend. `vercel.json` routes `/`, `/user`, and `/crm` cleanly.
