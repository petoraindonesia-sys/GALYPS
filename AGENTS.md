# AGENTS.md — GALYPS

Dokumen ini adalah kontrak kerja otoritatif untuk semua agen dan kontributor di repository GALYPS. Jika instruksi lain bertentangan, ikuti keputusan produk yang tercatat di `docs/DECISIONS.md`; konflik yang belum dapat diselesaikan harus ditandai dan dimintakan keputusan pemilik proyek.

## 1. Misi produk

Bangun website katalog dan galeri bisnis gypsum ornamen, plafon, dan list. Website menampilkan produk dan hasil pemasangan, lalu mengarahkan calon pembeli ke WhatsApp. GALYPS bukan platform transaksi online.

## 2. Teknologi yang diizinkan

- Astro dengan adapter Cloudflare untuk route on-demand/SSR yang memerlukan runtime.
- Tailwind CSS untuk styling.
- Cloudflare Workers + Static Assets sebagai hosting website dan runtime.
- Cloudflare D1 (SQLite) untuk produk, galeri, dan pengaturan.
- Cloudflare R2 untuk foto.
- Cloudflare Worker API untuk admin CRUD, upload, dan validasi request.
- Cloudflare Access One-Time PIN untuk autentikasi admin.
- GitHub Codespaces sebagai lingkungan pengembangan.

Jangan menggunakan Cloudflare Pages untuk target deployment GALYPS. Jangan mengganti stack, menambahkan backend/database eksternal, atau menghubungkan platform deployment lain tanpa persetujuan pemilik proyek. GitHub hanya untuk version control dan dokumentasi; jangan menggunakannya sebagai CMS atau target deployment.

Astro yang sepenuhnya pre-rendered tidak memerlukan adapter Cloudflare. Gunakan adapter resmi `@astrojs/cloudflare` ketika route memerlukan on-demand rendering atau binding runtime. Pilihan prerender vs SSR harus mengikuti kebutuhan data nyata dan diuji terhadap kuota Workers Free.

## 3. Batasan produk yang wajib dipertahankan

- Halaman publik: Beranda, Katalog, Detail Produk, Galeri, Tentang, Kontak.
- Kategori katalog: Ornamen, Plafon, List.
- Detail produk menampilkan foto, ukuran, bahan, keterangan pemasangan, dan tombol WhatsApp dengan nama produk terisi.
- Admin mengelola produk, galeri, dan pengaturan umum.
- Tidak membuat keranjang, checkout, pembayaran, atau alur e-commerce.
- Nomor WhatsApp, alamat, jam kerja, dan tautan peta dikelola melalui pengaturan admin.

## 4. Prinsip desain

- Arah visual mengikuti karakter desain Apple: bersih, tenang, lapang, dan berorientasi pada konten.
- Gunakan latar putih atau off-white, ruang kosong yang cukup, foto sebagai fokus, dan teks singkat.
- Gunakan system font stack; jangan menggunakan atau mengunduh SF Pro.
- Jangan menggunakan logo, ikon, nama, atau aset merek Apple.
- Warna tombol utama: `#0071e3`.
- Pastikan responsif dan dapat digunakan melalui HP maupun laptop.
- Aksesibilitas bukan opsional: semantic HTML, label form, focus state, kontras, alt text, dan navigasi keyboard.

## 5. Aturan implementasi

- Baca PRD, arsitektur, model data, keamanan, operasi konten, keputusan, dan progres sebelum mengubah codebase.
- Pilih solusi paling sederhana yang memenuhi kontrak.
- Jangan membuat mock data sebagai pengganti integrasi yang diminta. Seed SQL hanya untuk data awal pengembangan/demo yang diberi label jelas.
- Jangan meninggalkan TODO, stub, tombol mati, form palsu, endpoint dummy, atau fitur setengah jadi pada pekerjaan yang diklaim selesai.
- Validasi input di server meskipun browser juga melakukan validasi.
- Gunakan prepared statements/bind parameters untuk query D1.
- Periksa otorisasi pada setiap endpoint admin; menyembunyikan tombol di UI bukan kontrol keamanan.
- Jangan pernah mengirim credential atau secret ke browser.
- Jangan melakukan operasi destructive pada database, R2, konfigurasi Cloudflare, atau branch utama tanpa persetujuan eksplisit.
- Jangan menambahkan analytics, cookie banner, third-party trackers, atau integrasi pesan otomatis tanpa kebutuhan dan persetujuan.

## 6. Aturan foto dan konten

- Foto dioptimalkan di browser sebelum upload, tetapi Worker tetap memvalidasi tipe file, ukuran, dan konten aktual.
- Simpan file di R2; simpan metadata dan object key di D1.
- Gunakan nama object key yang aman dan tidak bergantung pada nama file asli.
- Jangan mengandalkan ekstensi atau header Content-Type dari browser sebagai validasi tunggal.
- Sediakan alt text yang bermakna untuk foto publik.
- Hapus object R2 yang tidak lagi direferensikan hanya setelah memastikan tidak ada relasi yang masih menggunakannya.

## 7. Kuota dan performa

- Rancang query D1 agar terindeks dan hemat pembacaan.
- Hindari N+1 query dan pembacaan berulang untuk data pengaturan yang jarang berubah.
- Gunakan pagination atau batas jumlah item pada daftar admin.
- Cache halaman publik dan aset secara terukur; invalidasi cache harus mengikuti prosedur terdokumentasi.
- Jangan mengklaim kepatuhan kuota Free tanpa pengukuran dan pemeriksaan limit resmi yang berlaku saat implementasi.
- Perhatikan batas CPU Workers Free; SSR yang melebihi CPU budget harus dioptimalkan atau dialihkan ke prerender/cache yang aman.

## 8. Keamanan

- Admin dilindungi Cloudflare Access One-Time PIN dan allowlist email.
- Worker API harus memverifikasi identitas Access secara server-side, termasuk JWT assertion, issuer, audience, signature, dan expiry sesuai mekanisme resmi Cloudflare.
- Semua endpoint tulis wajib memeriksa identitas dan metode HTTP.
- Terapkan validasi schema, batas payload, proteksi CSRF bila menggunakan cookie/session browser, serta pembatasan upload.
- Jangan menulis PII, secret, token, atau isi header autentikasi ke log.
- Detail kontrol keamanan ada di `docs/SECURITY.md`.

## 9. Disiplin progres wajib

Sebelum pekerjaan: periksa repository dan `progres.md`, lalu tambahkan entri `IN_PROGRESS` berisi tujuan, scope, dan acceptance criteria.

Selama pekerjaan: tambahkan catatan baru jika scope, risiko, atau blocker berubah. Jangan menimpa riwayat.

Sesudah pekerjaan: perbarui entri dengan file berubah, pengujian yang benar-benar dijalankan, hasil, commit SHA, PR/CI bila ada, dan status akhir.

Status yang diperbolehkan: `NOT_STARTED`, `IN_PROGRESS`, `IMPLEMENTED`, `VERIFIED`, `BLOCKED`. Status `VERIFIED` wajib didukung bukti pengujian/CI. Catat `UNMERGED` sampai perubahan benar-benar masuk ke `main`.

## 10. Pelaporan

Laporan harus sama persis dengan fakta di repository dan `progres.md`. Bedakan dengan jelas:
- dibuat vs diuji;
- build berhasil vs fitur terintegrasi;
- lokal vs deployed;
- branch kerja vs sudah merged;
- verified vs belum diverifikasi.

Jangan mengarang commit, PR, hasil test, deployment, atau konfigurasi akun.
