# Operasi Konten — GALYPS

## 1. Alur kerja admin

1. Admin membuka halaman admin yang dilindungi Cloudflare Access.
2. Admin memilih Produk, Galeri, atau Pengaturan.
3. Admin membuat/mengubah data dan mengisi field wajib.
4. Foto dipilih dari kamera/file picker HP atau file picker laptop.
5. Browser mengoptimalkan foto dan menampilkan preview.
6. Worker memvalidasi file dan menyimpan object ke R2.
7. Metadata dan relasi disimpan di D1.
8. Cache publik diinvalidasi sesuai strategi yang aktif.
9. UI menampilkan status yang sesuai dengan hasil setiap langkah.

## 2. Optimasi foto di browser

- Resize berdasarkan batas dimensi yang ditetapkan pada keputusan implementasi.
- Kompresi sesuai format yang didukung dan kualitas visual.
- Pertahankan rasio aspek; jangan memotong otomatis tanpa kontrol admin.
- Sediakan preview sebelum upload.
- Tampilkan ukuran file sebelum/sesudah optimasi.
- Jangan menganggap optimasi browser sebagai kontrol keamanan; Worker tetap memvalidasi payload.

Batas dimensi, ukuran maksimum, format final, dan kualitas kompresi masih `PENDING` dan harus diuji dengan foto nyata dari HP/laptop.

## 3. Penyimpanan R2

- Gunakan bucket privat atau publik melalui jalur delivery yang diputuskan secara sadar.
- Jika foto harus publik, hanya object foto yang memang dipublikasikan yang boleh diakses.
- Object key dibuat server-side dan tidak memuat nama asli file atau informasi pribadi.
- Metadata file disimpan di D1.
- Hapus object hanya setelah memastikan tidak ada referensi aktif.
- R2 binding dan bucket name berasal dari konfigurasi environment, bukan hardcode di source.

## 4. Perubahan konten dan konsistensi

Alur simpan yang disarankan:
1. Validasi seluruh field dan file.
2. Upload object baru ke R2 bila diperlukan.
3. Simpan/update metadata D1.
4. Pastikan record publik hanya menunjuk object yang valid.
5. Invalidasi cache halaman/data publik yang terdampak.
6. Bersihkan object lama setelah referensinya tidak lagi digunakan.

Karena D1 dan R2 tidak berbagi transaksi atomik, implementasi harus menangani kegagalan parsial melalui kompensasi/retry dan status yang jelas. Jangan melaporkan operasi sepenuhnya gagal jika data telah tersimpan; berikan status parsial dan mekanisme pemulihan.

## 5. Cache publik

- Cache hanya halaman dan data publik.
- Admin, API admin, draft, dan respons yang berisi data sensitif tidak boleh masuk cache publik.
- Cache harus memiliki TTL sebagai fallback.
- Setelah perubahan konten, invalidasi harus menjangkau halaman yang relevan (beranda, katalog/kategori, detail produk, galeri, kontak bila pengaturan berubah).
- Kegagalan purge harus dapat dideteksi, dicatat tanpa data sensitif, dan dapat di-retry.
- Jangan mengasumsikan Cache API Worker sama dengan purge global CDN; pilih mekanisme yang benar dan uji terhadap URL publik.

Strategi final (Cache API, cache tag, purge by URL melalui API, atau kombinasi) harus dikunci setelah domain/zone dan kemampuan plan diverifikasi. Jangan menaruh token purge di browser.

## 6. Hemat pembacaan D1

- Ambil hanya kolom yang diperlukan.
- Gabungkan query terkait; hindari N+1.
- Cache hasil pembacaan publik yang stabil.
- Jangan polling data konten.
- Gunakan pagination di admin.
- Hindari refresh penuh setelah setiap field berubah.
- Ukur query per request dan pantau metrik pemakaian D1.

## 7. Status publikasi

- Draft tidak terlihat oleh pengunjung dan mesin pencari.
- Published tampil di halaman publik dan sitemap bila memenuhi syarat.
- Penghapusan produk/proyek harus menangani URL lama secara konsisten (404 atau redirect sesuai keputusan SEO).
- Perubahan nomor WhatsApp/alamat/jam kerja harus terlihat setelah invalidasi cache.
