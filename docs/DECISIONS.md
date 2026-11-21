# Decision Register — GALYPS

Dokumen ini mencatat keputusan produk/teknis yang telah disampaikan pemilik proyek. Hal yang belum ditetapkan tetap `PENDING`; agen tidak boleh mengarang nilainya.

## Keputusan disetujui

| ID | Area | Keputusan |
|---|---|---|
| GAL-001 | Tujuan produk | Website katalog dan galeri bisnis gypsum ornamen, plafon, dan list |
| GAL-002 | Model bisnis | Katalog/informasi; tidak ada keranjang, checkout, atau pembayaran online |
| GAL-003 | Konversi | Calon pembeli diarahkan untuk bertanya melalui WhatsApp |
| GAL-004 | Kategori | Ornamen, Plafon, dan List |
| GAL-005 | Halaman publik | Beranda, Katalog, Detail Produk, Galeri, Tentang, Kontak |
| GAL-006 | Admin | CRUD produk, galeri, dan pengaturan umum |
| GAL-007 | Desain | Gaya minimal terinspirasi Apple; putih/off-white, whitespace, foto dominan, teks singkat |
| GAL-008 | Tipografi | System font stack; bukan SF Pro |
| GAL-009 | Merek | Tidak menggunakan logo atau nama merek Apple |
| GAL-010 | Warna utama | Tombol utama `#0071e3` |
| GAL-011 | Frontend | Astro + adapter Cloudflare untuk route on-demand |
| GAL-012 | Hosting (keputusan awal, superseded) | Cloudflare Pages; digantikan oleh GAL-022 setelah audit kompatibilitas |
| GAL-013 | Database | Cloudflare D1 (SQLite) |
| GAL-014 | Foto | Cloudflare R2 |
| GAL-015 | API | Cloudflare Workers |
| GAL-016 | Login admin | Cloudflare Access One-Time PIN dengan email allowlist |
| GAL-017 | Development | GitHub Codespaces |
| GAL-018 | Peran GitHub | Source control dan dokumentasi saja; bukan deployment atau CMS |
| GAL-019 | Optimasi foto | Foto dikecilkan/dioptimalkan di browser sebelum upload |
| GAL-020 | Cache | Halaman publik di-cache; perubahan admin harus menginvalidasi cache agar konten segar |
| GAL-021 | Biaya | Mengutamakan paket gratis dan menjaga pembacaan D1 di dalam kuota Free |
| GAL-022 | Hosting (disetujui) | Cloudflare Workers + Static Assets; gunakan `@astrojs/cloudflare` hanya untuk route on-demand/SSR. Deployment memakai Wrangler; tidak membuat resource/deployment tanpa persetujuan eksplisit. |

## Pending — perlu konfirmasi pemilik

| ID | Area | Pertanyaan/keputusan yang dibutuhkan | Status |
|---|---|---|---|
| GAL-P01 | Identitas bisnis | Nama bisnis, profil final, alamat, nomor WhatsApp | PENDING |
| GAL-P02 | Konten awal | Foto produk, motif, spesifikasi, foto proyek, kota dan jenis proyek | PENDING |
| GAL-P03 | Admin allowlist | Daftar email admin yang boleh mengakses Cloudflare Access | PENDING |
| GAL-P04 | Domain | Domain produksi dan subdomain admin/API, jika dipisah | PENDING |
| GAL-P05 | Infrastruktur | Nama Worker, D1 database, R2 bucket, dan route | PENDING |
| GAL-P06 | Upload | Format yang diizinkan, ukuran maksimum, dimensi maksimum, kualitas kompresi | PENDING |
| GAL-P07 | Foto | Kebijakan foto utama, urutan, retensi, dan penghapusan | PENDING |
| GAL-P08 | Cache | Mekanisme final invalidasi dan ketersediaan purge API pada konfigurasi akun | PENDING |
| GAL-P09 | URL | Pola slug dan kebijakan URL lama ketika produk dihapus/diubah | PENDING |
| GAL-P10 | Jam kerja | Format jam kerja dan dukungan beberapa rentang per hari | PENDING |
| GAL-P11 | Profil bisnis | Apakah konten Tentang dikelola sebagai field pengaturan atau halaman konten terpisah | PENDING |
| GAL-P12 | Batas paket | Verifikasi limit Free aktual untuk Workers, D1, R2, dan Access saat setup; kelayakan akun/resource tetap perlu dicek | PENDING |

## Riwayat keputusan hosting

- GAL-P13 (sebelumnya PENDING): diselesaikan melalui persetujuan Owner "lanjutkan" pada 21 Nov 2026. Hasilnya dicatat sebagai GAL-022: Cloudflare Workers + Static Assets. Tidak ada keputusan hosting Pages yang masih terbuka.

## Aturan perubahan

- Keputusan disetujui tidak boleh diubah diam-diam.
- Usulan perubahan dicatat dengan alasan, dampak, dan status menunggu persetujuan.
- Keputusan baru diberi ID baru; riwayat lama dipertahankan.
- Dokumentasi lain harus diperbarui setelah keputusan berubah.
