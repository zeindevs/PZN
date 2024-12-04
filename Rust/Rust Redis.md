# Rust Redis

## Sebelum Belajar

- Rust Dasar
- Rust Unit Test
- Rust Concurrency
- Redis

## #1 Rust Redis

### Rust Redis

- Redis adalah salah satu In Memory database yang sangat populer, dan banyak digunakan
- Memahami Redis sekarang sudah menjadi salah satu hal yang sangat diperlukan, terutama untuk meningkatkan performa aplikasi yang kita buat
- Di kelas ini, kita akan belajar bagaimana berkomunikasi dengan Redis menggunakan Rust

### Rust Redis Client

- Untuk terkoneksi ke Redis, kita butuh library Redis Client, hal ini karena Rust tidak memiliki library bawaan untuk terkoneksi ke Redis
- <https://crates.io/crates/redis>
- <https://docs.rs/redis/0.27.5/redis/>

## #2 Membuat Project

### Membuat Project

- `cargo new belajar-rust-redis`

### Menambah Library

- `cargo add tokio --features full`
- `cargo add futures`
- `cargo add redis --features tokio-comp`

## #3 Client

### Client

- Hal pertama yang perlu kita lakukan saat ingin menggunakan Redis dari Rust, adalah membuat koneksi ke Redis
- Untuk membuat koneksi ke Redis, kita perlu membuat object Client
- <https://docs.rs/redis/0.27.5/redis/struct.Client.html>

### Kode: Client

```rust
#[test]
fn test_connection() {
	let mut client = Client::open("redis://localhost:6379/").unwrap();

	let _: () = client.set("name", "Eko").unwrap();
	let value: String = client.get("name").unwrap();

	println!("{}", value);
}
```

### Async Client

- Library redis juga menyediakan fitur Async jika kita ingin menggunakan Rust Async IO
- Kita bisa pilih library Async yang akan kita gunakan, contohnya disini kita akan menggunakan `Tokio`

### Kode: Async Client

```rust
async fn get_client() -> Result<MultiplexedConnection, RedisError> {
	let client = Client::open("redis://localhost:6379/");
	client.get_multiplexed_tokio_connection().await
}

#[tokio::test]
async fn test_async_connection() Result<(), RedisError> {
	let mut con = get_client().await?;
	let _: () = con.set("name", "Eko").await?;
	let value: String = con.get("name").await?;

	println!("{}", value);

	Ok(())
}
```

## #4 String

### String

- Struktur data yang sering digunakan di Redis adalah String
- Nama-nama method nya hampir sama dengan perintah-perintah di Redis
- <https://redis.io/docs/latest/commands/?group=string>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: String

```rust
#[tokio::test]
async fn test_string() -> Result<(), RedisError> {
	let mut con = get_client().await?;
	let _: () = con.set_ex("name", "Eko", 2).await?;
	let value: String = con.get("na").await?;
	println!("{}", value);

	tokio::time::sleep(Duration::from_secs(5)).await?;

	let values: Result<String, RedisError> = con.get("name").await?;
	assert_eq!(true, value.is_err());

	Ok(())
}
```

## #5 List

### List

- Sekarang kita akan menggunakan struktur data List di Redis
- <https://redis.io/docs/latest/commands/?group=list>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: List

```rust
let mut con = get_client().await?;

let _: () = con.del("names").await?;
let _: () = con.rpush("names", "Eko").await?;
let _: () = con.rpush("names", "Kurniawan").await?;
let _: () = con.rpush("names", "Khannedy").await?;

let len: i32 = con.llen("names").await?;
assert_eq!(3, len);

let names: Vec<String> = con.lrange("names", 0, -1).await?;
assert_eq!(vec!["Eko", "Kurniawan", "Khannedy"], names);

let name: Vec<String> = con.lpop("names", NonZero::new(1)).await?;
assert_eq!(vec!["Eko"], name);
let name: Vec<String> = con.lpop("names", NonZero::new(1)).await?;
assert_eq!(vec!["Kurniawan"], name);
let name: Vec<String> = con.lpop("names", NonZero::new(1)).await?;
assert_eq!(vec!["Khannedy"], name);
```

## #6 Set

### Set

- Sekarang kita akan menggunakan struktur data Set di Redis
- <https://redis.io/docs/latest/commands/?group=set>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Set

