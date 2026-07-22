# BHT-Nexus Software Requirements Specification

Repository ini menyimpan dokumen kebutuhan resmi BHT-Nexus, yaitu sistem yang disiapkan untuk membantu CoE BHT mengelola anggota, aktivitas, publikasi, dokumen, KPI, pelaporan, serta proses otomatisasi scraper dan RAG.

## Mulai membaca

- [SRS BHT-Nexus v1.2.1](bht-nexus-srs.md) — sumber utama yang digunakan dalam proses review.
- [PDF SRS BHT-Nexus v1.2.1](output/pdf/bht-nexus-srs-v1.2.1.pdf) — versi siap baca dan siap dibagikan.
- [Panduan kontribusi](CONTRIBUTING.md) — cara mengusulkan perubahan tanpa menimpa pekerjaan tim.

Status versi 1.2.1 adalah **menunggu review dan persetujuan tim**. Versi ini mempertahankan isi kebutuhan yang telah direview, kemudian menyesuaikan susunan dokumen dan cover berdasarkan masukan supervisor serta template SRS yang disepakati tim.

## Isi SRS

SRS menjelaskan:

- tujuan, ruang lingkup, dan batas BHT-Nexus;
- pihak yang menggunakan sistem serta kebutuhan organisasinya;
- fungsi pengelolaan data, scraper, dokumen, RAG, KPI, audit, dan pelaporan;
- kebutuhan keamanan, keandalan, ketersediaan, deployment, dan operasi;
- kriteria penerimaan untuk setiap kebutuhan; dan
- matriks verifikasi yang menghubungkan kebutuhan dengan bukti pengujian.

## Prinsip utama

```text
Data masuk
    ↓
Diproses sebagai kandidat
    ↓
Diperiksa pengguna berwenang
    ↓
Baru menjadi data resmi
```

Hasil scraper, impor, ekstraksi dokumen, maupun proses otomatis lainnya tidak boleh langsung mengubah data resmi tanpa pemeriksaan manusia.

## Repository terkait

- [BHT-Nexus API](https://github.com/TelU-CoE-BHT-Command-Center-Internship/bht-nexus-api)
- [POC scraper Google Scholar dan SINTA](https://github.com/TelU-CoE-BHT-Command-Center-Internship/scrapper-google-scholar-sinta)
- [POC RAG Document](https://github.com/TelU-CoE-BHT-Command-Center-Internship/rag-document)

POC (*proof of concept*, yaitu percobaan untuk membuktikan kemampuan dasar) digunakan sebagai referensi awal dan tidak otomatis dianggap siap produksi.

## Memberikan masukan

Masukan dapat diberikan melalui issue atau komentar pada pull request yang sedang aktif. Jelaskan bagian yang dibahas, alasan perubahan, dan hasil yang diharapkan agar keputusan dapat ditelusuri oleh seluruh tim.
