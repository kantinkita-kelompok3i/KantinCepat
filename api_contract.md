# API Contract — KantinCepat

## 1. Informasi API

**Nama Aplikasi:** KantinCepat  
**Jenis:** RESTful API  
**Versi:** v1  
**Base URL:** `/api/v1`

Aplikasi KantinCepat merupakan aplikasi mobile untuk melakukan pre-order makanan dan minuman di kantin kampus.

---

# 2. Authentication

API menggunakan Bearer Token untuk endpoint yang membutuhkan autentikasi.

Format header:

Authorization: Bearer {access_token}

Contoh:

Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

Endpoint yang dapat diakses tanpa autentikasi:

- POST /auth/register
- POST /auth/login
- GET /categories
- GET /products
- GET /products/{product_id}

Endpoint lainnya membutuhkan autentikasi.

---

# 3. Standard Response

## 3.1 Success Response

Semua response berhasil menggunakan format:

{
  "status": "success",
  "message": "Request berhasil",
  "data": {}
}

## 3.2 Error Response

Semua response error menggunakan format:

{
  "status": "error",
  "message": "Request gagal",
  "errors": {}
}

---

# 4. HTTP Status Code

| Status | Keterangan |
|---|---|
| 200 | Request berhasil |
| 201 | Data berhasil dibuat |
| 204 | Data berhasil dihapus |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Data tidak ditemukan |
| 422 | Validasi gagal |
| 500 | Internal Server Error |

---

# 5. Role Pengguna

Aplikasi memiliki dua role:

| Role | Keterangan |
|---|---|
| customer | Mahasiswa/pengguna yang melakukan pemesanan |
| admin | Pengelola data makanan dan pesanan |

---

# 6. API ENDPOINTS

## 6.1 Authentication

### 1. Register User

**POST** `/auth/register`

Digunakan untuk membuat akun pengguna baru.

### Request Body

{
  "name": "Riska Dinar",
  "email": "riska@example.com",
  "password": "password123"
}

### Success Response — 201

{
  "status": "success",
  "message": "Registrasi berhasil",
  "data": {
    "user_id": 1,
    "name": "Riska Dinar",
    "email": "riska@example.com",
    "role": "customer"
  }
}

### Error Response

**422 — Validation Error**

{
  "status": "error",
  "message": "Data tidak valid",
  "errors": {
    "email": "Email sudah digunakan"
  }
}

**400 — Bad Request**

{
  "status": "error",
  "message": "Format data tidak valid",
  "errors": {}
}


---

## 2. Login

**POST** `/auth/login`

Digunakan untuk masuk ke dalam aplikasi.

### Request Body

{
  "email": "riska@example.com",
  "password": "password123"
}

### Success Response — 200

{
  "status": "success",
  "message": "Login berhasil",
  "data": {
    "access_token": "jwt-token",
    "user": {
      "user_id": 1,
      "name": "Riska Dinar",
      "email": "riska@example.com",
      "role": "customer"
    }
  }
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Email atau password salah",
  "errors": {}
}

**422 — Validation Error**

{
  "status": "error",
  "message": "Data login tidak lengkap",
  "errors": {
    "email": "Email wajib diisi"
  }
}


---

## 3. Logout

**POST** `/auth/logout`

Digunakan untuk mengakhiri sesi pengguna.

### Authentication

Required.

### Success Response — 200