```rust
let mut con = get_client().await?;

let _: () = con.del("names").await?;
let _: () = con.sadd("names", "Eko").await?;
let _: () = con.sadd("names", "Eko").await?;
let _: () = con.sadd("names", "Kurniawan").await?;
let _: () = con.sadd("names", "Kurniawan").await?;
let _: () = con.sadd("names", "Khannedy").await?;
let _: () = con.sadd("names", "Khannedy").await?;

let len: i32 = con.scard("names").await?;
assert_eq!(3, len);

let names: Vec<String> = con.smembers("names").await?;
assert_eq!(vec!["Eko", "Kurniawan", "Khannedy"], names);
```

## #7 Sorted Set

### Sorted Set

- Sekarang kita akan menggunakan struktur data Sorted Set di Redis
- <https://redis.io/docs/latest/commands/?group=sorted-set>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Sorted Set

```rust
let mut son = get_client().await?;

let _: () = con.del("names").await?;
let _: () = con.radd("names", "Eko", 100).await?;
let _: () = con.radd("names", "Kurniawan", 85).await?;
let _: () = con.radd("names", "Khannedy", 95).await?;

let len: i32 = con.zcard("names").await?;
assert_eq!(3, len);

let names: Vec<String> = con.zrange("names", 0, -1).await?;
assert_eq!(vec!["Kurniawan", "Khannedy", "Eko"], names);

Ok(())
```

## #8 Hash

### Hash

- Sekarang kita akan menggunakan struktur data Hash di Redis
- <https://redis.io/docs/latest/commands/?group=hash>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Hash

```rust
#[tokio::test]
async fn test_hash() -> Result<(), RedisError> {
	let mut con = get_client().await?;

	let _: () = con.del("user:1").await?;
	let _: () = con.hset("user:1", "id", "1").await?;
	let _: () = con.hset("user:1", "name", "Eko").await?;
	let _: () = con.hset("user:1", "email", "eko@gmail.com").await?;

	let user: HashMap<String, String> = con.hgetall("user:1").await?;
	assert_eq!("1", user.get("id").unwrap());
	assert_eq!("Eko", user.get("name").unwrap());
	assert_eq!("eko@gmail.com", user.get("email").unwrap());

	Ok(())
}
```

## #9 Geo Point

### Geo Point

- Sekarang kita akan menggunakan struktur data Geo Point di Redis
- <https://redis.io/docs/latest/commands/?group=geo>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Geo Point

```rust
#[tokio::test]
async fn test_geo_point() -> Result<(), RedisError> {
	let mut con = get_client().await?;

	let _: () = con.del("sellers").await?;
	let _: () = con.geo_add("sellers", (106.822782, -6.177590, "Toko A")).await?;
	let _: () = con.geo_add("sellers", (106.828889, -6.174964, "Toko B")).await?;

	let distance: f64 = con.geo_dist("sellers", "Toko A", "Toko B", Unit::Kilometers).await?;
	assert_eq!(8.3543, distance);
	Ok(())
}
```

## #10 Hyper Log Log

### Hyper Log Log

- Sekarang kita akan menggunakan struktur data Hyper Log Log di Redis
- <https://redis.io/docs/latest/commands/?group=hyperloglog>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Hyper Log Log

```rust
#[tokio::test]
async fn test_hyper_log_log() {
	let mut con = get_client().await?;

	let _: () = con.del("visitors").await?;
	let _: () = con.pfadd("visitors", ("eko", "kurniawan", "Khannedy")).await?;
	let _: () = con.pfadd("visitors", ("eko", "budi", "joko")).await?;
	let _: () = con.pfadd("visitors", ("budi", "joko", "rully")).await?;

	let total: i32 = con.pfcount("visitors").await?;
	assert_eq!(6, total);

	Ok(())
}
```

## #11 Pipeline

### Pipeline

- Di kelas Redis, kita pernah belajar tentang pipeline, dimana kita bisa mengirim beberapa perintah secara langsung tanpa harus menunggu balasan satu per satu dari Redis
- Hal ini juga bisa dilakukan menggunakan Rust menggunakan method `redis::pipe()`
- Method `pipe()` akan mengembalikan object Pipeline yang bisa kita gunakan sebagai redis client yang akan dieksekusi semuanya menggunakan pipeline ketika memanggil method `exec_async()`
- <https://docs.rs/redis/0.27.5/redis/struct.Pipeline.html>

### Kode: Pipeline

