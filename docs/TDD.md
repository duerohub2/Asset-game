# Technical Design Document — Virtual City

Versi 0.1-draft. Aset final belum dibuat.

## Gameplay
Pemain mengamati kota melalui kamera pan/zoom dan memilih warga untuk melihat profil/aktivitas publik. Pemain tidak menggerakkan karakter, tidak memiliki joystick atau klik-untuk-jalan. Karakter bergerak otomatis. Interaksi sosial MVP: global chat dan pesan pribadi.

## Arsitektur target
- Client: React + TypeScript, PixiJS renderer.
- Backend: Node.js + TypeScript modular monolith.
- Database: PostgreSQL.
- Realtime: WebSocket.
- Redis opsional untuk presence/cache/pub-sub saat diperlukan.
- Mobile: Capacitor setelah web stabil.

Modul backend: auth, users, residents, world, buildings, simulation-scheduler, schedule-engine, situation-engine, activity-selector, navigation-engine, movement-simulator, simulation-persistence, chat-service, chat-moderation, admin, audit.

## Simulasi karakter hibrida
Alur: jadwal dasar -> evaluasi situasi -> pemilihan aktivitas -> pathfinding -> pembaruan posisi -> replikasi ke klien.
Jadwal normal meliputi rumah, kerja, makan, rekreasi, istirahat. Situasi mencakup acara, tempat tutup, kepadatan, gangguan area, dan tujuan tidak tersedia. Situasi valid dapat mengalihkan aktivitas; tujuan invalid memicu fallback. Cooldown/hysteresis mencegah karakter berganti tujuan tanpa henti. Server menjadi sumber kebenaran pada multiplayer; klien menginterpolasi gerak. Evaluasi berbasis event/interval, bukan setiap frame untuk semua warga.

## Skema data target
### Akun dan dunia
- users(id, username UNIQUE, status, role, created_at)
- sessions(id, user_id, expires_at, revoked_at)
- residents(id, user_id NULL UNIQUE, district_id, status); NPC memakai user_id NULL
- districts(id, name, metadata)
- world_chunks(id, chunk_x, chunk_y, version)
- buildings(id, district_id, building_type, status, metadata)
- building_tiles(building_id, tile_x, tile_y)
- terrain_tiles(chunk_id, tile_x, tile_y, terrain_type, walkable)
- residences(id, building_id, capacity)
- employment_positions(id, building_id, job_definition_id, capacity)
### Simulasi
- resident_schedules(id, resident_id, day_of_week, start_minute, end_minute, activity_type, target_type, target_id, priority)
- resident_simulation_state(resident_id, current_activity, activity_started_at, target_type, target_id, state_version, updated_at)
- resident_positions(resident_id, chunk_id, tile_x, tile_y, direction, movement_state, updated_at)
- resident_routes(resident_id, route_data, next_step_index, navigation_version, updated_at)
- city_situations(id, situation_type, scope_type, scope_id, severity, starts_at, ends_at, payload, status)
- resident_activity_history(id, resident_id, activity_type, reason_code, started_at, ended_at)
### Chat
- chat_conversations(id, type global/direct, created_at)
- chat_participants(conversation_id, user_id, joined_at, last_read_message_id)
- chat_messages(id, conversation_id, sender_user_id, body, created_at, edited_at, deleted_at, moderation_status)
- chat_blocks(blocker_user_id, blocked_user_id, created_at)
- chat_reports(id, message_id, reporter_user_id, reason, status, reviewed_by, reviewed_at)
- chat_message_receipts optional for delivery/read timestamps

Constraints: direct messages are readable only by participants; unique direct pair; index conversation_id + created_at + id; don't log private message bodies; persist simulation checkpoints, not every rendered frame; administrative mutations write audit_logs. One account maps to at most one main resident unless product rules change.

## API target
Prefix `/api/v1`.
- GET /world/metadata
- GET /world/chunks/:chunkId
- GET /residents/:residentId
- GET /residents/:residentId/activity
- GET /residents/:residentId/position
- GET /buildings/:buildingId
- GET /chat/global/messages?cursor=...
- POST /chat/global/messages
- GET /chat/conversations
- POST /chat/conversations/direct
- GET /chat/conversations/:id/messages?cursor=...
- POST /chat/conversations/:id/messages
- POST /chat/conversations/:id/read
- POST /chat/users/:userId/block and DELETE equivalent
- POST /chat/messages/:messageId/report
- DELETE /chat/messages/:messageId for authorized moderation

No public player endpoint sets character position/destination. WebSocket server events: resident.position_updated, resident.activity_updated, city.situation_updated, chat.global_message_created, chat.direct_message_created, chat.message_deleted, chat.read_updated.

## Security
Authenticate sessions; enforce account status and permissions server-side; rate-limit chat; limit and validate message length; support block/report/moderation; check direct-message membership on every read and subscription; audit admin actions; back up database. Never trust client position/activity/role.

## Acceptance criteria
1. Characters move without player movement input; camera pan/zoom doesn't change simulation state.
2. Schedule controls normal behavior; relevant situations can override with valid fallback.
3. Activity selection does not oscillate; important state recovers from checkpoints.
4. Eligible players exchange global messages in real time and paginate history.
5. Direct messages are accessible only to participants; block/report workflows work.
6. Rate limiting, validation, and moderation are server-enforced.
7. Suspended/pending accounts cannot use restricted features; movement spoofing is rejected; admin actions are audited.

## Phases
1. Isometric grid, chunks, camera, placeholder buildings.
2. Autonomous schedule-based movement and navigation.
3. Situational adaptation and fallback.
4. Auth and permissions.
5. Global chat and direct messages.
6. Persistence, moderation, load tests, mobile packaging.

## Prototype limitation
Prototype 0.1 is local and uses procedural placeholders. It has no accounts, server-authoritative simulation, persistent storage, or real multiplayer chat.