{
  "status": "success",
  "message": "Logout berhasil",
  "data": null
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

**400 — Bad Request**

{
  "status": "error",
  "message": "Logout gagal",
  "errors": {}
}


---

# 6.2 User

## 4. Get Current User

**GET** `/users/me`

Menampilkan informasi pengguna yang sedang login.

### Authentication

Required.

### Success Response — 200

{
  "status": "success",
  "message": "Data pengguna berhasil diambil",
  "data": {
    "user_id": 1,
    "name": "Riska Dinar",
    "email": "riska@example.com",
    "role": "customer"
  }
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

**404 — Not Found**

{
  "status": "error",
  "message": "Pengguna tidak ditemukan",
  "errors": {}
}


---

## 5. Update Current User

**PATCH** `/users/me`

Digunakan untuk mengubah data profil pengguna.

### Authentication

Required.

### Request Body

{
  "name": "Riska Dinar Andini"
}

### Success Response — 200

{
  "status": "success",
  "message": "Profil berhasil diperbarui",
  "data": {
    "user_id": 1,
    "name": "Riska Dinar Andini",
    "email": "riska@example.com"
  }
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

**422 — Validation Error**

{
  "status": "error",
  "message": "Data tidak valid",
  "errors": {
    "name": "Nama wajib diisi"
  }
}


---

# 6.3 Categories

## 6. Get Categories

**GET** `/categories`

Menampilkan seluruh kategori makanan.

### Success Response — 200

{
  "status": "success",
  "message": "Kategori berhasil diambil",
  "data": [
    {
      "category_id": 1,
      "category_name": "Makanan"
    },
    {
      "category_id": 2,
      "category_name": "Minuman"
    }
  ]
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Kategori tidak ditemukan",
  "errors": {}
}

**500 — Internal Server Error**

{
  "status": "error",
  "message": "Gagal mengambil data kategori",
  "errors": {}
}


---

## 7. Create Category

**POST** `/categories`

Khusus admin untuk menambahkan kategori.

### Authentication

Required — Admin.

### Request Body

{
  "category_name": "Snack"
}

### Success Response — 201

{
  "status": "success",
  "message": "Kategori berhasil dibuat",
  "data": {
    "category_id": 3,
    "category_name": "Snack"
  }
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}


---

# 6.4 Products

## 8. Get Products

**GET** `/products`

Menampilkan daftar makanan dan minuman.

### Query Parameter

`category_id` — filter berdasarkan kategori  
`search` — pencarian produk  
`page` — nomor halaman  
`limit` — jumlah data

Contoh:

GET `/products?category_id=1&search=nasi&page=1&limit=10`

### Success Response — 200

{
  "status": "success",
  "message": "Produk berhasil diambil",
  "data": [
    {
      "product_id": 1,
      "category_id": 1,
      "product_name": "Nasi Goreng Spesial Kampus",
      "description": "Nasi goreng dengan telur dan sayuran",
      "price": 15000,
      "image_url": "/images/nasi-goreng.jpg",
      "stock": 20,
      "is_available": true
    }
  ]
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Produk tidak ditemukan",
  "errors": {}
}

**422 — Validation Error**

{
  "status": "error",
  "message": "Parameter tidak valid",
  "errors": {
    "page": "Page harus berupa angka"
  }
}


---

## 9. Get Product Detail

**GET** `/products/{product_id}`

Menampilkan detail produk.

### Success Response — 200

{
  "status": "success",
  "message": "Detail produk berhasil diambil",
  "data": {
    "product_id": 1,
    "category_id": 1,
    "product_name": "Nasi Goreng Spesial Kampus",
    "description": "Nasi goreng dengan telur dan sayuran",
    "price": 15000,
    "image_url": "/images/nasi-goreng.jpg",
    "stock": 20,
    "is_available": true
  }
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Produk tidak ditemukan",
  "errors": {}
}

**400 — Bad Request**

{
  "status": "error",
  "message": "ID produk tidak valid",
  "errors": {}
}


---

## 10. Create Product

**POST** `/products`

Khusus admin untuk menambahkan produk.

### Authentication

Required — Admin.

### Request Body

{
  "category_id": 1,
  "product_name": "Nasi Goreng Spesial Kampus",
  "description": "Nasi goreng dengan telur",
  "price": 15000,
  "image_url": "/images/nasi-goreng.jpg",
  "stock": 20
}

### Success Response — 201

{
  "status": "success",
  "message": "Produk berhasil dibuat",
  "data": {
    "product_id": 1,
    "product_name": "Nasi Goreng Spesial Kampus",
    "price": 15000
  }
}

### Error Response

**403 — Forbidden**

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}

**422 — Validation Error**

{
  "status": "error",
  "message": "Data produk tidak valid",
  "errors": {
    "price": "Harga harus lebih besar dari 0"
  }
}


---

## 11. Update Product

**PATCH** `/products/{product_id}`

Khusus admin untuk mengubah data produk.

### Request Body

{
  "price": 16000,
  "stock": 25,
  "is_available": true
}

### Success Response — 200

{
  "status": "success",
  "message": "Produk berhasil diperbarui",
  "data": {
    "product_id": 1,
    "price": 16000,
    "stock": 25,
    "is_available": true
  }
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Produk tidak ditemukan",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}


---

## 12. Delete Product

**DELETE** `/products/{product_id}`

Khusus admin untuk menghapus produk.

### Success Response — 200

{
  "status": "success",
  "message": "Produk berhasil dihapus",
  "data": null
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Produk tidak ditemukan",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}


---

# 6.5 Orders

## 13. Create Order

**POST** `/orders`

Digunakan customer untuk membuat pesanan.

### Authentication

Required — Customer.

### Request Body

{
  "items": [
    {
      "product_id": 1,
      "quantity": 2
    },
    {
      "product_id": 2,
      "quantity": 1
    }
  ],
  "pickup_time": "2026-10-06T12:30:00"
}

### Success Response — 201

{
  "status": "success",
  "message": "Pesanan berhasil dibuat",
  "data": {
    "order_id": 1,
    "order_code": "KC-20261006-001",
    "total_amount": 35000,
    "order_status": "pending",
    "pickup_time": "2026-10-06T12:30:00"
  }
}

### Error Response

**422 — Validation Error**

{
  "status": "error",
  "message": "Data pesanan tidak valid",
  "errors": {
    "items": "Minimal satu produk harus dipilih"
  }
}

**404 — Not Found**

{
  "status": "error",
  "message": "Produk tidak ditemukan",
  "errors": {}
}


---

## 14. Get My Orders

**GET** `/orders`

Menampilkan daftar pesanan milik customer.

### Authentication

Required.

### Success Response — 200

{
  "status": "success",
  "message": "Daftar pesanan berhasil diambil",
  "data": [
    {
      "order_id": 1,
      "order_code": "KC-20261006-001",
      "total_amount": 35000,
      "order_status": "processing",
      "pickup_time": "2026-10-06T12:30:00"
    }
  ]
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

**404 — Not Found**

{
  "status": "error",
  "message": "Pesanan tidak ditemukan",
  "errors": {}
}


---

## 15. Get Order Detail

**GET** `/orders/{order_id}`

Menampilkan detail pesanan.

### Success Response — 200

{
  "status": "success",
  "message": "Detail pesanan berhasil diambil",
  "data": {
    "order_id": 1,
    "order_code": "KC-20261006-001",
    "total_amount": 35000,
    "order_status": "processing",
    "items": [
      {
        "product_id": 1,
        "product_name": "Nasi Goreng",
        "quantity": 2,
        "unit_price": 15000,
        "subtotal": 30000
      }
    ]
  }
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Pesanan tidak ditemukan",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Anda tidak memiliki akses ke pesanan ini",
  "errors": {}
}


---

## 16. Update Order Status

**PATCH** `/orders/{order_id}/status`

Digunakan admin untuk memperbarui status pesanan.

### Authentication

Required — Admin.

### Request Body

{
  "status": "ready",
  "note": "Pesanan siap diambil"
}

### Success Response — 200

{
  "status": "success",
  "message": "Status pesanan berhasil diperbarui",
  "data": {
    "order_id": 1,
    "order_status": "ready"
  }
}

### Error Response

**403 — Forbidden**

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}

**422 — Validation Error**

{
  "status": "error",
  "message": "Status pesanan tidak valid",
  "errors": {
    "status": "Status tidak tersedia"
  }
}


---

## 17. Get Order Status History

**GET** `/orders/{order_id}/status-history`

Menampilkan riwayat perubahan status pesanan.

### Authentication

Required.

### Success Response — 200

{
  "status": "success",
  "message": "Riwayat status berhasil diambil",
  "data": [
    {
      "status": "pending",
      "note": "Pesanan dibuat",
      "created_at": "2026-10-06T10:00:00"
    },
    {
      "status": "processing",
      "note": "Pesanan sedang diproses",
      "created_at": "2026-10-06T10:05:00"
    }
  ]
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Pesanan tidak ditemukan",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Anda tidak memiliki akses",
  "errors": {}
}


---

# 6.6 Payments

## 18. Create Payment

**POST** `/orders/{order_id}/payments`

Digunakan customer untuk memilih metode pembayaran.

### Authentication

Required.

### Request Body

{
  "payment_method": "qris"
}

### Success Response — 201

{
  "status": "success",
  "message": "Pembayaran berhasil dibuat",
  "data": {
    "payment_id": 1,
    "order_id": 1,
    "payment_method": "qris",
    "payment_status": "pending"
  }
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Pesanan tidak ditemukan",
  "errors": {}
}

**422 — Validation Error**

{
  "status": "error",
  "message": "Metode pembayaran tidak valid",
  "errors": {
    "payment_method": "Metode pembayaran tidak tersedia"
  }
}


---

## 19. Get Payment Detail

**GET** `/orders/{order_id}/payment`

Menampilkan informasi pembayaran pesanan.

### Authentication

Required.

### Success Response — 200

{
  "status": "success",
  "message": "Data pembayaran berhasil diambil",
  "data": {
    "payment_id": 1,
    "order_id": 1,
    "payment_method": "qris",
    "payment_status": "paid",
    "paid_at": "2026-10-06T10:10:00"
  }
}

### Error Response

**404 — Not Found**

{
  "status": "error",
  "message": "Data pembayaran tidak ditemukan",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Anda tidak memiliki akses",
  "errors": {}
}


---

# 6.7 Admin Orders

## 20. Get All Orders

**GET** `/admin/orders`

Digunakan admin untuk melihat seluruh pesanan.

### Authentication

Required — Admin.

### Query Parameter

`status` — filter berdasarkan status pesanan.

Contoh:

GET `/admin/orders?status=processing`

### Success Response — 200

{
  "status": "success",
  "message": "Daftar pesanan berhasil diambil",
  "data": [
    {
      "order_id": 1,
      "order_code": "KC-20261006-001",
      "customer_name": "Riska Dinar",
      "total_amount": 35000,
      "order_status": "processing",
      "pickup_time": "2026-10-06T12:30:00"
    }
  ]
}

### Error Response

**401 — Unauthorized**

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

**403 — Forbidden**

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}


---

# 7. Role-Permission Matrix

| Endpoint | Guest | Customer | Admin |
|---|---:|---:|---:|
| POST /auth/register | ✓ | ✓ | ✓ |
| POST /auth/login | ✓ | ✓ | ✓ |
| POST /auth/logout | - | ✓ | ✓ |
| GET /users/me | - | ✓ | ✓ |
| PATCH /users/me | - | ✓ | ✓ |
| GET /categories | ✓ | ✓ | ✓ |
| POST /categories | - | - | ✓ |
| GET /products | ✓ | ✓ | ✓ |
| GET /products/{id} | ✓ | ✓ | ✓ |
| POST /products | - | - | ✓ |
| PATCH /products/{id} | - | - | ✓ |
| DELETE /products/{id} | - | - | ✓ |
| POST /orders | - | ✓ | - |
| GET /orders | - | ✓ | - |
| GET /orders/{id} | - | ✓ | Admin |
| PATCH /orders/{id}/status | - | - | ✓ |
| GET /orders/{id}/status-history | - | ✓ | ✓ |
| POST /orders/{id}/payments | - | ✓ | - |
| GET /orders/{id}/payment | - | ✓ | ✓ |
| GET /admin/orders | - | - | ✓ |

---

# 8. Error Handling

API menggunakan error handling standar HTTP.

## 401 Unauthorized

Digunakan ketika pengguna belum login atau token tidak valid.

Contoh:

{
  "status": "error",
  "message": "Token tidak valid",
  "errors": {}
}

## 403 Forbidden

Digunakan ketika pengguna sudah login tetapi tidak memiliki hak akses.

Contoh:

{
  "status": "error",
  "message": "Akses hanya untuk admin",
  "errors": {}
}

## 404 Not Found

Digunakan ketika data yang diminta tidak ditemukan.

Contoh:

{
  "status": "error",
  "message": "Produk tidak ditemukan",
  "errors": {}
}

## 422 Validation Error

Digunakan ketika data request tidak memenuhi aturan validasi.

Contoh:

{
  "status": "error",
  "message": "Data tidak valid",
  "errors": {
    "price": "Harga harus lebih besar dari 0"
  }
}

---

# 9. Changelog

## Version 1.0.0

Tanggal: 2026-10-05

Perubahan:

- Menambahkan authentication API.
- Menambahkan endpoint user.
- Menambahkan endpoint kategori.
- Menambahkan endpoint produk.
- Menambahkan endpoint pemesanan.
- Menambahkan endpoint pembayaran.
- Menambahkan endpoint status pesanan.
- Menambahkan role customer dan admin.
- Menetapkan standard response JSON.
- Menetapkan standard error handling.
- Menetapkan role-permission matrix.

---

# 10. API Design Rules

1. Semua endpoint menggunakan prefix `/api/v1`.
2. Nama resource menggunakan bentuk jamak.
3. URL menggunakan lowercase.
4. Pemisah URL menggunakan tanda `/`.
5. Data JSON menggunakan `snake_case`.
6. Authentication menggunakan Bearer Token.
7. POST digunakan untuk membuat data.
8. GET digunakan untuk mengambil data.
9. PATCH digunakan untuk memperbarui sebagian data.
10. DELETE digunakan untuk menghapus data.
11. Response selalu menggunakan `status` dan `message`.
12. Response berhasil menggunakan `data`.
13. Response error menggunakan `errors`.
14. Endpoint admin harus menggunakan authorization.
15. Validasi input menggunakan HTTP 422.
