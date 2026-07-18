# Keputusan Final Stack, Perubahan SRS, dan Pesan WhatsApp BHT-Nexus

**Tanggal:** 18 Juli 2026
**Status:** ringkasan final yang siap dibagikan kepada tim
**Tujuan:** menjawab perbedaan keputusan lama dan baru, stack yang dipakai, alasan yang sesuai kebutuhan BHT-Nexus dan kemampuan tim, perubahan SRS, serta pesan WhatsApp yang siap dikirim

---

## 1. TL;DR

Keputusan final yang dipakai:

| Bagian | Keputusan |
|---|---|
| Frontend | Next.js 16.2 + React 19.2 + TypeScript |
| Backend API | **Node.js 24 LTS + NestJS 11 + Express bawaan** |
| ORM | **Drizzle ORM** |
| Database | **PostgreSQL 18** |
| Dokumentasi API | **OpenAPI/Swagger** |
| Scraper dan RAG | **Python 3.12 sebagai worker terpisah** |
| Hubungan scraper/RAG dengan web | seluruh permintaan pengguna masuk melalui NestJS API |
| Antrean awal | tabel job PostgreSQL, belum memakai Redis |
| Login | Passport + Argon2id + sesi database dalam cookie aman |
| Penyimpanan dokumen | volume server melalui storage adapter; R2 setelah penggunaan cloud disetujui |
| Bentuk arsitektur | modular monolith, bukan microservice |
| Deployment | Docker Compose + Caddy pada server Linux |

Jawaban paling singkat untuk chat terakhir:

> Relasi database lanjut dibuat memakai Drizzle + PostgreSQL. Backend final memakai NestJS 11 dengan Express bawaan pada Node.js 24 LTS. Scraper dan RAG tetap Python sebagai worker terpisah, tetapi endpoint untuk frontend disatukan melalui backend NestJS. Swagger tetap dipakai.

SRS **perlu diubah** karena masih menulis Prisma, belum menjelaskan Express, job worker, autentikasi final, antrean, penyimpanan, backup, pengujian, dan target kualitas. Bagian scraper/RAG juga perlu diselaraskan dengan keadaan kode POC dan target produksi.

### Arti status final

Stack ini dikunci sampai rilis pertama. Perubahan framework tidak dibuka ulang hanya karena muncul artikel, benchmark, atau preferensi baru. Perubahan hanya dilakukan jika:

- implementasi terbukti terblokir oleh ketidakcocokan yang tidak memiliki penyelesaian aman;
- dependency utama berhenti dipelihara atau memiliki risiko keamanan kritis;
- hasil pengujian menunjukkan keputusan sekarang gagal memenuhi target;
- perubahan ditulis dalam architecture decision record, diperiksa tim, dan memiliki rencana migrasi.

Dengan aturan ini, tim dapat mulai bekerja tanpa mengulang perdebatan stack setiap minggu.

---

## 2. Bagian yang tetap sama

Keputusan baru tidak membuang seluruh pembahasan sebelumnya. Hal-hal berikut tetap benar:

1. frontend dan backend berada pada repo serta proses yang terpisah;
2. frontend cukup memanggil API tanpa mengetahui cara scraper/RAG bekerja;
3. Drizzle dipakai sebagai ORM;
4. PostgreSQL dipakai sebagai database utama;
5. REST API dipakai sebagai cara komunikasi;
6. Swagger/OpenAPI dipakai sebagai dokumentasi API;
7. scraper dan RAG tetap memakai Python;
8. hasil scraper tidak langsung menjadi data resmi;
9. hasil ekstraksi RAG tidak langsung mengubah data resmi;
10. manusia tetap memeriksa kandidat perubahan;
11. Docker dipakai agar lingkungan tim konsisten;
12. repo API yang terpisah tetap dipakai.

Jadi, pembahasan tim mengenai backend terpisah, Drizzle, PostgreSQL, Swagger, serta endpoint scraper/RAG melalui backend tetap dipertahankan.

---

## 3. Bagian yang berubah dari panduan sebelumnya

