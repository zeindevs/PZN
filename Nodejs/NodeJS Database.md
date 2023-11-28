# NodeJS Database

## Sebelum Belajar

- Kelas JavaScript dari Programmer Zaman Now
- Kelas MySQL dari Programmer Zaman Now
- NodeJS Dasar
- NodeJS Unit Test

## #1 Pengenalan NodeJS Database

### NodeJS Standard Library

- Tidak seperti teknologi lain seperti PHP, Java, dan lain-lain yang menyediakan standard library untuk database
- Di NodeJS Standard Library sayangnya tidak menyediakan fitur untuk database
- Oleh karena itu, untuk menggunakan database kita harus menambahkan dependency lain, tergantung database nya

### Database Package

- Ada banyak database package library yang bisa kita gunakan, tergantung databasenya
- Hal ini juga menjadi masalah, karena akhirnya tidak database, kita harus menggunakan package library yang berbeda, dan cara menggunakannya pun berbeda-beda
- MySQL : <https://www.npmjs.com/package/mysql2>
- PostgreSQL : <https://www.npmjs.com/package/pg>
- Oracle : <https://www.npmjs.com/package/oracledb>
- Dan lain-lain

### Masalah dengan Database Package Library

- Karena menggunakan database package library terlalu spesific dengan library yang digunakan, akhirnya kebanyakan programmer NodeJS menggunakan library ORM untuk memanipulasi data di database
- Oleh karena itu, di kelas ini pun, kita tidak akan menggunakan database package library secara langsung, kita akan menggunakan library ORM untuk memanipulasi data di database nya
- Detail ORM akan kita bahas di materi selanjutnya

## #2 Pengenalan Object Relational Mapping

### Pengenalan ORM

- ORM (Object Relational Mapping) adalah teknik dalam pemrograman, dimana kita melakukan pemetaan data di database dalam konsep relational object
- Oleh karena itu, ORM sering banyak digunakan di database dengan jenis relational, seperti MySQL, PostgreSQL, Oracle, dan sejenisnya
- Konsep ORM sendiri adalah membuat representasi object di aplikasi yang merepresentasikan data (table) di database, sehingga ketika melakukan manipulasi data (table) di database, seakan-akan kita melakukan manipulasi object di pemrograman

### Diagram Cara Kerja ORM

![Diagram Cara Kerja ORM](./images/nodejs-database-01.jpg)

### Contoh Library ORM di NodeJS

- Sequalize : <https://sequelize.org/>
- TypeORM : <https://typeorm.io/>
- Prisma : <https://www.prisma.io/>

### Prisma

- Di kelas ini, kita akan menggunakan Prisma sebagai ORM nya
- Di awal kelas, kita akan belajar menggunakan RAW SQL (tanpa fitur ORM)
- Selanjutnya, kita akan belajar menggunakan ORM di Prisma

## #3 Pengenalan Prisma

- Prisma adalah salah satu ORM di NodeJS yang populer
- Prisma memiliki banyak fitur untuk mempermudah kita mengelola data di database, dari mulai migration, schema, dan juga type safe
- Prisma sendiri selain bisa digunakan ketika kita menggunakan JavaScript, Prisma juga terintegrasi dengan baik dengan TypeScript
- Prisma adalah library yang Free dan OpenSource
- <https://www.prisma.io/>

### Prisma Component

Prisma memiliki banyak komponen yang bisa digunakan, seperti

- Prisma Schema, untuk memetakan schema data di database dengan di aplikasi
- Prisma Client, sebagai client untuk ORM
- Prisma Migrate, sebagai database migration untuk mengelola versi schema di database
- Prisma CLI, aplikasi berbasis terminal untuk mengelola project prisma
- Prisma Studio, aplikasi berbasis UI untuk mengelola data di database kita secara mudah
- Introspection, untuk membuat Prisma Schema secara otomatis dari table di database yang sudah ada, cocok untuk migrasi dari aplikasi yang sudah jadi ke Prisma

## #4 Membuat Project

- `npm init`
- Ubah type menjadi module di `package.json`

### Menambah Library Jest untuk Unit Test

- `npm install --save-dev jest @types/jest`

### Menambah Library Babel

- `npm install --save-dev babel-jest @babel/preset-env`
- <https://babeljs.io/setup#installation>

## #5 Menginstall Prisma

- Untuk menginstall prisma, kita bisa menggunakan perintah :
- `npm install prisma --save-dev`
- Selanjutnya, kita tidak bisa langsung menggunakan prisma, kita perlu melakukan pengaturan terlebih dahulu menggunakan Prisma CLI yang terdapat di perintah :
- `npx prisma`

### Membuat Project Prisma

- Untuk membuat project prisma pertama kali, kita perlu menggunakan perintah :
- `npx prisma init`
- Secara otomatis prisma akan membuat file :
- `.env` yang berisi konfigurasi environment variable
- `prisma/schema.prisma`, yang berisikan schema pemetaan dengan database

## #6 Database Connection

- Secara default, saat membuat project prisma, prisma akan menggunakan koneksi database PostgreSQL
- Karena di kelas ini kita akan menggunakan MySQL, jadi kita akan ubah konfigurasinya ke MySQL
- Prisma menggunakan konsep database url untuk membuat koneksi ke databasenya, dan formatnya bisa kita lihat tergantung jenis database nya disini :
- <https://www.prisma.io/docs/concepts/database-connectors>

### Kode: Prisma Schema

```prisma
generator client {
	provider = "prisma-client.js"
}

datasource db {
	provider = "mysql"
	url 		 = env("DATABASE_URL")
}
```

### Kode: Environment Variable

```env
DATABASE_URL="mysql://root:password@localhost:3306/belajar_nodejs_database"
```

## #7 Generate

- Prisma memiliki fitur type safe (beberapa IDE bisa melakukan auto complete), hal ini dikarenakan prisma akan melakukan generate kode dari schema yang kita buat
- Setiap kita melakukan perubahan kode di file schema prisma, kita harus melakukan perintah :
- `npx prisma generate`
- Hal ini agar kode perubahan di schema akan dibuat dalam bentuk kode JavaScript

