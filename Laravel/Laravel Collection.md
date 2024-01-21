# Laravel Collection

## Sebelum Belajar

- Sudah mengikuti kelas PHP dari Programmer Zaman Now
- Sudah mengikuti kelas Laravel Dasar

## Agenda

- Pengenalan Laravel Collection
- Collection Method
- Lazy Collection
- Dan lain-lain

## #1 Laravel Collection

### Masalah Array di PHP

- Saat kita menggunakan PHP, kita sering sekali menggunakan data array
- Namun array hanyalah tempat untuk menyimpan data, ketika kita butuh melakukan operasi terhadap data di array nya, maka kita harus lakukan secara manual
- Hal ini kadang menyulitkan ketika operasi yang perlu kita lakukan sangat kompleks
- Di bahasa pemrograman seperti Java, terdapat tipe data Collection untuk mempermudah hal tersebut, sayangnya hal ini tidak ada di PHP

### Laravel Collection

- Laravel menyediakan tipe data Collection untuk menangani keterbatasan tipe data array
- Collection sendiri sebenarnya seperti array, hanya dia direpresentasikan dalam class, dan memiliki banyak sekali method/function yang bisa digunakan untuk mempermudah ketika kita butuh melakukan manipulasi data Collection tersebut
- <https://laravel.com/api/10.x/Illuminate/Support/Collection.html>

## #2 Membuat Project

- `composer create-project laravel/laravel=v10.0.3 belajar-laravel-collection`

### Membuat Collection

- Untuk membuat Collection, Laravel sudah menyediakan global function `collect(array)` yang digunakan untuk mengubah tipe data array menjadi Collection

### Kode: Membuat Collection

```php
class CollectionTest extends TestCase
{
	public function testCreateCollection()
	{
		$collection = collect([1, 2, 3]);
		$this->assertEquals([1, 2, 3], $collection->all());
	}
}
```

## #3 For Each

- Collection adalah struktur data turunan dari Iterable PHP
- Dengan demikian, kita bisa melakukan iterasi data Collection menggunakan perintah for PHP

### Kode: For Each

```php
public function testForEach()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	foreach ($collection as $key => $value) {
		self::assertEquals($Key + 1, $value);
	}
}
```

## #4 Manipulasi Collection

- Collection adalah sebuah class, oleh karena itu untuk memanipulasi data nya, kita perlu menggunakan method yang terdapat di Collection

### Collection Operations

| Method           | Keterangan                                      |
| ---------------- | ----------------------------------------------- |
| `push(data)`     | Menambah data ke paling belakang                |
| `pop()`          | Menghapus data mengambil data paling terakhir   |
| `prepend(data)`  | Menambah data ke paling depan                   |
| `pull(key)`      | Menghapus data mengambil data sesuai dengan key |
| `put(key, data)` | Menghapus data dengan key                       |

### Kode: Manipulasi Collection

```php
public function testCrud()
{
	$collection = collect([]);
	$collection->push(1, 2,3);
	assertEqualsCanonicalizing([1, 2, 3], $collection->all());

	$result = $collection->pop();
	assertEquals(3, $result);
	assertEqualsCanonicalizing([1, 2], $collection->all());
}
```

## #5 Mapping

- Mapping adalah transformasi (mengubah bentuk data) menjadi data lain
- Mapping membutuhkan function sebagai parameter yang digunakan untuk membentuk data lainnya
- Urutan Collection hasil mapping adalah sama dengan urutan collection aslinya

### Mapping Operations

| Method                  | Keterangan                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `map(function)`         | Iterasi seluruh data, dan mengirim seluruh data ke function                                                                                |
| `mapInfo(class)`        | Iterasi seluruh data, dan membuat object baru untuk class dengan mengirim parameter tiap data                                              |
| `mapSpread(function)`   | Iterasi seluruh data, dan mengirim tiap data sebagai parameter di function                                                                 |
| `mapToGroups(function)` | Iterasi seluruh data, dan mengirim tiap data ke function harus mengembalikan single key-value array untuk di group sebagai collection baru |