| Sebelumnya | Final | Alasan perubahan |
|---|---|---|
| Bun sebagai runtime backend | Node.js 24 LTS | jalur LTS, ekosistem NestJS paling luas, dan lebih mudah diserahterimakan |
| Elysia sebagai framework backend | NestJS 11 | BHT-Nexus memiliki banyak modul, izin, audit, review, impor, dan pekerjaan otomatis |
| Fastify sempat dipertimbangkan sebagai adapter Nest | Express bawaan NestJS | jalur default paling matang dan mengurangi masalah kompatibilitas bagi tim pemula |
| Prisma pada SRS | Drizzle ORM | sesuai kesepakatan tim, pengalaman nyata anggota, dan lebih dekat dengan SQL |
| Better Auth sebagai baseline | Passport + sesi database | integrasi NestJS Better Auth dikelola komunitas; baseline internal lebih jelas jika sesi dimiliki aplikasi |
| MinIO atau R2 | volume aman lebih dahulu, lalu R2 | MinIO OSS telah diarsipkan; cloud hanya dipakai untuk klasifikasi data yang disetujui |
| Redis dapat muncul sejak awal | antrean PostgreSQL | volume awal belum membutuhkan server antrean tambahan |
| SQLite + FAISS sebagai arah RAG | hanya POC; produksi PostgreSQL + pgvector | metadata, izin, chunk, dan vektor perlu konsisten serta dapat dibackup bersama |
| Model embedding MPNet dikunci | multilingual-e5-base menjadi kandidat evaluasi | model POC sekarang hanya membaca maksimal 128 token dan berisiko memotong chunk |
| Node/Bun package workflow | npm + package-lock.json | paling umum untuk Next.js dan NestJS pada Node |

### Perubahan paling penting

Perubahan terbesar hanya pada fondasi backend:

~~~text
SEBELUMNYA
Frontend Next.js
       |
Bun + Elysia + Drizzle
       |
PostgreSQL

FINAL
Frontend Next.js
       |
Node.js 24 LTS + NestJS 11 + Express + Drizzle
       |
PostgreSQL 18
       |
Worker Python scraper dan RAG
~~~

---

## 4. Alasan final memakai NestJS + Express

### 4.1 Kebutuhan BHT-Nexus

BHT-Nexus bukan hanya API CRUD sederhana. Backend akan menangani:

- login dan sesi;
- role dan permission;
- data anggota;
- publikasi;
- impor spreadsheet;
- kandidat perubahan;
- pemeriksaan dan persetujuan;
- dokumen dan versinya;
- KPI;
- audit perubahan;
- scraper;
- RAG;
- antrean pekerjaan;
- ekspor;
- backup dan pemantauan.

Semakin banyak fitur, semakin besar risiko kode fleksibel berubah menjadi campuran yang sulit dipahami. NestJS memberi pola module, controller, service, provider, guard, dan dependency injection. Pola tersebut menjadi pagar agar kode hasil manusia maupun vibe coding tetap konsisten.

### 4.2 Kemampuan tim

Bukti chat dan GitHub menunjukkan kombinasi kemampuan berikut:

| Kemampuan yang tersedia | Pemanfaatan final |
|---|---|
| React, Next.js, dan TypeScript | frontend |
| pengalaman Drizzle dan backend TypeScript | ORM serta implementasi API |
| Python, data, AI, vision, NLP, dan MLOps | scraper, OCR, embedding, dan RAG |
| SQL dan data processing | skema, impor, deduplikasi, KPI |
| Docker dan GitHub yang sedang berkembang | lingkungan lokal dan CI/CD |

Biaya belajar yang nyata:

- salah satu anggota lebih nyaman dengan Elysia;
- belum ada bukti tim telah mahir NestJS;
- dependency injection membutuhkan penyesuaian;
- tiga hari awal dialokasikan untuk belajar dan membuat contoh baku.

NestJS tetap dipilih karena proyek ini akan terus berkembang dan kemungkinan diteruskan oleh anggota lain. Struktur yang seragam menurunkan risiko serah-terima dan membantu vibe coding mengikuti pola yang sama.

### 4.3 Mengapa bukan Elysia

Elysia bukan teknologi buruk. Elysia:

- cepat dibuat;
- ringkas;
- performanya tinggi;
- sudah lebih nyaman bagi salah satu anggota.

