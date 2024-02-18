# Database ACID

## Sebelum Belajar

- Kelas MySQL dari Programmer Zaman Now

## #1 Pengenalan Database ACID

- ACID merupakan compliance (kepatuhan) untuk Sistem Basis Data yang memiliki karakter Atomicity, Consistency, Isolation dan Durability
- ACID adalah sekumpulan properti transaksi basis data yang dimaksudkan untuk menjamin validitas data meskipun ada kesalahan, kegagalan daya, dan kecelakaan lainnya.

### Atomicity

- Setiap statement dalam transaksi (baik itu membaca, menulis, mengubah atau menghapus), diperlakukan sebagai satu kesatuan
- Jika transaksi berhasil, maka seluruh statement harus berhasil
- Jika transaksi gagal, maka seluruh statement harus tidak boleh ada yang berhasil, atau digagalkan
- Properti ini mencegah terjadinya kehilangan atau kerusakan data, misal jika ditengah transaksi terjadi kegagalan aplikasi

### Consistency

- Memastikan bahwa transaksi hanya bisa mengubah data dari satu kondisi konsisten ke kondisi konsisten lainnya
- Setiap data yang ditulis ke database harus valid sesuai dengan semua aturan yang sudah ditetapkan
- Hal ini mencegah data data menjadi tidak konsisten, dan menjamin integritas relasi antar data

### Isolation

- Transaksi sering dieksekusi secara bersamaan (mis., Beberapa transaksi membaca dan menulis ke tabel pada waktu yang sama).
- Isolation memastikan bahwa eksekusi transaksi secara bersamaan meninggalkan database dalam keadaan yang sama yang akan diperoleh jika transaksi dieksekusi secara berurutan.
- Isolation adalah tujuan utama kontrol konkurensi; tergantung pada tingkat isolasi yang digunakan, efek dari transaksi yang tidak lengkap mungkin tidak terlihat oleh transaksi lain

### Durability

- Durability menjamin bahwa sekali transaksi telah disimpan, itu akan tetap disimpan bahkan dalam kasus kegagalan sistem (misalnya, pemadaman listrik atau crash).

### MySQL ACID

- MySQL sudah mengikuti kaidah ACID ketika menggunakan engine `InnoDB`
- https://dev.mysql.com/doc/refman/8.0/en/mysql-acid.html
- Di materi selanjutnya, kita akan coba praktekan ACID menggunakan database MySQL

## #2 Setup Project

- Gunakan database MySQL
- Buat database `belajar_acid`
- Buat table accounts dengan kolom :
  - `id varchar(100) primary key,`
  - `name varchar(100) not null,`
  - `balance bigint not null`

### Kode: Create Database Belajar ACID

```sql
CREATE DATABASE belajar_acid;

USE belajar_acid;
```

### Kode: Create Table Account

```sql
CREATE TABLE accounts
(
	id 			VARCHAR(100) NOT NULL PRIMARY KEY,
	name 		VARCHAR(100) NOT NULL,
	balance BIGINT 			 NOT NULL
) ENGINE InnoDB;

SELECT * FROM accounts;
```

## #3 Atomicity

- Setiap statement dalam transaksi (baik itu membaca, menulis, mengubah atau menghapus), diperlakukan sebagai satu kesatuan
- Jika transaksi berhasil, maka seluruh statement harus berhasil
- Jika transaksi gagal, maka seluruh statement harus tidak boleh ada yang berhasil, atau digagalkan
- Properti ini mencegah terjadinya kehilangan atau kerusakan data, misal jika ditengah transaksi terjadi kegagalan aplikasi

### Atomicity di MySQL

- Implementasi Atomicity di MySQL menggunakan database transaction
- Setiap operasi di MySQL, selalu menggunakan database transaction, secara default MySQL akan melakukan autocommit setiap kita mengeksekusi statement
- Kecuali kita lakukan database transaction secara manual

### Kode: Transaksi Sukses

```sql
START TRANSACTION;

INSERT INTO accounts (id, name, balance)
VALUES ('eko', 'Eko Kurniawan', 10000000);

INSERT INTO accounts (id, name, balance)
VALUES ('budi', 'Budi Nugraga', 20000000);

COMMIT;

SELECT * FROM accounts;
```

### Kode: Transaksi Rollback

```sql
START TRANSACTION;

DELETE FROM accounts WHERE id = 'eko';

DELETE FROM accounts WHERE id = 'budi';

ROLLBACK;

SELECT * FROM accounts;
```

## #4 Consistency

- Memastikan bahwa transaksi hanya bisa mengubah data dari satu kondisi konsisten ke kondisi konsisten lainnya
- Setiap data yang ditulis ke database harus valid sesuai dengan semua aturan yang sudah ditetapkan
- Hal ini mencegah data menjadi tidak konsisten, dan menjamin integritas relasi antar data

### Kode: Invalid Update

```sql
START TRANSACTION;

UPDATE accounts SET name = null;
WHERE id = 'eko';

COMMIT;

SELECT * FROM accounts;
```

## #5 Isolation

- Transaksi sering dieksekusi secara bersamaan (mis., Beberapa transaksi membaca dan menulis ke tabel pada waktu yang sama).
- Isolation memastikan bahwa eksekusi transaksi secara bersamaan meninggalkan database dalam keadaan yang sama yang akan diperoleh jika transaksi dieksekusi secara berurutan.
- Isolation adalah tujuan utama kontrol konkurensi; tergantung pada tingkat isolasi yang digunakan, efek dari transaksi yang tidak lengkap mungkin tidak terlihat oleh transaksi lain

### Kode: SQL Transfer

```sql
START TRANSACTION;

SELECT * FROM accounts WHERE if IN ('eko', 'budi') FOR UPDATE;

UPDATE accounts SET balance - 500000
WHERE id = 'eko';

UPDATE accounts SET balance + 500000
WHERE id = 'budi';
COMMIT;
```

## #6 Durability

- Durability menjamin bahwa sekali transaksi telah disimpan, itu akan tetap disimpan bahkan dalam kasus kegagalan sistem (misalnya, pemadaman listrik atau crash).

### Kode: Transfer Gagal

```sql
seluruh * FROM accounts;

START TRANSACTION;

SELECT * FROM accounts WHERE id IN ('eko', 'budi') FOR UPDATE;

UPDATE accounts SET balance = balance - 500000
WHERE id = 'eko';

SELECT * FROM accounts;
```

## #7 Penutup
