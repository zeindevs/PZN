# Laravel Redis

## Sebelum Balajar

- Kelas PHP dari Programmer Zaman Now
- Kelas Redis dari Programmer Zaman Now
- Kelas Laravel Web
- Kelas Laravel Eloquent

## #1 Laravel Redis

- Di kelas Redis, kita sudah tahu bahwa Redis adalah salah satu database In Memory yang paling populer di dunia
- Laravel, sejak awal bisa diintegrasikan dengan baik dengan database Redis, bahkan banyak fitur di Laravel bisa menggunakan Redis, seperti Cache, Session, sampai Rate Limiting
- Di kelas ini, kita akan bahas bagaimana cara menggunakan seluruh struktur data di Redis menggunakan Laravel
- Dan diakhir kelas, kita akan bahas fitur-fitur apa saja di Laravel yang menggunakan Redis

## #2 Membuat Project

- `composer create-project laravel/laravel=v10.2.9 belajar-laravel-redis`

### Menambah Library Predis

- Secara default, Redis menggunakan library phpredis (native menggunakan C)
- <https://github.com/phpredis/phpredis>
- Namun, di beberapa kasus, kadang sulit menginstall library phpredis karena membutuhkan kompilasi bahasa C
- Oleh karena itu, di kelas ini kita akan mengunakan library Predis, yang hanya menggunakan kode PHP
- https://github.com/predis/predis
- `composer require predis/predis`

## #3 Redis Facade

### Redis Configuration

- Secara default, semua konfigurasi untuk Redis di aplikasi Laravel terdapat di file `config/database.php`
- Kita bisa ubah konfigurasi redis di file tersebut
- Kita bisa ubah yang defaultnya menggunakan phpredis menjadi predis pada konfigurasi file nya

### Kode: Redis Configuration

```php
'redis' => [

	  'client' => env('REDIS_CLIENT', 'phpredis'),

	  'options' => [
	      'cluster' => env('REDIS_CLUSTER', 'redis'),
	      'prefix' => env('REDIS_PREFIX', Str::slug(env('APP_NAME', 'laravel'), '_').'_database_'),
	  ],

	  'default' => [
	      'url' => env('REDIS_URL'),
	      'host' => env('REDIS_HOST', '127.0.0.1'),
	      'username' => env('REDIS_USERNAME'),
	      'password' => env('REDIS_PASSWORD'),
	      'port' => env('REDIS_PORT', '6379'),
	      'database' => env('REDIS_DB', '0'),
	  ],

	  'cache' => [
	      'url' => env('REDIS_URL'),
	      'host' => env('REDIS_HOST', '127.0.0.1'),
	      'username' => env('REDIS_USERNAME'),
	      'password' => env('REDIS_PASSWORD'),
	      'port' => env('REDIS_PORT', '6379'),
	      'database' => env('REDIS_CACHE_DB', '1'),
	  ],
],
```

### Redis Facade

- Untuk menggunakan Redis di Laravel, kita bisa menggunakan Redis Facade yang secara otomatis membaca konfigurasi dari konfigurasi redis di Laravel
- <https://laravel.com/api/10.x/Illuminate/Support/Facades/Redis.html>

### Redis Command

- Untuk mengirim perintah ke Redis, kita bisa menggunakan method `command()` di Redis Facade
- Atau bisa langsung menggunakan nama method sesuai dengan command di Redis
- Redis Facade akan menggunakan Magic Method untuk mengubah nama method secara otomatis menjadi nama perintah yang dikirim ke Redis

### Kode: Ping Redis

```php
class RedisTest extends TestCase
{
	public function testPing()
	{
		$repsonse = Redis::command("ping");
		self::assertEquals("PONG", $repsonse);

		$response = Redis::ping();
		self::assertEquals("PONG", $repsonse);
	}
}
```

## #4 String

- Struktur data yang sering digunakan di Redis adalah String
- Command yang sering kita gunakan adalah menggunakan `set()`, `setEx()`, `get()`, `mGet()`, dan lain-lain

### Kode: String

```php
public function testString()
{
	Redis::setEx("name", 2, "Eko");
	$response = Redis::get("name");
	self::assertEquals("Eko", $repsonse);

	sleep(5);

	$response = Redis::get("name");
	self::assertNull($repsonse);
}
```

## #5 List

- Sekarang kita akan menggunakan struktur data List

### Kode: List

```php
public function testList()
{
	Redis::del("names");
	Redis::rpush("names", "Eko");
	Redis::rpush("names", "Kurniawan");
	Redis::rpush("names", "Khannedy");

	$response = Redis::lrange("names", 0, -1);
	self::assertEquals(["Eko", "Kurniawan", "Khannedy"], $response);

	self::assertEquals("Eko", Redis::lpop("names"));
	self::assertEquals("Kurniawan", Redis::lpop("names"));
	self::assertEquals("Khannedy", Redis::lpop("names"));
}
```

## #6 Set

- Sekarang kita akan menggunakan struktur data Set

### Kode: Set

