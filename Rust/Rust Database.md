# Rust Database

## Sebelum Belajar

- Rust Dasar
- Rust Unit Test
- Rust Concurrency
- Rust Date Time
- PostgreSQL

## #1 Rust Database

### Rust Database

- Saat dibuatnya video ini, Rust sendiri tidak memiliki library standard untuk terkoneksi ke database
- Mirip seperti bahasa pemrograman Golang atau JavaScript (NodeJS), Rust memerlukan library tambahan untuk terkoneksi ke database
- Ada banyak sekali library yang bisa kita gunakan untuk terkoneksi ke database di Rust, namun pada materi ini, kita akan menggunakan library SQLX, yaitu salah satu library yang populer di Rust

### SQLx

- SQLx merupakan library untuk terkoneksi ke database di Rust yang mendukung database PostgreSQL, MySQL, MariaDB dan SQLite
- <https://crates.io/crates/sqlx>
- Pada materi ini, kita akan menggunakan database PostgreSQL sebagai databasenya

## #2 Membuat Project

### Membuat Project

- `cargo new belajar-rust-database`

### Menambah SQLx

- `cargo add chrono`
- `cargo add tokio --features full`
- `cargo add sqlx --features runtime-tokio,chrono,postgres`

### Membuat Database

- Buatlah database dengan nama : `belajar_rust_database`

## #3 Connection

### Connection

- Hal yang pertama kali yang harus kita lakukan untuk terkoneksi ke database, adalah membuat Connection
- Untuk membuat koneksi ke PostgreSQL, kita bisa menggunakan struct 1
- <https://docs.rs/sqlx/0.8.2/sqlx/struct.PgConnection.html>
- Format koneksi database ke PostgreSQL di SQLx adalah menggunakan format :
- `postgres://user:password@host:port/nama_database`

### Kode: Connection

```rust
#[tokio::test]
async fn test_manual_connection() -> Result<(), Error> {
	let url = "postgres://khannedy:@localhost:5432/belajar_rust-database";
	let connection: PgConnection = PgConnection::connection(url).await?;

	connection.close().await?;
	Ok(())
}
```

## #4 Database Pool

### Database Pool

- Membuat koneksi ke database adalah salah satu hal yang mahal dalam aplikasi, oleh karena itu tidak dianjurkan selalu membuka tutup koneksi di aplikasi kita
- Salah satu praktik yang baik saat membuat koneksi ke database adalah membuat Database Pool (kumpulan koneksi database)
- Database Pool adalah konsep dimana kita membuat tempat untuk menampung koneksi ke database, sehingga ketika membutuhkan koneksi, kita bisa ambil dari Database Pool, dan ketika sudah tidak membutuhkan lagi, kita kembalikan ke Database Pool
- Database Pool juga bisa menjaga agar kita tidak terlalu banyak membuat koneksi ke database sehingga tidak membebani database terlalu berat

### Database Pool di SQLx

- Untuk membuat Database Pool di SQLx, terutama untuk PostgreSQL, kita bisa menggunakan struct `PgPool`
- <https://docs.rs/sqlx/0.8.2/sqlx/type.PgPool.html>
- Kita bisa melakukan banyak pengaturan ketika membuat Database Pool, seperti jumlah minimal dan maksimal koneksi menggunakan `PgPoolOptions`
- <https://docs.rs/sqlx/0.8.2/sqlx/postgres/type.PgPoolOptions.html>

### Kode: Database Pool

```rust
async fn get_pool() -> Result<Pool<Postgres>, Error> {
	let url = "postgres://khannedy:@localhost:5432/belajar_rust-database"
	PgPoolOptions::new()
		.max_connection(10)
		.min_connection(5)
		.acquire_timeout(Duration::from_secs(5))
		.idle_timeout(Duration::from_secs(60))
		.connect(url).await?;
}

#[tokio::test]
async fn test_pool_connection() -> Result<(), Error> {
	let pool = get_pool().await?;
	pool.close().await?;

	Ok(())
}
```

## #5 Execute SQL

### Query

- Untuk mengirim perintah SQL ke database, kita bisa menggunakan function `query()` pada module sqlx
- <https://docs.rs/sqlx/0.8.2/sqlx/fn.query.html>
- Hasil dari function query() tersebut adalah object Query
- <https://docs.rs/sqlx/0.8.2/sqlx/query/struct.Query.html>

### Execute SQL

- Sekarang kita bahas dulu perintah SQL yang tidak menghasilkan data
- Jika kita ingin mengeksekusi perintah SQL yang tidak menghasilkan data, kita bisa menggunakan method execute() pada object Query
- <https://docs.rs/sqlx/0.8.2/sqlx/query/struct.Query.html#method.execute>

