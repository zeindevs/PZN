# RabbitMQ Dasar

## Sebelum Balajar

- Mengerti Cara Menginstall Aplikasi
- Mengerti Menggunakan Cara Terminal
- Di Kelas ini akan ada praktek membuat aplikasi menggunakan Golang, NodeJS dan Java. Sehingga jika sudah mengerti salah satu bahasa tersebut, akan sangat direkomendasikan

## #1 Pengenalan Publish Subscribe

### Komunikasi Antar Aplikasi

- Saat kita membuat aplikasi, kita pasti akan melakukan komunikasi dengan aplikasi lain, paling sederhana misal aplikasi yang kita buat akan berkomunikasi dengan aplikasi seperti database
- Komunikasi tersebut kita sebut adalah RPC (Remote Procedure Call)
- Contoh protokol RPC yang sekarang sedang populer adalah RESTful API
- Salah satu keuntungan menggunakan RPC adalah, komunikasi bisa dilakukan secara synchronous dan realtime

### Contoh Kasus

- Misal kita membuat sistem toko online, dimana kita buat dalam banyak aplikasi
- Ada aplikasi product, promo, shopping cart, order, logistic dan payment
- Misal ketika pelanggan melakukan pembelian, maka semua barang akan disimpan ke shopping cart
- Untuk mendapatkan informasi produk dan promo yang sedang berjalan, maka shopping cart harus berkomunikasi dengan aplikasi product dan promo
- Setelah barang di shopping cart akan dibeli, maka shopping cart akan mengirim semua data product ke aplikasi order, yang nanti akan diproses untuk ke aplikasi logistic untuk menghitung ongkos kirim, dan ke aplikasi payment untuk diproses pembayarannya

### Diagram RPC (Remote Procedure Call)

![Diagram RPC (Remote Procedure Call)](./images/rabbitmq-dasar-01.jpg)

### Perubahan Kasus

- Misal pada kasus ini, kita ingin menambahkan aplikasi untuk mendeteksi penipuan, sehingga kita ingin menambahkan aplikasi fraud detection
- Proses pengecekan penipuan akan dilakukan ketika order dibuat, sehingga misal jika terjadi fraud order bisa dibatalkan secara otomatis
- Pada kasus ini, berarti data order akan dikirim ke aplikasi fraud detection
- Semakin banyak interaksi yang dilakukan oleh aplikasi, maka akan semakin kompleks, dan si pengirim harus menambah mekanisme untuk mengirim data ke aplikasi lainnya lagi

### Diagram RPC (Remote Procedure Call)

![Diagram RPC (Remote Procedure Call)](./images/rabbitmq-dasar-02.jpg)

### Pengenalan Publish Subscribe

- RPC adalah mekanisme komunikasi dimana pengirim menentukan siapa yang menerima data
- Selain RPC, terdapat mekanisme berkomunikasi bernama Messaging atau lebih dikenal dengan Publish/Subscribe
- Mekanisme Messaging berbeda dengan PRC, dalam Messaging, pengirim tidak menentukan siapa yang menerima data, melainkan pengirim akan mengirim data ke perantara (Message Broker)
- Seluruh penerima data akan mengambil data langsung dari perantara (Message Broker)
- Dengan begitu, ketika ada penerima data baru, pengirim data tidak harus tahu, karena tugas pengirim data hanya mengirim ke perantara (Message Broker)

### Diagram Messaging

![Diagram Messaging](./images/rabbitmq-dasar-03.jpg)

### Keuntungan Menggunakan Messaging

- Dengan menggunakan komunikasi Messaging, pengirim tidak perlu tahu kompleksitas yang akan dilakukan oleh penerima data
- Setiap terjadi perubahan jumlah penerima data, maka pengirim tidak perlu tahu lagi

### Diagram Messaging

![Diagram Messaging](./images/rabbitmq-dasar-04.jpg)

### Kekurangan Menggunakan Messaging

- Tidak realtime seperti PRC, artinya bisa terdapat delay (jeda waktu) data tidak konsisten dari pengirim mengirim data, sampai data diterima oleh penerima data
- Ketika terjadi kegagalan penerimaan data, pengirim tidak akan tahu, oleh karena itu penerima harus memiliki kemampuan untuk melakukan retry (mencoba ulang) atau memberi tahu pengirim bahwa penerima gagal memproses data

