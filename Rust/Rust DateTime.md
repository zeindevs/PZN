# Rust DateTime

## #1 Sebelum Belajar

### Sebelum Belajar

- Rust Dasar
- Rust Crate
- Rust Unit Test

## #2 Date Time Library

### Date Time Library

- Saat video ini dibuat, library bawaan Rust untuk memanipulasi Date dan Time sangat terbatas, oleh karena itu programmer Rust banyak menggunakan Library lain untuk memanipulasi Date dan Time
- Ada banyak pilihan library yang bisa kita gunakan di Rust untuk Date Time :
- <https://crates.io/crates/chrono>
- <https://crates.io/crates/time>
- <https://crates.io/crates/jiff>
- <https://crates.io/crates/datetime> dan lain-lain
- Pada materi ini, kita akan menggunakan library Chrono

## #3 Membuat Project

### Membuat Project

- Buatlah project dengan nama belajar-rust-datetime menggunakan perintah :
- `cargo new belajar-rust-datetime`

### Menambah Library Chrono

```sh
cargo add chrono
```

## #4 Date

### Date

- Chrono memiliki dua jenis tipe data waktu, tipe data waktu yang memiliki timezone dan yang tidak memiliki timezone
- Sekarang kita akan bahas dulu tipe data yang tidak memiliki timezone
- Dimulai dari tipe data `Date`
- Date adalah tipe data waktu tanggal bulan dan tahun
- Date direpresentasikan dengan struct `NaiveDate` di Chrono
- <https://docs.rs/chrono/0.4.38/chrono/struct.NaiveDate.html>

### Kode: Date

```rust
#[test]
fn test_date() {
	let date: NaiveDate = NaiveDate::from_ymd_opt(2024, 10, 20).unwrap();
	println!("{}", date.year());
	println!("{}", date.month());
	println!("{}", date.day());
}
```

## #5 Duration

### Duration

- Semua tipe data waktu di Chrono adalah Immutable, artinya tidak bisa berubah
- Jadi tidak ada method yang bisa kita gunakan untuk mengubah waktu pada object yang sudah dibuat
- Namun Chrono menyediakan operator Add (+) dan Sub (-) yang bisa kita gunakan untuk menambah/mengurangi waktu
- Namun operator ini hanya bisa digunakan dengan tipe data `Duration`, yaitu alias untuk `TimeDelta`
- <https://docs.rs/chrono/0.4.38/chrono/type.Duration.html>

### Kode: Duration

```rust
#[test]
fn test_duration() {
	let date: NaiveDate = NaiveDate::from_ymd_opt(2024, 10, 30).unwrap();
	let new_date: NaiveDate = date + Duration::days(10);
	println!("{}", new_date);
}
```

## #6 Time

### Time

- Chrono mendukung tipe data Time (jam menit detik nanosecond) tanpa timezone menggunakan struct `NaiveTime`
- <https://docs.rs/chrono/0.4.38/chrono/struct.NaiveTime.html>

### Kode: Time

```rust
#[test]
fn test_time() {
	let time: NaiveDate = NaiveDate::from_hms_milli_opt(10, 30, 0, 500).unwrap();
	println!("{}", time.hour());
	println!("{}", time.minute());
	println!("{}", time.second());
	println!("{}", time.nanosecond());
}
```

## #7 Date Time

- Untuk tipe data gabungan antara Date dan Time, di Chrono bisa menggunakan tipe data `NaiveDateTime`
- <https://docs.rs/chrono/0.4.38/chrono/struct.NaiveDateTime.html>

### Kode: Date Time

```rust
#[test]
fn test_date_time() {
	let date_time: NaiveDateTime = NaiveDateTime::new(
		NaiveDate::from_ymd_opt(2024, 10, 30).unwrap(),
		NaiveDate::from_hms_opt(10, 30, 0).unwrap(),
	);

	println!("{}", date_time.date());
	println!("{}", date_time.time());
}
```

## #8 Time Zone

### Time Zone

- Saat kita menggunakan tipe data tanggal dan waktu, kadang kita akan bersinggungan dengan zona waktu
- Chrono mendukung zona waktu menggunakan `TimeZone`
- <https://docs.rs/chrono/0.4.38/chrono/trait.TimeZone.html>

### Limitasi Library Chrono

- Karena data TimeZone di seluruh dunia sangat banyak, oleh karena itu pada Library Chrono, hanya dibatasi 2 TimeZone saja yang disediakan, yaitu Local dan UTC
- Jika kita ingin menggunakan TimeZone lain, maka kita harus lakukan secara manual menggunakan Offset (perbedaan waktu)
- <https://docs.rs/chrono/0.4.38/chrono/struct.FixedOffset.html>

### Kode: FixedOffset

