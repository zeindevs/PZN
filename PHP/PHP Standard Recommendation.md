# PHP Standard Recommendation

## Sebelum Belajar

- PHP Composer
- PHP Web

## #1 Pengenalan PSR

- PSR (PHP Standard Recommendation) merupakan rekomendasi untuk standarisasi library atau framework di PHP
- Salah satu tujuan dari PSR ini adalah membuat standarisasi yang sama untuk tiap kebutuhan di library atau framework di PHP
- PSR hanya menyediakan rekomendasi dan interface, untuk implementasinya dilakukan oleh library atau framework-nya
- Salah satu keuntungan menggunakan PSR adalah, kita bisa mengubah-ubah library atau framework tanpa takut mengubah terlalu banyak kode, karena sudah mengikuti standarisasi
- <https://www.php-fig.org/psr/>

### Daftar PSR

- Saat ini sudah ada banyak PSR yang sudah disetujui dan ada juga yang masih dalam tahap diskusi (belum final)
- <https://www.php-fig.org/psr/>
- Di materi ini, kita akan bahas beberapa PSR yang sering digunakan

## #2 Membuat Project

- Buatlah folder belajar-php-psr
- Lalu gunakan perintah :
- `composer init`
- Install PHP Unit :
- `composer require --dev phpunit/phpunit:^10`
- Tambahkan `autoload-dev` untuk folder test
- Gunakan PHP 8.3 :
- `composer require php:^8.3`

## #3 Coding Standard

### PSR-1: Basic Coding Standard

- PSR-1 Basic Coding Standard adalah standarisasi dari apa yang harus dianggap sebagai elemen pengkodean standar yang diperlukan untuk memastikan kesamaan gaya pembuatan kode PHP.
- <https://www.php-fig.org/psr/psr-1/>

### PSR-12: Extended Coding Style

- PSR-12: Extended Coding Style merupakan perluasan dari gaya pembuatan kode PHP yang direkomendasikan untuk dilakukan ketika membuat kode PHP sehingga antar programmer bisa menggunakan gaya pembuatan kode yang sama dan konsisten
- <https://www.php-fig.org/psr/psr-12/>

## #4 Logger

### PSR-3: Logger Interface

- PSR ini digunakan sebagai standarisasi untuk logging library
- Tujuan utama dari PSR ini adalah sebagai standarisasi kode library berupa `LoggerInterface` sebagai object untuk melakukan logging.
- Logging library perlu membuat implementasi dari `LoggerInterface` sebagai logger sehingga bisa kompatibel dengan berbagai logging library
- <https://www.php-fig.org/psr/psr-3/>

### Implementasi PSR-3: Logger Interface

- Salah satu library yang bisa kita gunakan sebagai implementasi dari PSR-3: Logger Interface adalah Monolog
- <https://github.com/Seldaek/monolog>
- Untuk menginstall monolog, kita bisa gunakan perintah :
- `composer require monolog/monolog`

### Kode: User Service

```php
namespace Khannedy\BelajarPhpPsr;

use Psr\Log\LoggerAwareTrait;
use Psr\Log\LogLevel;

class UserService
{
	use LoggerAwareTrait;

	public function save(string $name): void
	{
		$this->logger?->log(LogLevel::INFO, "User {$name} is saved");
	}
}
```

### Kode: Test User Service

```php
public function testSave()
{
	$logger = new Logger("belajar-php-psr");
	$streamHandler = new \Monolog\Handler\StreamHandler("php://stdout");
	$streamHandler->setFormatter(new JsonFormatter());
	$logger->pushHandler($streamHandler);

	$service = new UserService();
	$service->setLogger($logger);
	$service->save("Eko");

	self::assertNotNull($logger);
}
```

## #5 HTTP Server

### PSR-7: HTTP Message Interface

- PSR ini merupakan representasi dari HTTP Messages seperti HTTP Request dan HTTP Response
- <https://www.php-fig.org/psr/psr-7/>

### Implementasi HTTP Server

- Salah satu implementasi dari PSR untuk HTTP Message Interface dan HTTP Server adalah :
- <https://packagist.org/packages/nyholm/psr7>
- <https://packagist.org/packages/nyholm/psr7-server>

### Kode: HTTP Request

```php
<?php

require_once __DIR_ . "/../vendor/autoload.php";

$factory = new \Nyholm\Psr7\Factory\Psr1Factory();
$creator = new \Nyholm\Psr7\Factory\ServerRequestCreator($factory, $factory, $factory, $factory);

$request = $creator->fromGlobals();
var_dump($request);
```