### Log: Generate Schema

`npx prisma generate`

![Log: Generate Schema](./images/nodejs-database-02.jpg)

### Membuat Table

- Karena prisma meminta minimal terdapat model schema, maka kita akan membuat table contoh terlebih dahulu
- Detail untuk model di schema, akan kita bahas di materi tersendiri

### Kode: Create Table Sample

```sql
CREATE TABLE sample
(
	id VARCHAR(100) NOT NULL,
	name VARCHAR(100) NOT NULL,
	PRIMARY KEY (id)
) ENGINE InnoDB;
```

### Kode: Sample Model

```prisma
model sample {
	id 	 String @id
	name String
}
```

### Log: Generate Schema

`npx prisma generate`

![](./images/nodejs-database-03.jpg)

### Hasil Generate

![](./images/nodejs-database-04.jpg)

## #8 Prisma Client

- Prisma Client merupakan komponen utama di Prisma untuk fitur ORM nya, dimana kita import dari package `@prisma/client`
- Ingat Prisma Client digenerate setelah proses sebelumnya yang kita bahas, jika kita belum generate kode Prisma nya, maka Prisma Client tidak bisa dibuat
- Prisma Client akan secara otomatis membaca data dari environment file `.env` dan juga informasi koneksi database dari prisma schema file

### Kode: Prisma Client

```js
// tests/prisma-client.test.js
import { PrismaClient } from "@prisma/client";

describe("Prisma Client", () => {
	it("should be able to connect", async () => {
		const prisma = new PrismaClient();
		await prisma.$connect();

		// do something

		await prisma.$disconnect();
	});
});
```

### Jumlah Object Prisma Client

- Saat membuat aplikasi, Prisma Client itu hanya dibuat sekali saja, jadi bukan berarti tiap kita mau memanipulasi data di database, kita harus buat Prisma Client
- Oleh karena itu, sebaiknya kita buat file berisi variable object Prisma Client , dan jika membutuhkannya, cukup ambil dari variable tersebut

### Kode: Prisma Client Singleton

```js
// src/prisma-client.js
import { PrismaClient } from "@prisma/client";

export const prismaClient = new PrismaClient();
```

### Konfigurasi Prisma Client

- Saat membuat Prisma Client juga, kita bisa melakukan pengaturan Log untuk Prisma Client
- <https://www.prisma.io/docs/reference/api-reference/prisma-client-reference>
- Jika Prisma Client cukup dibuat satu kali, lantas bagaimana jika aplikasi kita banyak yang membutuhkan koneksi ke database? Prisma Client sudah mengatur jumlah koneksi dalam pool (tempat menyimpan koneksi)
- Kita juga bisa mengatur jumlah koneksi dan timeout pada konfigurasi database url kita
- <https://www.prisma.io/docs/concepts/database-connectors>

### Kode: Konfigurasi Database

```env
DATABASE_URL="mysql://root:password@localhost:3306/belajar_nodejs_database?connection_limit=5"
```

### Kode: Konfigurasi Log Prisma

```js
// src/prisma-client.js
import { PrismaClient } from "@prisma/client";

export const prismaClient = new PrismaClient({
	errorFormat: "pretty",
	log: ["query", "info", "warn"],
});
```

## #9 Tag Function

- Sebelum kita lanjut materi prisma, kita perlu paham dulu tentang Tag Function di JavaScript
- Tag Function adalah sebuah fitur yang sebenarnya jarang digunakan, namun di Prisma, hal ini digunakan untuk mempermudah kita mengirim perintah SQL ke database menggunakan Prisma Client
- Sebelumnya kita sudah tahu tentang fitur String Template di JavaScript, dimana kita bisa menangkap data dari variable di String
- Tag Function adalah fitur seperti String Template yang bisa dilakukan ketika kita memanggil function

### Kode: Tag Function

```js
function tagFunction(array, ...args) {
	console.log(array);
	console.log(args);
}
```

### Kode: Test Tag Function

```js
// tests/tag-function.test.js
test("tag function", () => {
	const name = "Eko";

	tagFunction`Hello ${name}!, how are you?`;
	tagFunction`Bye ${name}!, see you later!`;
});
```

### Tag Function Parameter

- Sederhananya, Tag Function adalah mengubah data String Template menjadi parameter yang dipecah dalam dua array
- Array pertama menjadi data String nya, dan Array kedua menjadi data parameter nya
- Misal untuk data `\`Hello ${name}!, how are you?`\`, dimana misal `$name` adalah Eko, akan diubah menjadi :
- `Array 1 : [“Hello ”, “!, how are you?”]`
- `Array 2 : [“Eko”]`

### Kode: Contoh SQL Menggunakan Tag Function

```js
// tests/tag-function.test.js
test("tag function sql", () => {
	const name = "Eko";
	const age = 30;

	tagFunction`SELECT * FROM users WHERE name = ${name} AND age = ${age}`;
});
```

## #10 Execute SQL

- Prisma Client memiliki method bernama `$executeRaw()`, yang bisa digunakan untuk mengirim data SQL untuk memanipulasi data, seperti `INSERT`, `UPDATE` atau `DELETE`
- Return value dari `$executeRaw()` adalah `Promise<Number>` yang berisi jumlah data yang terkena impact dari operasi SQL yang kita lakukan
- `$executeRaw()` menggunakan Tag Function untuk membuat SQL nya, dan ini sudah aman dari SQL Injection, sehingga kita tidak perlu takut orang mengirim data yang berbahaya yang bisa membuat kode SQL kita rusak

### Kode: Insert Simple

```js
// tests/execute-sql.test.js
import { prismaClient } from "../src/prisma-client.js";

