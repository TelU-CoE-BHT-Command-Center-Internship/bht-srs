# Panduan Kontribusi BHT-Nexus SRS

Terima kasih telah membantu menyempurnakan dokumentasi kebutuhan BHT-Nexus. Repository ini berisi SRS (*Software Requirements Specification*), panduan penulisan, catatan keputusan teknis, dan PDF yang dibagikan untuk proses review.

## Dokumen utama

| Berkas | Kegunaan |
| --- | --- |
| `bht-nexus-srs.md` | Sumber utama SRS BHT-Nexus yang dapat diperbarui melalui pull request. |
| `output/pdf/bht-nexus-srs-v1.2.0.pdf` | Versi PDF yang siap dibaca dan dibagikan. |
| `srs-guide.md` | Panduan isi untuk setiap bagian SRS. |
| `prompt-srs-guide.md` | Format penulisan setiap kebutuhan agar dapat diuji. |
| `eyd.md` | Acuan ejaan dan penulisan bahasa Indonesia. |
| `2026-07-12_bht_nexus_team_building_guide.md` | Panduan awal mengenai konteks produk dan arah teknis tim. |
| `2026-07-18_bht_nexus_final_stack_srs_revision_and_whatsapp_tldr(1).md` | Catatan penilaian stack dan kebutuhan revisi SRS pada 18 Juli 2026. |

## Sebelum mengusulkan perubahan

1. Baca bagian SRS yang akan diubah beserta panduan yang berkaitan.
2. Periksa issue dan pull request agar pekerjaan tidak dibuat dua kali.
3. Pertahankan isi yang masih benar dan jelaskan alasan setiap perubahan substantif.
4. Pastikan istilah teknis langsung diberi penjelasan singkat apabila pembaca nonteknis mungkin belum mengenalnya.
5. Jangan memasukkan percakapan pribadi, jalur folder komputer, token, kata sandi, berkas `.env`, atau data internal yang tidak diperlukan.

## Alur kontribusi

```text
Issue atau kebutuhan perubahan
            ↓
Branch kerja yang menjelaskan tujuan
            ↓
Perubahan Markdown dan PDF terkait
            ↓
Pemeriksaan isi dan tampilan
            ↓
Pull request
            ↓
Review dan persetujuan tim
            ↓
Merge ke main
```

Gunakan nama branch yang langsung menjelaskan pekerjaannya, misalnya:

- `docs/srs-v1.2.0` untuk revisi dokumen;
- `fix/requirement-traceability` untuk memperbaiki keterlacakan kebutuhan; atau
- `chore/repository-hygiene` untuk perawatan repository.

Commit mengikuti pola *Conventional Commits*:

```text
docs(srs): lengkapi kebutuhan dan verifikasi BHT-Nexus
fix(diagram): perjelas alur pemeriksaan data
chore(repo): rapikan berkas dokumentasi
```

## Standar perubahan SRS

Setiap kebutuhan pada Bab 3 harus memiliki:

- ID yang unik;
- judul;
- pernyataan kebutuhan;
- alasan;
- kriteria penerimaan;
- metode verifikasi; dan
- informasi tambahan apabila diperlukan.

Setiap ID kebutuhan juga harus memiliki satu baris yang sesuai pada matriks verifikasi. Perubahan arsitektur, kontrak API, skema data, keamanan, atau ruang lingkup perlu menyertakan sumber keputusan yang dapat ditinjau tim.

## Pemeriksaan PDF

Jika perubahan Markdown memengaruhi isi dokumen, PDF harus dibuat ulang. Sebelum diajukan untuk review, periksa bahwa:

- jumlah dan urutan halaman benar;
- judul, versi, penulis revisi, serta tanggal sesuai;
- diagram, tabel, dan daftar tetap terbaca;
- tidak ada tulisan terpotong, bertumpuk, atau keluar dari halaman;
- tidak ada halaman kosong yang tidak disengaja; dan
- isi PDF sama dengan SRS Markdown terbaru.

## Batas repository publik

Repository ini dapat dibaca pihak di luar tim. Hanya masukkan dokumen yang memang layak dibagikan. Catatan kerja pribadi, hasil percobaan sementara, rekaman komunikasi, dan panduan yang hanya relevan untuk komputer tertentu tetap disimpan di ruang kerja internal.
