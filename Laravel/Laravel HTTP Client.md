# Laravel HTTP Client

## Sebelum Belajar

- Kelas PHP dari Programmer Zaman Now
- Laravel RESTful API

## #1 Membuat Project

- `composer create-project laravel/laravel=v10.2.9 belajar-laravel-http-client`

## #2 HTTP Client

- Laravel memiliki fitur bernama HTTP Client
- Fitur ini digunakan untuk mempermudah kita melakukan request HTTP

### Http Facade

- Untuk menggunakan fitur HTTP Client di Laravel, kita bisa menggunakan Facade Http
- Ada banyak sekali method yang bisa kita gunakan ketika menggunakan Facade Http
- <https://laravel.com/api/10.x/Illuminate/Support/Facades/Http.html>

### Guzzle

- Laravel HTTP Client secara low level menggunakan Guzzle
- Guzzle adalah salah satu library HTTP Client untuk PHP yang sangat populer
- Kita tidak perlu mengerti Guzzle jika ingin menggunakan Laravel HTTP Client
- <https://github.com/guzzle/guzzle>

## #3 HTTP Method

- Saat kita melakukan request HTTP, hal yang pasti kita tentukan adalah jenis HTTP Method yang akan kita lakukan
- Http Facade sangat mudah digunakan, untuk mengirim request HTTP, kita cukup gunakan nama function sesuai dengan nama HTTP Method nya

### Http Facade Function

| Http Function | Keterangan  |
| ------------- | ----------- |
| `get(url)`    | HTTP GET    |
| `post(url)`   | HTTP POST   |
| `put(url)`    | HTTP PUT    |
| `patch(url)`  | HTTP PATCH  |
| `delete(url)` | HTTP DELETE |
| `head(url)`   | HTTP HEAD   |

### Kode: Http Method

```php
public function testSimpleGet()
{
	// mock http using https://public.requestbin.com

	$response = Http::get("https://enbnhzz1he30n.x.pipedream.net/");
	self::assertTrue($response->ok());
}
```

## #4 Response

- Setiap kita menggunakan function http method di Facade Http, maka akan menghasilkan object Response
- Kita bisa melihat informasi dari HTTP Response pada object Response tersebut
- <https://laravel.com/api/10.x/Illuminate/Http/Client/Response.htmz>

### Kode: Response

```php
public function testResponse()
{
	$response = Http::get("https://enbnhzz1he30n.x.pipedream.net/");
	self::assertEquals(200, $response->status());
	self::assertNotNull($response->headers());
	self::assertNotNull($response->body());

	$json = $response->json();
	self::assertTrue($json['success']);
}
```

## #5 Query Parameter

- Untuk menambahkan query parameter ke request HTTP yang akan kita lakukan, kita bisa menggunakan function `withQueryParameters(array)`

### Kode: Query Parameters

```php
public function testQueryParameter()
{
	$response = Http::withQueryParameters([
		'page' => 1,
		'limit' => 10,
	])->get('https://enbnhzz1he30n.x.pipedream.net/');

	self::assertTrue($response->ok());
}
```

## #6 Header

- Untuk menambahkan header ke request HTTP yang akan kita lakukan, kita bisa menggunakan function `withHeaders(array)`

### Kode: Header

```php
public function testHeader()
{
	$response = Http::withQueryParameters([
		'page' => 1,
		'limit' => 10,
	])->withHeaders([
		'Accept' => 'application/json',
		'X-Request-Id' => '1234567890',
	])->get('enbnhzz1he30n.x.pipedream.net/');

	self::assertTrue($response->ok());
}
```

## #7 Cookie

- Untuk menambahkan cookie ke request HTTP yang akan kita lakukan, kita bisa menggunakan function `withCookies(array, domain)`

### Kode: Cookie

```php

public function testCookie()
{
	$response = Http::withQueryParameters([
		'page' => 1,
		'limit' => 10,
	])->withHeaders([
		'Accept' => 'application/json',
		'X-Request-Id' => '1234567890',
	])->withCookie([
		'SessionId' => '1234567890',
		'UserId': "1",
	], 'enbnhzz1he30n.x.pipedream.net')
		->get('https://enbnhzz1he30n.x.pipedream.net/');

		self::assertTrue($response->ok());
}
```

