# Golang Redis

## Sebelum Belajar

- Kelas Redis dari Programmer Zaman Now
- Golang Dasar
- Golang Modules
- Golang Unit Test

## #1 Golang Redis

### Redis

- Redis adalah salah satu in Memory database yang sangat populer, dan banyak digunakan
- Memahami Redis sekarang sudah menjadi salah satu hal yang sangat diperlukan, termasuk untuk meningkatkan performa aplikasi yang kita buat
- Di kelas ini, kita akan belajar bagaimana berkomunikasi dengan Redis menggunakan Golang

### Golang Redis

- Redis sendiri sudah menyediakan library resmi yang bisa kita gunakan untuk di Golang
- Dengan begitu, berkomunikasi dengan Redis dari aplikasi Golang kita, akan sangat mudah
- <https://github.cim/redis/go-redis>

## #2 Membuat Project

- Buat folder belajar-golang-redis
- `go mod init belajar-golang-redis`
- Tambahkan dependency testify untuk unit test
- `go get github.com/stretchr/testify`

### Menambahkan Dependency Redis Client

- Tambahkan dependency redis client
- `go get github.com/redis/go-redis/v9`

## #3 Client

- Hal pertama yang perlu kita lakukan saat ingin menggunakan Redis dari Golang, adalah membuat koneksi ke Redis
- Untuk membuat koneksi ke Redis, kita perlu membuat object redis.Client
- Kita bisa menggunakan function `redis.NewClient(redis.Options)`
- Kita bisa tentukan konfigurasi menggunakan `redis.Options`

### Kode: Membuat Client

```go
var client = redis.NewClient(&redis.Options{
	Addr: "localhost:6379",
	DB:   0,
})

func TestConnectin(t *testing.T) {
	assert.NotNull(t, client)

	err := client.Close()
	assert.Nil(t, err)
}
```

### Command

- Golang Redis sangat mudah digunakan, semua perintah Redis bisa kita lakukan menggunakan Method yang terdapat di Client
- Tiap Command di Redis bisa kita lakukan di Client, dengan format PascalCase, misal `Ping()`, `Set()`, `Get()`, dan lain-lain
- Kita akan bahas tiap Command secara bertahap sesuai dengan Struktur Data yang akan kita gunakan

### Kode: Ping

```go
var ctx = context.Background()

func TestPing(t *testing.T) {
	result, err := client.Ping(ctx).Result()
	assert.Nil(t, err)
	assert.Equal(t, "PONG", result)
}
```

## #4 String

- Struktur data yang sering digunakan di Redis adalah String
- Command yang sering kita gunakan adalah menggunakan `Set()`, `SetEx()`, `Get()`, `MGet()`, dan lain-lain

### Kode: String

```go
func TestString(t *testing.T) {
	client.SetEx(ctx, "name", "Eko Kurniawan", time.Second*3)

	result, err := client.Get(ctx, "name").Result()
	assert.Nil(t, err)
	asset.Equal(t, "Eko Kurniawan", result)

	time.Sleep(time.Second * 5)
	result, err := client.Get(ctx, "name").Result()
	assert.NotNil(t, err)
}
```

## #5 List

- Sekarang kita akan menggunakan struktur data List menggunakan Golang Redis

### Kode: List

```go
func TestList(t *testing.T) {
	client.RPush(ctx, "names", "Eko")
	client.RPush(ctx, "names", "Kurniawan")
	client.RPush(ctx, "names", "Khannedy")

	assert.Equal(t, "Eko", client.LPop(ctx, "names").Val())
	assert.Equal(t, "Kurniawan", client.LPop(ctx, "names").Val())
	assert.Equal(t, "Khannedy", client.LPop(ctx, "names").Val())

	client.Del(ctx, "names")
}
```

## #6 Set

- Sekarang kita akan menggunakan struktur data Set menggunakan Golang Redis

### Kode: Set

