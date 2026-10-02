# LokalBites — Platform Pemesanan Makanan & Katering UMKM Lokal

> Proyek Praktikum Pemrograman Berorientasi Objek 2 (PBO 2) — Program Studi Teknik Informatika

| Informasi Mahasiswa | |
|---|---|
| **Nama** | Muhammad Abdi Nur Salam |
| **NPM** | 2410010482 |
| **Kelas** | TI 5C REG BJB |
| **Dosen Pengampu** | Mirza Yogy Kurniawan, M.Kom |
| **Repository Asli** | [mirzayogy/laravel5d](https://github.com/mirzayogy/laravel5d) |
| **Fork Repository** | [abdiins/laravel5d](https://github.com/abdiins/laravel5d) |
| **Branch Tugas** | `feature/database-relations` |

---

## 📌 Deskripsi Proyek
**LokalBites** adalah platform web berbasis Laravel yang dirancang untuk mendigitalkan pemesanan makanan dan layanan katering bagi pelaku UMKM kuliner lokal. Sistem ini memfasilitasi transaksi antara pelanggan dengan warung makan lokal, mulai dari penjelajahan menu, pemesanan kustom (*notes/spesifikasi khusus*), pelacakan pesanan, hingga pemberian ulasan (*rating & review*).

---

## 🗄️ Rancangan Basis Data & Relasi Eloquent
Struktur database mencakup 9 tabel entitas dan 2 tabel pivot yang mendemonstrasikan seluruh tipe relasi objek:

1. **One-to-One:**
   - `User` $\leftrightarrow$ `CustomerProfile`
   - `User` $\leftrightarrow$ `Merchant`
   - `Order` $\leftrightarrow$ `Review`
2. **One-to-Many:**
   - `Category` $\to$ `Menus`
   - `Merchant` $\to$ `Menus`
   - `User` (Customer) $\to$ `Orders`
   - `Merchant` $\to$ `Orders`
   - `Merchant` $\to$ `Reviews`
3. **Many-to-Many:**
   - `Menu` $\leftrightarrow$ `Tag` via `menu_tag` (Halal, Pedas, Khas Banjar, Best Seller)
4. **Many-to-Many (with Pivot Data):**
   - `Order` $\leftrightarrow$ `Menu` via `order_items` dengan atribut pivot: `quantity`, `unit_price`, `subtotal`, `special_notes`
5. **Has-Many-Through:**
   - `Merchant` $\to$ `OrderItems` (melalui model `Menu`)
   - `User` $\to$ `OrderItems` (melalui model `Order`)

Detail diagram ERD dapat dilihat pada dokumen: **[`docs/database/erd.md`](docs/database/erd.md)**.

---

## 🚀 Panduan Menjalankan Proyek Secara Lokal

```bash
# 1. Clone repository
git clone https://github.com/abdiins/laravel5d.git
cd laravel5d

# 2. Install dependensi PHP & Node
composer install
npm install

# 3. Konfigurasi Environment & Key
cp .env.example .env
php artisan key:generate

# 4. Migrasi & Isi Data Awal (Seed)
php artisan migrate:fresh --seed

# 5. Jalankan server lokal
npm run dev           # Terminal 1
php artisan serve     # Terminal 2
```

Buka **http://localhost:8000** pada browser.
