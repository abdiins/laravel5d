# LokalBites — Local MSME Food & Catering Ordering Platform

| Student Information | |
|---|---|
| **Student** | Muhammad Abdi Nur Salam |
| **NPM** | 2410010482 |
| **Class** | TI 5C REG BJB |
| **Lecturer** | Mirza Yogy Kurniawan, M.Kom |
| **Upstream Repository** | [mirzayogy/laravel5d](https://github.com/mirzayogy/laravel5d) |
| **Fork Repository** | [abdiins/laravel5d](https://github.com/abdiins/laravel5d) |
| **Branch** | `feature/database-relations` |

---

## 📌 Project Overview
**LokalBites** is a Laravel-based web platform designed to digitize food ordering and catering services for local culinary micro, small, and medium enterprises (MSMEs). The system streamlines transactions between customers and local eateries, enabling menu browsing, custom order instructions (*special notes*), order tracking, and customer reviews (*ratings & feedback*).

---

## 🗄️ Database Design & Eloquent Relationships
The database schema consists of 9 entity tables and 2 pivot tables demonstrating all primary object-relational mapping (ORM) relationships:

1. **One-to-One:**
   - `User` $\leftrightarrow$ `CustomerProfile` (Customer delivery address and contact details)
   - `User` $\leftrightarrow$ `Merchant` (Store profile owned and managed by the merchant user)
   - `Order` $\leftrightarrow$ `Review` (Each completed order can receive exactly one review)
2. **One-to-Many:**
   - `Category` $\to$ `Menus` (Categories classify multiple menus)
   - `Merchant` $\to$ `Menus` (Merchants offer multiple menus)
   - `User` (Customer) $\to$ `Orders` (Customers can place multiple orders)
   - `Merchant` $\to$ `Orders` (Merchants receive multiple incoming orders)
   - `Merchant` $\to$ `Reviews` (Merchants receive customer reviews)
3. **Many-to-Many:**
   - `Menu` $\leftrightarrow$ `Tag` via `menu_tag` (Dietary and promotional tags: Halal, Spicy, Local Special, Best Seller)
4. **Many-to-Many (with Pivot Data):**
   - `Order` $\leftrightarrow$ `Menu` via `order_items` with custom pivot attributes: `quantity`, `unit_price`, `subtotal`, and `special_notes`
5. **Has-Many-Through:**
   - `Merchant` $\to$ `OrderItems` (through the `Menu` model — allowing merchants to track all sold items)
   - `User` $\to$ `OrderItems` (through the `Order` model — allowing customers to view all purchased food items)

For the complete schema diagram and details, see: **[`docs/database/erd.md`](docs/database/erd.md)**.

---

## 🚀 Local Development Setup

```bash
# 1. Clone the repository
git clone https://github.com/abdiins/laravel5d.git
cd laravel5d

# 2. Install PHP & Node dependencies
composer install
npm install

# 3. Environment configuration & application key
cp .env.example .env
php artisan key:generate

# 4. Run database migrations & seed demo data
php artisan migrate:fresh --seed

# 5. Start development servers
npm run dev           # Terminal 1
php artisan serve     # Terminal 2
```

Access the application at **http://localhost:8000** in your browser.
