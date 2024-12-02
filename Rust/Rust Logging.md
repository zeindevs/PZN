# Rust Logging

## Sebelum Belajar

- Rust Dasar
- Rust Unit Test

## #1 Logging

### Logging

- Logging adalah proses memberi informasi kejadian yang sedang terjadi pada aplikasi
- Informasi yang diberikan dalam proses logging biasanya memiliki kategori/level yang bisa berbeda-beda
- Rust memiliki fitur untuk melakukan logging
- Sebelumnya, untuk memberi informasi, kita biasanya hanya menggunakan `println!()`

### Rust Logging

- Rust memiliki fitur untuk logging yang dibuat dalam Crate Log
- <https://crates.io/crates/log>
- Namun Crate Log tersebut hanyalah kontrak untuk melakukan logging, untuk implementasinya sendiri kita perlu memilih Crate lainnya
- Salah satu keuntungan menggunakan kontrak adalah, kita bisa berganti-ganti implementasi, tanpa harus mengubah kode logging nya

## #2 Membuat Project

### Membuat Project

- `cargo new belajar-rust-logging`

### Menambah Library Log

- `cargo add log`

## #3 Level

### Level

- Saat melakukan log, kita perlu menentukan level informasi yang akan kita log
- Rust Log mendukung beberapa level, dan level itu bertingkat jadi kita akan memilih log yang akan kita buat di tingkat level mana
- <https://docs.rs/log/0.4.22/log/enum.Level.html>

### Melakukan Log

- Untuk melakukan log, kita bisa menggunakan macro `log!(level, log)`
- <https://docs.rs/log/0.4.22/log/macro.log.html>
- Atau kita bisa gunakan shortcut macro
- `error!(log)` untuk level error
- `warn!(log)` untuk level warn
- `info!(log)` untuk level info
- `debug!(log)` untuk level debug
- `trace!(log)` untuk level trace
- <https://docs.rs/log/0.4.22/log/index.html#macros>

### Kode: Level

```rust
#[cfg(test)]
mod tests {
	use log::{debug, error, info, trace, warn};

	#[test]
	fn it_works() {
		error!("This a a error");
		warn!("This a a warn");
		info!("This a a info");
		debug!("This a a debug");
		trace!("This a a trace");
	}
}
```

## #4 Simple Logger

### Simple Logger

- Karena Crate Log hanya kontrak, maka kita harus pilih implementasinya
- Salah satu implementasi yang sederhana adalah Env Logger
- Env Logger bisa digunakan untuk menampilkan log ke Console / Terminal, dan level yang akan diaktifkan bisa di set via Env Variable sistem operasi
- <https://crates.io/crates/env_logger>

### Menambah Env Logger

- `cargo add env_logger`

### Kode: Env Logger

```rust
#[cfg(test)]
mod tests {
	use log::{debug, error, info, trace, warn};

	#[test]
	fn it_works() {
		env_logger::init();

		error!("This a a error");
		warn!("This a a warn");
		info!("This a a info");
		debug!("This a a debug");
		trace!("This a a trace");
	}
}
```

### Menjankan Env Logger

- Ubah Env Variable di sistem operasi dengan nama `RUST_LOG=level`

### Kode: Menjalankan Log

```sh
export RUST_LOG=info

cargo test -- --show-output
```

## #5 Complex Logger

### Complex Logger

- Env Logger hanya bisa digunakan untuk menampilkan log ke Console, bagaimana jika kita ingin menampilkan log ke tempat lain? Misal ke file
- Atau mengatur level tergantung module nya?
- Kita bisa menggunakan implementasi Logger yang lebih kompleks, contohnya adalah `log4rs`
- <https://crates.io/crates/log4rs>

### Menambah Library Log4rs

- `cargo add log4rs`

## #6 Configuration

### Configuration

- Untuk menggunakan Log4rs, kita bisa menyimpan semua konfigurasinya menggunakan file konfigurasi `yaml`
- Selanjutnya kita bisa baca file konfigurasi yaml tersebut menggunakan library `Log4rs`

### Kode: log4rs.yaml

```yaml
appenders:
	stdout:
		kind: console
root:
	level: info
	appenders:
		- stdout
```

### Kode: Menggunakan Log4rs

```rust
#[cfg(test)]
mod tests {
	use log::{debug, error, info, trace, warn};

	#[test]
	fn it_works() {
		log4rs::init_file("log4rs.yaml", Default::default()).unwrap();

		error!("This a a error");
		warn!("This a a warn");
		info!("This a a info");
		debug!("This a a debug");
		trace!("This a a trace");
	}
}
```

## #7 Appender

### Appender

- Pada kode konfigurasi sebelumnya, kita membuat appender dengan nama `stdout` dengan jenis `console`
- Appender adalah lokasi log akan disimpan, contoh kita buat dengan nama `stdout` dengan tujuan `console`
- Log4rs mendukung beberapa pilihan appender
- <https://docs.rs/log4rs/1.3.0/log4rs/config/index.html#appenders>

### Kode: Configuration File

```yaml
appenders:
	stdout:
		kind: console
	app_file:
		kind: file
		path: application.log
		append: true
root:
	level: info
	appenders:
		- stdout
		- app_file
```

## #8 Logger

### Logger

- Salah satu kelebihan Log4rs adalah, kita bisa mudah mengubah Level untuk module-module tanpa harus mengubah kode program
- Kita hanya perlu mengubah file konfigurasinya saja
- <https://docs.rs/log4rs/1.3.0/log4rs/config/index.html#loggers>

### Kode: Configuration File

```yaml
appenders:
	stdout:
		kind: console
	app_file:
		kind: file
		path: application.log
		append: true

loggers:
	belajar_rust_log::tests:
		level: trace
	belajar_rust_log::tests2:
		level: info

root:
	level: info
	appenders:
		- stdout
		- app_file
```

## #9 Materi Selanjutnya

### Materi Selanjutnya

- Rust Web
- Dan lain-lain