## #2 Pengenalan RabbitMQ

- RabbitMQ adalah salah satu aplikasi perantara (Message Broker) untuk komunikasi Messaging
- RabbitMQ adalah aplikasi yang OpenSource dan Gratis, sehingga kita bisa bebas menggunakannya
- RabbitMQ merupakan salah satu aplikasi Message Broker yang ringan dan bisa install di hampir semua sistem operasi, Windows, Linux dan Mac
- <https://www.rabbitmq.com/>

### Kenapa RabbitMQ?

- RabbitMQ sangat ringan, sehingga tidak butuh banyak resource hardware untuk menggunakannya
- Didukung oleh ekosistem yang besar sehingga dukungan untuk teknologi dan bahasa pemrograman sangat banyak, seperti Java, .NET, Ruby, C/C++, PHP, Golang, Scala, Python, dan lain-lain
- High Availability, bisa dijalankan dalam cluster sehingga tidak perlu takut terjadi down, bahkan mendukung cross beberapa zone / region datacenter

### AMQP

- RabbitMQ, mengikuti standard AMQP (Advanced Message Queuing Protocol)
- Karena RabbitMQ mengikuti standard AMQP, ini menjadikan cara berkomunikasi dengan RabbitMQ bisa dilakukan asalkan kita mengikuti standarisasi AMQP
- Ini menjadi salah satu keuntungan juga, ketika misal suatu saat kita tidak ingin menggunakan RabbitMQ lagi, kita cukup gunakan Message Broker lain yang mengikuti standard AMQP
- <https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol>

## #3 Menginstall RabbitMQ

### Download RabbitMQ

- Untuk menginstall RabbitMQ, kita perlu download distribution file RabbitMQ terlebih dahulu
- <https://rabbitmq.com/download.html>
- Silahkan download file distribution untuk RabbitMQ sesuai dengan sistem operasi yang digunakan, lalu install pada sistem operasi yang digunakan

### Executable File RabbitMQ

- Setelah menginstall RabbitMQ dengan benar, maka RabbitMQ akan menginstall banyak executable file berbasis terminal pada sistem operasi yang kita gunakan
- Kebanyakan executable file miliki RabbitMQ menggunakan awalan rabbitmq, misal `rabbitmqctl`, `rabbitmqadmin`, `rabbitmq-server`, `rabbitmq-plugins`, dan lain-lain

### Menjalankan RabbitMQ

- Untuk menjalankan aplikasi RabbitMQ, tiap sistem operasi berbeda-beda, oleh karena itu saya sarankan untuk membaca caranya di halaman <https://www.rabbitmq.com/download.html>
- Jika aplikasi RabbitMQ sudah berjalan dengan benar, kita bisa mengeceknya menggunakan terminal dengan perintah :
- `rabbitmqctl status`
- Jika statusnya error, berarti RabbitMQ belum berjalan dengan benar

## #4 Web Management

- RabbitMQ memiliki plugin bernama Web Management
- Plugin ini digunakan untuk mengoperasikan RabbitMQ menggunakan halaman Web
- Kita akan menginstall plugin ini, agar mempermudah saat mengoperasikan fitur-fitur yang terdapat di RabbitMQ
- <https://www.rabbitmq.com/management.html>

### Mengaktifkan Web Management

- Untuk mengaktifkan plugin web management, kita bisa gunakan perintah :
- `rabbitmq-plugins enable rabbitmq_management`
- Selanjutnya kita bisa membuka halaman web management RabbitMQ melalui :
- <http://localhost:15672/>
- Menggunakan user guest dan password guest

## #5 Exchange

- Saat pengirim data mengirim data ke RabbitMQ, data harus dikirim ke Exchange
- Jadi data, tidak langsung dikirimkan ke penerima data, melainkan akan diterima di Exchange terlebih dahulu
- Setelah diterima di Exchange, data akan ditentukan harus dilanjutkan kemana, sesuai dengan tipe dari Exchange-nya, yang akan dibahas di materi masing-masing untuk tiap tipe Exchange-nya

