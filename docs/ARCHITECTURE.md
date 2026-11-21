# Arsitektur Teknis — GALYPS

## 1. Prinsip

- Seluruh runtime aplikasi menggunakan layanan Cloudflare yang ditetapkan.
- Astro menangani website publik dan rendering sesuai kebutuhan.
- Cloudflare Workers + Static Assets menjadi hosting dan runtime website.
- D1 menyimpan data terstruktur; R2 menyimpan file foto.
- Worker server-side menjadi batas keamanan untuk seluruh operasi admin dan upload.
- Cloudflare Access melindungi admin; Worker tetap memverifikasi identitas secara independen.
- GitHub adalah source control, bukan sistem deployment atau CMS.

## 2. Komponen

| Komponen | Tanggung jawab |
|---|---|
| Astro | Routing, rendering halaman publik, SEO, komponen UI |
| `@astrojs/cloudflare` | Adapter untuk route on-demand/SSR dan akses binding runtime |
| Tailwind CSS | Utility styling dan responsive design |
| Cloudflare Workers + Static Assets | Hosting website publik, aset statis, dan runtime server |
| Cloudflare D1 | Produk, kategori, galeri, foto metadata, pengaturan |
| Cloudflare R2 | File foto asli/hasil optimasi yang dipilih untuk disimpan |
| Worker API routes | Admin CRUD, validasi, upload/delete object, cache invalidation |
| Cloudflare Access | One-Time PIN, email allowlist, proteksi area admin |
| GitHub Codespaces | Lingkungan development |

## 3. Strategi rendering

- Gunakan prerender untuk halaman/komponen yang datanya dapat dipaketkan pada saat build dan tidak membutuhkan data terbaru per request.
- Gunakan on-demand rendering melalui adapter Cloudflare untuk route yang memerlukan pembacaan D1 atau data runtime.
- Hindari SSR tanpa kebutuhan karena CPU Workers Free dibatasi per request.
- Jangan mengasumsikan seluruh halaman harus SSR. Uji strategi rendering setelah pola query D1 dan cache ditetapkan.
- Astro static-only tidak memerlukan adapter Cloudflare; konfigurasi Workers Static Assets tetap diperlukan untuk melayani hasil build.

## 4. Gambaran request

### Pengunjung publik

1. Browser meminta halaman atau aset ke Cloudflare Worker.
2. Static Assets melayani file build yang cocok.
3. Untuk route on-demand, Worker menjalankan Astro dan membaca data yang diperlukan dari D1 melalui binding.
4. Foto disajikan melalui URL publik R2/custom delivery path yang ditentukan.
5. Respons halaman/data publik di-cache sesuai kebijakan cache.
6. Halaman publik tidak memerlukan login.

### Admin

1. Admin membuka area admin yang dilindungi Cloudflare Access.
2. Access meminta One-Time PIN dan memeriksa email terhadap allowlist.
3. Setelah lolos, browser mengakses UI admin.
4. Browser mengirim request ke endpoint API Worker.
5. Worker memverifikasi JWT Access, audience, issuer, signature, expiry, dan kebijakan email.
6. Worker memvalidasi input dan menjalankan operasi D1/R2.
7. Worker menginvalidasi cache publik setelah perubahan yang berhasil.
8. Worker mengembalikan respons aman tanpa membocorkan detail internal.

## 5. Batas kepercayaan

- UI admin bukan batas keamanan.
- Cloudflare Access melindungi rute, tetapi Worker tetap harus memverifikasi assertion pada setiap request admin.
- D1 dan R2 hanya diakses melalui binding server-side; credential binding tidak boleh diekspos ke browser.
- Data publik hanya berasal dari record yang berstatus published/active.
- Endpoint admin dan respons personal tidak boleh di-cache oleh cache publik.
- Jangan menerima nama object key bebas dari browser untuk operasi delete.

## 6. Batas deployment

- Website dan API: Cloudflare Workers.
- Aset frontend: Workers Static Assets.
- Database: Cloudflare D1.
- Foto: Cloudflare R2.
- Domain, zone, Worker name, dan binding aktual perlu dikonfirmasi sebelum konfigurasi deployment.
- Tidak membuat project Cloudflare, resource, domain, token, atau deployment tanpa otorisasi dan akses yang tersedia.
- Deployment manual dari lingkungan pengembangan menggunakan Wrangler; GitHub Actions bukan target deployment dalam kontrak awal.

## 7. Cache dan konsistensi

- Cache hanya untuk halaman/data publik.
- Admin/API dikecualikan dari cache.
- Perubahan produk, galeri, dan pengaturan harus memperbarui konten publik.
- Strategi purge/invalidation harus aman dari race condition dan kegagalan parsial.
- Operasi penyimpanan harus memperhatikan urutan: validasi -> simpan object bila ada -> simpan metadata D1 -> invalidasi cache -> respons.
- Jika langkah invalidasi gagal setelah data tersimpan, sistem harus mencatat kegagalan dan menyediakan retry/strategi fallback; jangan mengembalikan pesan seolah seluruh operasi gagal tanpa menjelaskan status persistensi.
- Detail implementasi cache dan keputusan yang masih terbuka tercatat di `CONTENT-OPERATIONS.md` dan `DECISIONS.md`.

## 8. D1 dan kuota

- Gunakan SELECT kolom eksplisit, bukan SELECT * untuk query produksi.
- Tambahkan indeks sesuai pola filter dan sort.
- Gunakan pagination untuk daftar admin.
- Hindari query berulang per item.
- Hindari polling dan refresh otomatis yang tidak diperlukan.
- Evaluasi jumlah pembacaan pada halaman publik serta manfaat cache.
- Kuota dan limit layanan dapat berubah; verifikasi dokumentasi/akun Cloudflare pada saat implementasi.

## 9. Observability

- Log hanya metadata operasional yang tidak sensitif: request ID, route, hasil, durasi, dan kategori error.
- Jangan log Access JWT, email lengkap bila tidak diperlukan, nomor WhatsApp, isi form, atau data sensitif.
- Sediakan pesan error generik untuk pengguna dan detail teknis terbatas untuk log.
- Catat kegagalan upload, database, dan invalidasi cache.