```rust
#[tokio::test]
async fn test_pipeline() {
	let mut con = get_client().await?;

	redis::pipe()
		.set_ex("name", "Eko", 2)
		.set_ex("address", "Indonesia", 2)
		.exec_async(&mut con).await?;

	let name: String = con.get("name").await?;
	assert_eq("Eko", name);

	let address: String = con.get("address").await?;
	assert_eq("Indonesia", address);

	Ok(())
}
```

## #12 Transaction

### Transaction

- Kita tahu bahwa menggunakan Redis bisa melakukan transaction menggunakan perintah `MULTI` dan `EXEC`
- Untuk menggunakan fitur Transaction, kita bisa menggunakan pipeline seperti sebelumnya, namun di awal pipeline kita perlu memanggil method `atomic()`
- <https://docs.rs/redis/0.27.5/redis/struct.Pipeline.html#method.atomic>

### Kode: Transaction

```rust
#[tokio::test]
async fn test_transaction() -> Result<(), RedisError> {
	let mut client = get_client().await?;

	redis::pipe()
		.atomic()
		.set_ex("name", "Eko", 2)
		.set_ex("address", "Indonesia", 2)
		.exec_async(&mut con).await?;

	let name: String = con.get("name").await?;
	assert_eq("Eko", name);

	let address: String = con.get("address").await?;
	assert_eq("Indonesia", address);

	Ok(())
}
```

## #13 Stream

### Stream

- Sekarang kita akan menggunakan struktur data Stream di Redis
- <https://redis.io/docs/latest/commands/?group=stream>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Publish Stream

```rust
#[tokio::test]
async fn test_publish_stream() -> Result<(), RedisError> {
	let mut client = get_client().await?;

	for i in 0..10 {
		let mut map: HashMap<&str, String> = HashMap::new();
		map.insert("name", format!("Eko {}", i));
		map.insert("address", "Indonesia".to_string());

		let _: () = client.exec_map("members", "*", &map).await?;
	}
	Ok(())
}
```

### Kode: Create Consumer

```rust
#[tokio::test]
async fn test_create_consumer() -> Result<(), RedisError> {
	let mut client = get_client().await?;

	let _: () = client.xgroup_create("members", "group-1", "0").await?;
	let _: () = client.xgroup_createconsumer("members", "group-1", "consumer-1").await?;
	let _: () = client.xgroup_createconsumer("members", "group-1", "consumer-1").await?;

	Ok(())
}
```

### Kode: Get Stream

```rust
#[tokio::test]
async fn test_get_stream() -> Result<(), RedisError> {
	let mut con = get_client().await?;

	let setting = StreamReadOptions::default().group("group-1", "consumer-1").count(2).block(1000);
	let result: StreamReadReply = client.xread_options(&["members"], &[">"], &setting).await?;

	for key in result.keys {
		for item in key.ids {
			let map: HashMap<String, Value> = item.map;
			let name: String = match map.get("name").unwrap() {
				Value::BulkString(value) => String::from_utf8(value.to_vec())?,
				_ => "".to_string()
			};
			let address: String =  match map.get("address").unwrap() {
				Value::BulkString(value) => String::from_utf8(value.to_vec())?,
				_ => "".to_string()
			};
			println!("{:?}", name);
			println!("{:?}", address);
		}
	}

	Ok(())
}
```

## #14 Pub Sub

### Pub Sub

- Sekarang kita akan menggunakan fitur PubSub di Redis
- <https://redis.io/docs/latest/commands/?group=pubsub>
- <https://docs.rs/redis/0.27.5/redis/trait.AsyncCommands.html>

### Kode: Subscribe PubSub

```rust
#[tokio::test]
async fn name() -> Result<(), RedisError> {
	let client = Client::open("redis://loclhost:6379/")?;
	let mut pubsub = client.get_async_pubsub().await?;

	let _: () = pubsub.subscribe("members").await?;
	let mut pubsub_stream = pubsub.on_message();

	let message: String = pubsub_stream.next().await.unwrap().get_payload()?;
	println!("{:?}", message);

	Ok(())
}
```

### Kode: Publish PubSub

```rust
#[tokio::test]
async fn name() -> Result<(), RedisError> {
	let mut client = get_client().await?;

	let _: () = client.publish("members", "Hello PubSub").await?;

	Ok(())
}
```

## #15 Materi Selanjutnya

### Materi Selanjutnya

- Rust Web
- Dan lain-lain