describe("Prisma Client", () => {
	it("should be able to execute sql", async () => {
		const id = "1";
		const name = "Eko Kurniawan";

		const impacted =
			await prismaClient.$executeRaw`INSERT INTO sample(id, name) VALUES (${id}, ${name})`;
		expect(impacted).toBe(1);
	});
});
```

## #11 Query SQL

- Prisma Client memiliki method bernama `$queryRaw()`, yang bisa digunakan untuk mengirim SQL query untuk mengambil data menggunakan `SELECT`
- Return value dari `$queryRaw()` adalah `Promise<Array>` yang berisikan data hasil query nya
- `$queryRaw()` menggunakan Tag Function untuk membuat SQL nya, dan ini sudah aman dari SQL Injection juga

### Kode: Query SQL

```js
// tests/query-sql.test.js
import { prismaClient } from "../src/prisma-client.js";

describe("Prisma Client", () => {
	it("should be able to query sql", async () => {
		const id = "1";

		const samples =
			await prismaClient.$queryRaw`SELECT * FROM sample WHERE id = ${id}`;

		for (const sample of samples) {
			console.info(`Result sample id: ${sample.ud} and name ${sample.name}`);
		}
	});
});
```

## #12 Prisma ORM

- Menggunakan Prisma sebenarnya akan sangat jarang kita menggunakan Raw SQL seperti pada materi-materi sebelumnya
- Oleh karena itu, mulai sekarang kita akan fokus membahas fitur ORM di Prisma nya

## #13 Model

- Saat membuat table di database, maka kita perlu membuat model di Prisma, hal ini agar kode untuk memanipulasi data model tersebut di generate oleh Prisma CLI sehingga bisa digunakan menggunakan Prisma Client
- Untuk membuat model, kita bisa membuat model file prisma schema
- Nama model akan menjadi nama table di database, jika kita ingin mengubahnya, kita bisa gunakan `@@map(“namatable”)`
- Kolom yang boleh `null`, kita perlu tandai dengan `?` (tanda tanya)
- Untuk kolom `Primary Key`, perlu kita tandai dengan `@id`
- Setelah mengubah model, jangan lupa untuk generate Prisma Client menggunakan perintah :
- `npx prisma generate`

### Kode: Model Customer

```prisma
model Customer {
	id    String @id
	name  String
	email String @unique
	phone String @unique

	@@map('customers')
}
```

### Kode: Create Table Customer

```sql
CREATE TABLE customers
(
	id VARCHAR(100) NOT NULL,
	name VARCHAR(100) NOT NULL,
	email VARCHAR(100) NOT NULL,
	phone VARCHAR(100) NOT NULL,
	PRIMARY KEY (id),
	CONSTRAINT customers_email_unique UNIQUE (email),
	CONSTRAINT customers_phone_unique UNIQUE (phone)
) ENGINE InnoDB;
```

## #14 CRUD

- Setelah membuat generate code dari Model, secara otomatis kita bisa melakukan operasi CRUD (Create, Read, Update, dan Delete) menggunakan Prisma Client
- Prisma Client otomatis memiliki field dengan nama model yang kita buat dalam lowercase, misal saat sebelumnya kita menggunakan nama model Customer, maka akan dibuat field `PrismaClient.customer` yang merupakan Prisma Model Query
- <https://www.prisma.io/docs/reference/api-reference/prisma-client-reference#model-queries>

### Kode: Insert Customer

```js
// tests/crud.test.js
it("should be able to create customer", async () => {
	const customer = await prismaClient.customer.create({
		data: {
			id: "khannedy",
			email: "khannedy@example.com",
			name: "Eko Khannedy",
			phone: "0993137183",
		},
	});

	expect(customer.id).toBe("khannedy");
	expect(customer.email).toBe("khannedy@example.com");
	expect(customer.name).toBe("Eko Khannedy");
	expect(customer.phone).toBe("0993137183");
});
```

### Kode: Update Customer

```js
// tests/crud.test.js
it("should be able to update customer", async () => {
	const customer = await prismaClient.customer.update({
		data: {
			name: "Eko Kurniawan Khannedy",
		},
		where: {
			id: "khannedy",
		},
	});

	expect(customer.id).toBe("khannedy");
	expect(customer.email).toBe("khannedy@example.com");
	expect(customer.name).toBe("Eko Kurniawan Khannedy");
	expect(customer.phone).toBe("0993137183");
});
```

### Kode: Read Customer

```js
// tests/crud.test.js
it("should be able to read customer", async () => {
	const customer = await prismaClient.customer.findUnique({
		where: {
			id: "khannedy",
		},
	});

	expect(customer.id).toBe("khannedy");
	expect(customer.email).toBe("khannedy@example.com");
	expect(customer.name).toBe("Eko Kurniawan Khannedy");
	expect(customer.phone).toBe("0993137183");
});
```

### Kode: Delete Customer

```js
// tests/crud.test.js
it("should be able to delete customer", async () => {
	const customer = await prismaClient.customer.delete({
		where: {
			id: "khannedy",
		},
	});

	expect(customer.id).toBe("khannedy");
	expect(customer.email).toBe("khannedy@example.com");
	expect(customer.name).toBe("Eko Kurniawan Khannedy");
	expect(customer.phone).toBe("0993137183");
});
```

## #15 Transaction

- Kita tahu bahwa di database terdapat fitur transaction
- Di Prisma Client, kita bisa menggunakan method `$transaction()` untuk melakukan transaksi
- Terdapat dua mekanisme untuk melakukan transaksi di Prisma Client :
- `Sequential Transaction`, dimana kita kirim semua operasi Prisma Client sebagai array, atau
- `Interactive Transaction`, dimana kita bisa menggunakan callback function yang berisi kode transaksi nya

### Kode: Sequential Transaction

```js
const [eko, kurniawan] = await prismaClient.$transaction({
	prismaClient.customer.create({
		data: {
			id: "eko",
			email: "eko@example.com",
			name: "Eko Khannedy",
			phone: "082781728",
		}
	}),
	prismaClient.customer.create({
		data: {
			id: "kurniawan",
			email: "kurniawan@example.com",
			name: "Eko Khannedy",
			phone: "089178318",
		}
	}),
});