### Kode: Map

```php
public function testMap()
{
	$collection = collect([1, 2, 3]);
	$result = $collection->map(function ($item) {
		return $item * 2;
	});
	$this->assertEquals([2, 3, 6], $result->all());
}
```

### Kode: Map Into (1)

```php
namespace App\Data;

class Person
{
	var string $name;

	public function __construct(string $name)
	{
		$this->name = $name;
	}

	public function testMapInto()
	{
		$collection = collect(['Eko']);
		$result = $collection->mapInto(Person::class);
		$this->assertEquals([new Person("Eko")], $result->all());
	}
}
```

### Kode: Map Spread

```php
public function testMapSpread()
{
	$collection = collect([['Eko', 'Kurniawan'], ['Khannedy', 'Setiawan']]);
	$result = $collection->mapSpread(function ($firstName, $lastName) {
		$fullName = $firstName . ' ' . $lastName;
		return new Person($fullName);
	});
	assertEquals([
		new Person('Eko Kurniawan'),
		new Person('Khannedy Setiawan'),
	], $result->all());
}
```

### Kode: Map To Groups

```php
$collection = collect([
	[
		'name' => 'Eko',
		'deparment' => 'IT',
	],
	[
		'name' => 'Khannedy',
		'deparment' => 'IT',
	],
	[
		'name' => 'Budi',
		'deparment' => 'HR',
	]
]);

$result = $collection->mapToGroups(function ($item) {
	return [$item['deparment'] => $item['name']];
});
assertEquals([
	'IT' => collect(['Eko', 'Khannedy']),
	'HR' => collect(['Budi']),
], $result->all());
```

## #6 Zipping

- Zipping adalah transformasi yang digunakan untuk menggabungkan dua buah collection.

### Zipping Operations

| Method                      | Keterangan                                                                                          |
| --------------------------- | --------------------------------------------------------------------------------------------------- |
| `zip(collection/array)`     | Menggabungkan tiap item di collection sehingga menjadi collection baru                              |
| `concat(collection/array)`  | Menambahkan collection pada bagian akhir sehingga menjadi collection baru                           |
| `combine(collection/array)` | Menggabungkan collection sehingga collection pertama menjadi key dan collection kedua menjadi value |

### Kode: Zip

```php
public function testZip()
{
	$collection1 = collect([1, 2, 3]);
	$collection2 = collect([4, 5, 6]);
	$collection3 = $collection1->zip($collection2);

	assertEquals([
		collect([1, 4]),
		collect([2, 5]),
		collect([3, 6]),
	], $collection3->all());
}
```

### Kode: Concat

```php
public function testConcat()
{
	$collection1 = collect([1, 2, 3]);
	$collection2 = collect([4, 5, 6]);
	$collection3 = $collection1->concat($collection2);

	assertEquals([1, 2, 3, 4, 5, 6], $collection3->all());
}
```

### Kode: Combine

```php
public function testCombine()
{
	$collection1 = ['name', 'country'];
	$collection2 = ['Eko', 'Khannedy'];
	$collection3 = collect($collection1)->combine($collection2);

	assertEquals([
		'name' => 'Eko',
		'country' => "Indonesia",
	], $collection3->all());
}
```

## #7 Flattening

- Flattening adalah operasi transformasi untuk menjadikan nested collection menjadi flat

### Zipping Operations

| Method              | Keterangan                                                                                              |
| ------------------- | ------------------------------------------------------------------------------------------------------- |
| `collapse()`        | Mengubah tiap array di item collection menjadi flat collection                                          |
| `flatMap(function)` | iterasi tiap data, dikirim ke function yang menghasilkan collection, dan diubah menjadi flat collection |

### Kode: Collapse

```php
public function testCollapse()
{
	$collection = collect([
		[1, 2, 3],
		[4, 5, 6],
		[7, 8, 9],
	]);
	$result = $collection->collapse();
	assertEquals([1, 2, 3, 4, 5, 6, 7, 8, 9], $result->all());
}
```

### Kode: Flat Map

