# P01: Database Design and Table Relationships — LokalBites

| | |
|---|---|
| **Mahasiswa** | Muhammad Abdi Nur Salam |
| **NPM** | 2410010482 |
| **Kelas** | TI 5C REG BJB |
| **Proyek** | **LokalBites** (Platform Pemesanan Makanan & Katering UMKM Lokal) |
| **Status** | ✅ Done |
| **Branch** | `feature/database-relations` |
| **Pull Request** | <https://github.com/mirzayogy/laravel5d/pull/6> |

## Goal
Merancang dan mengimplementasikan basis data relasional untuk platform **LokalBites** menggunakan Eloquent ORM di Laravel 11/12, mencakup seluruh jenis relasi: One-to-One, One-to-Many, Many-to-Many, Many-to-Many dengan data pivot tambahan, serta Has-Many-Through.

## Jobs

| Code | Job | Status | Completed | Proof |
|---|---|---|---|---|
| J1 | Database design and ERD dengan Mermaid | ✅ Done | 2026-10-02 | [`docs/database/erd.md`](../database/erd.md) |
| J2 | Migrations: 9 tabel entitas & 2 pivot table | ✅ Done | 2026-10-02 | [`database/migrations`](../../database/migrations) |
| J3 | Eloquent Models and Relationships | ✅ Done | 2026-10-02 | [`app/Models`](../../app/Models) |
| J4 | Factories and Seeders | ✅ Done | 2026-10-02 | [`database/factories`](../../database/factories), [`database/seeders`](../../database/seeders) |
| J5 | Documentation and Verification | ✅ Done | 2026-10-02 | [`README.md`](../../README.md) |

---

### J1: Database design and ERD
- **Status:** ✅ Done
- **Penjelasan:** Merancang skema transaksi pemesanan kuliner UMKM lokal dengan 9 tabel utama dan relasi yang saling terhubung secara terstruktur.
- **Proof:** [`docs/database/erd.md`](../database/erd.md)

### J2: Migrations
- **Status:** ✅ Done
- **Penjelasan:** Membuat 9 berkas migrasi lengkap dengan foreign key constraint, cascade onDelete, restrict onDelete, dan unique key.
- **Proof:** [`database/migrations`](../../database/migrations)
- **Verifikasi:** Perintah `php artisan migrate:fresh --seed` berjalan 100% lancar tanpa error.

### J3: Models and relationships
- **Status:** ✅ Done
- **Penjelasan:** Membuat model Eloquent: `User`, `CustomerProfile`, `Merchant`, `Category`, `Menu`, `Tag`, `Order`, `OrderItem`, `Review`.
- **Proof:** [`app/Models`](../../app/Models)

### J4: Factories and seeders
- **Status:** ✅ Done
- **Penjelasan:** Membuat factory lengkap untuk setiap model, serta seeder realistis kuliner khas Banjar (Soto Banjar, Nasi Kuning, Wadai Bingka, Amparan Tatak, Es Limau Kuit).
- **Proof:** [`database/seeders/DatabaseSeeder.php`](../../database/seeders/DatabaseSeeder.php)

### J5: Documentation and Verification
- **Status:** ✅ Done
- **Verifikasi:** Pengujian otomatis dengan `php artisan test` berhasil lulus (**Passed**).