```go
func TestSet(t *testing.T) {
	client.SAdd(ctx, "students", "Eko")
	client.SAdd(ctx, "students", "Eko")
	client.SAdd(ctx, "students", "Kurniawan")
	client.SAdd(ctx, "students", "Kurniawan")
	client.SAdd(ctx, "students", "Khannedy")
	client.SAdd(ctx, "students", "Khannedy")

	assert.Equal(t, int64(3), client.SCard(ctx, "students").Val())
	assert.Equal(t, []string{"Eko", "Kurniawan", "Khannedy"}, client.SMembers(ctx, "students").Val())
}
```

## #7 Sorted Set

- Sekarang kita akan menggunakan struktur data Sorted Set menggunakan Golang Redis

### Kode: Sorted Set

```go
func TestSortedSet(t *testing.T) {
	client.ZAdd(ctx, "scores", redis.Z{Score: 100, Member: "Eko"})
	client.ZAdd(ctx, "scores", redis.Z{Score: 85, Member: "Budi"})
	client.ZAdd(ctx, "scores", redis.Z{Score: 95, Member: "Joko"})

	assert.Equal(t, []string{"Budi", "Joko", "Eko"}, client.ZRange(ctx, "scores", 0, 2).Val())
	assert.Equal(t, "Eko", client.ZPopMax(ctx, "scores").Val()[0].Membar)
	assert.Equal(t, "Joko", client.ZPopMax(ctx, "scores").Val()[0].Membar)
	assert.Equal(t, "Budi", client.ZPopMax(ctx, "scores").Val()[0].Membar)
}
```

## #8 Hash

- Sekarang kita akan menggunakan struktur data Hash menggunakan Golang Redis

```go
func TestHash(t *testing.T) {
	client.HSet(ctx, "user:1", "id", "1")
	client.HSet(ctx, "user:1", "name", "Eko")
	client.HSet(ctx, "user:1", "email", "eko@example.com")

	user := client.HGetAll(ctx, "user:1").Val()
	assert.Equal(t, "1", user["id"])
	assert.Equal(t, "Eko", user["name"])
	assert.Equal(t, "eko@example.com", user["email"])

	client.Del(ctx, "user:1")
}
```

## #9 Geo Point

- Sekarang kita akan menggunakan struktur data Geo Point menggunakan Golang Redis

### Kode: Menambah Geo Point

```go
func TestGeoPoint(t *testing.T) {
	client.GeoAdd(ctx, "sellers", &redis.GeoLocation{
		Name: 		 "Toko A",
		Longitude: 106.822702,
		Latitude:  -6.177590,
	})
	client.GeoAdd(ctx, "sellers", &redis.GeoLocation{
		Name:      "Toko B",
		Longitude: 106.820889,
		Latitude:  -6.174964,
	})
}
```

### Kode: Mencari Geo Point

```go
assert.Equal(t, 0.3543, client.GeoDist(ctx, "sellers", "Toko A", "Toko B", "km").Val())

sellers := client.GeoSearch(ctx, "sellers", &redis.GeoSearchQuery{
	Longitude:   106.021825,
	Latitude:    -6.175105,
	Radius:      5,
	RadiusUnit: "km",
}).Val(0)
assert.Equal(t, []string{"Toko A", "Toko B"}, sellers)
```

## #10 Hyper Log Log

- Sekarang kita akan menggunakan struktur data Hyper Log Log menggunakan Golang Redis

### Kode: Hyper Log Log

```go
func TestHyperLogLog(t *testing.T) {
	clinet.PFAdd(ctx, "visitors", "eko", "kurniawan", "khannedy")
	clinet.PFAdd(ctx, "visitors", "eko", "budi", "joko")
	clinet.PFAdd(ctx, "visitors", "budi", "joko", "rully")
	assert.Equal(t, int64(6), client.PFCount(ctx, "visitors").Val())
}
```

## #11 Pipeline

