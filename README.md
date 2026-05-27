# TypeRacer

A real-time multiplayer typing speed test. Compete with friends or race solo.
Live at: https://typeracer-lac.vercel.app

## Features
- Solo mode with difficulty levels (Easy / Medium / Hard) and time limits (15s / 30s / 60s)
- Real-time multiplayer races
- WPM and accuracy tracking
- User profiles and friends system

## Tech Stack
- Next.js 16, React 19, TypeScript
- Socket.IO (hosted on Railway)
- Drizzle ORM + Turso (SQLite)
- Tailwind CSS + shadcn/ui

## Getting Started

1. Clone the repo
   git clone https://github.com/shubham-chamoli/TypeRacer.git

2. Install dependencies
   npm install

3. Set up environment variables
   cp .env.example .env
   Then fill in your values.

4. Push the database schema
   npm run db:push

5. Run the dev server
   npm run dev

## Deployment

This project is split across three services:

- Turso for the database
- Railway for the Socket.IO server
- Vercel for the Next.js app

### 1. Turso

Create a database in Turso, then copy the database URL and auth token into:

- `TURSO_DATABASE_URL`
- `TURSO_AUTH_TOKEN`

Use the same values locally and in both deployments. After setting them locally, run `npm run db:push` once to create or sync the schema.

### 2. Railway

Deploy the socket server from this repository with the start command:

```bash
npm run start:socket
```

Set these Railway variables:

- `TURSO_DATABASE_URL`
- `TURSO_AUTH_TOKEN`
- `AUTH_SECRET`
- `SOCKET_CORS_ORIGINS`

`SOCKET_CORS_ORIGINS` should include your Vercel frontend URL, for example `https://your-app.vercel.app`.

Railway will usually provide `PORT` automatically. The server also accepts `SOCKET_PORT`, but you typically do not need to set it.

### 3. Vercel

Deploy the Next.js app to Vercel and set these variables:

- `TURSO_DATABASE_URL`
- `TURSO_AUTH_TOKEN`
- `AUTH_SECRET`
- `NEXT_PUBLIC_SOCKET_URL`

`NEXT_PUBLIC_SOCKET_URL` must point to your Railway Socket.IO URL, for example `https://your-socket-service.up.railway.app`.

### 4. Final env map

Local `.env` should look like this:

```bash
TURSO_DATABASE_URL=...
TURSO_AUTH_TOKEN=...
AUTH_SECRET=...
NEXT_PUBLIC_SOCKET_URL=http://localhost:3001
SOCKET_CORS_ORIGINS=http://localhost:3000
```

Production mapping:

- Turso: database URL + token
- Railway: database URL + token + auth secret + Vercel domain in `SOCKET_CORS_ORIGINS`
- Vercel: database URL + token + auth secret + Railway socket URL in `NEXT_PUBLIC_SOCKET_URL`

If you want, I can also turn this into a step-by-step deploy checklist with the exact Turso, Railway, and Vercel clicks/commands.
