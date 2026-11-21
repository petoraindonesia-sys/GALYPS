# Rencana Pengembangan — GALYPS

Rencana ini mengurutkan pekerjaan agar setiap lapisan dapat diuji sebelum melanjutkan. Fase berikutnya tidak boleh dianggap selesai hanya karena kode telah ditulis.

## Tahap 0 — Dokumentasi dan kontrak proyek
**Status awal:** IN_PROGRESS

Deliverables:
- README dan AGENTS.md.
- PRD, arsitektur, model data, keamanan, operasi konten.
- Decision register dan progres append-only.
- Review konsistensi spesifikasi.

Gerbang penerimaan:
- Dokumen saling konsisten.
- PENDING tercatat eksplisit.
- Tidak ada codebase aplikasi pada tahap ini.

## Tahap 1 — Audit kebutuhan dan keputusan tersisa
Deliverables:
- Konfirmasi identitas bisnis, konten awal, domain, email admin, Access application, bucket, D1 database, batas foto, strategi cache.
- Konfirmasi struktur URL dan kebutuhan SEO.
- Konfirmasi kebutuhan foto multi-format dan retensi.

Gerbang penerimaan:
- Keputusan yang memengaruhi schema dan infrastruktur telah disetujui.
- Tidak ada credential ditulis di dokumen.

## Tahap 2 — Fondasi codebase
Deliverables:
- Inisialisasi Astro + adapter Cloudflare.
- Konfigurasi Tailwind CSS.
- Konfigurasi Wrangler untuk Pages, Worker, D1, dan R2 sesuai resource yang disetujui.
- Struktur source, environment example, lint/format/typecheck/test.
- Instruksi local development di Codespaces.

Gerbang penerimaan:
- Dependency terpasang dan lockfile tersedia.
- Build berhasil.
- Dev server berjalan.
- Binding development dapat diuji.
- Tidak ada secret dalam repository.

## Tahap 3 — Database dan seed
Deliverables:
- Migrasi D1.
- Indeks dan constraint.
- Seed kategori dan data demo yang jelas.
- Repository/data access layer.

Gerbang penerimaan:
- Migrasi dapat diterapkan dari database kosong.
- Seed dapat dijalankan secara aman.
- CRUD/query teruji terhadap D1 lokal.
- Draft tidak bocor ke query publik.

## Tahap 4 — API admin dan keamanan
Deliverables:
- Worker API.
- Verifikasi Cloudflare Access JWT.
- CRUD produk, galeri, pengaturan.
- Validasi schema dan error handling.
- Pengujian endpoint langsung.

Gerbang penerimaan:
- Anonymous dan identity tidak sah ditolak.
- Semua endpoint admin dilindungi.
- CRUD benar-benar tersimpan dan terbaca dari D1.
- Test security lulus.

## Tahap 5 — R2 dan pipeline foto
Deliverables:
- Browser image optimization.
- Upload Worker ke R2.
- Metadata D1.
- Preview, progress, error state.
- Penghapusan dan recovery object.

Gerbang penerimaan:
- Upload dari HP dan laptop teruji.
- File tersimpan di R2 dan dapat disajikan sesuai kebijakan.
- File invalid/oversize ditolak.
- Tidak ada orphan yang tidak tertangani.

## Tahap 6 — Website publik
Deliverables:
- Beranda, katalog, detail produk, galeri, tentang, kontak.
- WhatsApp CTA.
- SEO, responsive, accessibility.
- Integrasi D1/R2 dan cache.

Gerbang penerimaan:
- Seluruh halaman menggunakan data nyata D1.
- CTA menghasilkan pesan benar.
- Konten draft tidak tampil.
- Cache/invalidation diuji.
- Mobile dan desktop diuji.

## Tahap 7 — Admin UI
Deliverables:
- Dashboard sederhana.
- Form produk dan galeri.
- Pengaturan umum.
- Upload foto.
- Status loading, success, empty, validation, dan error.

Gerbang penerimaan:
- Alur admin dari login hingga perubahan konten berfungsi end-to-end.
- Perubahan tampil pada website publik.
- UI dapat digunakan di HP dan laptop.

## Tahap 8 — Hardening dan release
Deliverables:
- Audit keamanan, performa, aksesibilitas, SEO.
- Uji kuota dan perilaku cache.
- Backup/restore dan prosedur operasional.
- Checklist deployment Cloudflare.

Gerbang penerimaan:
- Test suite dan build lulus.
- Uji manual end-to-end selesai.
- Resource dan domain telah diverifikasi.
- Deployment hanya dilakukan setelah persetujuan pemilik.
- Status VERIFIED memiliki bukti commit dan CI/test run.

## Aturan perpindahan fase

- Setiap fase memiliki entri IN_PROGRESS dan penutupan faktual di progres.md.
- Bug atau blocker dicatat tanpa menghapus riwayat.
- Perubahan scope memerlukan pembaruan dokumentasi dan persetujuan.
- Tidak ada fase yang dilompati untuk mengklaim produk siap.