Namun Elysia tidak memaksakan struktur. Tim harus merancang dan menjaga sendiri:

- batas module;
- dependency;
- pola service/repository;
- cara testing;
- cara guard dan permission;
- transaksi;
- audit;
- pekerjaan latar.

Untuk API kecil atau layanan yang sangat terfokus, kebebasan itu menguntungkan. Untuk BHT-Nexus yang memiliki banyak domain, kebebasan tersebut menambah risiko jangka panjang.

Penilaian berbobot yang sudah memasukkan kemampuan tim:

| Kriteria | Bobot | NestJS | Elysia |
|---|---:|---:|---:|
| Struktur banyak modul | 25% | 5 | 2 |
| Perawatan dan serah-terima | 20% | 5 | 3 |
| Kecepatan rilis 16 minggu | 15% | 3 | 5 |
| Kebiasaan dan biaya belajar tim | 15% | 2 | 4 |
| Ekosistem | 10% | 5 | 3 |
| Pengujian | 10% | 5 | 3 |
| Performa HTTP | 5% | 4 | 5 |
| **Nilai akhir** | **100%** | **4,20/5** | **3,30/5** |

Kemampuan tim sudah diberi bobot 15%, bukan dianggap tidak penting. NestJS tetap unggul setelah faktor tersebut dimasukkan.

### 4.4 Mengapa Express, bukan Fastify

NestJS memakai Express secara bawaan. Fastify memang lebih cepat pada benchmark routing, tetapi kebutuhan BHT-Nexus lebih banyak menghabiskan waktu pada:

- query database;
- pengambilan SINTA/Scholar/Crossref;
- pembacaan PDF;
- OCR;
- pembuatan embedding;
- proses AI.

Kecepatan routing bukan hambatan utama. Express dipakai karena:

- jalur resmi bawaan NestJS;
- dokumentasi dan contoh paling banyak;
- upload Multer, Passport, middleware, dan testing lebih langsung;
- lebih mudah dipelajari;
- menghilangkan satu lapisan penyesuaian.

Fastify baru dinilai ulang jika pengujian produksi membuktikan Express menjadi hambatan setelah query dan worker dioptimalkan.

### 4.5 Mengapa Drizzle tetap dipakai

Drizzle sesuai karena:

- sudah pernah dipakai salah satu anggota;
- TypeScript-native;
- query dekat dengan SQL;
- cocok dengan PostgreSQL;
- migration menghasilkan SQL yang dapat diperiksa;
- lebih mudah memahami query sebenarnya.

Prisma bukan legacy. Drizzle dipilih karena kecocokan proyek dan tim, bukan karena Prisma dianggap mati.

---

## 5. Bentuk final hubungan web, API, scraper, dan RAG

~~~mermaid
flowchart TB
    U[Pengguna] --> W[Next.js Web]
    W --> A[NestJS API]
    A --> D[(PostgreSQL)]
    A --> F[(Penyimpanan dokumen)]
    A --> J[Tabel job PostgreSQL]
    J --> S[Worker scraper Python]
    J --> R[Worker RAG Python]
    S --> X[SINTA, Scholar, Crossref]
    S --> T[Staging data]
    R --> T
    T --> H[Pemeriksaan manusia]
    H --> O[Data resmi]
~~~

Jawaban untuk pembahasan endpoint:

- benar, endpoint yang dipakai frontend disatukan di backend;
- scraper dan RAG tidak ditulis ulang menjadi TypeScript;
- NestJS membuat job dan memberi status kepada frontend;
- worker Python mengambil job di belakang layar;
- worker menulis hasil ke staging;
- pemeriksa menerima atau menolak kandidat;
- hanya API yang mempromosikan hasil menjadi data resmi.

Contoh endpoint:

~~~text
POST /api/v1/scraper-jobs
GET  /api/v1/jobs/{jobId}
POST /api/v1/documents
POST /api/v1/documents/{documentId}/index-jobs
POST /api/v1/rag/queries
GET  /api/v1/reviews
POST /api/v1/reviews/{reviewId}/approve
POST /api/v1/reviews/{reviewId}/reject
~~~

---

## 6. SRS perlu diubah

