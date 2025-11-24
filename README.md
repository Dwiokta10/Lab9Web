# PHP MODULAR

- **Nama**  : Dwi Okta Ramadhani  
- **NIM**   : 312410056  
- **Kelas** : TI.24.A1  
- **Mata Kuliah** : Bahasa Pemrograman Web 1 

Aplikasi **Manajemen Barang** dengan fitur **login, register, dan logout** berbasis PHP & MySQL.
Hanya pengguna yang sudah login yang bisa mengakses halaman utama `index.php`. 

## Fitur Aplikasi

- **Autentikasi**
  - Login dengan username & password (menggunakan `password_hash` / `password_verify`)
  - Register akun baru (role default `user`)
  - Logout (hapus session, kembali ke halaman login)
  - Proteksi halaman `index.php` sehingga hanya bisa diakses setelah login

- **Manajemen Data Barang**
  - Menampilkan daftar barang dalam tabel
  - Menambah data barang baru (`tambah.php`)
  - Mengubah data barang (`ubah.php`)
  - Menghapus data barang (`hapus.php`)
  - Upload gambar barang (opsional)

- **Antarmuka**
  - Halaman utama: `index.php` (Daftar Barang)
  - Navigasi dengan menu: Beranda, Tambah Barang, Tentang, Logout
  - Styling menggunakan `assets/style.css`
  - Ikon dari Font Awesome
  - Efek UI sederhana dengan `assets/main.js`

## Struktur Folder (Utama)

```text
lab9_php_modular/
├── assets/
│   ├── style.css        # Styling utama
│   └── main.js          # Efek UI (navbar, smooth scroll, dll.)
├── config/
│   └── database.php     # Koneksi & inisialisasi database lab9_php_modular (auth)
├── modules/
│   └── auth/
│       ├── login.php    # Halaman login
│       ├── logout.php   # Logout & destroy session
│       └── register.php # Halaman daftar akun baru
├── index.php            # Halaman utama (Daftar Barang) – butuh login
├── tambah.php           # Form tambah barang
├── ubah.php             # Form edit barang
├── hapus.php            # Hapus data barang
├── koneksi.php          # Koneksi ke database barang (tabel data_barang)
└── README.md
```

> Catatan:  
> - `config/database.php` digunakan untuk modul autentikasi (`login`, `register`, `logout`).  
> - `koneksi.php` digunakan untuk modul manajemen barang (`index.php`, `tambah.php`, `ubah.php`, `hapus.php`) dengan database/tabel `data_barang`.

## Persiapan Database

### 1. Database untuk Login (`lab9_php_modular`)

```sql
CREATE DATABASE IF NOT EXISTS lab9_php_modular;
USE lab9_php_modular;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    role VARCHAR(20) NOT NULL DEFAULT 'user',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

Tambahkan user admin (opsional, jika belum dibuat via register):

```sql
INSERT INTO users (username, password, role)
VALUES (
  'dwiokta',
  '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', -- 12345678
  'admin'
);
```

### 2. Database untuk Barang (misal: `latihan1`)

```sql
CREATE DATABASE IF NOT EXISTS latihan1;
USE latihan1;

CREATE TABLE data_barang (
    id_barang INT(10) NOT NULL AUTO_INCREMENT,
    nama VARCHAR(100) NOT NULL,
    kategori ENUM('Elektronik','Pakaian','Makanan','Minuman','Lainnya') NOT NULL,
    harga_beli DECIMAL(10,0) NOT NULL,
    harga_jual DECIMAL(10,0) NOT NULL,
    stok INT(4) NOT NULL,
    gambar VARCHAR(100) DEFAULT NULL,
    PRIMARY KEY (id_barang)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

`koneksi.php` dikonfigurasi untuk mengarah ke database ini.

## Cara Menjalankan

- Jalankan **Apache** dan **MySQL** di XAMPP.  
- Letakkan folder `lab9_php_modular` di `htdocs`.  
- Buat database dan tabel seperti di bagian *Persiapan Database*.  
- Buka browser dan akses: `http://localhost/lab9_php_modular/`  
  - Jika belum login → otomatis diarahkan ke `modules/auth/login.php`.  
  - Login dengan user yang sudah dibuat (`dwiokta / 12345678` atau akun dari `register.php`).

## Alur Autentikasi Singkat

- Akses `http://localhost/lab9_php_modular/`  
  → jika belum login, redirect ke halaman login.  
- Login sukses  
  → redirect ke `index.php` (Daftar Barang).  
- Klik **Logout** di navbar  
  → session dihapus, kembali ke halaman login.

