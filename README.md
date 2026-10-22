# Virtual City — Original Game Project

Fondasi game virtual city isometrik original. Pemain hanya mengamati karakter yang bergerak otomatis dan berinteraksi melalui global chat serta pesan pribadi.

## Keputusan inti
- Tidak ada kontrol gerakan karakter oleh pemain.
- Simulasi karakter menggabungkan jadwal dan adaptasi situasi.
- Kamera dapat digeser dan di-zoom.
- MVP online mencakup global chat dan pesan pribadi.
- Belum ada aset final; prototipe memakai bentuk prosedural/placeholder.

## Isi repo
- `prototype/index.html`: prototipe browser lokal, tanpa dependency.
- `docs/TDD.md`: desain teknis.

## Menjalankan
Buka `prototype/index.html` di browser modern. Chat saat ini hanya demo lokal, belum multiplayer.

## Target produksi
Client React + TypeScript/PixiJS; backend Node.js + TypeScript modular monolith; PostgreSQL; WebSocket; Capacitor untuk mobile setelah web stabil.

Status: Prototype 0.1 — bukan game online siap produksi.