SRS perlu direvisi sebelum dibagikan sebagai dokumen final dan sebelum implementasi teknis dimulai. Revisi tidak berarti SRS lama buruk. Struktur besarnya tetap dapat dipakai, tetapi keputusan implementasi dan bagian kosong harus diselaraskan.

### 6.1 Perubahan metadata dan kualitas dokumen

| Lokasi saat ini | Perubahan |
|---|---|
| baris awal For {{project name}} | ganti dengan BHT-Nexus |
| metadata penulis/tanggal yang belum final | isi nama organisasi, versi, tanggal, dan revision history |
| catatan personal | ubah menjadi bahasa organisasi |
| Assumnptions and Dependencies | perbaiki menjadi Assumptions and Dependencies |
| referensi | tambahkan meeting, source materials, repo POC, serta keputusan arsitektur |
| dev dan main | main menjadi sumber resmi; dev disinkronkan agar tidak menampilkan keputusan berbeda |

### 6.2 Perubahan stack pada bagian 2.5

#### Development Framework

Ganti atau lengkapi tabel menjadi:

| Komponen | Teknologi final | Keterangan |
|---|---|---|
| Frontend Framework | Next.js 16.2 + React 19.2 | aplikasi web |
| Backend Framework | NestJS 11 | API modular |
| HTTP Adapter | Express | adapter bawaan NestJS |
| Backend Runtime | Node.js 24 LTS | runtime produksi |
| Python Worker | Python 3.12 | scraper, OCR, dan RAG |

#### Programming Language

Tetapkan:

- TypeScript 5.9 untuk frontend dan API;
- Python 3.12 untuk worker;
- SQL migration untuk perubahan database.

#### Database and Persistence

Ubah:

~~~text
Prisma ORM
~~~

menjadi:

~~~text
Drizzle ORM + node-postgres
~~~

Tambahkan:

- PostgreSQL 18;
- migration SQL harus diperiksa;
- drizzle push tidak dipakai pada database bersama/produksi;
- transaksi dipakai untuk perubahan data resmi;
- pgvector dipakai untuk target produksi RAG;
- unique constraint dan provenance wajib.

#### API and System Integration

Tambahkan:

- REST API dengan awalan /api/v1;
- OpenAPI/Swagger menjadi kontrak;
- frontend client dibuat dari OpenAPI;
- scraper dan RAG tidak diekspos langsung ke browser;
- pekerjaan berat memakai asynchronous job;
- setiap request dan job memiliki ID pelacakan.

#### File Storage

Ubah Object Storage Service yang terlalu umum menjadi:

- tahap awal memakai volume server melalui storage adapter;
- metadata, hash, versi, klasifikasi, dan izin disimpan di PostgreSQL;
- R2 dipakai setelah penggunaan cloud disetujui;
- MinIO OSS tidak menjadi baseline;
- file selalu dibackup di luar host.

### 6.3 Perubahan Software Interfaces

Tabel SINTA saat ini menyatakan tidak ada pembatasan dan dapat diandalkan. Ubah menjadi:

| Sumber | Status final |
|---|---|
| SINTA | sumber publik best effort; halaman dan batas akses dapat berubah |
| Google Scholar | sumber sekunder best effort; CAPTCHA/sign-in adalah kegagalan yang wajar |
| Crossref | sumber terstruktur untuk DOI; memakai contact user-agent, cache, dan rate limit |
| Spreadsheet | sumber internal yang selalu melalui preview dan review |

Tidak ada scraper yang boleh:

- melewati CAPTCHA;
- memaksa login;
- menulis langsung ke data resmi;
- mengabaikan robots.txt dan ketentuan akses.

### 6.4 Perubahan fungsi scraper

Pertahankan kebutuhan:

- profil dan karya SINTA;
- profil dan publikasi Scholar secara best effort;
- pencocokan DOI;
- attempt log;
- staging dan review.

Tambahkan kebutuhan:

- snapshot mentah;
- parser version;
- source URL dan waktu pengambilan;
- response hash;
- retry dengan backoff;
- circuit breaker;
- unique key dan idempotency;
- fixture HTML dan automated test;
- laporan perubahan struktur halaman;
- fallback impor manual.

Koreksi keadaan implementasi:

- POC sekarang baru memiliki identitas Scholar, belum seluruh metadata publikasi Scholar sesuai janji SRS;
- POC mengekspor CSV, belum seluruh JSONL/Markdown yang disebut;
- belum ada automated test;
- seluruh perbedaan tersebut harus ditulis sebagai target, bukan seolah-olah sudah selesai.

### 6.5 Perubahan fungsi RAG

Pertahankan:

- PDF sebagai sumber awal;
- OCR;
- pencarian potongan;
- jawaban dengan referensi;
- penggunaan lokal untuk POC;
- human review untuk candidate extraction.

Ubah spesifikasi model:

| SRS saat ini | Revisi |
|---|---|
| llama3.2:3b wajib selamanya | model POC; produksi harus melewati evaluasi kualitas, privasi, lisensi, kapasitas, dan biaya |
| multilingual MPNet wajib | menjadi baseline lama pembanding; multilingual-e5-base menjadi kandidat evaluasi |
| FAISS + SQLite | hanya POC |
| pgvector nanti | PostgreSQL + pgvector menjadi target produksi |

Alasan koreksi embedding:

- model MPNet POC hanya menerima maksimal 128 token;
- chunk POC sekitar 1.200 karakter;
- sebagian isi berisiko terpotong tanpa terlihat;
- chunk produksi memakai ukuran berbasis token;
- model baru tetap harus diuji, bukan dipercaya hanya dari nama.

Tambahkan kebutuhan:

- hash dan versi dokumen;
- hak akses sebelum pencarian;
- quarantine dan malware scan;
- model revision;
- chunking version;
- validasi kutipan;
- penolakan jawaban bila bukti tidak cukup;
- dataset uji Indonesia/Inggris;
- citation precision minimal 95%;
- kebocoran dokumen tanpa izin harus 0;
- hasil ekstraksi hanya menjadi candidate change.

### 6.6 Isi bagian Quality of Service yang masih kosong

#### Performance

- API biasa p95 di bawah 500 ms pada 50 request bersamaan;
- pekerjaan OCR, scraper, dan embedding selalu asynchronous;
- daftar selalu memakai pagination;
- concurrency worker dibatasi.

#### Security

- Passport local;
- Argon2id;
- sesi database dalam Secure HttpOnly cookie;
- permission dan record-level access;
- CSRF, CSP, HSTS, rate limit, dan validasi input;
- upload quarantine dan antivirus;
- secret tidak masuk Git/log;
- audit untuk perubahan penting.

#### Reliability

- transaksi database;
- idempotency key;
- lease dan retry job;
- dead-letter status;
- unique constraint;
- worker tidak boleh menulis tabel resmi.

#### Availability

- target pilot 99,5%;
- health/live dan health/ready;
- maintenance terjadwal;
- prosedur rollback.

#### Observability

- log JSON;
- request_id, correlation_id, dan job_id;
- metric latency, error, job gagal, dan queue depth;
- alarm P1/P2;
- isi dokumen dan token tidak masuk log.

### 6.7 Isi bagian Design and Implementation yang masih kosong

Tambahkan keputusan:

| Bagian | Isi final |
|---|---|
| Installation | Docker Compose pada Windows/WSL2 untuk lokal dan Linux untuk server |
| Build and Delivery | npm ci, uv sync --locked, test, Docker build |
| Distribution | container image yang dapat dilacak ke commit SHA |
| Maintainability | modular monolith dan module template |
| Reusability | storage adapter, job contract, generated API client |
| Portability | Node LTS, Python, PostgreSQL, Docker |
| Cost | lokal gratis, satu server pilot, layanan berbayar bertahap |
| Deadline | rencana 16 minggu |
| POC | POC scraper/RAG bukan production-ready |
| Change Management | issue, branch, pull request, CI, review, migration |

### 6.8 Isi bagian AI/ML yang masih kosong

Guardrails:

- jawaban hanya dari potongan yang diizinkan;
- jawaban wajib menampilkan sumber;
- sistem menolak bila bukti kurang;
- AI tidak mengubah data resmi;
- seluruh kandidat diperiksa manusia.

Ethics:

- tidak melewati CAPTCHA;
- data pribadi diminimalkan;
- dokumen rahasia tidak dikirim ke AI eksternal;
- bias dan kualitas diuji dalam Bahasa Indonesia dan Inggris.

Human-in-the-Loop:

- reviewer menerima, menolak, atau meminta perbaikan;
- pembuat kandidat tidak menyetujui kandidatnya sendiri untuk perubahan sensitif;
- keputusan reviewer masuk audit.

Model Lifecycle:

- nama, revision, checksum, dan tanggal model disimpan;
- model baru membuat evaluasi serta indeks baru;
- rollback model tersedia;
- model tidak diperbarui diam-diam.

### 6.9 Isi bagian Verification

Setiap requirement harus dipetakan ke:

- test ID;
- jenis tes;
- data uji;
- hasil yang diharapkan;
- bukti;
- status;
- commit atau release yang diuji.

Contoh:

| Requirement | Verifikasi |
|---|---|
| login aman | unit, integration, security test |
| data scraper masuk staging | integration test |
| CAPTCHA tidak dilewati | fixture/error-path test |
| worker tidak menulis data resmi | database permission test |
| RAG menyertakan sumber | evaluation test |
| dokumen tanpa izin tidak bocor | security E2E |
| backup dapat dipulihkan | restore drill |

---

## 7. Keputusan yang dapat langsung dikerjakan sekarang

Urutan berikut tidak menunggu perubahan fitur lain:

1. lanjutkan perancangan relasi PostgreSQL memakai Drizzle;
2. jangan mengisi scaffold Bun/Elysia;
3. revisi fondasi repo kosong menjadi NestJS + Express;
4. buat tiga contoh baku: health, auth skeleton, dan publications;
5. tambahkan Swagger;
6. siapkan job, staging, review, dan audit pada desain database;
7. revisi SRS sesuai bagian 6;
8. port scraper/RAG setelah fondasi API dan job contract stabil;
9. jangan commit/push scaffold lama sebelum struktur baru selesai ditinjau.

Relasi database dapat mulai dibuat, tetapi harus memuat kelompok berikut:

~~~text
identity dan access
├── users
├── members
├── credentials
├── sessions
├── roles
└── permissions

business data
├── publications
├── publication_authors
├── publication_identifiers
├── documents
├── document_versions
└── kpi_definitions

data governance
├── import_batches
├── staging_records
├── review_cases
├── review_decisions
├── audit_events
└── data_sources

automation
├── automation_jobs
├── automation_attempts
├── automation_results
├── document_chunks
└── embeddings
~~~

---

## 8. Pesan WhatsApp siap kirim

### 8.1 Versi yang direkomendasikan

Salin pesan ini:

> Lanjut buat relasi databasenya pakai Drizzle + PostgreSQL dulu aja mas, itu tetap final 👍
>
> Untuk hasil audit dan pertimbanganku, backend finalnya aku lebih condong kita pakai **Node.js 24 LTS + NestJS 11 dengan Express bawaan**, bukan Bun + Elysia.
>
> Alasannya kebutuhan BHT-Nexus nantinya bukan API kecil. Ada auth dan permission, anggota, publikasi, impor, review, audit, KPI, job scraper, dokumen, serta RAG. Nest lebih ada pola dan batas modulnya, jadi menurutku lebih aman buat jangka panjang, handover, dan vibe coding biar struktur kodenya enggak beda-beda.
>
> Aku tetap memperhitungkan kalau Elysia lebih familiar dan development awalnya lebih cepat. Makanya Nest-nya pakai Express bawaan aja, enggak Fastify, supaya learning curve dan masalah kompatibilitasnya lebih kecil. Kita alokasikan sekitar 3 hari awal buat belajar DI dan bikin module contoh, setelah itu modul lain tinggal mengikuti pola.
>
> Drizzle, PostgreSQL, backend terpisah, REST API, dan Swagger tetap sesuai diskusi kita. Scraper sama RAG juga tetap Python, cuma semua request dari frontend masuk satu pintu lewat NestJS API. Untuk proses berat, API bikin job lalu worker Python yang mengerjakan di belakang.
>
> SRS perlu aku revisi sedikit: Prisma jadi Drizzle, tambahin Node/Express, auth-session, job queue, staging-review-audit, target produksi RAG/pgvector, storage-backup, security, testing, dan bagian quality yang masih kosong. Repo API juga masih kosong dan belum ada commit, jadi sekarang waktu paling aman buat menyesuaikan fondasinya.
>
> Jadi final dari aku: **NestJS + Express + Drizzle + PostgreSQL**, frontend tetap Next.js, worker scraper/RAG tetap Python. Aku lanjut rapikan SRS dan panduan repo berdasarkan stack ini.