```php
public function testFlatMap()
{
	$collection = collect([
		[
			'name' => "Eko",
			'hobbies' => ['Coding', "Gaming"],
		],
		[
			'name' => 'Khannedy',
			'hobbies' => ['Reading', 'Writing']
		]
	]);

	$hobbies = $collection->flatMap(function ($size) {
		return $item['hobbies'];
	});

	assertEquals(['Coding', 'Gaming', 'Reading', 'Writing'], $hobbies->all());
}
```

## #8 String Representation

- String Representations adalah operasi transformasi untuk mengubah collection menjadi string

### String Representation Operations

| Method                           | Keterangan                                                                                                  |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `join(glue = '', finalGlue= '')` | Mengubah tiap item menjadi string dengan menggabungkan dengan separator glue, dan separator akhir finalGlue |

### Kode: String Representaion

```php
public function testJoin()
{
	$collection = collect(['Eko', 'Khannedy', "Khannedy"]);

	assertEquals('Eko-Khannedy-Khannedy', $collection->join('-'));
	assertEquals('Eko-Khannedy_Khannedy', $collection->join('-', '_'));
}
```

## #9 Filtering

- Filtering adalah salah satu operasi collection yang banyak digunakan
- Filtering membutuhkan function sebagai parameter
- Jika function mengembalikan true, maka data akan diambil, jika false, maka data akan dibuang
- Hati-hati ketika menggunakan data array dengan index number, karena data array bisa dihapus sehingga index akan hilang di Collection baru hasil filter

### Filtering Operations

| Method             | Keterangan                                                                                           |
| ------------------ | ---------------------------------------------------------------------------------------------------- |
| `filter(function)` | Iterasi setiap data, dikirim ke function, jika true maka data diambil, jika false, maka data dibuang |

### Kode: Filter

```php
public function testFilter()
{
	$collection = collect([
		'Eko' => 100,
		'Budi' => 80,
		'Joko' => 90,
	]);
	$result = $collection->filter(function ($item, $key) {
		return $item => 90;
	});

	assertEquals([
		'Eko' => 100,
		'Joko' => 90,
	], $result->all());
}
```

## #10 Partitioning

- Pada filtering, kita akan kehilangan data yang tidak masuk kondisi filter function nya
- Dengan partitioning, kita bisa mendapatkan dua collection yang terdiri dari collection yang masuk filter dan yang tidak masuk filter

### Partitioning Operations

| Method                | Keterangan                                                                                                                                          |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partition(function)` | Iterasi setiap data, dikirim ke function, jika true maka data akan masuk ke collection partama, jika false maka data akan masuk ke collection kedua |

### Kode: Partition

```php
public function testPartition()
{
	$collection = collect([
		'Eko' => 100,
		'Budi' => 80,
		'Joko' => 90,
	]);
	[$result1, $result2] = $collection->partition(function($item, $key) {
		return $item => 90;
	});
	assertEquals(['Eko' => 100, 'Joko' => 90], $result1->all());
	assertEquals(['Budi' => 80], $result2->all());
}
```

## #11 Testing

- Testing adalah operasi untuk mengecek isi data di collection
- Hasil dari testing adalah boolean, dimana true jika sesuai kondisi, dan false jika tidak sesuai kondisi

### Testing Operations

| Method                 | Keterangan                                                                                    |
| ---------------------- | --------------------------------------------------------------------------------------------- |
| `has(array)`           | Mengecek apakah collection memiliki semua key data                                            |
| `hasAny(array)`        | Mengecek apakah collection memiliki salah satu key data                                       |
| `contains(array)`      | Mengecek apakah collection memiliki data value                                                |
| `contains(key, array)` | Mengecek apakah collection memiliki data key dengan value                                     |
| `contains(function)`   | Iterasi tiap data, mengirim ke function dan mengecek apakah salah satu data menghasilkan true |

### Kode: Testing

```php
public function testTesting()
{
	$collection = collect(['Eko', 'Kurniawan', 'Khannedy']);
	self::assertTrue($collection->contains('Eko'));
	self::assertTrue($collection->contains(function ($value, $key) {
		return $value == 'Eko';
	}));
}
```

## #12 Grouping

- Grouping adalah operasi untuk meng-grup kan element-element yang ada di collection.

### Grouping Operations

| Method              | Keterangan                                       |
| ------------------- | ------------------------------------------------ |
| `groupBy(key)`      | Menggabungkan data collection per key            |
| `groupBy(function)` | Menggabungkan data collection per hasil function |

### Kode: Grouping

```php
$collection = collect([
	[
		'name' => 'Eko',
		'deparment' => 'IT',
	],
	[
		'name' => 'Khannedy',
		'deparment' => 'IT',
	],
	[
		'name' => 'Budi',
		'deparment' => 'HR',
	]
]);
$result = $collection->groupBy('deparment');
assertEquals([
	'IT' => collect([
		[
		'name' => 'Eko',
		'deparment' => 'IT',
		],
		[
			'name' => 'Khannedy',
			'deparment' => 'IT',
		],
	]),
	"HR" => collect([
		[
			'name' => 'Budi',
			'deparment' => 'HR',
		]
	])
], $result->all());

