# Golang Database Migration

## Sebelum Belajar

- MySQL Database
- Golang Database

## Agenda

- Pengenalan Database Migration
- Pengenalan Golang Migrate
- Menginstall Golang Migrate
- Membuat File Migration
- Menjalankan Migration
- Rollback Migration

## #1 Pengenalan Database Migration

- Saat ini, kebanyakan aplikasi yang dibuat akan membutuhkan database
- Saat aplikasi berjalan, biasanya database sudah siap digunakan, artinya tabel, kolom dan semua relasinya dusah dibuat di awal sebelum aplikasi dijalankan
- Apa yang terjadi ketika misal pada saat aplikasi sudah berjalan, kita perlu menambahkan fitur baru, lalu butuh mengubah stuktur table di database>
- Biasanya kita akan mengubahnya di database langsung, lalu melakukan perubahan kode program
- Hal ini mungkin terlihat sederhana, namun ketika skalanya sudah besar, dan anggota tim sudah banyak, maka peribahan langsung ke database bukanlah hal sederhana lagi
- Kita harus bisa melakukan tracking apa saja yang berubah, dan memastikan semua anggota tim tahu perubahannya, sehingga bisa dilakukan hal yang sama di komputer masing-masing

### Keuntungan Database Migration

- Oleh karena itu, Database Migration sangat diperlukan
- Database Migration adalah mekanisme untuk melakukan tracking peribahan struktur database, dari mulai awal dibuat sampai peribahan terakhir yang dilakukan
- Mirip seperti Git, dimana melakukan tracking semua perubahan kode program
- Dengan menggunakan Database Migration, semua tim member bisa melihat peribahan struktur database, dan bisa dengan mudah menjalankan perubahan tersebut di tiap komputer masing-masing
- Selain itu, dengan adanya Database Migration, kita bisa melakukan review terlebih dahulu, sebelum menjalankan perubahan di database, jaga-jaga ada perubahan yang salah, yang bisa berdampak berbahaya e database

## #2 Pengenalan Golang Migrate

- Golang Migrate adalah salah satu tool untuk Database Migration yang populer digunakan oleh programmer Golang
- Golang Migrate bisa diintegrasikan dengan aplikasi, atau dijalankan sebagai aplikasi standalone
- Golang Migrate mendukung banyak sekali database, seperti MySQL, PostgreSQL, Sqlite, MongoDB, Cassandra, dan lain-lain
- <https://github.com/golang-migrate/migrate>

## #3 Menginstall Golang Migrate

- Untuk menginstall Golang Migrate, sangat mudah, kita bisa gunakan perintah berikut:
- `go install -tags 'database1,database2' github.com/golang-migrate/migrate/v4/cmd/migrate@latest`
- Sesuaikan dengan database yang ingin kita gunakan, bisa lebih dari satu dengan cara menambahkan koma

### Kode: Menginstall Golang Migrate

```bash
go install -tags 'postgres,mysql' github.com/golang-migrate/migrate/v4/cmd/migrate@latest
```

### Aplikasi migrate

- Saat menginstall Golang Migrate, secara otomatis terdapat executable file di folder `$GOPATH/bin/` dengan nama migrate
- File migrate tersebut adalah aplikasi Golang Migrate yang akan kita gunakan untuk membuat Database Migration

### Kode: Aplikasi Migrate

![Aplikasi Migrate](./images/golang-database-migration-1.jpg)

## #4 Membuat Project

- Sebagai contoh, kita akan menggunakan project Golang RESTful API yang pernah kita buat, lalu kita akan tambahkan Database Migration ke project tersebut
- <https://github.com/ProgrammerZamanNow/belajar-goang-restful-api>

## #5 Membuat Database Migration

- Untuk membuat database migration, kita bisa gunakan perintah:
- `migrate create -ext sql -dir db/migrations name_file_migration`
- `-ext` adalah file extension, artinya kita membuat file .sql
- `-dir` adalah folder tempat disimpan
- Usahakan tidak menggunakan spasi pada nama file migration

### Kode: Membuat Database Migration