### 8.2 Versi lebih singkat

> Lanjut relasi databasenya pakai Drizzle + PostgreSQL aja mas, itu tetap final 👍
>
> Untuk backend setelah aku audit lagi, finalnya aku usul **Node.js 24 LTS + NestJS 11 pakai Express bawaan**. Elysia memang lebih cepat dibuat, tapi modul BHT-Nexus bakal banyak: auth, permission, impor, review, audit, job, scraper, dokumen, dan RAG. Nest lebih aman buat struktur jangka panjang dan handover.
>
> Scraper/RAG tetap Python sebagai worker, tapi endpoint ke frontend satu pintu lewat Nest API. Swagger tetap. SRS nanti aku revisi dari Prisma ke Drizzle dan lengkapi bagian job, staging-review, security, backup, serta RAG production. Repo API masih kosong, jadi aman disesuaikan sekarang.

### 8.3 Versi paling pendek untuk menjawab segera

> Lanjut relasi DB pakai Drizzle + PostgreSQL dulu aja mas. Final backend dari aku: **Node.js 24 LTS + NestJS 11 + Express bawaan**. Scraper/RAG tetap Python sebagai worker dan endpoint frontend satu pintu lewat Nest API. Swagger tetap. Aku pilih Nest karena modul kita bakal banyak dan lebih aman buat struktur, handover, serta jangka panjang. SRS nanti aku sesuaikan dari Prisma ke Drizzle dan lengkapi job, review, audit, security, storage, serta RAG.

### 8.4 Balasan lanjutan bila ditanya mengenai Elysia

> Elysia bukan jelek mas, malah lebih cepat buat development awal dan aku paham itu lebih familiar. Cuma untuk kasus kita, performa routing bukan bottleneck utama karena proses beratnya ada di DB, scraping, PDF/OCR, dan AI. Aku pilih Nest supaya pola module, service, dependency, guard, dan testing lebih konsisten saat fitur serta anggota tim bertambah. Biar learning curve enggak terlalu besar, kita pakai Express bawaan Nest, bukan Fastify.

### 8.5 Balasan lanjutan bila ditanya mengenai relasi database

> Boleh lanjut mas. Tolong relasinya sekalian pisahkan area data resmi, staging, review, audit, dan automation job. Worker scraper/RAG hanya boleh menulis ke staging/result, lalu API yang memindahkan data yang sudah disetujui ke tabel resmi. Jadi desain relasinya dari awal sudah aman buat alur review.

---

## 9. Isi laporan progress untuk meeting

Versi laporan lisan sekitar satu menit:

> Progress minggu ini, tim sudah menyelesaikan spesifikasi teknis awal, SRS, POC scraper SINTA/Google Scholar, dan POC RAG dokumen. Setelah audit ulang terhadap kebutuhan proyek, kode POC, serta kemampuan tim, fondasi final yang dipilih adalah Next.js untuk frontend, Node.js 24 LTS dengan NestJS dan Express untuk backend API, Drizzle dan PostgreSQL untuk data, serta Python worker untuk scraper dan RAG. Seluruh proses otomatis masuk melalui job backend dan hasilnya berada di staging sampai diperiksa manusia. SRS akan diselaraskan dari Prisma ke Drizzle serta dilengkapi pada bagian keamanan, kualitas layanan, backup, observability, pengujian, job, dan lifecycle AI. Repo API masih berupa scaffold kosong sehingga penyesuaian dapat dilakukan sebelum implementasi dimulai.

Milestone yang dapat dilaporkan:

- konteks bisnis dan kebutuhan utama telah dipahami;
- stack final telah dipilih;
- SRS telah diaudit dan daftar revisi tersedia;
- POC scraper dan RAG berhasil divalidasi pada level kode;
- risiko produksi POC telah diidentifikasi;
- struktur frontend/API/worker/database telah ditetapkan;
- langkah berikutnya adalah revisi SRS, fondasi NestJS, skema Drizzle, dan CI awal.

---

## 10. Checklist sebelum SRS dibagikan Rabu, 22 Juli 2026

- [ ] nama proyek dan metadata lengkap;
- [ ] revision history diisi;
- [ ] Prisma diganti Drizzle;
- [ ] Node.js 24, NestJS 11, dan Express ditulis;
- [ ] Python worker dijelaskan;
- [ ] diagram arsitektur web/API/job/worker/database/storage ditambahkan;
- [ ] endpoint scraper/RAG melalui backend dijelaskan;
- [ ] SINTA ditulis best effort, bukan dijamin;
- [ ] Crossref ditambahkan;
- [ ] output POC dan target dipisahkan;
- [ ] model RAG POC dan target produksi dipisahkan;
- [ ] auth, session, permission, dan audit diisi;
- [ ] job, retry, lease, idempotency, dan dead-letter diisi;
- [ ] staging, review, dan provenance diisi;
- [ ] performance, security, reliability, availability, observability diisi;
- [ ] installation, build, delivery, cost, deadline, change management diisi;
- [ ] guardrails, ethics, human review, dan model lifecycle diisi;
- [ ] verification matrix diisi;
- [ ] placeholder, typo, dan catatan personal dibersihkan;
- [ ] main dan dev tidak lagi memiliki keputusan yang bertentangan;
- [ ] tidak ada secret, nomor pribadi, atau percakapan internal dalam repo publik.

---

## 11. Sumber keputusan

Dokumen rinci:

- local_context/05_ai_agent_outputs/2026-07-18_bht_nexus_complete_architecture_reassessment_and_final_decision.md
- local_context/05_ai_agent_outputs/2026-07-18_bht_nexus_zero_to_sustainable_release_team_guide.md
- implementation_guides/2026-07-18_01_bht_nexus_api_repository_foundation.md

Sumber proyek:

- transkrip rapat minggu pertama;
- chat WhatsApp sampai 18 Juli 2026;
- SRS branch main dan dev;
- repo scrapper-google-scholar-sinta;
- repo rag-document;
- repo bht-nexus-api;
- profil dan pengalaman publik anggota tim;
- laporan environment komputer pengembangan.

Dokumentasi resmi:

- [NestJS Modules](https://docs.nestjs.com/modules)
- [NestJS Providers](https://docs.nestjs.com/providers)
- [NestJS Performance](https://docs.nestjs.com/techniques/performance)
- [NestJS File Upload](https://docs.nestjs.com/techniques/file-upload)
- [NestJS OpenAPI](https://docs.nestjs.com/openapi/introduction)
- [Elysia Best Practice](https://elysiajs.com/essential/best-practice)
- [Drizzle PostgreSQL](https://orm.drizzle.team/docs/get-started-postgresql)
- [Drizzle Migrations](https://orm.drizzle.team/docs/migrations)
- [Node.js release schedule](https://nodejs.org/en/about/previous-releases)
- [PostgreSQL versioning](https://www.postgresql.org/support/versioning/)
- [pgvector](https://github.com/pgvector/pgvector)
- [multilingual MPNet model card](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2)
- [multilingual-e5-base model card](https://huggingface.co/intfloat/multilingual-e5-base)

---

## 12. Keputusan penutup

Keputusan sudah final untuk memulai implementasi:

> **Next.js + Node.js 24 LTS + NestJS 11 + Express + Drizzle + PostgreSQL, dengan Python worker untuk scraper dan RAG.**

Keputusan ini tidak memilih teknologi paling cepat dalam benchmark atau paling nyaman bagi satu orang. Keputusan ini mengambil titik tengah antara:

- target pengembangan yang cepat;
- kemampuan nyata tim;
- biaya belajar yang masih masuk akal;
- struktur yang aman untuk vibe coding;
- banyaknya fitur BHT-Nexus;
- kemudahan serah-terima;
- dan keberlanjutan sistem setelah periode proyek saat ini selesai.
