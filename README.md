# Cloudflare Workflows Starter (cfpage02)

Demo interaktif yang memperagakan **Cloudflare Workflows**: sebuah alur kerja multi-langkah yang berjalan di backend lalu statusnya ditampilkan secara real-time di UI lewat WebSocket. Ditujukan untuk developer yang ingin belajar/mencoba cara membangun durable workflow (step dengan delay, pause menunggu event, dsb.) di atas Cloudflare Workers.

![Screenshot](assets/template-screenshot.png)

## Fitur Utama

- Menjalankan workflow 4 langkah (`process data` → `wait 2 seconds` → `wait for approval` → `final`) menggunakan Cloudflare Workflows (`worker/workflow.ts`).
- Update status workflow secara real-time ke browser via WebSocket, didukung oleh Durable Object dengan hibernation API (`worker/durable-object.ts`).
- Step `wait for approval` dapat dipicu dari API (`POST /api/workflow/event/:id`) untuk mensimulasikan persetujuan/event eksternal.
- Visualisasi diagram alur workflow beserta status tiap step (`src/components/WorkflowDiagram.tsx`).
- Tampilan kode sumber workflow dengan highlight baris sesuai step yang sedang berjalan (`src/components/CodeDisplay.tsx`).
- API untuk memulai workflow baru dan mengecek statusnya (`POST /api/workflow/start`, `GET /api/workflow/status/:id`).

## Tech Stack

- **Frontend**: React 19, TypeScript, Tailwind CSS 4, Vite 7
- **Backend**: Cloudflare Workers, Cloudflare Workflows, Durable Objects (SQLite-backed)
- **Tooling**: Wrangler, ESLint, Vitest (`@cloudflare/vitest-pool-workers`)

## Cara Menjalankan Lokal

### 1. Instalasi

```bash
npm install
```

### 2. Environment

Proyek ini tidak memerlukan variabel environment (`.env`) untuk berjalan secara lokal — semua binding (Workflow, Durable Object) sudah didefinisikan di `wrangler.jsonc`.

### 3. Menjalankan

```bash
npm run dev
```

Buka `http://localhost:5173`.

### 4. Build

```bash
npm run build
```

### Skrip lain yang tersedia

- `npm run lint` — menjalankan ESLint
- `npm run test` — menjalankan test Vitest (termasuk test workflow)
- `npm run check` — type-check + dry-run deploy Wrangler
- `npm run cf-typegen` — generate tipe `Env` dari `wrangler.jsonc`

## Struktur Folder

```
src/
  components/   Komponen UI (diagram workflow, tampilan kode, dekorasi background)
  hooks/         Hook React (koneksi WebSocket ke status workflow)
  App.tsx        Komponen utama aplikasi
  types.ts       Tipe bersama & definisi langkah workflow
worker/
  index.ts         Entry point Worker: routing API & upgrade WebSocket
  workflow.ts       Definisi Cloudflare Workflow (MyWorkflow)
  durable-object.ts Durable Object untuk state & broadcast WebSocket
test/            Test untuk workflow (Vitest)
assets/          Aset gambar (screenshot)
wrangler.jsonc   Konfigurasi Cloudflare Workers/Workflows/Durable Objects
```

## Deploy

Proyek ini di-deploy sebagai Cloudflare Worker (bukan aplikasi mobile), sehingga tidak ada rilis APK. Untuk deploy ke Cloudflare:

```bash
npm run deploy
```

Perintah ini menjalankan build lalu `wrangler deploy` sesuai konfigurasi di `wrangler.jsonc`.

## Kredit

Dibuat oleh **Ranupane Rimba Argana**, founder [Kukerjain](https://kukerjain.dev).

- 🌐 [kukerjain.dev](https://kukerjain.dev)
- 👤 [ranupane.kukerjain.dev](https://ranupane.kukerjain.dev)
