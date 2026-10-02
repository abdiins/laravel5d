# Database Design — Entity Relationship Diagram (LokalBites)

Dokumen ini menjelaskan skema basis data dan perancangan relasi Eloquent ORM untuk platform **LokalBites** (Platform Pemesanan Makanan & Katering UMKM Lokal).

---

## 1. Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    USERS ||--o| CUSTOMER_PROFILES : "has one"
    USERS ||--o| MERCHANTS : "owns / manages"
    USERS ||--o{ ORDERS : "places"
    USERS ||--o{ REVIEWS : "writes"

    MERCHANTS ||--o{ MENUS : "offers"
    MERCHANTS ||--o{ ORDERS : "receives"
    MERCHANTS ||--o{ REVIEWS : "receives"

    CATEGORIES ||--o{ MENUS : "classifies"
    MENUS ||--o{ MENU_TAG : "tagged via"
    TAGS ||--o{ MENU_TAG : "labels"

    ORDERS ||--o{ ORDER_ITEMS : "contains"
    MENUS ||--o{ ORDER_ITEMS : "ordered in"

    ORDERS ||--o| REVIEWS : "reviewed via"

    USERS {
        bigint id PK
        string name
        string email UK
        string password
        timestamp created_at
    }

    CUSTOMER_PROFILES {
        bigint id PK
        bigint user_id FK "UK"
        string phone_number
        string avatar
        text address
        string city
        string postal_code
        text delivery_notes
    }

    MERCHANTS {
        bigint id PK
        bigint user_id FK "UK"
        string store_name
        string slug UK
        text description
        text address
        string phone_number
        boolean is_open
        decimal rating
    }

    CATEGORIES {
        bigint id PK
        string name
        string slug UK
        string icon
        text description
    }

    MENUS {
        bigint id PK
        bigint merchant_id FK
        bigint category_id FK
        string name
        string slug
        decimal price
        boolean is_available
        int preparation_time_minutes
    }

    TAGS {
        bigint id PK
        string name
        string slug UK
        string color
    }

    MENU_TAG {
        bigint id PK
        bigint menu_id FK
        bigint tag_id FK
    }

    ORDERS {
        bigint id PK
        string order_code UK
        bigint user_id FK
        bigint merchant_id FK
        decimal total_amount
        decimal delivery_fee
        enum status
        text delivery_address
        string payment_method
        enum payment_status
    }

    ORDER_ITEMS {
        bigint id PK
        bigint order_id FK
        bigint menu_id FK
        int quantity
        decimal unit_price
        decimal subtotal
        string special_notes
    }

    REVIEWS {
        bigint id PK
        bigint order_id FK "UK"
        bigint user_id FK
        bigint merchant_id FK
        tinyint rating
        text comment
        text merchant_reply
    }
```

---

## 2. Pemetaan Relasi Eloquent

| Tipe Relasi | Model Asal | Model Tujuan | Keterangan & Metode Eloquent |
|---|---|---|---|
| **One-to-One** | `User` | `CustomerProfile` | Satu user memiliki satu profil pelanggan: `$this->hasOne(CustomerProfile::class)` |
| **One-to-One** | `User` | `Merchant` | Satu user pemilik toko mengelola satu merchant: `$this->hasOne(Merchant::class)` |
| **One-to-One** | `Order` | `Review` | Setiap pesanan yang selesai hanya dapat diulas satu kali: `$this->hasOne(Review::class)` |
| **One-to-Many** | `Category` | `Menu` | Kategori mengelompokkan banyak menu: `$this->hasMany(Menu::class)` |
| **One-to-Many** | `Merchant` | `Menu` | Merchant memiliki banyak pilihan menu: `$this->hasMany(Menu::class)` |
| **One-to-Many** | `User` | `Order` | Customer dapat membuat banyak pesanan: `$this->hasMany(Order::class)` |
| **One-to-Many** | `Merchant` | `Order` | Merchant menerima banyak pesanan: `$this->hasMany(Order::class)` |
| **Many-to-Many** | `Menu` | `Tag` | Menu dapat memiliki banyak label (Halal, Pedas, Best Seller) via `menu_tag` |
| **Many-to-Many (Pivot Data)** | `Order` | `Menu` | Pesanan mencakup banyak menu via `order_items` dengan atribut pivot: `quantity`, `unit_price`, `subtotal`, `special_notes` |
| **Has-Many-Through** | `Merchant` | `OrderItem` | Merchant dapat melacak semua item yang terjual melalui menu miliknya: `$this->hasManyThrough(OrderItem::class, Menu::class)` |
| **Has-Many-Through** | `User` | `OrderItem` | Customer dapat mengakses semua riwayat item makanan yang pernah dibeli melalui order: `$this->hasManyThrough(OrderItem::class, Order::class)` |

---

## 3. Aturan Integritas Data (Constraints)

1. **Foreign Key Cascade:**
   - Menghapus `User` akan menghapus `CustomerProfile` dan data pesanan terkait.
   - Menghapus `Order` akan menghapus rincian `OrderItems` dan `Review`.
2. **Foreign Key Restrict:**
   - `Category` tidak dapat dihapus jika masih terdapat `Menu` aktif di dalamnya.
   - `Menu` tidak dapat dihapus jika pernah tercatat dalam transaksi `OrderItem`.
3. **Unique Keys:**
   - `orders.order_code` bersifat unik.
   - `reviews.order_id` bersifat unik (1 order = 1 review).
   - Pasangan `[menu_id, tag_id]` pada tabel pivot `menu_tag` bersifat unik.
