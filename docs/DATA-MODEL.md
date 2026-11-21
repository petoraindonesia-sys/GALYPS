# Model Data — Cloudflare D1

Model berikut adalah kontrak konseptual untuk schema awal. Bentuk SQL final, tipe, constraint, dan indeks akan dibuat pada fase implementasi database dan harus diuji dengan migrasi D1.

## 1. Prinsip data

- D1 menyimpan data terstruktur dan metadata foto.
- R2 menyimpan binary foto.
- Gunakan ID stabil (UUID atau format ID yang konsisten).
- Simpan timestamp dalam format UTC yang konsisten.
- Slug produk/proyek harus unik pada ruang lingkup yang ditentukan.
- Record publik wajib memiliki status publikasi yang eksplisit.
- Jangan menyimpan binary foto di D1.

## 2. Entitas

### categories
Kategori katalog yang ditetapkan: Ornamen, Plafon, List.

Kolom konseptual:
- `id`
- `name`
- `slug` (unik)
- `description` (opsional)
- `sort_order`
- `is_active`
- `created_at`, `updated_at`

Kategori inti tidak boleh dihapus jika masih memiliki produk; perubahan nama/slug perlu menjaga URL dan referensi.

### products
- `id`
- `category_id` -> categories
- `name`
- `slug` (unik)
- `short_description` (opsional)
- `installation_notes` (opsional)
- `dimensions` (teks terstruktur/tervalidasi; format final ditentukan saat implementasi)
- `material`
- `status` (draft/published)
- `sort_order`
- `created_at`, `updated_at`

Produk draft tidak boleh muncul pada halaman publik atau sitemap.

### product_images
- `id`
- `product_id` -> products
- `object_key` (R2 key, unik)
- `alt_text`
- `mime_type`
- `byte_size`
- `width`, `height`
- `sort_order`
- `created_at`

Satu produk dapat memiliki banyak foto. Foto utama adalah foto dengan urutan teratas atau penanda eksplisit yang diputuskan pada implementasi; jangan membuat dua sumber kebenaran.

### gallery_projects
- `id`
- `title`
- `slug` (unik)
- `project_type` (contoh: rumah, ruko, kantor; daftar final dapat diperluas melalui keputusan)
- `city`
- `description` (opsional)
- `status` (draft/published)
- `sort_order`
- `created_at`, `updated_at`

### gallery_images
- `id`
- `project_id` -> gallery_projects
- `object_key` (R2 key, unik)
- `alt_text`
- `mime_type`
- `byte_size`
- `width`, `height`
- `sort_order`
- `created_at`

### site_settings
Pengaturan singleton, bukan tabel multi-tenant.

Kolom konseptual:
- `id` (satu record tetap)
- `business_name` (PENDING nilai)
- `business_profile`
- `whatsapp_number`
- `address`
- `business_hours` (format terstruktur)
- `map_url`
- `updated_at`

Jangan memasukkan nilai contoh ke data produksi tanpa label dan persetujuan.

## 3. Relasi

- categories 1 -> N products
- products 1 -> N product_images
- gallery_projects 1 -> N gallery_images
- site_settings merupakan singleton

## 4. Indeks yang diperkirakan

- categories(`slug` UNIQUE), categories(`is_active`, `sort_order`)
- products(`slug` UNIQUE), products(`category_id`, `status`, `sort_order`)
- product_images(`product_id`, `sort_order`)
- gallery_projects(`slug` UNIQUE), gallery_projects(`status`, `sort_order`)
- gallery_images(`project_id`, `sort_order`)

Finalisasi indeks berdasarkan query aktual dan hasil EXPLAIN, bukan menambahkan indeks tanpa kebutuhan.

## 5. Penghapusan dan konsistensi R2

- Penghapusan produk/proyek harus menangani metadata foto dan object R2.
- Gunakan strategi dua tahap atau mekanisme retry agar kegagalan R2 tidak menghasilkan referensi yang hilang tanpa jejak.
- Jangan menghapus object yang masih direferensikan.
- Operasi multi-step yang tidak atomik antara D1 dan R2 harus memiliki prosedur kompensasi dan pencatatan kegagalan.

## 6. Seed data

- Seed SQL hanya berisi kategori inti dan konten dummy yang jelas diberi label sebagai data demo.
- Jangan memasukkan nomor WhatsApp, alamat, jam kerja, atau tautan peta fiktif sebagai informasi bisnis nyata.
- Seed harus idempotent atau memiliki instruksi reset yang jelas untuk lingkungan lokal.
- Seed tidak boleh dijalankan ke database produksi tanpa pemeriksaan dan persetujuan.