### Diagram Exchange

![](./images/rabbitmq-dasar-05.jpg)

### Membuat Exchange

- Untuk membuat Exchange, kita bisa menggunakan Web Management pada bagian Exchanges
- Saat membuat Exchange, kita harus menentukan seperti :
- `Name`, nama exchange nya
- `Type`, tipe exchange nya, yang akan kita bahas nanti di tiap materi sendiri
- `Durability`, apakah exchange tetap ada setelah RabbitMQ di restart, `Durable` (tetap ada setelah di restart), `Transient` (hilang setelah di restart)
- `Auto Delete`, apakah Exchange otomatis dihapus ketika semua Queue di unbind
- `Internal`, artinya Exchange tidak bisa digunakan oleh Producer, hanya bisa digunakan di internal RabbitMQ
- `Alternate-`Exchange, jika data tidak berhasil dikirim ke Queue, maka akan dikirim ke Alternate-Exchange

### Latihan

- Buatlah Exchange dengan nama notification, dan gunakan type direct

## #6 Queue

- Queue (antrian), adalah tempat untuk menyimpan data yang diterima oleh Exchange
- Exchange itu seperti gerbang untuk menerima data, Exchange nanti akan meneruskan data yang diterima ke Queue
- Jika kita membuat Exchange yang tidak memiliki Queue, maka data akan hilang di Exchange, karena Exchange bukan untuk menyimpan data
- Queue digunakan oleh Consumer (penerima data)

### Diagram Queue

![Diagram Queue](./images/rabbitmq-dasar-06.jpg)

### Membuat Queue

- Untuk membuat Queue, sangat mudah, kita bisa menggunakan halaman Queue
- Virtual Host adalah informasi Virtual Host, yang akan dibahas di materi tersendiri
- `Type`, tipe untuk Queue, Saat ini direkomendasikan menggunakan `Quorum`, `Classic` sudah tidak disarankan untuk digunakan, dan `Stream` digunakan untuk kebutuhan Data Streaming
- `Name`, untuk nama queue nya
- `Arguments`, untuk informasi tambahan Queue

![Membuat Queue](./images/rabbitmq-dasar-07.jpg)

### Latihan

- Buat beberapa queue :
- email
- sms
- whatsapp

## #7 Binding

- Binding merupakan koneksi yang merelasikan antara Exchange dan Queue
- Saat kita membuat Exchange dan Queue, secara default kedua hal itu tidak akan terhubung, kita harus menghubungkannya dengan melakukan Binding

### Diagram Binding

![Diagram Binding](./images/rabbitmq-dasar-08.jpg)

### Binding Lebih Dari Satu

- Di RabbitMQ, kita bisa melakukan Binding dari satu Exchange ke beberapa Queue
- Begitu juga sebaliknya, bisa melakukan Binding dari satu Queue ke beberapa Exchange

### Diagram Binding Lebih Dari Satu

![Diagram Binding Lebih Dari Satu](./images/rabbitmq-dasar-09.jpg)

### Latihan

- Coba lakukan Binding untuk Exchange notification ke Queue email, sms dan whatsapp

## #8 Direct Exchange

- Sebelumnya kita sudah membuat Exchange menggunakan type direct
- Direct Exchange adalah tipe exchange yang ketika menerima data, dia akan merutekan data tersebut ke Queue yang sesuai dengan routing key saat proses binding
- Jika kita mengirim data ke Exchange dengan routing key yang tidak ada di Binding ke Queue, maka secara otomatis data akan dikirim ke Alternate-Exchange, jika tidak ada Alternate Exchange, maka data akan hilang secara otomatis

### Diagram Direct Exchange

![Diagram Direct Exchange](./images/rabbitmq-dasar-10.jpg)

### Routing

- Saat melakukan perutean menggunakan Direct Exchange, tidak ada batasan untuk melakukan binding ke Queue yang sama, bahkan menggunakan routing key yang berbeda
- Misal, kita membuat Queue baru bernama `all_notification`, lalu membuat binding untuk routing key email, sms dan whatsapp
- Hal itu diperbolehkan

### Diagram Multiple Routing Key

![Diagram Multiple Routing Key](./images/rabbitmq-dasar-11.jpg)