### HTTP Response Emitter

- Menurut PSR-7, HTTP Response dibuat dalam bentuk `ResponseInterface`
- Namun, artinya kita harus mengubah object `ResponseInterface` menjadi HTTP Response menggunakan PHP
- Kita bisa gunakan library untuk melakukan ini secara mudah
- <https://packagist.org/packages/laminas/laminas-httphandlerrunner>

### Kode: HTTP Response

```php
$response = new \Nyholm\Psr7\Response(
	status: 200,
	headers: [
		"Content-Type": "application/json"
	],
	body: $factory->createStream(json_encode([
		"message" => "Hello, World!"
	]))
);

$emitter = new \Laminas\HttpHandlerRunner\Emitter\SapiEmitter();
$emitter->emit($response);
```

## #6 HTTP Client

### PSR-18: HTTP Client

- PSR ini merupakan standarisasi untuk melakukan HTTP Request ke Server
- <https://www.php-fig.org/psr/psr-18/>

### Implementasi HTTP Client

- Salah satu implementasi untuk PSR HTTP Client adalah :
- <https://packagist.org/packages/guzzlehttp/guzzle>

### Kode: HTTP Client

```php
$stream = \GuzzleHttp\Psr7\Utils::streamFor(json_encode([
	"username": "admin",
	"password": "admin"
]));

$request = new \GuzzleHttp\Psr7\Request(
	method: "POST",
	uri: "https://en5r7ltxv3tvo.x.pipedream.net",
	headers: [
		"Content-Type": "application/json"
	],
	body: $stream
);

$client = new \GuzzleHttp\Client();
$response = $client->sendRequest($request);

self::assertNotNull($response);
self::assertEquals(200, $response->getStatusCode());
```

## #7 Dependency Injection

- Sudah biasanya kita membuat object yang membutuhkan object lain
- Namun ketika aplikasi yang kita buat sangat kompleks, kadang agak sulit melakukan manajemen ini secara manual
- Dependency Injection adalah dimana sebuah object menerima object yang lainnya yang dibutuhkan
- Misal Controller membutuhkan Service, dan Service membutuhkan Repository
- Untuk melakukan Dependency Injection, caranya sangat mudah, kita bisa tambahkan object yang dibutuhkan menggunakan parameter constructor misalnya

### Kode: Repository, Service dan Controller

```php
class ProductRepository
{

}

class ProductService
{
	public function __construct(public ProductService $productRepository)
	{

	}
}

class ProductControlller
{
	public function __construct(public ProductService $productService)
	{

	}
}
```

### Kode: Dependency Injection

```php
public function testDependencyInjection()
{
	$repository = new ProductRepository();
	$service = new ProductService($repository);
	$controller = new ProductControlller($service);

	$this->assertInstanceOf(ProductRepository::class, $service->productRepository);
	$this->assertInstanceOf(ProductService::class, $controller->productService);
}
```

### PSR-11: Container Interface

- PSR ini merupakan standarisasi untuk melakukan dependency injection secara otomatis
- Caranya adalah dengan menyediakan Container (tempat menyimpan object), dan kita bisa mengambil object lewat Container secara otomatis, dan Container akan mengurus dependency injection yang dibutuhkan secara otomatis -<https://www.php-fig.org/psr/psr-11/>

### Implementasi Dependency Injection

- Contoh implementasi PRS Dependency Injection ini adalah
- <https://php-di.org/>
- <https://packagist.org/packages/php-di/php-di>

### Kode: Dependency Injection Otomatis

```php
public function testContainerInterface()
{
	$container = new \Di\Container();
	$controller = $container->get(ProductControlller::class);
	$service = $container->get(ProductService::class);
	$repository = $container->get(ProductRepository::class);

	$this->assertInstanceOf(ProductControlller::class, $controller);
	$this->assertInstanceOf(ProductControlller::class, $service);
	$this->assertSame($service->productRepository, $repository);
	$this->assertSame($controller->productService, $service);
}
```

## #8 PSR Lainnya

- Masih banyak PSR yang lain, baik itu yang sudah direkomendasikan, masih draft atau bahkan sudah tidak digunakan lagi
- Disarankan untuk membaca tentang PSR lainnya langsung di halaman web PSR nya
- <https://www.php-fig.org/psr/>

## #9 Penutup