```rust
#[test]
fn test_fixed_offset() {
	let utc_date_time: NaiveDateTime = NaiveDateTime::new(
		NaiveDate::from_ymd_opt(2024, 10, 30).unwrap(),
		NaiveDate::from_hms_opt(10, 30, 0).unwrap(),
	);

	let asia_jakarta: FixedOffset = FixedOffset::east_opt(7 * 3600).unwrap();
	let asia_jakarta_date_time = asia_jakarta::from_utc_datetime(&utc_date_time);

	println!("{}", utc_date_time);
	println!("{}", asia_jakarta_date_time);
}
```

### Chrono TimeZone

- Chrono menyediakan library terpisah untuk data seluruh TimeZone di dunia, yaitu Chrono `TimeZone`
- <https://crates.io/crates/chrono-tz>
- Kita bisa menginstallnya dengan menggunakan perintah
- `cargo add chrono-tz`

### Kode: TimeZone

```rust
#[test]
fn test_time_zone() {
	let utc_date_time: NaiveDateTime = NaiveDateTime::new(
		NaiveDate::from_ymd_opt(2024, 10, 30).unwrap(),
		NaiveDate::from_hms_opt(10, 30, 0).unwrap(),
	);

	let asia_jakarta = Asia::Jakarta;
	let asia_jakarta_time = asia_jakarta::from_utc_datetime(&utc_date_time);

	println!("{}", utc_date_time);
	println!("{}", asia_jakarta_time);
}
```

## #9 Date Time dengan Time Zone

### Date Time dengan Time Zone

- Sebelumnya kita sudah bahas tentang Date tanpa Time Zone, sekarang kita akan bahas tipe data waktu dan tanggal menggunakan Time Zone
- Date Time yang memiliki Time Zone di Chrono bisa menggunakan tipe data `DateTime`
- <https://docs.rs/chrono/0.4.38/chrono/struct.DateTime.html>

### Kode: Date Time dengan Time Zone

```rust
#[test]
fn test_date_time_with_time_zone() {
	let utc_date_time: DateTime<Utc> = Utc::now();
	let asia_jakarta_date_time: DateTime<Tz> = Asia::Jakarta.from_utc_datetime(&utc_date_time.naive_utc());

	println!("{}", utc_date_time);
	println!("{}", asia_jakarta_time);

	let local_date_time: DateTime<Local> Local::now();
	let asia_jakarta_date_time: DateTime<Tz> = Asia::Jakarta.from_local_datetime(&local_date_time.naive_local()).unwrap();

	println!("{}", local_date_time);
	println!("{}", asia_jakarta_time);
}
```

## #10 Parsing

### Parsing

- Salah satu yang biasa kita lakukan ketika membuat aplikasi adalah, melakukan parsing data String menjadi tipe data waktu/tanggal
- Chrono sudah menyediakan fitur untuk melakukan parsing, sehingga memudahkan kita ketika ingin mengkonversi dari tipe data String menjadi waktu/tanggal
- Untuk melakukan parsing, kita bisa menggunakan method `parse(string, format)` pada tipe data yang ada di Chrono, seperti `NaiveDate`, `NaiveTime`, `NaiveDateTime` dan `DateTime`
- <https://docs.rs/chrono/0.4.38/chrono/struct.DateTime.html#method.parse_from_str>

### Formatting

- Sebelum melakukan parsing, kita harus tahu bagaimana format sintaks untuk melakukan parsing nya
- Kita bisa baca dokumentasinya pada module `strftime`
- <https://docs.rs/chrono/0.4.38/chrono/format/strftime/index.html>

### Kode: Parsing

```rust
#[test]
fn test_parsing() {
	let string = String::from("2024-10-10 10:10:10 +0700");
	let time = DateTime::parse_from_str(&string, "%Y-%m-%d %H:%M:%S %z").unwrap();

	println!("{}", time);
	println!("{}", time.year());
	println!("{}", time.month());
	println!("{}", time.day());
	println!("{}", time.hour());
	println!("{}", time.minute());
	println!("{}", time.second());
	println!("{}", time.nanosecond());
	println!("{}", time.timezone());
}
```

## #11 Format

### Format

- Selain melakukan parsing dari String ke waktu/tanggal, Chrono juga bisa digunakan untuk melakukan proses format (kebalikannya), yaitu mengubah tipe data waktu/tanggal ke String
- Kita bisa menggunakan method format pada semua tipe data yang ada di Chrono
- <https://docs.rs/chrono/latest/chrono/struct.DateTime.html#method.format>

### Kode: Format

```rust
#[test]
fn test_format() {
	let now = Local::now();
	let formatted = now.format("%Y-%m-%d %H:%M:%S %z").to_string();

	println!("{}", formatted);
}
```

## #12 Materi Selanjutnya

### Materi Selanjutnya

- Rust Database
- Rust Web
- Dan lain-lain