export(eko.name).toBe("Eko Khannedy");
export(kurniawan.name).toBe("Eko Khannedy")
```

### Kode: Interactive Transaction

```js
const [eko, kurniawan] = await prismaClient.$transaction(async (prisma) => {
	const eko = await prisma.customer.create({
		data: {
			id: "eko",
			email: "eko@example.com",
			name: "Eko Khannedy",
			phone: "082781728",
		},
	});
	const kurniawan = await prisma.customer.create({
		data: {
			id: "kurniawan",
			email: "kurniawan@example.com",
			name: "Eko Khannedy",
			phone: "089178318",
		},
	});

	return [eko, kurniawan];
});
```

## #16 CRUD Many

- Sebelumnya kita sudah bahas tentang CRUD menggunakan Prisma Client
- Namun hanya untuk satu data
- Prisma Client juga mendukung untuk melakukan CRUD untuk lebih dari satu data, kita bisa menggunakan method dengan suffix Many

### Kode: Create Many Customer

```js
// tests/crud-many.test.js
it("should be able to create many", async () => {
	const { count } = await prismaClient.customer.createMany({
		data: [
			{
				id: "eko",
				email: "eko@example.com",
				name: "Eko Khannedy",
				phone: "082781728",
			},
			{
				id: "kurniawan",
				email: "kurniawan@example.com",
				name: "Eko Khannedy",
				phone: "089178318",
			},
		],
	});
});
```

### Update Many

- Update Many di Prisma Client bukan dengan mengirim banyak data, lalu data tersebut di update satu per satu
- Update Many di Prisma Client itu adalah operasi update, yang bisa berdampak ke banyak data
- Lantas apa bedanya dengan update single? bedanya untuk update single, - Prisma akan melakukan query terlebih dahulu, untuk memastikan bahwa cuma ada 1 data yang tersedia, dan untuk query nya pun hanya bisa di primary key atau unique colom
- Sedangkan di Update Many, kita bisa update dengan query pada kolom apapun, karena bisa berdampak ke banyak data

### Kode: Update Customer

```js
// tests/crud-many.test.js
it("should be able to update many", async () => {
	const { count } = await prismaClient.customer.updateMany({
		data: {
			email: "ekologi@example.com",
		},
		where: {
			name: "Eko",
		},
	});

	expect(count).toBe(1);
});
```

### Delete Many

- Delete Many juga sama dengan Update Many, dimana kita bisa melakukan operasi Delete yang bisa berdampak ke banyak data
- Dan bedanya dengan Delete Single adalah sama seperti Update Single, Prisma Client akan melakukan select terlebih dahulu untuk memastikan bahwa yang dihapus adalah satu data

### Kode: Delete Many

```js
// tests/crud-many.test.js
it("should be able to delete many", async () => {
	const { count } = await prismaClient.customer.deleteMany({
		where: {
			name: "Tidak Ada",
		},
	});

	expect(count).toBe(0);
});
```

### Kode: Read Many

```js
// tests/crud-many.test.js
it("should be able to read many", async () => {
	const customers = await prismaClient.customer.findMany();

	expect(customers.length).toBe(3);
});
```

## #17 Paging

- Method `findMany()` di Prisma Client juga mendukung paging menggunakan parameter skip dan take
- `Take` adalah jumlah maksimal data yang diambil
- `Skip` adalah jumlah data yang akan di skip diawal

### Kode: Paging

```js
// tests/paging.test.js
const page1 = await prismaClient.customer.findMany({
	skip: 0,
	take: 1,
});
expect(page1.length).toBe(1);

const page2 = await prismaClient.customer.findMany({
	skip: 1,
	take: 1,
});
expect(page2.length).toBe(1);

const page2 = await prismaClient.customer.findMany({
	skip: 2,
	take: 1,
});
expect(page2.length).toBe(1);
```

## #18 Sorting

- Method `findMany()` juga memiliki parameter `orderBy` untuk mengurutkan datanya
- Kita bisa tentukan apakah akan diurutkan berdasarkan `asc` atau `desc`

### Kode: Sorting

```js
// tests/sorting.test.js
it("should be able to read many with sorting", async () => {
	const customers = await prismaClient.customer.findMany({
		skip: 0,
		take: 10,
		orderBy: [
			{
				name: "desc",
			},
			{
				email: "asc",
			},
		],
	});

	for (const customer of customers) {
		console.info(customer.name);
	}
});
```

## #19 Select Fields

- Saat menggunakan Prisma Client, hampir semua operasi CRUD dan juga `findMany()` akan melakukan select ke semua field / kolom
- Lantas bagaimana jika kita ingin membatasi field / kolom yang di select? Kita bisa tambahkan option select ketika melakukan operasi CRUD atau `findMany()`

### Kode: Create with Select

```js
// tests/select-fields.test.js
it("should be able create and select", async () => {
	const customer = await prismaClient.customer.create({
		data: {
			id: "budi",
			email: "budi@example.com",
			name: "Budi Nugraha",
			phone: "0918291",
		},
		select: {
			id: true,
			name: true,
		},
	});

	expect(customer.id).toBe("budi");
	expect(customer.name).toBe("Budi Nugraha");
	expect(customer.email).toBeUndefined();
	expect(customer.phone).toBeUndefined();
});
```

### Kode: Find Many with Select

```js
// tests/select-fields.test.js
it("should be able find many", async () => {
	const customers = await prismaClient.customer.findMany({
		select: {
			id: true,
			name: true,
		},
	});

	for (const customer of customers) {
		expect(customer.id).toBeDefined();
		expect(customer.name).toBeDefined();
		expect(customer.email).toBeUndefined();
		expect(customer.phone).toBeUndefined();
	}
});
```

## #20 Count

- Prisma Client memiliki method untuk melakukan query aggregate count dengan method `count()`
- Cara penggunaannya mirip dengan method `findMany()`

### Kode: Count

```js
// tests/count.test.js
it("should be able count", async () => {
	const total = await prismaClient.customer.count({
		where: {
			name: "Eko",
		},
	});

	expect(total).toBe(1);
});
```

## #21 Aggregate

- Kita tahu bahwa function aggregate di database tidak hanya count, lantas bagaimana dengan jenis function aggregate lainnya?
- Prisma Client menyediakan method bernama `aggregate()` yang bisa digunakan untuk melakukan query aggregate

### Kode: Create Table Product

```sql
CREATE TABLE products
(
	id VARCHAR(100) NOT NULL,
	name VARCHAR(100) NOT NULL,
	price INT NOT NULL,
	stock INT NOT NULL,
	category VARCHAR(100) NOT NULL,
	PRIMARY KEY (id)
) ENGINE InnoDB;
```

### Kode: Insert Data Product

```sql
INSERT INTO products (id, name, price, stock, category)
VALUES ('P0001', 'A', 1000, 100, "K1"),
			 ('P0002', 'B', 2000, 200, "K1"),
			 ('P0003', 'C', 3000, 300, "K1"),
			 ('P0004', 'D', 4000, 400, "K2"),
			 ('P0005', 'E', 5000, 500, "K2");