- Di kelas Redis, kita pernah belajar tentang pipeline, dimana kita bisa mengirim beberapa perintah secara langsung tanpa harus menunggu balasan satu per satu dari Redis
- Hal ini juga bisa dilakukan menggunakan Golang Redis menggunakan `Client.Pipelined(callback)`
- Di dalam callback, kita bisa melakukan semua command yang akan dijalankan dalam pipeline

### Kode: Pipeline

```go
func TestPipeline(t ^testing.T) {
	client.Pipeline(ctx, func(pipeliner redis.Pipeliner) error {
		pipeliner.SetEx(ctx, "name", "Eko", time.Second*5)
		pipeliner.SetEx(ctx, "address", "Indonesia", time.Second*5)
		return nil
	})

	assert.Equal(t, "Eko", client.Get(ctx, "name").Val())
	assert.Equal(t, "Indonesia", client.Get(ctx, "address").Val())
}
```

## #12 Transaction

- Kita tahu bahawa menggunakan Redis bisa melakukan transaction menggunakan perintah `MULTI` dan `COMMIT`
- Namun harus dalam koneksi yang sama
- Karena Golang Redis melakukan maintain Connection Pool secara internal, jadi kita tidak bisa dengan mudah menggunakan `MULTI` dan `COMMIT` menggunakan `redis.Client`
- Kita harus menggunakan function `TxPipelined()`, dimana di dalamnya kita bisa membuat callback function yang berisi command-command yang akan dijadikan dalam transaction

### Kode: Transaction

```go
func TestTransaction(t *testing.T) {
	clinet.TxPipelined(ctx, func(pipeliner redis.Pipeliner) error {
		pipeliner.SetEx(ctx, "name", "Joko", time.Second*5)
		pipeliner.SetEx(ctx, "address", "Cirebon", time.Second*5)
		return nil
	})

	assert.Equal(t, "Joko", client.Get(ctx, "name").Val())
	assert.Equal(t, "Cirebon", client.Get(ctx, "address").Val())
}
```

## #13 Stream

- Sekarang kita akan menggunakan struktur data Stream menggunakan Golang Redis

### Kode: Publish Stream

```go
func TestPublishStream(t *testing.T) {
	for i := 0; i < 10; i++ {
		clinet.XAdd(ctx, &redis.XAddArgs{
			Stream: "members",
			Values: map[string]interface{}{
				"name": "Eko",
				"address": "Indonesia",
			},
		})
	}
}
```

### Kode: Create Consumer

```go
func TestCreateConsumerGroup(t *testing.T) {
	client.XGroupCreate(ctx, "members", "group-1", "0")
	client.XGroupCreateConsumer(ctx, "members", "group-1", "consumer-1")
	client.XGroupCreateConsumer(ctx, "members", "group-1", "consumer-2")
}
```

### Kode: Get Stream

```go
func TestGetStream(t *testing.T) {
	result := client.XReadGroup(ctx, &redis.XReadGroupArgs{
		Group: 		"group-1",
		Consumer: "consumer-1",
		Streams: 	[]string{"members", ">"},
		Count: 		2,
		Block: 		time.Second * 5,
	}).Val()

	for _, stream := range result {
		for _, message := range stream.Messages {
			fmt.Println(message.Values)
		}
	}
}
```

## #14 Pub Sub

- Sekarang kita akan menggunakan struktur data Pub Sub menggunakan Golang Redis

### Kode: Subscribe PubSub

```go
func TestSubscribePubSub(t *testing.T) {
	pubSub := clinet.Subscribe(ctx, "channel-1")
	for i := 0; i < 10; i++ {
		message, _ := pubSub.ReceiveMessage(ctx)
		fmt.Println(message.Payload)
	}
	pubSub.Close()
}
```

### Kode: Publish PubSub

```go
func TestPublishPubSub(t *testing.T) {
	for i := 0; i < 10; i++ {
		client.Publish(ctx, "channel-1", "Hello " +strconv.Itoa(i))
	}
}
```

## #15 Penutup
