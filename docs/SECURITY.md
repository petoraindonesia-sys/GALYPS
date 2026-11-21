# Keamanan — GALYPS

## 1. Autentikasi admin

- Gunakan Cloudflare Access One-Time PIN.
- Akses admin dibatasi pada allowlist email yang disetujui pemilik proyek.
- Jangan membuat login password/PIN aplikasi tambahan.
- Konfigurasi Access harus mencakup halaman admin dan endpoint API admin.
- Domain dan Access Application Audience (AUD) ditetapkan saat konfigurasi lingkungan.

## 2. Verifikasi identitas pada Worker

Setiap endpoint admin harus memverifikasi assertion dari Cloudflare Access di server:
- token tersedia dan berasal dari header yang diharapkan;
- signature diverifikasi menggunakan key/JWKS resmi Cloudflare;
- issuer sesuai team/domain Access;
- audience cocok dengan Access Application yang benar;
- token belum kedaluwarsa dan klaim waktu valid;
- email/identitas memenuhi kebijakan allowlist yang aktif.

Jangan hanya membaca header email yang dapat dipalsukan. Jangan menerima token dari body atau query string. Cache JWKS dengan aman dan tangani rotasi key.

## 3. Otorisasi endpoint

- Terapkan middleware/helper otorisasi bersama, tetapi setiap route harus tetap menggunakannya secara eksplisit.
- Endpoint baca admin juga memerlukan autentikasi.
- Mutasi memerlukan metode HTTP yang tepat dan validasi schema.
- Batasi operasi hanya pada resource yang dikelola aplikasi.
- Gunakan status code konsisten: 400 validasi, 401 tidak terautentikasi, 403 tidak diizinkan, 404 tidak ditemukan, 409 konflik, 413 payload terlalu besar, 429 rate limit bila tersedia.

## 4. Validasi input dan query

- Validasi body, query, params, dan content type di Worker.
- Terapkan panjang maksimum string, enum, batas array, dan format URL.
- Gunakan prepared statements/bind parameters D1.
- Hindari menyusun SQL dari string input pengguna.
- Validasi slug dan cegah path traversal pada object key.
- Jangan menerima nama tabel/kolom dari request.

## 5. Keamanan upload

- Batasi ukuran file sebelum membaca seluruh payload dan tetapkan batas final melalui keputusan pemilik.
- Validasi magic bytes/content, bukan hanya ekstensi atau MIME dari browser.
- Izinkan hanya format gambar yang didukung implementasi; format final harus diuji pada browser target.
- Tolak SVG atau format aktif lain kecuali ada sanitasi yang teruji dan alasan produk.
- Batasi dimensi/pixel untuk mencegah decompression bomb.
- Buat object key acak dan server-generated.
- Jangan menggunakan nama file asli sebagai path.
- Terapkan rate limit/kuota upload yang sesuai kemampuan layanan.
- Pastikan upload gagal tidak membuat record published tanpa object valid.

## 6. Browser dan HTTP

- Gunakan HTTPS.
- Terapkan Content Security Policy dan header keamanan yang sesuai.
- Hindari inline script yang tidak diperlukan.
- Proteksi CSRF jika autentikasi browser menggunakan cookie yang memungkinkan request lintas situs; evaluasi Origin/Referer dan SameSite sesuai desain.
- Batasi CORS ke origin aplikasi admin yang benar; jangan gunakan wildcard dengan credential.
- Endpoint API mengirim `Cache-Control: no-store` untuk respons admin.
- Jangan mengekspos binding D1/R2 atau secret ke client bundle.

## 7. Secret dan konfigurasi

- Secret disimpan melalui Cloudflare secret bindings dan secret lokal yang dikecualikan dari Git.
- Sediakan `.dev.vars.example` tanpa nilai credential bila diperlukan.
- Jangan commit `.dev.vars`, token API, JWT, private key, atau file konfigurasi yang berisi secret.
- Gunakan hak akses minimum pada API token Cloudflare.
- Pisahkan konfigurasi development dan production.

## 8. Data dan log

- Jangan mengumpulkan data pengunjung yang tidak diperlukan.
- Jangan log token, header autentikasi, nomor telepon, isi pesan WhatsApp, atau konten form.
- Hindari menampilkan stack trace, query SQL, atau detail internal pada UI.
- Tentukan kebijakan backup dan retensi sebelum data produksi digunakan.

## 9. Verifikasi keamanan wajib

- Email tidak di-allowlist ditolak.
- JWT invalid, expired, wrong issuer, atau wrong audience ditolak.
- Request tanpa Access assertion ditolak.
- Semua endpoint admin terlindungi, termasuk endpoint upload dan delete.
- SQL injection payload tidak mengubah struktur query.
- Upload format palsu, terlalu besar, dan dimensi ekstrem ditolak.
- Admin/API tidak tersimpan di cache publik.
- Uji akses langsung ke URL API, bukan hanya navigasi UI.
