## 💻 Langkah-langkah PostgreSQL dengan DBeaver

```sql
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
1. Connect database 'neris' yang sudah dibuat di terminal ke DBeaver
   
![image](https://github.com/user-attachments/assets/c24b17b4-f7d7-44cb-8264-e11f14f90319)

2. Lakukan konfigurasi untuk database 'neris' yang akan di connect

![image](https://github.com/user-attachments/assets/37191dd2-03e7-4f9d-aecd-5e26da4363fd)

3. Connect agar database 'neris' muncul di DBeaver

![image](https://github.com/user-attachments/assets/0bedc777-4a0d-4e05-8061-4628b4b9f056)

```sql
# Membuat tabel users di DBeaver
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

#Cek apakah tabel sudah ada di list (di cmd)
\dt
```

```sql
# Tipe data enum untuk jenis akun: personal atau business
create type account_type as enum ('personal', 'business');

# Tipe data enum untuk status akun: aktif, dibekukan, atau ditutup
create type account_status as enum ('active', 'frozen', 'closed');
```

```sql
# Membuat table accounts
create table accounts (
	id SERIAL primary key, -- ID unik untuk setiap akun (auto-increment)
	user_id INTEGER not null references users(id) on delete cascade, -- Relasi ke tabel users, hapus akun jika user dihapus
	account_number VARCHAR(30) unique not null, -- Nomor akun unik, tidak boleh kosong
	account_type account_type default 'personal', -- Jenis akun, default: personal
	balance NUMERIC(15, 2) default 0.00, -- Saldo akun dengan 2 digit desimal
	account_status default 'active', -- Status akun, default: active
	created_at TIMESTAMP default CURRENT_TIMESTAMP, -- Waktu pembuatan akun
	updated_at TIMESTAMP default CURRENT_TIMESTAMP,-- Waktu terakhir pembaruan akun
	deleted_at TIMESTAMP -- Waktu penghapusan akun (soft delete)
);
```

```sql
# Tipe enum untuk jenis mutasi: penambahan (credit) atau pengurangan (debit)
create type mutation_type as enum ('credit','debit');
```

```sql
# Membuat table mutations
create table mutations (
	id SERIAL primary key, -- ID unik untuk setiap mutasi (auto-increment)
	account_id INTEGER not null references accounts(id) on delete cascade, -- Relasi ke tabel accounts, hapus mutasi jika akun dihapus
	mutation_type mutation_type not null, -- Jenis mutasi: credit atau debit
	amount NUMERIC(15, 2) not null check (amount > 0), -- Jumlah mutasi, harus lebih dari 0
	description TEXT, -- Deskripsi mutasi (opsional)
	reference_code VARCHAR(50), -- Kode referensi eksternal (opsional)
	created_at TIMESTAMP default CURRENT_TIMESTAMP -- Waktu pencatatan mutasi
);
```
Jika sudah create table dan type, maka akan muncul seperti ini

![image](https://github.com/user-attachments/assets/7b790331-f16e-47bb-891e-1a4128223048)

```sql
# Memasukkan data ke tabel
insert into users (full_name, date_of_birth, address, email, phone_number)
values
('Neris','2002-03-04','Tangerang','nerissa123@gmail.com','012345678901');

# Untuk validasi data
select * from users where full_name = 'Neris';
```


