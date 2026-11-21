# Progres Pengembangan GALYPS

Dokumen ini adalah catatan append-only untuk status pekerjaan. Entri lama tidak boleh dihapus atau ditimpa; koreksi dibuat sebagai entri baru.

## Status
- `NOT_STARTED`: belum dikerjakan.
- `IN_PROGRESS`: sedang dikerjakan.
- `IMPLEMENTED`: perubahan telah dibuat, belum memiliki bukti verifikasi memadai.
- `VERIFIED`: lulus pengujian yang dicatat dan memiliki bukti commit/PR/CI.
- `BLOCKED`: pekerjaan terhambat oleh dependensi atau keputusan yang belum tersedia.

Status integrasi branch dicatat terpisah: `UNMERGED` sampai perubahan benar-benar masuk ke `main`.

---

## GALYPS-DOCS-001 — Project Documentation & Foundation

- **Status:** IN_PROGRESS
- **Branch:** main (bootstrap; dokumentasi lanjutan akan berada pada branch kerja tersendiri)
- **Tujuan:** Menetapkan kebutuhan produk, arsitektur, model data, keamanan, operasi konten, panduan agen, dan rencana pengembangan sebelum implementasi codebase.
- **Scope:**
  - README proyek dan panduan kontribusi.
  - AGENTS.md sebagai instruksi otoritatif repository.
  - PRD, arsitektur teknis, model data, keamanan, operasi konten, dan rencana fase.
  - Dokumentasi keputusan teknis dan batasan paket Cloudflare Free.
- **Acceptance criteria:**
  - Dokumen konsisten dengan spesifikasi GALYPS yang disetujui.
  - Tidak ada implementasi aplikasi pada tahap dokumentasi ini.
  - Keputusan yang belum ditentukan ditandai `PENDING`, bukan diasumsikan.
  - Risiko dan batasan platform didokumentasikan.
  - Semua file dokumentasi dapat ditinjau melalui GitHub.
- **File berubah:** `progres.md` (bootstrap).
- **Test/CI:** Belum dijalankan; repository masih kosong dan belum memiliki codebase.
- **Commit SHA:** Akan dicatat setelah perubahan dokumentasi selesai.
- **PR:** Belum dibuat.
- **Integrasi:** UNMERGED / dokumentasi belum dibuat.
- **Catatan:** Tahap ini hanya menyusun dokumen dan kontrak kerja. Implementasi kode dimulai setelah dokumentasi ditinjau dan tahap ini ditutup.


---

## GALYPS-DOCS-001 — Catatan penutupan dokumentasi

- **Status:** IMPLEMENTED
- **Branch kerja:** `docs/project-foundation`
- **Tujuan:** Menyelesaikan baseline dokumentasi dan kontrak kerja sebelum pembuatan codebase aplikasi.
- **File dibuat pada branch kerja:**
  - `README.md`
  - `AGENTS.md`
  - `docs/PRD.md`
  - `docs/ARCHITECTURE.md`
  - `docs/DATA-MODEL.md`
  - `docs/SECURITY.md`
  - `docs/CONTENT-OPERATIONS.md`
  - `docs/DEVELOPMENT-PLAN.md`
  - `docs/DECISIONS.md`
- **File progres:** `progres.md` diperbarui dengan catatan penutupan ini; entri awal dipertahankan sebagai riwayat.
- **Pemeriksaan:** File dokumentasi berhasil dibuat melalui GitHub Contents API. Belum ada test suite, build, atau CI karena codebase aplikasi belum dibuat.
- **Status verifikasi:** IMPLEMENTED; belum VERIFIED.
- **Commit SHA:** Akan dicatat setelah commit penutupan progres.
- **PR:** Belum dibuat.
- **Integrasi:** UNMERGED; branch kerja belum digabung ke `main`.
- **Catatan:** Tidak ada kode aplikasi, migrasi D1, Worker, konfigurasi Pages, atau deployment yang dibuat pada tahap ini. Keputusan GAL-P01 sampai GAL-P12 masih PENDING.


## GALYPS-DOCS-001 — Bukti pull request