## #8 Form Post

- Untuk mengirim request dalam bentuk Form Request, kita bisa menggunakan function `asForm()`, lalu datanya dikirim ketika kita menggunakan function `post(url, form)`

### Kode: Form Post

```php
public function testFormPost()
{
	$response = Http::asForm()->post('https://enbnhzz1he30n.x.pipedream.net/'. [
		'username' => 'admin',
		'password' => '123456',
	]);

	self::assertTrue($response->ok());
}
```

## #9 Multipart

- Untuk mengirim HTTP Request jenis Multipart, seperti upload file, kita bisa menggunakan function `asMultipart()`
- Lalu untuk mengirim file, kita bisa gunakan function `attach(key, content, name)`
- Dan untuk data bukan file, kita bisa gunakan cara seperti Form Post menggunakan `post(url, form)`

### Kode: Multipart

```php
public function testMultipart()
{
	$response = Http::asMultipart()
		->attach('profile', file_get_contents(__DIR__, '/../../responses/pzn.jpg', 'profile.jpg'))
		->post('https://enbnhzz1he30n.x.pipedream.net/', [
			'username' => 'admin',
			'password' => '123456',
		]);

	self::assertTrue($response->ok());
}
```

## #10 JSON

- Untuk mengirim request dalam bentuk JSON, kita bisa menggunakan function `asJson()`
- Data JSON bisa dikirim di parameter body milik `post(url, body)`, `put(url, body)` atau `patch(url, body)`

### Kode: JSON

```php
public function testJson()
{
	$response = Http::asJson()
		->post('https://enbnhzz1he30n.x.pipedream.net/', [
			'username' => 'admin',
			'password' => '123456',
		]);

	self::assertTrue($response->ok());
}
```

## #11 Timeout

- Saat kita melakukan request, kadang ada baiknya kita menentukan waktu timeout
- Timeout adalah waktu menyerah ketika menunggu response
- Hal ini biasanya dilakukan agar aplikasi kita response nya tidak terlalu lambat, karena harus menunggu response dari aplikasi lain yang terlalu lama
- Kita bisa gunakan function `timeout(second)` untuk menentukan berapa lama waktu timeout nya

### Kode: Timeout

```php
public function testJson()
{
	$response = Http::timeout(1)->asJson()
		->post('https://enbnhzz1he30n.x.pipedream.net/', [
			'username' => 'admin',
			'password' => '123456',
		]);

	self::assertTrue($response->ok());
}
```

## #12 Retry

- Laravel HTTP Client juga memiliki fitur untuk melakukan retry, jadi misal ketika terjadi error, kita bisa meminta HTTP Client untuk melakukan retry menggunakan function `retry(times, sleep)`
- Ketika kita tambahkan informasi retry, secara otomatis Laravel HTTP Client akan mencoba melakukan request lagi sejumlah total times yang kita tentukan

### Kode: Retry

```php
public function testRetry()
{
	$response = Http::timeout(1)->retry(5, 1000)->asJson()
		->post('https://enbnhzz1he30n.x.pipedream.net/', [
			'username' => 'admin',
			'password' => '123456',
		]);

	self::assertTrue($response->ok());
}
```

## #13 Throw Error

- Laravel HTTP Client, ketika mendapatkan response dengan status code bukan 2xx, dia tidak akan throw `Error`
- Namun, jika kita ingin menjadikan ketika mendapat response 4xx (client error) atau 5xx (server error), kita bisa menggunakan function `throw()` pada Response
- Jika error, maka akan mengembalikan error `RequestException`

### Kode: Throw Error

```php
public function testException()
{
	$this->assertThrows(function () {
		$response = Http::get('https://enbnhzz1he30n.x.pipedream.net/not-found');
		$response->throw();
	}, RequestException::class);
}
```

## #14 Penutup
