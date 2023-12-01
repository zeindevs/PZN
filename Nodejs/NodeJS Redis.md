# NodeJS Redis

## Sebelum Belajar

- Kelas JavaScript dari Programmer Zaman Now
- NodeJS Dasar
- NodeJS NPM
- NodeJS Unit Test

## #1 NodeJS Redis

### Redis

- Redis adalah salah satu in Memory database yang sangat populer, dan banyak digunakan
- Memahami Redis sekarang sudah menjadi salah satu hal yang sangat diperlukan, terutama untuk meningkatkan performa aplikasi yang kita buat
- Di kelas ini, kita akan belajar bagaimana berkomunikasi dengan Redis menggunakan NodeJS

### NodeJS Redis Client

- Saat ini, Redis menyediakan dua Redis Client untuk NodeJS, node-redis dan ioredis
- <https://github.com/redis/node-redis>
- <https://github.com/redis/ioredis>
- Di kelas ini, kita akan menggunakan ioredis, hal ini karena dari segi performa dan kecepatan ioredis lebih bagus dibandingkan node-redis

## #2 Membuat Project

- Buat folder belajar-nodejs-redis
- `npm init`
- Buka `package.json`, dan tambahkan type module

### Menambahkan Library Jest untuk Unit Test

- `npm install --save-dev jest @types/jest`
- <https://www.npmjs.com/package/jest>

### Menambahkan Library Babel

- `npm install --save-dev babel-jest @babel/preset-env`
- <https://babeljs.io/setup#installation>

### Menambahkan Libary Redis

- `npm install ioredis`
- <https://www.npmjs.com/package/ioredis>

## #3 Client

- Hal pertama yang perlu kita lakukan saat ingin menggunakan Redis dari NodeJS, adalah membuat koneksi ke Redis
- Untuk membuat koneksi ke Redis, kita perlu membuat object Redis

### Kode: Membuat Client

```ts
let redis = null;

beforeEach(async () => {
	redis = new Redis({
		host: "localhost",
		port: 6379,
		db: 0,
	});
});

afterEach(async () => {
	await redis.quit();
});
```

### Command

- ioredis sangat mudah digunakan, semua perintah Redis bisa kita lakukan nenggunakan Method yang terdapat di Client
- Tiap Command di Redis bisa kita lakukan di Client, dengan format lowercase, misal ping(), setex(), get(), dan lain-lain
- Kita akan bahas tiap Command secara bertahap sesuai dengan Struktur Data yang akan kita gunakan

### Kode: Ping

```ts
it("should can ping", async () => {
	const pong = await redis.ping();
	expect(pong).toBe("PONG");
});
```

## #4 String

- Struktur data yang sering digunakan di Redis adalah String
- Command yang sering kita gunakan adalah menggunakan set(), setEx(), get(), mGet(), dan lain-lain

### Kode: String

```ts
if('should support string', async() => {
	await redis.setex('name', 2, 'eko')
	let name = await redis.get('name')
	expect(name).toBe('Eko')

	// sleep 5 seconds
	await new Promise(resolve => setTimeout(resolve, 3000))
	name = await redis.get('name')
	expect(name).toBeNull()
})
```

## #5 List

- Sekarang kita akan menggunakan struktur data List menggunakan NodeJS Redis

### Kode: List

```ts
it("should support list data structure", async () => {
	await redis.rpush("names", "Eko");
	await redis.rpush("names", "Kurniawan");
	await redis.rpush("names", "Khannedy");

	expect(await redis.llen("names")).toBe(1);
	const names = await redis.lrange("names", 0, -1);
	expect(names).toEqual(["EKo", "Kurniawan", "Khannedy"]);

	expect(await redis.lpop("names")).toBe("Eko");
	expect(await redis.rpop("names")).toBe("Khannedy");
	expect(await redis.llen("names")).toBe(1);

	await redis.del("names");
});
```

## #6 Set

- Sekarang kita akan menggunakan struktur data Set menggunakan NodeJS Redis

### Kode: Set

```ts
it("should suppert set data structure", async () => {
	await redis.sadd("names", "Eko");
	await redis.sadd("names", "Eko");
	await redis.sadd("names", "Kurniawan");
	await redis.sadd("names", "Kurniawan");
	await redis.sadd("names", "Khannedy");
	await redis.sadd("names", "Khannedy");

	expect(await redis.scard("names")).toBe(3);
	const names = await redis.smembers("names");
	expect(names).toEqual(["Eko", "Kurniawan", "Khannedy"]);

	await redis.del("names");
});
```

## #7 Sorted Set

- Sekarang kita akan menggunakan struktur data Sorted Set menggunakan NodeJS Redis

### Kode: Sorted Set

```ts
it("should suppert sorted set", async () => {
	await redis.zadd("names", 100, "Eko");
	await redis.zadd("names", 85, "Budi");
	await redis.zadd("names", 95, "Joko");

	expect(await redis.zcard("names")).toBe(3);
	const names = await redis.zrange("names", 0, -1);
	expect(names).toEqual(["Budi", "Joko", "Eko"]);

	expect(await redis.zpopmax("names")).toEqual(["Eko", "100"]);
	expect(await redis.zpopmax("names")).toEqual(["Joko", "95"]);
	expect(await redis.zpopmax("names")).toEqual(["Budi", "85"]);

	await redis.del("names");
});
```

## #8 Hash