```

### Kode: Model Product

```prisma
model Product {
	id 	     String @id
	name     String
	price    Int
	stock    Int
	category String

	@@map("products")
}
```

### Function Aggregate

- Berikut adalah beberapa function aggregate yang didukung oleh Prisma Client :
- `_count, _min, _max, _sum, _avg`
- Lantai bagaimana dengan function aggregate lainnya? Maka kita harus lakukan manual menggunakan Raw SQL

### Kode: Function Aggregate

```js
// tests/aggregate.test.js
it("should be able query using aggregate", async () => {
	const result = await prismaClient.product.aggregate({
		_min: {
			price: true,
		},
		_max: {
			price: true,
		},
		_avg: {
			price: true,
		},
	});

	expect(result._min.price).toBe(1000);
	expect(result._max.price).toBe(5000);
	expect(result._avg.price).toBe(3000);
});
```

### Group By

- Saat menggunakan function aggregate, kadang kita butuh melakukan `GROUP BY`
- Pada kasus ini, kita bisa menggunakan function `groupBy()`, tidak menggunakan function `aggregate()` lagi

### Kode: Group By

```js
// tests/groupby.test.js
it("should be able query using aggregate and group by", async () => {
	const result = await prismaClient.product.groupBy({
		by: ["category"],
		_min: {
			price: true,
		},
		_max: {
			price: true,
		},
		_avg: {
			price: true,
		},
	});

	for (const item of result) {
		console.info(
			`Category: ${item.category}, min ${item._min.price}, max ${item._max.price}, avg ${item._avg.price}`,
		);
	}
});
```

### Having

- Pada method `groupBy()` juga terdapat attribute having yang bisa digunakan untuk kondisi `HAVING` pada SQL query yang akan kita lakukan

### Kode: Having

```js
// tests/having.test.js
const result = await prismaClient.product.groupBy({
	by: ["category"],
	_min: {
		price: true,
	},
	_max: {
		price: true,
	},
	_avg: {
		price: true,
	},
	having: {
		price: {
			_avg: {
				gt: 3000,
			},
		},
	},
});
```

## #22 Where Condition and Operator

- Sampai saat ini, kita hanya selalu melakukan operasi equals `(=)` ketika melakukan query ke database
- Prisma Client mendukung banyak sekali where condition dan operator, yang bisa kita gunakan agar bisa membuat query secara flexible
- Semua condition dan operator yang didukung bisa dilihat di halaman dokumentasinya
- <https://www.prisma.io/docs/reference/api-reference/prisma-client-reference#filter-conditions-and-operators>

### Kode: Where Condition dan Operator

```js
// tests/where-cond-and-oper.test.js
const products = await prismaClient.product.findMany({
	where: {
		OR: [{ name: "A" }, { name: "B" }],
	},
	orderBy: [
		{
			name: "asc",
		},
	],
});