### Latihan

- Buatlah Queue all_notification
- Lakukan binding untuk routine key web, sms dan whatsapp dari Exchange notification ke Queue all_notification

## #9 Publish Message

### Message

- Seperti yang sudah dibahas di awal, untuk mengirim data atau publish message, hal ini dilakukan ke Exchange
- RabbitMQ memiliki standard format message

### Message Format

- Routing Key, informasi routing key yang digunakan untuk jenis Direct Exchange untuk merutekan message ke Queue
- Headers, informasi tambahkan key-value yang bebas ditambahkan
- Properties, mirip seperti Headers, namun sudah baku key nya
- Payload, data yang dikirim oleh Producer

![Message Format](./images/rabbitmq-dasar-12.jpg)

### Publish Message

- Untuk melakukan publish message, kita bisa menggunakan Web Management
- Silahkan buka Exchange yang ingin dikirimkan data, pada bagian bawah terdapat form untuk Publish Message

### Tugas

- Coba kirim beberapa data ke Exchange notification, dengan routing email, sms dan whatsapp
- Pastikan message masuk ke Queue sesuai dengan routing, kita bisa cek dengan melihat jumlah data di Queue

## #10 Consume Message

### Queue

- Membaca data dari Queue disebut dengan Consume Message
- Message yang sudah di Consume, otomatis akan hilang dari Queue
- Queue itu merupakan struktur data dalam bentuk antrian, artinya siapa yang pertama masuk, dia yang akan diproses duluan

### Diagram Queue

![Diagram Queue](./images/rabbitmq-dasar-13.jpg)

### Consume Message

- Untuk melakukan Consume Message, kita bisa menggunakan Web Management di RabbitMQ
- Kita bisa buka Queue yang ingin kita baca message nya, lalu buka pada menu Get Messages

### Latihan

- Silahkan coba Consume Message dari Queue email, sms dan whatsapp
- Gunakan Automatic Ack (Acknowledge), yang artinya data akan diakui sukses dibaca
- Data yang sudah Acknowledge, secara otomatis akan hilang dari Queue

## #11 Fanout Exchange

- Fanout Exchange adalah tipe Exchange yang akan selalu merutekan setiap data ke seluruh Queue
- Artinya setiap data yang kita kirim ke Exchange, akan diteruskan ke seluruh Queue
- Fanout Exchange mirip seperti Broadcast, dimana semua Queue akan menerima data yang sama

### Diagram Fanout Exchange

![Diagram Fanout Exchange](./images/rabbitmq-dasar-14.jpg)

### Latihan

- Buat Exchange dengan tipe fanout bernama announcement
- Buat Queue dengan nama customers dan sellers
- Buat Binding dari Queue customers dan sellers ke Exchange announcement
- Publish Message ke Exchange announcement
- Pastikan semua message diterima di Queue customers dan sellers

## #12 Topic Exchange

- Topic Exchange merupakan Exchange mirip dengan Direct Exchange, dimana Exchange akan merutekan message ke Queue sesuai dengan routine key nya
- Hanya saja pada Topic Exchange, routing key menggunakan kata-kata yang dipisahkan menggunakan titik
- Misal, `customers.event.update`, `customers.event.new`, `customers.event.delete`

### Topic Exchange Binding

- Saat Queue melakukan Binding ke Topic Exchange, kita bisa menggunakan tanda :
- `*` (star), digunakan untuk satu kata
- `#` (hash), digunakan untuk nol atau lebih kata
- Misal `customers.event.*`, artinya bisa untuk routing key `customers.event.new`, `customers.event.update`, `customers.event.delete`
- Misal `customers.#`, artinya bisa untuk routing key `customers.a`, `customers.a.b`, `customers.a.b.c`
- Misal `*.event.*`, artinya bisa untuk `customers.event.a`, `sellers.event.b`
- Misal `#`, artinya bisa untuk semua routing

### Diagram Topic Exchange

![Diagram Topic Exchange](./images/rabbitmq-dasar-15.jpg)

### Latihan