```php
public function testSet()
{
	Redis::del("names");
	Redis::sadd("names", "Eko");
	Redis::sadd("names", "Eko");
	Redis::sadd("names", "Kurniawan");
	Redis::sadd("names", "Kurniawan");
	Redis::sadd("names", "Khannedy");
	Redis::sadd("names", "Khannedy");

	$response = Redis::smembers("names");
	self::assertEquals(["Eko", "Kurniawan", "Khannedy"], $response);
}
```

## #7 Sorted Set

- Sekarang kita akan menggunakan struktur data Sorted Set

### Kode: Sorted Set

```php
public function testSortedSet()
{
	Redis::del("names");

	Redis::zadd("names", 100, "Eko");
	Redis::zadd("names", 85, "Kurniawan");
	Redis::zadd("names", 95, "Khannedy");

	$response = Redis::zraange("names", 0, -1);
	self::assertEquals(["Kurniawan", "Khannedy", "Eko"], $response);
}
```

## #8 Hash

- Sekarang kita akan menggunakan struktur data Hash

### Kode: Hash

```php
public function testHash()
{
	Redis::del("user:1");
	Redis::hset("user:1", "name", "Eko");
	Redis::hset("user:1", "email", "eko@localhost");
	Redis::hset("user:1", "age", 30);

	$response = Redis::hgetall("user:1");
	self::assertEquals([
		"name" => "Eko",
		"email" => "eko@localhost",
		"age" => "30",
	], $response);
}
```

## #9 Geo Point

- Sekarang kita akan menggunakan struktur data Geo Point

### Kode: Geo Point

```php
public function testGeoPoint()
{
	Redis::del("sellers");
	Redis::geoadd("sellers", 106.822702, -6.177590, "Toko A");
	Redis::geoadd("sellers", 106.820889, -6.1774964, "Toko B");

	$result = Redis::geodist("sellers", "Toko A", "Toko B", "km");
	self::assertEquals(0.3543, $result);

	$result = Redis::geosearch("sellers", new FromLonLat(106.821825, -6.175105), new ByRadius(5, "km"));
	self::assertEquals(["Toko A", "Toko B"], $result);
}
```

## #10 Hyper Log Log

- Sekarang kita akan menggunakan struktur data Hyper Log Log

### Kode: Hyper Log Log

```php
public function testHyperLogLog()
{
	Redis::pfadd("visitors", "eko", "kurniawan", "khannedy");
	Redis::pfadd("visitors", "eko", "budi", "joko");
	Redis::pfadd("visitors", "rully", "budi", "joko");

	$total = Redis::pfcount("visitors");
	self::assertEquals(6, $total);
}
```

## #11 Pipeline

- Di kelas Redis, kita pernah belajar tentang pipeline, dimana kita bisa mengirim beberapa perintah secara langsung tanpa harus menunggu balasan satu per satu dari Redis
- Hal ini juga bisa dilakukan menggunakan Laravel
- Kita bisa menggunakan method `pipeline()`, dimana kita bisa tambahkan callback function yang berisi perintah-perintah yang akan dikerjakan dalam pipeline tersebut

### Kode: Pipeline

```php
public function testPipeline()
{
	Redis::pipeline(function ($pipeline) {
		$pipeline->setex("name", 2, "Eko");
		$pipeline->setex("address", 2, "Indonesia");
	});

	$response = Redis::get("name");
	self::assertEquals("Eko", $repsonse);
	$response = Redis::get("address");
	self::assertEquals("Indonesia", $repsonse);
}
```

## #12 Transaction

- Kita tahu bahwa menggunakan Redis bisa melakukan transaction menggunakan perintah `MULTI` dan `COMMIT`
- Untuk melakukan proses Redis Transaction di Laravel, kita tidak perlu melakukan manual `MULTI` dan `COMMIT`
- Kita bisa menggunakan method `transaction()`, dan cara penggunaannya sama seperti method `pipeline()`

### Kode: Transaction

```php
public function testTransaction()
{
	Redis::transaction(function ($transaction) {
		$transaction->setex("name", 2, "Eko");
		$transaction->setex("address", 2, "Indonesia");
	});

	$response = Redis::get("name");
	self::assertEquals("Eko", $repsonse);
	$response = Redis::get("address");
	self::assertEquals("Indonesia", $repsonse);
}
```

## #13 Pub Sub

- Sekarang kita akan menggunakan fitur PubSub

### Laravel Command

- Untuk melakukan pengetesan Subscriber, kita akan membuat Laravel Command
- Yaitu fitur untuk membuat perintah berbasis terminal
- Perintah itu nanti akan kita gunakan untuk menjalankan Redis Subscriber
- Kita bisa gunakan perintah :
- `php artisan make:command NamaCommand`

### Kode: Subscribe PubSub

```php
class TestSubsciber extends Command
{
	protected $signature = "app:test-subsciber";

	protected $description = "Command description";

	public function handle()
	{
		Redis::subscribe(["channel-1", "channel-2"], function (string $message) {
			echo $message . PHP_EOL;
		});
	}
}
```