expect(products).toHaveLength(1);
expect(products[0].name).toBe("A");
expect(products[1].name).toBe("B");
```

## #23 Prisma Schema

- Sebelumnya kita sudah membuat model di Prisma Schema, sekarang kita akan bahas lebih detail tentang Prisma Schema
- Prisma Schema berisikan informasi tentang database yang digunakan oleh Prisma Client, dari mulai koneksi sampai model data
- Hal ini digunakan untuk men-generate kode Prisma Client
- Semua detail referensi Prisma Schema bisa kita baca disini :
- <https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference>

### Model

- Saat kita membuat model di Prisma Schema, kita harus memiliki sebuah field/column yang unique
- Biasanya berupa primary key
- Dan untuk model field, biasanya merepresentasikan nama kolom di table di database
- <https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model>

### Tipe Data

- Setiap kita menambahkan field di Model, kita harus menentukan tipe data yang akan digunakan
- Tambahkan `?` (tanda tanya) pada tipe data tersebut jika tipe data tersebut bisa bernilai `null`
- Ada banyak sekali tipe data yang didukung oleh Prisma Schema, kita bisa melihatnya di halaman dokumentasi nya
- <https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model-field-scalar-types>

### Model Attribute

- Saat kita membuat model, kadang ada informasi tambahan yang perlu kita beritahu ke Prisma Schema
- Contoh sebelumnya misal kita ingin membuat nama model berbeda dengan nama table, maka kita gunakan `@@map()`
- Masih banyak model attribute yang bisa kita gunakan, kita bisa lihat di dokumentasinya
- <https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#attributes>

### Enum

- Di database MySQL dan PostgreSQL, kita bisa membuat tipe data Enum
- Kita juga bisa memberi tahu tentang enum di Prisma Schema
- Kita bisa membuat enum seperti pada dokumentasinya :
- <https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#enum>

## #24 Auto Increment

- Beberapa database biasanya mendukung auto increment pada Primary Key, seperti MySQL dan PostgreSQL
- Kita bisa memberitahu Prisma Schema bahwa field tersebut adalah auto increment, sehingga kita tidak wajib memasukkan datanya ketika menggunakan Prisma Client
- Kita cukup gunakan attribute `@default()` dan pilih jenis auto increment yang tersedia di dokumentasi :
- <https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#attribute-functions>

### Kode: Create Table Category

```sql
CREATE TABLE categories
(
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(100) NOT NULL,
	PRIMARY KEY (id)
) ENGINE InnoDB;
```

### Kode: Prisma Schema

```prisma
model Category {
	id   Int @id @default(autoincrement())
	name String

	@@map("categories")
}
```

### Kode: Auto Increment

```js
// tests/auto-increment.test.js
it("should be able to execute sql", async () => {
	const category = await prismaClient.category.create({
		data: {
			name: "Food",
		},
	});

	console.info(category);
	expect(category).toHaveProperty("id");
});
```

## #25 One to One Relationship

- Ketika kita membuat table dengan relasi one to one, kita juga perlu memberitahunya di Prisma Schema
- Hal ini agar urusan join bisa dilakukan secara otomatis oleh Prisma Client
- Untuk membuat relasi one to one, kita bisa menggunakan @relation, dan itu ditempatkan di model yang melakukan join column nya

### Kode: Create Table Wallet

```sql
CREATE TABLE wallet
(
	id VARCHAR(100) NOT NULL,
	balance INT NOT NULL,
	customer_id VARCHAR(100) NOT NULL,
	PRIMARY KEY (id),
	CONSTRAINT wallet_customer_id_fk FOREIGN KEY (customer_id) REFERENCES customers (id),
	CONSTRAINT wallet_customer_id_unique UNIQUE (customer_id)
) ENGINE InnoDB;
```

### Kode: Prisma Schema

```prisma
model Customer {
	id 		 String @id
	name 	 String
	email  String @unique
	phone  String @unique
	wallet Wallet?

	@@map("customers")
}

model Wallet {
	id					String @id
	balance 		Int
	customer 		Customer @relation(fields: [customer_id], references:[id])
	customer_id String @unique

	@@map("wallet")
}
```

### Kode: Insert with Include

```js
// tests/one-relation.test.js
it("should be able to create one to one", async () => {
	const wallet = await prismaClient.wallet.create({
		data: {
			id: "eko",
			customer_id: "eko",
			balance: 1000000,
		},
		include: {
			customer: true,
		},
	});

	console.info(wallet);
});
```

### Kode: Insert with Relation

```js
// tests/one-relation.test.js
it("should be able to create one to one with relation", async () => {
	const customer = await prismaClient.customer.create({
		data: {
			id: "joko",
			name: "Joko",
			email: "joko@example.com",
			phone: "0889218728",
			wallet: {
				create: {
					id: "eko",
					balance: 1000000,
				},
			},
		},
		include: {
			wallet: true,
		},
	});

	console.info(customer);
});
```

### Kode: Find dengan Include

```js
// tests/one-relation.test.js
it("should be able to find one to one", async () => {
	const customer = await prismaClient.customer.findUnique({
		where: {
			id: "eko",
		},
		include: {
			wallet: true,
		},
	});

	console.info(customer);
});
```

### Relation Operator

- Ketika kita mau melakukan Read data untuk relasi, kita bisa gunakan relation operator
- Namun kita harus cek jenis relasi, contoh beberapa operator hanya bisa untuk relasi To Many misalnya
- <https://www.prisma.io/docs/reference/api-reference/prisma-client-reference#relation-filters>

### Kode: Find Many dengan Include

```js
// tests/one-relation.test.js
it("should be able to find one to one", async () => {
	const customers = await prismaClient.customer.findMany({
		where: {
			wallet: {
				isNot: null,
			},
		},
		include: {
			wallet: true,
		},
	});

	console.info(customers);
});
```

## #26 One to Many Relationship

- Relasi one to many sebenarnya mirip dengan one to one, yang membedakan adalah di table yang memiliki join column, tidak terdapat aturan unique, sehingga bisa lebih dari satu kali
- Contoh kasus misal Customer dengan Comment, dimana Customer bisa membuat banyak data Comment
- Sehingga dari Customer ke Comment adalah relasi One to Many, sedangkan kebalikannya adalah relasi Many to One
- Untuk menambah informasi One to Many, sama seperti One to One, menggunakan `@relation()`, namun tipe datanya perlu ditambahkan `[]` sebagai penanda bahwa itu array/list

### Kode: Create Table Comment

```sql
CREATE TABLE comments
(
	id INT NOT NULL AUTO_INCREMENT,
	customer_id VARCHAR(100) NOT NULL,
	title VARCHAR(200) NOT NULL,
	description TEXT,
	PRIMARY KEY (id)
	CONSTRAINT comments_customer_id_fk FOREIGN KEY (customer_id) REFERENCES customers (id)
) ENGINE InnoDB;
```

### Kode: Insert Comment

```sql
INSERT INTO comments(customer_id, title, description)
VALUES ('eko', 'Comment 1', "Sample comment 1"),
			 ('eko', 'Comment 2', "Sample comment 2"),
			 ('budi', 'Comment 1', "Sample comment 1"),
			 ('budi', 'Comment 2', "Sample comment 2")
```

### Kode: Prisma Schema

```prisma
model Customer {
	id 		 String @id
	name 	 String
	email  String @unique
	phone  String @unique
	wallet Wallet?
	comments Comment[]

	@@map("customers")
}

model Comment {
	id Int @id @default(autoincrement())
	title String
	description String?
	customer_id String
	customer Customer @relation(fields: [customer_id], references: [id])

	@@map("comments")
}
```

### Kode: Insert with Include

```js
const comment = await prismaClient.comment.create({
	data: {
		customer_id: "eko",
		title: "Insert Comment",
		description: "Description Comment",
	},
	include: {
		customer: true,
	},
});