- **PR:** [#1 — docs: establish GALYPS product and engineering foundation](https://github.com/petoraindonesia-sys/GALYPS/pull/1)
- **Status PR:** OPEN, belum merged.
- **Head commit saat PR dibuat:** `e4bf9e66c23e10265f69ca574b6dea44982f056c`
- **Jumlah file pada PR saat dibuat:** 10
- **Status pekerjaan:** IMPLEMENTED; belum VERIFIED (tidak ada build/test karena belum ada codebase).
- **Integrasi:** UNMERGED.


## GALYPS-REQ-001 — Audit kebutuhan dan validasi teknis

- **Status:** IN_PROGRESS
- **Branch:** `docs/project-foundation` (melanjutkan PR #1 yang masih OPEN)
- **Tujuan:** Memvalidasi keputusan arsitektur GALYPS terhadap dokumentasi Cloudflare terkini, memetakan keputusan PENDING, dan menyiapkan gerbang yang jelas sebelum fondasi codebase.
- **Scope:**
  - Verifikasi batas dan ketersediaan paket Cloudflare Free untuk komponen yang dipilih.
  - Tinjau kesesuaian pilihan Pages, Workers API, D1, R2, Access, dan strategi cache.
  - Identifikasi keputusan yang benar-benar memblokir implementasi dibanding keputusan yang dapat ditunda.
  - Perbarui dokumentasi/Decision Register hanya berdasarkan bukti dan keputusan eksplisit.
- **Acceptance criteria:**
  - Klaim limit/fitur Cloudflare memiliki rujukan resmi dan tanggal pemeriksaan.
  - Risiko perubahan platform atau batas paket ditandai jelas.
  - Keputusan yang belum disetujui tetap PENDING.
  - Tidak ada resource Cloudflare atau deployment yang dibuat tanpa persetujuan.
  - Hasil dan blocker dicatat append-only di `progres.md`.
- **Test/CI:** Belum ada; tahap ini adalah audit dokumentasi/teknis.
- **Commit SHA:** Akan dicatat setelah pembaruan selesai.
- **PR:** #1, OPEN, UNMERGED.
- **Integrasi:** UNMERGED.


## GALYPS-REQ-001 — Hasil audit dan blocker keputusan hosting

- **Status:** BLOCKED
- **Branch:** `docs/project-foundation`
- **Tujuan:** Menutup pemeriksaan awal batas paket dan kompatibilitas hosting sebelum fondasi codebase.
- **Hasil validasi dokumentasi resmi (21 Nov 2026):**
  - Workers Free: 100.000 request/hari; CPU 10 ms/request.
  - D1 Free: 5 juta row read/hari, 100.000 row write/hari, total storage 5 GB. Batas harian yang terlampaui dapat menyebabkan query gagal sampai reset.
  - R2 Standard Free: 10 GB-month, 1 juta operasi Class A/bulan, 10 juta Class B/bulan; egress internet gratis.
  - Cloudflare Zero Trust Free: hingga 50 pengguna; Access One-Time PIN dikirim ke email yang lolos policy dan PIN berlaku 10 menit.
  - Pages Free: 500 build/bulan, satu build berjalan bersamaan, 20.000 file per site, batas ukuran satu asset 25 MiB.
- **Temuan kompatibilitas utama:** Dokumentasi Astro terbaru menyebut dukungan Cloudflare Pages pada Astro Cloudflare adapter telah dihapus. Cloudflare/Astro merekomendasikan Workers untuk proyek baru. Karena GAL-012 sebelumnya menetapkan Pages, perubahan ke Workers Static Assets tidak boleh dilakukan tanpa persetujuan Owner.
- **Keputusan baru dicatat:** GAL-P13 pada `docs/DECISIONS.md`, status PENDING: memilih tetap Pages dengan kombinasi versi/arsitektur yang kompatibel atau beralih ke Workers + Static Assets.
- **Sumber resmi yang ditinjau:**
  - https://v6.docs.astro.build/en/guides/integrations-guide/cloudflare/
  - https://docs.astro.build/en/guides/deploy/cloudflare/
  - https://developers.cloudflare.com/workers/static-assets/migration-guides/migrate-from-pages/
  - https://developers.cloudflare.com/workers/platform/limits/
  - https://developers.cloudflare.com/d1/platform/pricing/
  - https://developers.cloudflare.com/d1/reference/faq/
  - https://developers.cloudflare.com/r2/pricing/
  - https://www.cloudflare.com/sase/products/access/
  - https://developers.cloudflare.com/cloudflare-one/integrations/identity-providers/one-time-pin/
  - https://developers.cloudflare.com/pages/platform/limits/
- **File berubah pada tahap ini:** `docs/DECISIONS.md` (menambahkan GAL-P13 dan memperjelas catatan GAL-P12); `progres.md` (catatan hasil audit).
- **Commit SHA perubahan Decision Register:** `2fb9fb48d02d48231d2b5b173025fbce87dad758`.
- **Test/CI:** Tidak dijalankan; tidak ada kode aplikasi pada tahap audit.
- **Status verifikasi:** Audit dokumen selesai berdasarkan sumber resmi yang tersedia; belum ada verifikasi konfigurasi akun Cloudflare.
- **PR:** #1, OPEN; head akan mengikuti commit terbaru branch.
- **Integrasi:** UNMERGED.
- **Blocker:** Persetujuan Owner untuk GAL-P13 diperlukan sebelum memilih adapter/runtime dan memulai Tahap 2.