```bash
migrate create -ext sql -dir db/migrations create_table_category
```

### File Migration

- File migration akan diawali dengan waktu ketika kita membuat file migration, lalu diikuti dengan nama migration, dan diakhiri dengan tipe migration
- misal `202209211103313_create_table_category.up.sql`
- Kenapa diawali dengan waktu? Agar file migration selalu berurut sesuai waktu kita membuat file migration tersebut

## #6 Migration Up

- Saat kita membuat file database migration, file dengan akhiran up adalah file yang harus kita isi dengan perubahan yang ingin kita tambahkan
- Misal, sekarang kita akan tambahkan table category, sesuai dengan aplikasi RESTful API yang sudah kita buat

### Kode: Migration Up

```sql
CREATE TABLE category
(
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL,
	PRIMARY KEY (id)
) ENGINE = InnoDB;
```

## #7 Migration Down

- Setiap file migration, selain file up, terdapat juga file down
- File down ini adalah file yang berisikan kode untuk mengembalikan perubahan yang kita lakukan di file up
- Kenapa ini diperlukan? Karena misal terjadi masalah di aplikasi, namun database migration terlanjur dijalankan, kita bisa melakukan rollback dengan cara menjalankan file down, karena berisikan kode untuk mengembalikan perubahan di file up
- Pada kasus ini, misal kita akan mengapus lagi table category

### Kode: Migration Down

```sql
DROP TABLE IF EXISTS category;
```

## #8 Membuat Database

- Sebelum menjalankan Database Migration, sekarang kita perlu membuat dulu database nya
- Hal ini karena pembuatan database tidak dilakukan di database migration, biasanya dilakukan manual diawal
- Pada kasus ini, kita menggunakan database mysql, dan kita perlu ubah juga kode koneksi database di aplikasi agar terhubung dengan database baru

### Kode: Golang Connection

```go
...
func NewDB() *sql.DB {
	db, err := sql.Open("mysql", "root@tcp(localhost:3306)/belajar_golang_database_migration")
	helper.PanicIfError(err)

	db.SrtMaxIdleConns(5)
	db.SrtMaxOpenConns(20)
	db.SrtConnMaxLifetime(60 * time.Minute)
	db.SrtConnMaxIdleTime(10 * time.Minute)
}
```

### Kode: Membuat Database

```sql
CREATE DATABASE belajar_golang_database_migration;
```

## #9 Menjalankan Migration

- Selanjutnya, setelah selesai, kita bisa menjalankan database migration menggunakan perintah `migrate -database "koneksidatabase" -path folder up`
- `-database` harus berisikan koneksi database, misal untuk mysql, bisa menggunakan: `mysql://user:password@tcp(host:port)/nama_database`
- Untuk database lainnya, bisa lihat di halaman dokumentasinya: <https://github.com/golang-migrate/migrate#database>
- `-path` adalah lokasi folder file database migration
- `up` adalah printah untuk menjalankan database migration dengan mode `up`

### Kode: Menjalankan Migration

```bash
migrate -datbase "mysql://root:tcp(localhost:3306)/belajar_database_migration" -path db/migrations up
```

## #10 Migration State

- Saat kita sudah melakukan migration, lalu kita menambah file migration baru, apa yang terjadi jika kita menjalankan migration lagi?
- Golang Migrate akan menyimpan state terakhir kita menjalankan database migration, artinya tidak akan dijalankan dari awal lagi, melainkan dari file terakhir yang sukses di migrasi
- Jadi kita tidak perlu takut file akan dijalankan lagi, jadi tidak perlu dihapus file migration lama-nya
- Semua informasi state tersebut disimpan dalam table schema_migrations

### Kode: Schema Migration

![Schema Migration](./images/golang-database-migration-2.jpg)

## #11 Rollback Migration

- Pada waktu misal terjadi masalah pada aplikasi, yang menyebabkan kita harus melakukan rollback perubahan, apa yang kita harus lakukan?
- Fitur itu sudah ada di Golang Migrate, jadi kita bisa menjalankan mode down untuk melakukan rollback dengan perintah:
- `migrate -database "koneksidatabase" -path folder down`