- Buatlah Topic Exchange dengan nama events
- Buat Queue all_events dan Binding `*.events.*` ke Exchange events
- Buat Queue all_messages dan Binding # ke Exchange events
- Buat Queue all_customer_messages dan binding `customers.events.*` ke Exchange events
- Publish beberapa message, dan pastikan masuk ke Queue yang sesuai dengan Topic nya

## #13 Header Exchange

- Header Exchange adalah Exchange yang melakukan perutean data ke Queue menggunakan Header di Message
- Exchange akan merutekan sesuai dengan Attribute di Binding yang sesuai dengan Header di Message

### Diagram Header Exchange

![Diagram Header Exchange](./images/rabbitmq-dasar-16.jpg)

### Latihan

- Buatlah Header Exchange dengan nama download
- Buatlah Queue download_report_pdf dengan Binding type report, format pdf ke Exchange download
- Buatlah Queue download_report_excel dengan Binding type report, format excel ke Exchange download
- Buatlah Queue download_summary_pdf dengan Binding type summary, format pdf ke Exchange download
- Coba publish beberapa message dengan header, dan pastikan message diterima dengan Queue yang sesuai dengan Binding

## #14 Round-Robin Dispatching

- Salah satu keuntungan menggunakan Queue adalah, kita bisa menjalankan Consumer lebih dari satu
- Dengan kemampuan ini, jika kita bisa menjadikan pemrosesan data di Queue bisa dilakukan secara paralel dan dilakukan lebih cepat

### Diagram Round-robin dispatching

![Diagram Round-robin dispatching](./images/rabbitmq-dasar-17.jpg)

## #15 Virtual Host

- RabbitMQ memiliki konsep seperti Virtual Machine bernama Virtual Host
- Dengan menggunakan Virtual Host, kita bisa membuat Exchange dan Queue terpisah-pisah dalam Virtual Host
- Misal, saat RabbitMQ digunakan oleh banyak sekali tim, kita bisa buat tiap tim memiliki Virtual Host masing-masing, sehingga Exchange dan Queue antar tim tidak akan bentrok
- Saat pertama kali menginstall RabbitMQ, secara default terdapat 1 Virtual Host bernama / (slash)

### Diagram Virtual Host

![Diagram Virtual Host](./images/rabbitmq-dasar-18.jpg)

### Membuat Virtual Host

- Untuk membuat Virtual Host, kita bisa menggunakan menu Admin -> Virtual Host
- Setelah membuat Virtual Host, kita bisa menggunakan Virtual Host tersebut ketika membuat Exchange atau Queue

### Latihan

- Silahkan buat beberapa Virtual Host
- Lalu buat Exchange dan Queue pada Virtual Host yang sudah kita buat

## #16 Policy

- Saat kita membuat Exchange atau Queue, kita bisa menambahkan Arguments
- Policy adalah fitur dimana kita bisa menambahkan Arguments secara otomatis pada Exchange atau Queue secara otomatis tanpa harus menambahkan satu per satu secara manual

### Membuat Policy

- Untuk membuat Policy, kita bisa menggunakan menu Admin -> Policies
- Saat membuat Policy, kita perlu menentukan beberapa informasi :
- Name, untuk nama Policy nya
- Pattern, untuk regular expression pemilihan Exchange atau Queue yang ingin ditambahkan Policy nya
- Priority, jika terdapat Policy yang berbeda untuk Exchange atau Queue yang sama, maka akan diurutkan sesuai priority, karena Policy cuma bisa digunakan satu saja
- Definition, Arguments yang akan ditambahkan ke Exchange atau Queue

### Tugas

- Buat beberapa Policy untuk Exchange atau Queue
- Informasi Policy yang sudah digunakan pada Exchange atau Queue bisa kita lihat di bagian Features di Exchange atau Queue

## #17 User Management

- Saat pertama kali menginstall RabbitMQ dan Plugin Web Management, terdapat default user yaitu guest yang bisa digunakan untuk masuk ke Web Management
- Saat nanti kita Publish Message atau Consume Message pun, nanti kita perlu menggunakan user
- Untuk melihat informasi User yang terdapat di RabbitMQ, kita bisa menggunakan menu Admin -> Users

### Membuat User