- Sekarang kita akan menggunakan struktur data Hash menggunakan NodeJS Redis

### Kode: Hash

```ts
it("should support hash", async () => {
	await redis.hset("user:1", {
		id: "1",
		name: "eko",
		email: "eko@example.com",
	});

	const user = await redis.hgetall("user:1");
	expect(user).toEqual({
		id: "1",
		name: "eko",
		email: "eko@example.com",
	});

	await redis.del("user:1");
});
```

## #9 Geo Point

- Sekarang kita akan menggunakan fitur Geo Point menggunakan NodeJS Redis

### Kode: Geo Point

```ts
it("should support geo point", async () => {
	await redis.geoadd("sellers", 106.822702, -6.17759, "Toko A");
	await redis.geoadd("sellers", 106.820889, -6.174964, "Toko B");

	const distance = await redis.geodist("sellers", "Toko A", "Toko B", "KM");
	expect(distance).toBe(String(0.3543));

	const result = await redis.geosearch(
		"sellers",
		"fromlonlat",
		106.821825,
		-6.175105,
		"byradius",
		5,
		"km",
	);
	expect(result).toEqual(["Toko A", "Toko B"]);
});
```

## #10 Hyper Log Log

- Sekarang kita akan menggunakan fitur Hyper Log Log menggunakan NodeJS Redis

### Kode: Hyper Log Log

```ts
it("should support hyper log log", async () => {
	await redis.pfadd("visitors", "eko", "kurniawan", "khannedy");
	await redis.pfadd("visitors", "eko", "budi", "joko");
	await redis.pfadd("visitors", "budi", "joko", "rully");

	const total = await redis.pfcount("visitors");
	expect(total).toBe(6);
});
```

## #11 Pipeline

- Di kelas Redis, kita pernah balajar tentang pipeline, dimana kita bisa mengirim beberapa perintah secara langsung tanpa harus menunggu balasan satu per satu dari Redis
- Hal ini juga bisa dilakukan menggunakan ioredis menggunakan method pipeline()
- Method pipeline() akan mengembalikan object yang bisa kita gunakan sebagai redis client yang akan dieksekusi semuanya menggunakan pipeline ketika memanggil method exec()

### Kode: Pipeline

```ts
it("should support pipeline", async () => {
	const pipeline = redis.pipeline();

	pipeline.setex("name", 2, "Eko");
	pipeline.setex("address", 2, "Indonesia");

	await pipeline.exec();

	expect(await redis.get("name")).toBe("eko");
	expect(await redis.get("address")).toBe("Indonesia");
});
```

## #12 Transaction

- Kita tahu bahwa menggunakan Redis bisa melakukan transaction menggunakan perintah MULTI dan COMMIT
- Namun harus dalam koneksi yang sama
- Karena NodeJS Redis menggunakan maintain Connection Pool secara internal, jadi kita tidak bisa dengan mudah menggunakan MULTI dan COMMIT menggunakan Redis Client
- Kita harus menggunaka function multi(), yang mengembalikan object yang bisa kita gunakan sebagai Redis Client, dan jika mau melakukan COMMIT, kita bisa menggunakan method exec()
- Jadi penggunaanya mirip seperti Pipeline

### Kode: Transaction

```ts
it("should support transaction", async () => {
	const transaction = redis.multi();

	transaction.setex("name", 2, "Eko");
	transaction.setex("address", 2, "Indonesia");

	await transaction.exec();

	expect(await redis.get("name")).toBe("eko");
	expect(await redis.get("address")).toBe("Indonesia");
});
```

## #13 Stream

- Sekarang kita akan menggunakan fitur Stream menggunakan NodeJS Redis

### Kode: Publish Stream

```ts
it("should support publish stream", async () => {
	for (let i = 0; i < 10; i++) {
		await redis.xadd(
			"members",
			"*",
			"name",
			`Eko ${i}`,
			"address",
			"Indonesia",
		);
	}
});
```

### Kode: Create Consumer

```ts
it("should support consumer group stream", async () => {
	await redis.xgroup("CREATE", "members", "group-1", "0");
	await redis.xgroup("CREATECONSUMER", "members", "group-1", "consumer-1");
	await redis.xgroup("CREATECONSUMER", "members", "group-1", "consumer-2");
});
```

### Kode: Get Stream

```ts
it("should support consume stream", async () => {
	const result = await redis.xreadgroup(
		"GROUP",
		"group-1",
		"consumer-1",
		"COUNT",
		2,
		"BLOCK",
		3000,
		"STREAMS",
		"members",
		">",
	);
	expect(result).not.toBeNull();
	console.info(JSON.stringify(result, null, 2));
});
```

## #14 Pub Sub

- Sekarang kita akan menggunakan fitur Pub Sub menggunakan NodeJS Redis

### Kode: Subscribe PubSub

```ts
if('should can subsribe pubsub', async () => {
	redis.subscribe('channel-1')
	redis.on('message', (channel, message) => {
		console.info(`Message from channel ${channel}: ${message}`)
	})

	// wait 60 seconds
	await new Promise(resolve => setTimeout(resolve, 60000))
}, 60000)
```

### Kode: Publish PubSub

```ts
it("should can publish pubsub", async () => {
	for (let i = 0; i < 10; i++) {
		await redis.publish("channel-1", "Hello World " + 1);
	}
});
```

## #15 Penutup