### Kode: Migration Down

```bash
migrate -datbase "mysql://root:tcp(localhost:3306)/belajar_database_migration" -path db/migrations down
```

### Kode: Schema Migration

![Schema Migration](./images/golang-database-migration-3.jpg)

## #12 Migrasi ke Versi Terbaru

- Sat menggunakan meode up atau down, secara otomatis akan melakukan migrasi sluruh file
- Kadang pada kenyataanya, kita mungkin hanya ingin melakukan up atau down ke versi tertentu daja
- Misal jika ingin melakukan rollback, mungkin kita hanya ingin rollback satu versi saja, tidak mau melakukan rollback semua versi
- Untuk kasus inin, setelah perintah up atau down, kita bisa memasukkan angka, yaitu jumlah migration yang ingin kita eksekusi

### Tugas

- Buatlah 3 migration file baru

### Kode: Mambuat Migration

```bash
migrate create -ext sql -dir db/migrations create_table_first
migrate create -ext sql -dir db/migrations create_table_second
migrate create -ext sql -dir db/migrations create_table_third
```

### Tugas

- Lakukan up
- Lakukan down
- Lakukan up beberapa versi
- Lakukan down beberapa versi

## #13 Dirty State

- Saat kita membuat database migration, kadang kesalahan sering terjadi
- Misal saja, kita melakukan type sehingga membuat perintah SQL nya salah
- jika kite terlanjut menjalankan database migration, maka state akan berubah menjadi Dirty State
- State dimana kita tidak bisa melakukan up atau down lagi
- Pada kasus ini, kita bisa perbaiki manual, kenapa manual? Karena tdak ada cara otomatis memperbaikinya

### Kode: Migration Error

```sql
CREATE TABLE correct (
	id 	 INT 					NOT NULL AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL,
	PRIMARY KEY (id)
) ENGINE = InnoDB;

CREATE TABLE wrong (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHIR(255) NOT NULL,
	PRIMARY KEY (id)
) ENGINE = InnoDB;
```

### Kode: Menjalankan Migration

```bash
migrate -datbase "mysql://root:tcp(localhost:3306)/belajar_database_migration" -path db/migrations up
```

### Permasalahan

- Permasalahan migration ini adalah, kita membuat dua table di file migration, pada pembuatan table pertama sukses, namun pada table kedua gagal
- Artinya file migration tidak sempurna, dan kita juga tidak bisa melakukan rollback, karena table kedua belum sukses dibuat
- Pada kondisi ini, terjadi namanya Dirty State, diaman kita tidak bisa melakukan up atau down yang perlu kita lakukan adalah memperbaiki secara manual

### Kode: Schema Migrations

![Schema Migrations](./images/golang-database-migration-4.jpg)

### Kode: Perbaiki Manual

```bash
mysql> drop table correct
```

### Mengubah Versi

- Setelah kita memperbaiki secara manual, selanjutnya kita perlu mengubah versi migration di table schema_migrations
- Kita bisa lakukan manual, atau bisa otomatis menggunakan perintah:
- `migration -database "koneksi_database" -path folder docer versi`
- Dimana versi adalah versi dari file database migration
- Pada kasus ini, kita akan gunakan satu versi sebelum migration yang gagal

### Kode: Mengubah Versi Migration

```bash
migrate -datbase "mysql://root:tcp(localhost:3306)/belajar_database_migration" -path db/migrations force 20220921110755
migrate -datbase "mysql://root:tcp(localhost:3306)/belajar_database_migration" -path db/migrations version
```

### Selanjutnya

- Selanjutnya kita bisa perbaiki file database migration nya
- Lalu ulangi jalankan file migration nya

## #14 Mencoba Aplikasi

- Sekarang sebelum kita menjalankan aplikasi, selalu jalankan database migration terlebih dahulu
- Dan selanjutnya kita bisa mencoba menjalankan aplikasinya

## #15 Materi Selanjutnya

- Belajar Framework dan Library Go-Lang
- Studi Kasus Membuat Aplikasi menggunakan Go-Lang