- Untuk membuat User, kita hanya cukup menambahkan informasi Username, Password dan Tags
- Tags adalah informasi yang digunakan Web Management untuk mengecek kemampuan apa yang bisa dilakukan oleh User tersebut

### User Tag

| Tag           | Keterangan                                                                                                                                |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| (none)        | Tidak bisa mengakses web manegement                                                                                                       |
| management    | Bisa mengakses web management, mengakses virtual host yang diperbolehkan, exchange, queue dan binsings di virtual host yang diperbolehkan |
| policymaker   | semua di management, plus mengelola policy                                                                                                |
| monitoring    | semua di management, dan melihat semua informasi di virtual host                                                                          |
| administrator | bisa melakukan apapun                                                                                                                     |

### Latihan

- Buatlah beberapa User dengan tag yang berbeda-beda
- Lalu login menggunakan User tersebut, dan lihat perbedaannya

### Permission

- Secara default, saat kita membuat User, walaupun bisa login dan masuk ke web management, namun User tidak bisa mengakses Virtual Host
- Oleh karena itu, kita perlu menambahkan permission untuk bisa mengakses Virtual Host
- Saat menambahkan Permission, kita harus menentukan regular expression untuk operasi mana yang diperbolehkan
- RabbitMQ menggunakan standard operasi AMQP, yang bisa kita lihat di slide selanjutnya
- Operasi di-grup kan berdasarkan 3 grup, Configure, Write dan Read

![Permission](./images/rabbitmq-dasar-19.jpg)

### Latihan

- Tambahkan permission untuk User yang sudah dibuat
- Coba login dengan User tersebut, dan lihatlah perbedaannya

## #18 Client Library

### Application

- Sekarang kita sudah tahu bagaimana mengelola RabbitMQ menggunakan Web Management yang terdapat di RabbitMQ
- Pada kenyataannya, nanti Publish Message dan Consume Message akan dilakukan di aplikasi yang kita buat
- Di materi selanjutnya, kita akan coba membuat aplikasi menggunakan beberapa teknologi untuk melakukan Publish Message dan Consume Message ke RabbitMQ

### Client Library

- Untuk berinteraksi dengan RabbitMQ, kita bisa menggunakan Client Library
- Client Library adalah kode-kode yang disediakan untuk digunakan agar bisa berinteraksi dengan RabbitMQ dengan mudah
- Client Library harus disesuaikan dengan teknologi yang kita gunakan
- RabbitMQ mendukung banyak sekali Client Library
- <https://www.rabbitmq.com/devtools.html>
- Di materi-materi selanjutnya, kita akan menggunakan Golang, NodeJS, Java dan PHP untuk berinteraksi dengan RabbitMQ

## #19 Golang RabbitMQ

### Membuat Project Golang

- Buat folder belajar-golang-rabbitmq
- `go mod init belajar-golang-rabbitmq`
- `go get github.com/rabbitmq/amqp091-go`
- <https://github.com/rabbitmq/amqp091-go>

### Kode: Membuat Connection

```go
connection, err := amqp091.Dial("amqp://guest:guest@localhost:5672/")
if err != nil {
	panic(err)
}
defer connection.Close()

channel, err := connection.Channel()
if err != nil {
	panic(err)
}
```

### Kode: Membuat Consumer

```go
ctx := context.Background()
emailConsumer, err := channel.ConsumeWithContext(ctx, "email", "consumer-email", true, false, false, false, nil)
if err != nil {
	panic(err)
}

for message := range emailConsumer {
	println("Routing Key: " + message.RoutingKey)
	println("Body: " + string(message.Body))
}
```

### Kode: Membuat Producer

```go
ctx := context.Background()
for i := 0; i < 10; i++ {
	message := amqp091.Publishing{
		Headers: amqp091.Table{
			"sample": "value",
		},
		Body: []byte("Hello " + strconv.Itoa(i)),
	}
	err := channel.PublishWithContext(ctx, "notification", "email", false, false, message)
	if err != nil {
		panic(err)
	}
}
```

## #20 NodeJS RabbitMQ

### Membuat Project NodeJS

- Buat folder belajar-nodej-rabbitmq
- `npm init`
- `npm install amqplib`
- `npm install @types/amqplib`
- <https://github.com/amqp-node/amqplib>
- Ubah type di `package.json` menjadi module

