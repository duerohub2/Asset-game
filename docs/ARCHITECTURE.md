# Asset-game architecture decision

**Status:** Accepted · **Date:** 2026-10-23

## Decisions
- Browser-first, no installation; static frontend deploys to Vercel.
- Hybrid 2.5D: Three.js low-poly 3D environment with 2D resident sprites.
- Realtime world process runs separately from Vercel's static frontend.
- Keep the existing original SVG atlases in `assets/`; do not use ripped game art.

## Components
- **Browser:** Vite + TypeScript + Three.js; rendering, camera, resident selection, chat UI.
- **Realtime server:** Node.js + WebSocket; server-owned NPC positions, presence and chat transport.
- **Later:** accounts and durable persistence once product rules are defined.

Vercel Functions support WebSockets in current documentation, but a persistent autonomous world is easier to reason about with a dedicated long-running process owning its simulation tick and in-memory state.

## Local development
```sh
npm install
npm run dev
```
Build static files:
```sh
npm run build
npm run typecheck
```
Run realtime server separately:
```sh
cd server && npm install && npm run dev
```
Set `VITE_REALTIME_URL=ws://localhost:8080` in root `.env.local` to connect locally. In deployment, use the server's `wss://` URL. Configure `ALLOWED_ORIGINS` as a comma-separated allowlist of trusted origins.

## WebSocket protocol v1
Client: `world:join`, `chat:send`.
Server: `world:hello`, `world:snapshot`, `world:presence`, `world:chat`, `world:system`, `world:error`.

## Limitations
This is a foundation, not a finished multiplayer game. The 3D environment is procedural geometry. The original resident atlas is a static sheet, not a walk-cycle animation. The realtime server keeps state in memory and currently has no accounts, durable storage, rate limiting, moderation or reconnect strategy. The UI does not yet expose another player's peer ID, so private chat is only scaffolded. Do not treat this as production-ready.
