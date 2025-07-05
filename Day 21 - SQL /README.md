```bash
# Masuk ke command prompt dan masuk ke username psql
psql -U postgres

# Masukkan password untuk user psql
Password for user postgres:

# Jika sudah berhasil masuk, akan muncul seperti ini
postgres=#

# Buat database
CREATE DATABASE neris;

# Masuk ke user psql 'postgres' dan database 'neris'
psql -U postgres -d neris

# Jika sudah berhasil masuk, akan muncul seperti ini
neris=#
```

```bash
# Buat tabel users
CREATE TABLE users (
  id SERIAL PRIMARY KEY,                      -- ID unik otomatis (auto increment) sebagai primary key
  full_name VARCHAR(50) NOT NULL,             -- Nama lengkap, maksimal 50 karakter, wajib diisi
  date_of_birth DATE NOT NULL,                -- Tanggal lahir, wajib diisi
  address TEXT,                               -- Alamat, bisa panjang, boleh kosong (nullable)
  email VARCHAR(50) UNIQUE,                   -- Email maksimal 50 karakter, harus unik antar user
  phone_number VARCHAR(20),                   -- Nomor telepon maksimal 20 karakter, opsional
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   -- Tanggal dan waktu saat data dibuat, default: waktu saat ini
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,   -- Tanggal dan waktu saat data terakhir diubah, default: waktu saat ini
  deleted_at TIMESTAMP                        -- Tanggal dan waktu jika data dihapus secara soft delete
);

#Cek apakah tabel sudah ada di list
\dt