### Kode: Membuat Connection

```ts
import { connect } from "amqplib";

const connection = await connect("amqp://guest:guest@localhost:5672/");
const channel = await connection.createChannel();

// TODO: consumer or produce here

await channel.close();
await connection.close();
```

### Kode: Membuat Consumer

```ts
import { connect } from "amqplib";

const connection = await connect("amqp://guest:guest@localhost:5672/");
const channel = await connection.createChannel();

await channel.consume(
	"sms",
	function (message) {
		console.lof(message.fields.routingKey);
		console.lof(message.content.toString());
	},
	{
		noAck: true,
	},
);
```

### Kode: Membuat Producer

```ts
import { connect } from "amqplib";

const connection = await connect("amqp://guest:guest@localhost:5672/");
const channel = await connection.createChannel();

for (int i = 0; i < 10; i++) {
	channel.publish("notification", "sms", Buffer.from(`SMS ${i}`), {
		headers: {
			"sample": "value",
		}
	});
}

await channel.close();
await connection.close();
```

## #21 Java RabbitMQ

### Membuat Project java

- <https://start.spring.io/>
- Tambahkan dependency RabbitMQ

### Kode: Membuat Connection

```java
ConnectionFactory factory = new ConnectionFactory();
factory.setUrl("amqp://guest:guest@localhost:5672/");

Connection connection = factory.newConnection();
Channel channel = connection.createChannel();

// TODO: create producer or consumer

channel.close();
connection.close();
```

### Kode: Membuat Consumer

```java
DeliverCallback deliverCallback = (tag, message) -> {
	System.out.println(message.getEnvelope().getRoutingKey());
	System.out.println(new String(message.getBody()));
}

CancelCallback cancelCallback = (tag) -> {
	System.out.println("Consumer is canceled");
}

channel.basicConsume("whatsapp", true, deliverCallback, cancelCallback);
```

### Kode: Membuat Producer

```java
for (int i = 0; i < 10; i++) {
	String message = "Whatsapp " + i;

	AMQP.BasicProperties properties = new AMQP.BasicProperties()
		.builder()
		.headers(Map.of("sample", "value"))
		.build();

	channel.basicPublish("notification", "whatsapp", properties, message.getBytes());
}

channel.close();
connection.close();
```

## #22 PHP RabbitMQ

### Membuat Project PHP

- Buat folder belajar-php-rabbitmq
- `composer init`
- `composer require php-amqplib/php-amqplib`
- `composer dump-autoload`
- <https://github.com/php-amqplib/php-amqplib>

### Kode: Membuat Connection

```php
<?php
require_once __DIR__ . "/../vendor/autoload.php";

use PhpAmqpLib\Connection\AMQPStreamConnection;

$connection = new AMQPStreamConnection("localhost", 5662, "guest", "guest", "/");
$channel = $connection->channel();

$channel->close();
$connection->close();
```

### Kode: Membuat Consumer

```php
<?php
require_once __DIR__ . "/../vendor/autoload.php";

use PhpAmqpLib\Connection\AMQPStreamConnection;
use PhpAmqpLib\Message\AMQPMessage;

$connection = new AMQPStreamConnection("localhost", 5662, "guest", "guest", "/");
$channel = $connection->channel();

$channel->basic_consume("email", "email-consumer", false, true, false, false, function (AMQPMessage $message) {
	echo $message->getRoutingKey() . PHP_EOL;
	echo $message->getBody() . PHP_EOL;
});
$channel->consume();
```

### Kode: Membuat Producer

```php
<?php
require_once __DIR__ . "/../vendor/autoload.php";

use PhpAmqpLib\Connection\AMQPStreamConnection;

$connection = new AMQPStreamConnection("localhost", 5662, "guest", "guest", "/");
$channel = $connection->channel();

for ($i = 0; $i < 10; $i++) {
	$headers = new AMQPTable();
	$headers->set("sample", "value");

	$message = new AMQPMessage("Email $i");
	$message->set("application_headers", $headers);
	$channel->basic_publish($message, "notification", "email");
}

$channel->close();
$connection->close();
```

## #23 Penutup
