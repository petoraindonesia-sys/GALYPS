# Product Requirements Document — GALYPS

- **Nama:** GALYPS
- **Jenis:** Website katalog dan galeri bisnis gypsum ornamen, plafon, dan list.
- **Tujuan utama:** Membantu calon pembeli memahami pilihan produk dan melihat bukti hasil pemasangan, lalu menghubungi bisnis melalui WhatsApp.
- **Model:** Website informasi dan katalog; bukan transaksi online.

## 1. Pengguna dan kebutuhan

### Pengunjung
Pengunjung membutuhkan:
- menemukan kategori dan motif produk dengan cepat;
- melihat foto produk yang jelas;
- memahami ukuran, bahan, dan keterangan pemasangan;
- melihat contoh proyek yang pernah dikerjakan;
- menghubungi bisnis tanpa mengisi checkout atau membuat akun.

### Admin
Admin membutuhkan:
- login yang dibatasi pada email yang diizinkan;
- mengelola produk dan kategori;
- mengelola foto dan informasi galeri proyek;
- mengubah informasi kontak dan bisnis;
- mengerjakan semua aktivitas tersebut dari HP atau laptop.

## 2. Struktur dan kebutuhan halaman publik

### 2.1 Beranda
- Menampilkan foto proyek unggulan.
- Menampilkan pengantar singkat bisnis.
- Menampilkan ringkasan kategori Ornamen, Plafon, dan List.
- Menampilkan pilihan produk/proyek yang relevan.
- Menyediakan CTA WhatsApp yang menggunakan nomor dari pengaturan.
- Tidak mengisi halaman dengan paragraf panjang atau dekorasi yang mengalahkan foto.

### 2.2 Katalog
- Menampilkan kategori Ornamen, Plafon, dan List.
- Menampilkan thumbnail dan nama motif/produk.
- Pengunjung dapat berpindah kategori.
- Hanya produk berstatus publik/aktif yang terlihat.
- Urutan produk mengikuti urutan yang ditentukan admin.

### 2.3 Detail produk
Informasi minimum:
- nama produk;
- kategori;
- satu atau lebih foto;
- ukuran;
- bahan;
- keterangan pemasangan;
- tombol **Tanya produk ini**.

Tombol WhatsApp membuka percakapan ke nomor bisnis dengan pesan awal yang memuat nama produk. Pesan harus di-URL-encode dan nomor dinormalisasi dengan benar. Bila nomor belum dikonfigurasi, CTA tidak boleh menghasilkan tautan rusak; tampilkan keadaan kontak yang jelas.

### 2.4 Galeri
- Menampilkan foto hasil pemasangan.
- Setiap proyek dapat memiliki beberapa foto.
- Informasi proyek meliputi jenis proyek (misalnya rumah, ruko, kantor) dan kota.
- Admin dapat mengatur urutan dan status publikasi.

### 2.5 Tentang
- Menampilkan profil singkat bisnis.
- Konten harus dapat dikelola tanpa mengubah kode bila memungkinkan melalui pengaturan/admin content.

### 2.6 Kontak
- Menampilkan nomor/tautan WhatsApp, alamat, jam kerja, dan tautan peta.
- Tautan peta berasal dari pengaturan admin.
- Jangan menanamkan alamat atau nomor telepon contoh sebagai data produksi.

## 3. Kebutuhan admin

### 3.1 Akses admin
- Area admin dilindungi Cloudflare Access One-Time PIN.
- Hanya email dalam allowlist yang boleh masuk.
- Tidak membangun sistem password/PIN lokal tambahan kecuali kelak disetujui.
- Semua API admin memverifikasi identitas Access di sisi server.

### 3.2 Manajemen produk
Admin dapat:
- membuat produk;
- mengubah nama, kategori, ukuran, bahan, keterangan pemasangan;
- mengunggah beberapa foto;
- mengubah urutan foto dan produk;
- mengatur status draft/published;
- menghapus produk dengan konfirmasi.

### 3.3 Manajemen galeri
Admin dapat:
- membuat dan mengubah entri proyek;
- mengatur jenis proyek dan kota;
- mengunggah beberapa foto;
- mengatur urutan dan status publikasi;
- menghapus entri dengan konfirmasi.

### 3.4 Pengaturan umum
Admin dapat mengubah:
- nomor WhatsApp;
- alamat bisnis;
- jam kerja;
- tautan peta;
- profil singkat bisnis;
- informasi dasar website yang diperlukan.

### 3.5 Upload dari perangkat
- Form upload dapat digunakan dari kamera/file picker HP dan file picker laptop.
- Browser mengoptimalkan foto sebelum upload.
- Admin memperoleh indikator proses, keberhasilan, dan kegagalan.
- Upload gagal tidak boleh meninggalkan record publik yang rusak atau object tanpa penanganan.

## 4. Kebutuhan nonfungsional

- Responsif pada ponsel, tablet, dan desktop.
- Navigasi keyboard dan screen reader yang wajar.
- Foto memiliki alt text dan ukuran responsif.
- SEO dasar: title, description, canonical URL, Open Graph, sitemap, dan robots.
- Halaman publik cepat dan memanfaatkan cache Cloudflare.
- Admin dan API tidak boleh masuk cache publik.
- Error ditangani dengan pesan yang dapat dipahami; jangan membocorkan stack trace.
- Query D1 dibatasi, diindeks, dan dipantau terhadap kuota.
- Rahasia hanya berada di environment/secret Cloudflare atau secret lokal yang tidak dikomit.

## 5. Arah visual

- Terinspirasi prinsip visual Apple: minimal, rapi, fokus konten, tipografi jelas, ruang kosong cukup.
- Latar putih/off-white.
- Foto produk dan proyek sebagai fokus utama.
- Teks singkat dan hierarki visual yang konsisten.
- System font stack, bukan SF Pro.
- Tidak menggunakan logo/nama/aset merek Apple.
- Tombol utama `#0071e3`.
- Animasi, jika ada, ringan dan menghormati reduced motion.

## 6. Acceptance criteria tingkat produk

1. Pengunjung dapat membuka semua halaman publik dan berpindah antarhalaman.
2. Katalog menampilkan produk berdasarkan tiga kategori yang ditentukan.
3. Detail produk menampilkan seluruh informasi dan foto yang tersedia.
4. CTA detail menghasilkan pesan WhatsApp yang memuat nama produk.
5. Galeri menampilkan foto, jenis proyek, dan kota.
6. Kontak menampilkan data dari pengaturan, bukan hardcode.
7. Email di luar allowlist tidak dapat mengakses admin.
8. Admin yang lolos Access dapat mengelola produk, galeri, dan pengaturan.
9. Foto dari HP/laptop dapat dioptimalkan, diunggah, disimpan di R2, dan ditampilkan kembali.
10. Data produk/galeri/pengaturan tersimpan dan dibaca dari D1.
11. Perubahan konten memicu strategi invalidasi cache yang telah diuji.
12. Tidak ada fitur e-commerce atau pembayaran online.

## 7. Definisi selesai

Fitur hanya boleh disebut selesai jika UI, API, autentikasi/otorisasi, persistensi D1/R2, validasi, error state, dan pengujian relevan telah diverifikasi. Build sukses saja tidak cukup.

## 8. Hal yang menunggu keputusan

Detail yang belum ditentukan dicatat sebagai `PENDING` di [DECISIONS.md](./DECISIONS.md), termasuk identitas bisnis, konten awal final, domain, batas ukuran upload, dan kebijakan retensi foto.