### Kode: Membuat Tabel

```sql
create table category
(
	id varchar(100) primary key,
	name varchar(100) not null,
	description text
)
```

### Kode: Execute SQL

```rust
#[tokio::test]
async fn test_execute_sql() -> Result<(), Error> {
	let pool = get_pool().await?;
	sqlx::query("insert into category (id, name, description) values ('A', 'Contoh', 'Contoh Deskripsi')")
		.execute(&pool).await?;
	Ok(())
}
```

## #6 Prepare Statement

### Prepare Statement

- Salah salah masalah ketika kita membuat SQL menggunakan string concat adalah SQL Injection
- <https://owasp.org/www-community/attacks/SQL_Injection>
- Oleh karena itu tidak disarankan menggunakan string concat untuk membuat perintah SQL yang akan kita kirim menggunakan SQLx
- SQLx Query mendukung Prepare Statement, sehingga jika kita ingin mengirim parameter ke perintah SQL, kita bisa menggunakan method `bind()`
- <https://docs.rs/sqlx/0.8.2/sqlx/fn.query.html>

### Kode: Prepare Statement

```rust
#[tokio::test]
async fn test_execute_sql() -> Result<(), Error> {
	let pool = get_pool().await?;
	sqlx::query("insert into category (id, name, description) values ($1, $2, $3)")
		.bind("B").bind("Contoh").bind("Contoh Deskripsi")
		.execute(&pool).await?;
	Ok(())
}
```

## #7 Query SQL

### Query SQL

- Untuk membuat perintah SQL yang menghasilkan data, kita bisa menggunakan function yang sama, yaitu `query()`
- Namun hasil object Query nya kita tidak menggunakan method `execute()`, melainkan menggunakan method `fetch()`
- Ada banyak method `fetch()`, kita bisa gunakan sesuai kebutuhan

### Fetch Method

- `fetch_optional(): Result<Option>`, jika menghasilkan satu data atau kosong
- `fetch_one(): Result, `jika menghasilkan satu data, jika tidak maka akan error
- `fetch_all(): Vec<Result>`, untuk data banyak
- `fetch(): Stream<Result>`, untuk mengambil data dalam bentuk stream (lazy)

### Kode: Fetch Optional

```rust
#[tokio::test]
async fn test_fetch_optional() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: Option<PgRow> = sqlx::query("select * from category where id = $1")
		.bind("A")
		.fetch_optional(&pool).await?

	if let Some(row) = result {
		let id: String = row.get("id");
		let name: String = row.get("name");
		let description: String = row.get("description");
		println!("id: {}, name: {}, description: {}", id, name, description);
	} else {
		println!("Data is not found");
	}

	Ok(())
}
```

### Kode: Fetch One

```rust
#[tokio::test]
async fn test_fetch_one() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: PgRow>= sqlx::query("select * from category where id = $1")
		.bind("A")
		.fetch_one(&pool).await?

	let id: String = row.get("id");
	let name: String = row.get("name");
	let description: String = row.get("description");
	println!("id: {}, name: {}, description: {}", id, name, description);

	Ok(())
}
```

### Kode: Fetch All

```rust
#[tokio::test]
async fn test_fetch_all() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: Vec<PgRow> = sqlx::query("select * from category")
		.fetch_all(&pool).await?

	for row in result {
		let id: String = row.get("id");
		let name: String = row.get("name");
		let description: String = row.get("description");
		println!("id: {}, name: {}, description: {}", id, name, description);
	}

	Ok(())
}
```

### Fetch

- Khusus untuk method `fetch()`, hasil return dari method nya berupa Stream (versi async dari Iterator)
- Oleh karena itu, kita perlu menambah library futures untuk mengambil data di Stream tersebut
- Silahkan tambahkan library futures menggunakan perintah :
- `cargo add futures`

### Kode: Fetch

```rust
use futures::TryStreamExt;

#[tokio::test]
async fn test_fetch_stream() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result = sqlx::query("select * from category")
		.fetch(&pool).await?

	while in Some(row) = result.try_next().await? {
		let id: String = row.get("id");
		let name: String = row.get("name");
		let description: String = row.get("description");
		println!("id: {}, name: {}, description: {}", id, name, description);
	}

	Ok(())
}
```

## #8 Result Mapping

### Result Mapping

- Salah satu yang biasa kita lakukan ketika membuat Query SQL adalah, mengubah hasil tiap baris menjadi object Struct
- Untuk mengubah data hasil Query dalam bentuk Row menjadi Struct, kita bisa memanfaatkan method `map()` pada Query

