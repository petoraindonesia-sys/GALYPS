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