console.info(comment);
```

### Kode: Insert with Relation

```js
const customer = await prismaClient.customer.create({
	data: {
		id: "alex",
		name: "Alex",
		email: "alex@example.com",
		phone: "09089182912",
		comments: {
			createMany: {
				data: [
					{
						title: "Comment 1",
						description: "Description 1",
					},
					{
						title: "Comment 2",
						description: "Description 2",
					},
				],
			},
		},
	},
	include: {
		comments: true,
	},
});

console.info(customer);
```

### Kode: Find Many with Relation

```js
const customers = await prismaClient.customer.findMany({
	where: {
		comments: {
			some: {
				title: {
					contains: "Comment",
				},
			},
		},
	},
	include: {
		comments: true,
	},
});

console.info(customers);
```

## #27 Many to Many Relationship

- Jenis relasi terakhir yang paling kompleks adalah Many to Many
- Untuk Many to Many, kita juga bisa lakukan di Prisma Schema
- Seperti yang sudah kita ketahui, relasi Many to Many Relationship membutuhkan table penghubung, sehingga kita perlu membuat table penghubung terlebih dahulu
- Sekarang kita akan coba buat relasi dari Customer ke Product, misal kita akan buat fitur Likes, dimana satu product bisa di like oleh banyak customer, dan satu customer bisa like banyak product

### Kode: Create Table Likes

```sql
CREATE TABLE likes
(
	customer_id VARCHAR(100) NOT NULL,
	product_id VARCHAR(100) NOT NULL,
	PRIMARY KEY (customer_id, product_id),
	CONSTRAINT likes_customer_id_fk FOREIGN KEY (customer_id) REFERENCES customers (id),
	CONSTRAINT likes_product_id_fk FOREIGN KEY (product_id) REFERENCES products (id)
) ENGINE InnoDB;
```

### Kode: Prisma Schema (1)

```prisma
model Customer {
	id 		 	 String @id
	name 	 	 String
	email  	 String @unique
	phone  	 String @unique
	wallet 	 Wallet?
	comments Comment[]
	likes 	 Like[]

	@@map("customers")
}

model Product {
	id 		 	 String @id
	name 		 String
	price 	 Int
	stock 	 Int
	category String
	likedBy  Like[]

	@@map("products")
}
```

### Kode: Prisma Schema (2)

```prisma
model Like {
	customer_id String
	product_id  String
	customer 		Customer @relation(fields: [customer_id], references: [id])
	product  		Product @relation(fields: [product_id], references: [id])

	@@id([customer_id, product_id])
	@@map("likes")
}
```

### Kode: Create Relation

```js
const like = await prismaClient.like.create({
	data: {
		customer_id: "eko",
		product_id: "P0001",
	},
	include: {
		customer: true,
		product: true,
	},
});

console.info(like);
```

### Kode: Find with Include

```js
const customer = await prismaClient.customer.findUnique({
	where: {
		id: "eko",
	},
	include: {
		likes: {
			include: {
				product: true,
			},
		},
	},
});

console.infp(JSON.stringify(customer));
```

### Kode: Find Many with Include

```js
const customers = await prismaClient.customer.findMany({
	where: {
		likes: {
			some: {
				product: {
					name: {
						contains: "A"
					}
				}
			}
		}
	},
	include: {
		likes: {
			include: {
				product true
			}
		}
	}
});

console.infp(JSON.stringify(customers));
```

### Implicit Many to Many Relationship

- Sebelumnya saat membuat relasi Many to Many, kita menyebutkan model untuk table penghubungnya, atau dibilang explicit many to many
- Prisma juga memiliki fitur implicit many to many, dimana kita tidak perlu menyebutkan model penghubungnya
- Namun untuk fitur implicit many to many, ada aturannya
- Nama table harus `_` diikuti nama relasi, dan nama kolom harus A dan B, dimana A adalah kolom model pertama, dan B adalah kolom model kedua, diurutkan secara asc nama model nya
- Misal kita akan coba buat relasi lagi many to many dari Customer ke Product untuk fitur Loves

### Kode: Create Table Loves

```sql
CREATE TABLE _loves
(
	A VARCHAR(100) NOT NULL,
	B VARCHAR(100) NOT NULL,
	PRIMARY KEY (A, B),
	CONSTRAINT customer_loves_fk FOREIGN KEY (A) REFERENCES customers (id),
	CONSTRAINT product_loves_fk FOREIGN KEY (B) REFERENCES products (id)
) ENGINE InnoDB;
```

### Kode: Model

```prisma
model Customer {
	id 		 	 String @id
	name 	 	 String
	email  	 String @unique
	phone  	 String @unique
	wallet 	 Wallet?
	comments Comment[]
	likes 	 Like[]
	loves  	 Product[] @relation("loves")

	@@map("customers")
}

model Product {
	id 		 	 String @id
	name 		 String
	price 	 Int
	stock 	 Int
	category String
	likedBy  Like[]
	lovedBy  Customer[] @relation("loves")

	@@map("products")
}
```

### Kode: Create Implicit Relation

```js
const customer = await prismaClient.customer.update({
	where: {
		id: "eko",
	},
	data: {
		loves: {
			connect: [{ id: "P0001" }, { id: "P0002" }],
		},
	},
	include: {
		loves: true,
	},
});

console.info(JSON.stringify(customer));
```

### Kode: Find Many Implicit Relation

```js
const customers = await prismaClient.customer.findMany({
	where: {
		loves: {
			some: {
				name: {
					contains: "A",
				},
			},
		},
	},
	include: {
		loves: true,
	},
});