### Kode: Category Struct

```rust
#[derive(Debug)]
struct Category {
	id: String,
	name: String,
	description: String,
}
```

### Kode: Result Mapping

```rust
#[tokio::test]
async fn test_result_mapping() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: Vec<Category> = sqlx::query("select * from category")
		.map(|row: PgRow| {
			Category {
				id: row.get("id"),
				name: row.get("name"),
				description: row.get("description"),
			}
		})
		.fetch_all(&pool).await?

	for category in result {
		println!("{:?}", category);
	}

	Ok(())
}
```

### Automatic Result Mapping

- SQLx memiliki fitur untuk melakukan mapping secara otomatis, kita bisa menggunakan attribute `FromRow` dari SQLx
- Caranya kita harus menambahkan attribute `FromRow` pada Struct yang kita buat, dan pastikan hasil query kolom sama dengan nama field di Struct
- Dan untuk membuat Query SQL, kita perlu mengganti dari function `query()` menjadi `query_as()`
- <https://docs.rs/sqlx/latest/sqlx/fn.query_as.html>

### Kode: Automatic Result Mapping

```rust
#[derive(FormRow, Debug)]
struct Category {
	id: String,
	name: String,
	description: String,
}

#[tokio::test]
async fn test_automatic_result_mapping() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: Vec<Category> = sqlx::query_as("select * from category")
		.fetch_all(&pool).await?

	for category in result {
		println!("{:?}", category);
	}

	Ok(())
}
```

## #9 Data Type

### Data Type

- Saat membuat perintah SQL kadang kita perlu mengirim data, atau bisa menghasilkan data
- Tiap database, memiliki jenis data masing-masing, dan bisa berbeda dengan jenis data di Rust
- Bagaimana proses pemetaan antara tipe data di Database dan Rust? Kita bisa cek sesuai dengan jenis databasenya
- PostgreSQL : <https://docs.rs/sqlx/latest/sqlx/postgres/types/index.html>
- MySQL : <https://docs.rs/sqlx/latest/sqlx/mysql/types/index.html>
- Sqlite : <https://docs.rs/sqlx/latest/sqlx/sqlite/types/index.html>

### Kode: Membuat Tabel Brand

```sql
create table brands
(
	id varchar(100) primary key,
	name varchar(100) not null,
	description text,
	created_at timestamp not null default current_timestamp,
	updated_at timestamp not null default current_timestamp
)
```

### Kode: Insert Brand

```rust
#[tokio::test]
async fn test_insert_brands() -> Result<(), Error> {
	let pool = get_pool().await?;
	sqlx::query("insert into brands (id, name, description, created_at, updated_at) values ($1, $2, $3, $4, $5)")
		.bind("B")
		.bind("Contoh")
		.bind("Contoh Deskripsi")
		.bind(Local::now().naive_local())
		.bind(Local::now().naive_local())
		.execute(&pool).await?;
	Ok(())
}
```

### Kode: Brand Struct

```rust
#[derive(FormRow, Debug)]
struct Brand {
	id: String,
	name: String,
	description: String,
	created_at: NaiveDateTime,
	updated_at: NaiveDateTime,
}
```

### Kode: Query Brand

```rust
#[tokio::test]
async fn test_result_mappping_brand() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: Vec<Category> = sqlx::query_as("select * from brands")
		.fetch_all(&pool).await?

	for brand in result {
		println!("{:?}", brand);
	}

	Ok(())
}
```

## #10 Transaction

### Transaction

- Salah satu fitur yang sangat penting di Database adalah Transaction
- SQLx juga memiliki fitur yang bisa digunakan untuk membuat database transaction
- Kita bisa menggunakan method `begin()` yang menghasilkan object Transaction
- <https://docs.rs/sqlx/latest/sqlx/type.PgPool.html#method.begin>
- <https://docs.rs/sqlx/latest/sqlx/struct.Transaction.html>

### Kode: Transaction

```rust
#[tokio::test]
async fn test_transaction() -> Result<(), Error> {
	let pool = get_pool().await?;
	let mut transaction: Transaction<Postgres> = pool.begin().await?;

	sqlx::query("insert into brands (id, name, description, created_at, updated_at) values ($1, $2, $3, $4, $5)")
		.bind("B")
		.bind("Contoh")
		.bind("Contoh Deskripsi")
		.bind(Local::now().naive_local())
		.bind(Local::now().naive_local())
		.execute(&mut *transaction).await?;

	sqlx::query("insert into brands (id, name, description, created_at, updated_at) values ($1, $2, $3, $4, $5)")
		.bind("C")
		.bind("Contoh")
		.bind("Contoh Deskripsi")
		.bind(Local::now().naive_local())
		.bind(Local::now().naive_local())
		.execute(&mut *transaction).await?;

	transaction.commit().await?;
	Ok(())
}
```