### Kode: Publish PubSub

```php
public function testPublish()
{
	for ($i = 0; $i < 10; $i++) {
		Redis::publish("channel-1", "Hello World $i");
		Redis::publish("channel-2", "Good Bye $i");
	}
	self::assertTrue(true);
}
```

## #14 Stream

- Saat dibuatnya video ini, Predis versi 2 belum mendukung tipe data STREAM
- Tapi di phpredis mendukung Stream, jadi saya akan praktekan menggunakan phpredis

### Kode: Publish Stream

```php
public function testPublishStream()
{
	for ($i = 0; $i < 10; $i++) {
		Redis::xadd("members", "*", [
			"name" => "Eko $i",
			"address" => "Indonesia",
		]);
	}
	self::assertTrue(true);
}
```

### Kode: Create Consumer

```php
public function testCreateConsumerGroup()
{
	Redis::xgroup("create", "members", "group1", "0");
	Redis::xgroup("createconsumer", "members", "group1", "consumer-1");
	Redis::xgroup("createconsumer", "members", "group1", "consumer-2");
	self:;assertTrue(true);
}
```

### Kode: Get Stream

```php
public function testConsumerStream()
{
	$result = Redis::xreadgroup("group1", "consumer-1", ["members" => ">"], 5, 3000);

	echo json_encode($result, JSON_PRETTY_PRINT);
	self.assertNotNull($result);
}
```

## #15 Cache

### Laravel Cache

- Laravel memiliki fitur bernama Cache untuk menyimpan data sementara
- Cache biasanya digunakan untuk menyimpan data secara sementar, agar tidak perlu mengambil data dari sumber aslinya (misal database atau aplikasi lain)
- Tujuan Cache adalah mempercepat proses, agar data yang sering diakses, tidak perlu diambil dari sumbernya yang lambat, karena Cache lebih cepat

### Redis

- Larave sebenarnya menyediakan tempat menyimpan Cache banyak, bisa Database, Redis, Memcached dan DynamoDB
- Kita akan coba gunakan Redis sebagai tempat penyimpanan Cache
- Kita bisa ubah konfigurasi Cache di file `config/cache.php`
- Saat melakukan unit test, di `phpunit.xml`, konfigurasi `CACHE_DRIVER` diubah menjadi array, pastikan diubah menjadi redis jika ingin menggunakan redis

### Cache Facade

- Melakukan Cache di Laravel cukup mudah, kita bisa menggunakan Cache Facade
- <https://laravel.com/api/10.x/Illuminate/Support/Facades/Cache.html>
- Banyak method yang bisa digunakan ketika menggunakan Cache Facade, intinya untuk menyimpan, mengambil dan menghapus data dari Cache

### Kode: Cache Test

```php
public function testCache()
{
	Cache::put("name", "Eko", 2);
	Cache::put("country", "Indonesia", 2);

	$response = Cache::get("name");
	self::assertEquals("Eko", $repsonse);
	$response = Cache::get("country");
	self::assertEquals("Indonesia", $repsonse);

	sleep(5);
	$response = Cache::get("name");
	self::assertNull($repsonse);
	$response = Cache::get("country");
	self::assertNull($repsonse);
}
```

## #16 Session

- Secara default, Session di Laravel akan disimpan di File
- Masalah dengan disimpan di File adalah, ketika nanti kita menjalankan Laravel di beberapa Server, maka data Session akan bermasalah, karena tidak sinkron antar aplikasi Laravel
- Laravel bisa menyimpan data Session di Redis
- Kita bisa mengubah konfigurasi session di file `config/session.php`
- Saat menjalankan unit test, file `phpunitt.xml` mengubah `SESSION_DRIVER` menjadi array, jadi kita harus ubah menjadi redis jika ingin menggunakan Redis di unit test

### Kode: Session Controller

```php
class SessionController extends Controller
{
	function set(Request $request)
	{
		$name = $request->query("name");
		$request->session()->put($name, [
			"name" => fake()->name(),
			"country" => fake()->country(),
		]);
		return response()->json([
			"status" => "success",
		]);
	}

	function get(Request $request)
	{
		$name = $request->query("name");
		$value = $request->session()->get($name);
		return response()->json($value);
	}
}

```

## #17 Rate Limiting

- Laravel memiliki fitur bernama Rate Limiting, fitur ini digunakan untuk membatasi jumlah eksekusi
- Secara default Rate Limiting menyimpan data di Cache, artinya jika Cache menggunakan Redis, maka secara otomatis informasi Rate Limiting juga akan disimpan di Redis
- Untuk menggunakan Rate Limiting, kita bisa menggunakan Facade `RateLimiter`
- <https://laravel.com/api/10.x/Illuminate/Support/Facades/RateLimiter.html>

### Kode: Rate Limiting

```php
public function testRateLimiter()
{
	$success = RateLimiter::attempt("send-message-1", 100, function() {
		echo "send-message-1";
	});

	$this->assertTrue($success);
}
```

## #18 Penutup