console.info(JSON.stringify(customers));
```

## #28 Prisma Migrate

- Sampai sekarang, kita selalu membuat table secara manual
- Prisma sendiri memiliki fitur Prisma Migrate, dimana kita bisa membuat database migration untuk membuat schema database menggunakan Prisma
- Keuntungan menggunakan Prisma Migrate adalah, kita tidak perlu mengeksekusi perintah SQL untuk membuat schema database secara manual lagi

### Mental Model

Sebelum kita buat migration, kita perlu tahu dua jenis mental model dalam Prisma Migrate

- `Model/Entity-first migration`, yaitu dimana programmer biasa membuat Model terlebih dahulu, lalu Prisma Migrate akan membuatkan perintah SQL DDL nya. Cara ini biasanya digunakan ketika membuat aplikasi baru atau melakukan perubahan baru
- `Database-first migration`, yaitu dimana programmer biasa membuat SQL DDL nya terlebih dahulu, lalu menggunakan Prisma Migrate untuk melakukan introspect database untuk membuat model sesuai dengan database. Cara ini biasanya dilakukan ketika menggunakan Prisma untuk database yang sudah ada

### Model/Entity-first migration

![Model/Entity-first migration](./images/nodejs-database-05.jpg)

### Database-first migration

![Database-first migration](./images/nodejs-database-06.jpg)

## #29 Model-first Migration

- Sebelumnya, kita selalu membuat table secara manual, sekarang kita akan coba buat database baru, dan coba lakukan Model-first migration, dimana kita sudah selesai membuat semua model di Prisma Schema nya
- Untuk menjalankan Model-first migration, kita bisa gunakan perintah :
- `npx prisma migrate dev --create-only --name nama_migration`
- Secara otomatis akan dibuatkan file SQL DDL dari model di folder migrations
- Kita bisa review atau edit terlebih dahulu jika mau, dan dan pastikan jika ada yang perlu diubah, tidak hanya di SQL DDL, kita juga perlu ubah di Model nya
- Setelah selesai, kita bisa jalankan migration menggunakan perintah :
- `npx prisma migrate dev`

### Kode: Create New Database

```sql
CREATE DATABASE belajar_nodejs_prisma;

USE belajar_nodejs_prisma;
```

```env
DATABASE_URL="mysql://root:password@localhost:3306/belajar_nodejs_database?connection_limit=5"
```

### Log: Create Migration Script

![Log: Create Migration Script](./images/nodejs-database-07.jpg)

### Hasil Migration Script

![Hasil Migration Script](./images/nodejs-database-08.jpg)

### Tipe Data di Database

- Kadang beberapa jenis tipe data perlu disesuaikan dengan yang ada di database
- Oleh karena itu, kadang kita perlu update model nya agar sesuai dengan tipe data yang sesuai di database
- Kita bisa gunakan attribute di model menggunakan `@db`
- Misal saat kita gunakan String, tipe data di database bisa saja bukan `VARCHAR`, oleh karena itu kita perlu beritahu di model Prisma nya

### Kode: Mengubah Tipe Data Model

```prisma
model Comment {
	id Int @id @default(autoincrement())
	title String @db.VarChar(200)
	description String? @db.Text
	customer_id String @db.VarChar(100)
	customer Customer @relation(fields: [customer_id], references: [id], map: "customers_to_comment_fk")

	@@map("comments")
}
```

### Log: Menjalankan Migration

![Log: Menjalankan Migration](./images/nodejs-database-09.jpg)

### Migration Selanjutnya

- Selanjutnya, setiap melakukan perubahan model, kita perlu membuat migration baru
- Secara otomatis perubahan tersebut akan dibuat SQL DDL nya oleh Prisma Migrate

### Migration Information

- Prisma Migrate melakukan maintain versi file migrasi yang sudah dieksekusi secara otomatis menggunakan table `_prisma_migrations`
- Setiap file migration yang sudah dieksekusi, maka kita tidak boleh mengubahnya lagi, hal ini dikarenakan file migration yang sudah dieksekusi tidak akan pernah dijalankan lagi, jadi percuma saja mengubah file migration lama
- Selain itu, informasi checksum file migration disimpan di table `_prisma_migrations`, jadi jika sampai kita ubah, maka akan terjadi error karena checksum nya akan berubah

## #30 Database-first Migration

- Database-first Migration adalah kebalikan dari Model-first Migration
- Ini biasanya dilakukan ketika database nya sudah ada, dan kita ingin menggunakan Prisma untuk mengelola database nya
- Untuk membuat model dari schema database yang sudah ada, kita bisa gunakan perintah :
- `npx prisma db pull`
- Selanjutnya, kita bisa review perubahan di Schema nya, dan ubah jika dibutuhkan

### Contoh

- Sekarang misal kita akan hapus semua isi model di Prisma Schema
  Lalu kita akan lakukan perintah untuk pull schema db menggunakan :
- `npx prisma db pull`

### Log: Pull Database Schema

![Log: Pull Database Schema](./images/nodejs-database-10.jpg)

### Kode: Prisma Model

```prisma
model categories {
	id 	 Int @id @default(autoincrement())
	name String
}

model comments {
	id 		 			Int @id @default(autoincrement())
	title				String @db.VarChar(200)
	description String? @db.Text
	customer_id String @db.VarChar(100)
	customers 	customers @relation(fields: [customer_id], references: [id], map: "customers_to_comment_fk")

	@@index([customer_id], map: "customer_to_comment_fk")
}
```

## #31 Prisma Studio

- Prisma Studio adalah fitur yang bisa kita gunakan untuk menjalankan web visual untuk memanipulasi data di database berdasarkan Prisma Schema
- Ini sangat cocok dalam proses development ketika kita ingin mudah melihat atau mengubah data di database, tanpa harus menggunakan perintah SQL
- Kita bisa jalankan Prisma Studio dengan perintah :
- `npx prisma studio`
- Secara otomatis akan menjalankan web di localhost port 5555

### Tampilan Prisma Studio

![Tampilan Prisma Studio](./images/nodejs-database-11.jpg)

## #32 Materi Selanjutnya

- NodeJS Validation
- NodeJS RESTful API
- Dan lain-lain