## #11 Auto Increment

### Auto Increment

- Beberapa database, kadang memiliki fitur Auto Increment, misal pada MySQL dan PostgreSQL
- SQLx sendiri tidak punya fitur untuk mendapatkan nilai terakhir Auto Increment, oleh karena itu kita harus lakukan query secara manual
- Untungnya tiap database biasanya punya cara untuk mendapatkan nilai Auto Increment terakhir
- Namun perlu diingat, untuk dapat nilai Auto Increment, kita harus menggunakan koneksi yang sama pada SQLx, jika menggunakan Database Pool, kita bisa saja mendapat koneksi yang berbeda
- Oleh karena itu kita bisa memanfaatkan juga Transaction, karena akan menggunakan koneksi yang sama

### Kode: Membuat Tabel

```sql
create table sellers
(
	id serial primary key,
	name varchar(100) not null
)
```

## Kode: Auto Increment (1)

```rust
#[tokio::test]
async fn test_auto_increment() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let result: PgRow = sqlx::query("insert into sellers (name) values ($1) returning id")
		.bind("Contoh")
		.fetch_one(&pool).await?;

	let id: i32 = result.get("id");
	println!("id: {}", id);
	Ok(())
}
```

## Kode: Auto Increment (2)

```rust
#[tokio::test]
async fn test_auto_increment_with_transaction() -> Result<(), Error> {
	let pool: Pool<Postgres> = get_pool().await?;
	let mut transaction: Transaction<Post> = pool.begin().await?;

	sqlx::query("insert into sellers (name) values ($1) returning id")
		.bind("Contoh")
		.fetch_one(&mut *transaction).await?;

	let result: PgRow = sqlx::query("select lastval() as id")
		.fetch_one(&mut *transaction).await?;

	let id: i32 = result.get("id");
	println!("id: {}", id);

	transaction.commit().await?;
	Ok(())
}
```

## #12 Database Migration

### Database Migration

- SQLx memiliki fitur bernama database migration, yang bisa kita gunakan untuk melakukan management versi skema perubahan database
- Fitur ini sangat berguna, terutama ketika membuat aplikasi dengan ukuran besar, sehingga tidak perlu lagi melakukan manajemen perubahan skema database secara manual
- Fitur database migration ini dibuat dalam library yang berbeda, dan berbasis CLI (command line interface), yaitu SQLx-CLI
- <https://crates.io/crates/sqlx-cli>

### SQLx CLI

- SQLx-CLI tidak perlu di tambahkan dalam aplikasi kita, kita cukup menginstallnya sehingga menjadi program yang berjalan via terminal menggunakan perintah
- `cargo install sqlx-cli`
- Selanjutnya kita bisa menggunakan perintah sqlx via terminal

### Setup Database Migration

- SQLx menggunakan environment variable untuk mendeteksi lokasi database nya
- Kita bisa menggunakan nama environment variable `DATABASE_URL` yang berisi url menuju ke database
- Atau, kita bisa menggunakan file `.env`

### Kode: File `.env`

```sh
DATABASE_URL="postgres://khannedy:@localhost:5432/belajar_rust-database"
```

### Membuat Database

- Jika database nya belum ada, kita bisa meminta SQLx untuk membuatkan database, kita bisa gunakan perintah
- `sqlx database create`

### Membuat Migrations

- Untuk membuat file migration, kita bisa menggunakan perintah
- `sqlx migrate add -r <nama_migration>`
- Secara otomatis file migration akan dibuat di folder migrations

```sh
sqlx migrate add -r create_table_categories

sqlx migrate add -r create_table_brands
```

### Menjalankan Migration

- Untuk menjalankan migration, kita bisa gunakan perintah
- `sqlx migrate run --target-version <version>`
- Atau jika ingin menjalankan seluruh file migration yang belum dijalankan
- `sqlx migrate run`

```sh
sqlx migrate run
```

### Membatalkan Migration

- Untuk membatalkan migration, kita bisa menggunakan perintah
- `sqlx migrate revert --target-version <version>`
- Atau jika ingin membatalkan satu migration
- `sqlx migrate revert`

```sh
sqlx migrate revert

sqlx migrate revert

sqlx migrate revert
```

## #13 Materi Selanjutnya

### Materi Selanjutnya

- Rust Web
- Dan lain-lain