assertEquals([
	'IT' => collect([
		[
		'name' => 'Eko',
		'deparment' => 'IT',
		],
		[
			'name' => 'Khannedy',
			'deparment' => 'IT',
		],
	]),
	"HR" => collect([
		[
			'name' => 'Budi',
			'deparment' => 'HR',
		]
	])
], $collection->groupBy(function ($value, $key) {
	return $value['deparment'];
})->all());
```

## #13 Slicing

- Slicing adalah operasi untuk mengambil sebagian data di Collection

### Slicing Operations

| Method                 | Keterangan                                           |
| ---------------------- | ---------------------------------------------------- |
| `slice(start)`         | Mengambil data mulai dari start sampai data terakhir |
| `slice(start, length)` | Mengambil data mulai dari start sepanjang length     |

### Kode: Slicing

```php
public function testSlice()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->slice(3);
	assertEqualsCanonicalizing([4, 5, 6, 7, 8, 9], $result->all());

	$result = $collection->slice(3, 2);
	assertEqualsCanonicalizing([4, 5], $result->all());
}
```

## #14 Take & Skip

- Untuk mengambil sebagian element di collection, selain menggunakan slice, kita juga bisa menggunakan operator take dan skip

### Take Operations

| Method                | Keterangan                                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| `take(length)`        | Mengambil data dari awal sepanjang length, jika length negative artinya proses mengambil dari posisi belakang |
| `takeUntil(function)` | Iterasi tiap data, ambil tiap data sampai function mengembalikan nilai true                                   |
| `takeWhile(function)` | Iterasi tiap data, ambil tiap data sampai function mengembalikan nilai false                                  |

### Kode: Take

```php
public function testTake()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->take(3);
	assertEqualsCanonicalizing([1, 2, 3], $result->all());

	$result = $collection->takeUntil(function ($value, $kay) {
		return $value == 3;
	});
	assertEqualsCanonicalizing([1, 2], $result->all());

	$result = $collection->takeWhile(function ($value, $key) {
		return $value < 3;
	});
	assertEqualsCanonicalizing([1, 2], $result->all());
}
```

### Skip Operations

| Method                | Keterangan                                                                            |
| --------------------- | ------------------------------------------------------------------------------------- |
| `skip(length)`        | Ambil seluruh data kecuali sejumlah length data diawal                                |
| `skipUntil(function)` | Iterasi tiap data, jangan ambil tiap data sampai function mengembalikan nilai `true`  |
| `skipWhile(function)` | Iterasi tiap data, jangan ambil tiap data sampai function mengembalikan nilai `false` |

### Kode: Skip

```php
public function testSkip()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->skip(3);
	assertEqualsCanonicalizing([4, 5, 6, 7, 8, 9], $result->all());

	$result = $collection->skipUntil(function ($value, $kay) {
		return $value == 3;
	});
	assertEqualsCanonicalizing([3, 4, 5, 6, 7, 8, 9], $result->all());

	$result = $collection->skipWhile(function ($value, $key) {
		return $value < 3;
	});
	assertEqualsCanonicalizing([3, 4, 5, 6, 7, 8, 9], $result->all());
}
```

## #15 Chunked

- Chunked adalah operasi untuk memotong collection menjadi beberapa collection

### Chunked Operations

| Method          | Keterangan                                                                                       |
| --------------- | ------------------------------------------------------------------------------------------------ |
| `chunk(number)` | Potong collection menjadi lebih kecil dimana tiap collection memiliki sejumlah total data number |

### Kode: Chunked

```php
public function testChunked()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->chunk(3);

	assertEqualsCanonicalizing([1, 2, 3], $result->all()[0]->all());
	assertEqualsCanonicalizing([4, 5, 6], $result->all()[1]->all());
	assertEqualsCanonicalizing([7, 8, 9], $result->all()[2]->all());
}
```

## #16 Retrieve

- Retrieve adalah operasi untuk mengambil data di Collection

### First Operations

| Method                   | Keterangan                                                                                          |
| ------------------------ | --------------------------------------------------------------------------------------------------- |
| `first()`                | Mengambil data pertama di collection, atau null jika tidak ada                                      |
| `firstOrFail()`          | Mengambil data pertama di collection, atau error `ItemNotFoundException` jika tidak ada             |
| `first(function)`        | Mengambil data pertama di collection yang sesuai dengan kodisi function jika menghasilkan data true |
| `firstWhere(key, value)` | Mengambil data pertama di collection dimana key sama dengan value                                   |

### Kode: First

```php
public function testFirst()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->first();
	assertEquals(1, $result);

	$result = $collection->first(function ($value, $key) {
		return $Value > 5;
	});
	assertEquals(6, $result);
}
```

### Last Operations

| Method           | Keterangan                                                                                            |
| ---------------- | ----------------------------------------------------------------------------------------------------- |
| `last()`         | Mengambil data terakhir di collection, atau null jika tidak ada                                       |
| `last(function)` | Mengambil data terakhir di collection yang sesuai dengan kondisi function jika menghasilkan data true |

### Kode: Last

```php
public function testLast()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->last();
	assertEquals(9, $result);

	$result = $collection->last(function ($value, $key) {
		return $value > 5;
	});
	assertEquals(4, $result);
}
```

## #17 Random

- Random adalah operasi untuk mengambil data di collection dengan posisi random

### Random Operations

| Method          | Keterangan                                                    |
| --------------- | ------------------------------------------------------------- |
| `random()`      | Mengambil satu data collection dengan posisi random           |
| `random(total)` | Mengambil sejumlah total data collection dengan posisi random |

### Kode: Random

```php
public function testRandom()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->random();
	self::assertTrue(in_array($request, [1, 2, 3, 4, 5, 6, 7, 8, 9]));
}
```

## #18 Checking Existence

- Checking Existence merupakan operasi untuk mengecek apakah terdapat data yang dicari di Collection

### Checking Existence Operations

| Method               | Keterangan                                                                                 |
| -------------------- | ------------------------------------------------------------------------------------------ |
| `isEmpty(): bool`    | Mengecek apakah collection kosong                                                          |
| `isNotEmpty()`       | Mengecek apakah collection tidak kosong                                                    |
| `contains(value)`    | Mengecek apakah collection memiliki value                                                  |
| `contains(function)` | Mengecek apakah collection memiliki value dengan kondisi function yang menghasilkan `true` |
| `containsOneItem()`  | Mengecek apakah collection hanya memiliki satu data                                        |

### Kode: Checking Existence

```php
public function testCheckingExistence()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	self::assertTrue($collection->isNotEmpty());
	self::assertFalse($collection->isEmpty());
	self::assertTrue($collection->contains(8));
	self::assertFalse($collection->contains(10));
	self::assertTrue($collection->contains(function ($value, $key) {
		return $value == 8;
	}));
}
```

## #19 Ordering

- Ordering adalah operasi untuk melakukan pengurutan data di Collection

### Ordering Operations

| Method                 | Keterangan                                                  |
| ---------------------- | ----------------------------------------------------------- |
| `sort()`               | Mengurutkan secara ascending                                |
| `sortBy(function)`     | Mengurutkan secara ascending berdasarkan key atau function  |
| `sortDesc()`           | Mengurutkan secara descending                               |
| `sortByDesc(function)` | Mengurutkan secara descending berdasarkan key atau function |
| `sortKeys()`           | Mengurutkan secara ascending berdasarkan keys               |
| `sortKeysDesc()`       | Mengurutkan secara descending berdasarkan keys              |
| `reverse()`            | Membalikan urutan collection                                |

### Kode: Ordering

```php
public function testOrdering()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->sort();
	assertEqualsCanonicalizing([1, 2, 3, 4, 5, 6, 7, 8, 9], $result->all());

	$result = $collection->sortDesc();
	assertEqualsCanonicalizing([9, 8, 7, 6, 5, 4, 3, 2, 1], $result->all());
}
```

## #20 Aggregate

- Laravel collection juga memiliki beberapa method untuk melakukan aggregate

### Aggregate Operations

| Method              | Keterangan                    |
| ------------------- | ----------------------------- |
| `min()`             | Mengambil data paling kecil   |
| `max()`             | Mengambil data paling besar   |
| `avg() / average()` | Mengambil rata-rata data      |
| `sum()`             | Mengambil seluruh jumlah data |
| `count()`           | Mengambil total seluruh data  |

### Kode: Aggregate

```php
public function testAggregate()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->sum();
	assertEquals(45, $result);

	$result = $collection->avg();
	assertEquals(5, $result);

	$result = $collection->min();
	assertEquals(1, $result);

	$result = $collection->max();
	assertEquals(9, $result);
}
```

## #21 Reduce

- Jika kita ingin membuat aggregate secara manual, kita bisa menggunakan function reduce
- Reduce merupakan operasi yang dilakukan pada tiap data yang ada di collection secara sequential dan mengembalikan hasil
- Hasil dari reduce sebelumnya akan digunakan di iterasi selanjutnya

### Reduce Operations

| Method                          | Keterangan                                                                                |
| ------------------------------- | ----------------------------------------------------------------------------------------- |
| `reduce(function(carry, item))` | Pada iterasi pertama, carray akan bernilai data pertama, dan item adalah data selanjutnya |
|                                 | Pada iterasi selanjutnya, carray adalah hasil dari iterasi sebelumnya                     |

### Kode: Reduce

```php
public function testReduce()
{
	$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
	$result = $collection->reduce(function ($carry, $item) {
		return $carry + $item;
	});
	assertEquals(45, $result);
}
```

## #22 Method Lainnya

- Sekarang kita sudah membahas hampir sebagian besar operasi yang bisa kita lakukan di Laravel Collection
- Namun sebenarnya masih banyak method yang bisa kita gunakan di Laravel Collection
- <https://laravel.com/api/10.x/Illuminate/Support/Collection.html>
- <https://laravel.com/docs/10.x/collections#available-methods>

## #23 Lazy Collection

- Saat belajar PHP, kita pernah membuat Generator (Lazy Array/Iterable)
- Di Laravel juga kita bisa membuat hal seperti itu, bernama Lazy Collection
- Keuntungan menggunakan Lazy Collection adalah kita bisa melakukan manipulasi data besar, tanpa harus takut semua operasi dieksekusi sebelum dibutuhkan
- Saat membuat Lazy Collection, kita perlu menggunakan PHP Generator
- <https://laravel.com/api/10.x/Illuminate/Support/LazyCollection.html>

### Kode: Lazy Collection

```php
public function testLazyCollection()
{
	$collection = LazyCollection::make(function () {
		$value = 0;
		while (true) {
			yield $value;
			$value++;
		}
	});

	$result = $collection->take(10);
	assertEqualsCanonicalizing([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], $result->all());
}
```

## #24 Materi Selanjutnya

- Laravel Database
- Laravel Eloquent ORM
