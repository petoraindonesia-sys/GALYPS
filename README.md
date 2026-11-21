# GALYPS

Website katalog dan galeri untuk bisnis gypsum ornamen, plafon, dan list.

GALYPS membantu calon pembeli melihat katalog produk dan dokumentasi hasil pemasangan, kemudian menghubungi bisnis melalui WhatsApp. Website ini bukan toko online.

## Status proyek

Tahap saat ini: **Project Documentation & Foundation**. Repository sedang disiapkan; codebase aplikasi belum dimulai. Lihat [progres.md](./progres.md) untuk status faktual dan bukti perubahan.

## Cakupan produk

### Website publik
- Beranda dengan proyek unggulan, ringkasan kategori, dan CTA WhatsApp.
- Katalog produk berdasarkan kategori Ornamen, Plafon, dan List.
- Detail produk: foto, ukuran, bahan, keterangan pemasangan, dan CTA WhatsApp dengan nama produk terisi.
- Galeri proyek di rumah, ruko, atau kantor, dilengkapi jenis proyek dan kota.
- Tentang dan kontak: profil, alamat, jam kerja, serta tautan peta.

### Admin
- Mengelola produk, galeri, dan pengaturan umum.
- Mengunggah foto dari HP atau laptop.
- Mengoptimalkan foto di browser sebelum upload.
- Admin dilindungi Cloudflare Access One-Time PIN dengan allowlist email.

## Teknologi yang ditetapkan

| Area | Teknologi |
|---|---|
| Web | Astro + adapter Cloudflare |
| Styling | Tailwind CSS |
| Hosting publik | Cloudflare Pages |
| Database | Cloudflare D1 (SQLite) |
| Object storage | Cloudflare R2 |
| API admin | Cloudflare Workers |
| Proteksi admin | Cloudflare Access One-Time PIN |
| Development | GitHub Codespaces |
| Source control | GitHub |

GitHub digunakan untuk source code dan dokumentasi. GitHub bukan platform deployment maupun CMS. Deployment dilakukan ke Cloudflare.

## Dokumentasi

- [PRD](./docs/PRD.md) — kebutuhan dan acceptance criteria produk.
- [Arsitektur](./docs/ARCHITECTURE.md) — komponen dan aliran request.
- [Model data](./docs/DATA-MODEL.md) — entitas D1 dan relasi.
- [Keamanan](./docs/SECURITY.md) — autentikasi, otorisasi, upload, dan perlindungan data.
- [Operasi konten](./docs/CONTENT-OPERATIONS.md) — alur admin, optimasi foto, R2, dan cache.
- [Rencana pengembangan](./docs/DEVELOPMENT-PLAN.md) — fase dan gerbang verifikasi.
- [Keputusan teknis](./docs/DECISIONS.md) — keputusan yang disetujui dan hal yang masih pending.
- [AGENTS.md](./AGENTS.md) — instruksi wajib untuk agen dan kontributor.
- [progres.md](./progres.md) — riwayat progres append-only.

## Prinsip implementasi

1. Implementasi harus nyata dan dapat diverifikasi dari UI sampai database, autentikasi, dan pengujian.
2. Tidak boleh ada tombol, form, atau alur yang hanya menjadi placeholder.
3. Pertahankan ruang lingkup dan keputusan yang tercatat; perubahan memerlukan persetujuan pemilik proyek.
4. Gunakan layanan Cloudflare yang telah ditetapkan dan pantau kuota Free secara eksplisit.
5. Jangan mengklaim fitur selesai atau terverifikasi tanpa bukti pengujian.
6. Jangan menyimpan secret, token, credential, atau data pribadi sensitif di repository.

## Status implementasi

Belum ada aplikasi, schema D1, konfigurasi Wrangler, atau deployment yang dibuat pada tahap dokumentasi ini.
