# Laravel Eloquent API Resource

## Sebelum Belajar

- Kelas PHP dari Programmer Zaman Now
- Kelas MySQL dari Programmer Zaman Now
- Laravel Eloquent

## #1 Pengenalan Laravel Eloquent API Resource

- Di kelas Laravel Eloquent, kita sudah belajar bagaimana cara melakukan proses Serialization untuk mengubah object Model menjadi data Array / JSON
- Namun pada kasus tertentu, kita sering membuat jenis format Array / JSON yang berbeda-beda menggunakan Model yang sama
- Atau misal menggunakan attribute yang berbeda antara Array / JSON, dan attribute yang terdapat di Model
- Eloquent memiliki fitur bernama API Resource, yang bisa digunakan untuk melakukan transformasi dari data Model menjadi Array

## #2 Membuat Project

```sh
composer create-project laravel/laravel=v10.2.5 belajar-laravel-eloquent-api-resource
```

## #3 Membuat Database

- Buatlah database di MySQL dengan nama :
- `belajar_laravel_eloquent_api_resource`
- Atur database tersebut di project yang sudah dibuat

## #4 Membuat Model

- Buatlah model Category dan Product, dimana Category memiliki relasi one to many ke Product

### Kode: Membuat Model

```sh
php artisan make:model Category --migration --seed
```

### Kode: Category Migration

```php
Schema::create('categories', function(Blueprint $table) {
	$table->id();
	$table->string('name', 100)->nullable(false);
	$table->text('description')->nullable();
	$table->timestamps();
});
```

### Kode: Category Model

```php
class Category extends Model
{
	protected $table = "categories";
	protected $primaryKey = "id";
	protected $keyType = "int";
	public $incrementing = true;
	public $timestamps = true;
}
```

### Kode: Membuat Product

```sh
php artisan make:model Product --migration --seed
```

### Kode: Product Migration

```php
Schema::create("products", function(Blueprint $table) {
	$table->id();
	$table->string('name', 100)->nullable(false);
	$table->bigInteger('price')->nullable(false)->default(0);
	$table->integer('stock')->nullable(false)->default(0);
	$table->unsignedBigInteger('category_id')->nullable(false);
	$table->timestamps();
	$table->forign('category_id')->on('categories')->references('id');
});
```

### Kode: Product Model

```php
class Product extends Model
{
	protected $table = 'products';
	protected $primaryKey = 'id';
	protected $keyType = 'int';
	public $incrementing = true;
	public $timestamps = true;

	public function category(): BelongsTo
	{
		return $this->belongsTo(Category::class, 'category_id', 'id');
	}
}
```

### Kode: Category Relation

```php
class Category extends Model
{
	protected $table = "categories";
	protected $primaryKey = "id";
	protected $keyType = "int";
	public $incrementing = true;
	public $timestamps = true;

	public function products(): HasMany
	{
		return $this->hasMany(Product::class, 'category_id', 'id');
	}
}
```

## #5 Resource

- Resource merupakan representasi dari cara melakukan transformasi dari Model menjadi Array / JSON yang kita inginkan
- Untuk membuat Resource, kita bisa menggunakan perintah :
- `php artisan make:resource NamaResource`
- Class Resource adalah class turunan dari class JsonResource, dan kita perlu mengubah implementasi dari method toArray nya

### Kode: Category Resource

```php
class CategoryResource extends JsonResource
{
	public function toArray(Request $request): array
	{
		return parent::toArray($request);
	}
}
```

### Cara Kerja Resource

- Resource adalah representasi dari single object data yang ingin kita transform menjadi Array / JSON
- Semua data attribute di model, bisa kita akses menggunakan $this, hal ini karena Resource akan melakukan proxy call ke model yang sedang digunakan
- Setelah resource dibuat, kita bisa kembalikan di Controller atau di Route, dan Laravel secara otomatis mengerti bahwa response ini berupa Resource

### Kode: Category Resource

```php
class CategoryResource extends JsonResource
{
	public function toArray(Request $request): array
	{
		return [
			'id' => $this->id,
			'name' => $this->name,
			'created_at' => $this->created_at,
			'updated_at' => $this->updated_at,
		];
	}
}
```

### Kode: Route

```php
Route::get("/categories/{id}", function ($id) {
	$category = Category::findOrFail($id);
	return new CategoryResource($category);
});
```

### Kode: Category Seed

```php
class CategorySeeder extends Seeder
{
	public function run(): void
	{
		Category::create([
			'name' => "Food",
		]);
		Category::create([
			'name' => "Gadget",
		]);
	}
}
```

### Kode: Resource Test

```php
public function testResource()
{
	$this->seed([CategorySeeder::class]);
	$category = Category::first();

	$this->get("/api/categories/$category->id")
		->assertStatus(200)
		->assertJson([
			'data' => [
				'id' => $category->id,
				'name' => $category->name,
				'created_at' => $category->created_at->toJSON(),
				'updated_at' => $category->updated_at->toJSON(),
			]
		]);
}
```

### Wrap Attribute

- Secara default, data JSON yang kita kembalikan dalam method `toArray()` akan di wrap dalam attribute bernama `"data"`
- Jika kita ingin ubah nama attribute di JSON nya, kita bisa ubah menggunakan attribute `$wrap` di Resource nya

## #6 Resource Collection

- Secara default, Resource yang sudah kita buat, bisa kita gunakan untuk menampilkan data multiple object atau dalam bentuk JSON Array
- Kita bisa menggunakan static method `collection()` ketika membuat Resource nya, dan gunakan parameter berisi data collection

### Kode: Resource Collection

```php
Route::get("/categories", function () {
	$categories = Category::all();
	return CategoryResource::collection($categories);
});
```

### Kode: Test Resource Collection

```php
public function testResourceCollection()
{
	$this->seed([CategorySeeder::class]);
	$categories = Category::all();

	$this->get("/api/categories")
		->assertStatus(200)
		->assertJson([
			'data' => [
				[
					'id' => $categories[0]->id,
					'name' => $categories[0]->name,
					'created_at' => $categories[0]->created_at->toJSON(),
					'updated_at' => $categories[0]->updated_at->toJSON(),
				],
				[
					'id' => $categories[1]->id,
					'name' => $categories[1]->name,
					'created_at' => $categories[1]->created_at->toJSON(),
					'updated_at' => $categories[1]->updated_at->toJSON(),
				]
			]
		]);
}
```

## #7 Custom Resource Collection

- Kadang, kita ingin membuat class Resource Collection secara manual, tanpa menggunakan Resource Class yang sudah kita buat
- Pada kasus ini, kita bisa membuat Resource, namun menggunakan tambahan parameter `--collectio`n :
- `php artisan make:resource NamaCollection --collection`
- Secara otomatis class Resource adalah turunan dari class `ResourceCollection`
- Untuk mengambil informasi collection nya, kita bisa menggunakan attribute

### Kode: Resource Collection

```php
class CategoryCollection extends ResourceCollection
{
	public function toArray(Request $request): array
	{
		return [
			'data' => $this->collection,
			'total' => count($this->collection),
		];
	}
}
```

### Kode: Route

```php
Route::get("/categories-custom", function () {
	$categories = Category::all();
	return new \App\Http\Resources\CategoryCollection($categories);
});
```

### Kode: Test Custom Resource Collection

```php
public function testResourceCollectionCustom()
{
	$this->seed([CategorySeeder::class]);
	$categories = Category::all();

	$this->get("/api/categories-custom")
		->assertStatus(200)
		->assertJson([
			'total' => 2,
			'data' => [
				[
					'id' => $categories[0]->id,
					'name' => $categories[0]->name,
					'created_at' => $categories[0]->created_at->toJSON(),
					'updated_at' => $categories[0]->updated_at->toJSON(),
				],
				[
					'id' => $categories[1]->id,
					'name' => $categories[1]->name,
					'created_at' => $categories[1]->created_at->toJSON(),
					'updated_at' => $categories[1]->updated_at->toJSON(),
				]
			]
		]);
}
```

## #8 Nested Resource

- Saat kita menggunakan Resource, contoh pada Resource Collection, kita juga bisa menggunakan Resource lainnya
- Secara default, method `toArray()` akan dikonversi menjadi JSON
- Namun, kita bisa menggunakan Resource lain jika kita mau

### Kode : Category Simple Resource

```php
class CategorySimpleResource extends JsonResource
{
	public function toArray(Request $request): array
	{
		return [
			'id' => $this->id,
			'name' => $this->name,
		];
	}
}
```

### Kode: Category Collection

```php
class CategoryCollection extends ResourceCollection
{
	public function toArray(Request $request): array
	{
		return [
			'data' => CategorySimpleResource::collection($this->collection),
			'total' => count($this->collection),
		];
	}
}
```

### Kode: Test Nested Resource

```php
public function testNestedResource()
{
	$this->seed([CategorySeeder::class]);
	$categories = Category::all();

	$this->get("/api/categories-custom")
		->assertStatus(200)
		->assertJson([
			'total' => 2,
			'data' => [
				[
					'id' => $categories[0]->id,
					'name' => $categories[0]->name,
				],
				[
					'id' => $categories[1]->id,
					'name' => $categories[1]->name,
				]
			]
		]);
}
```

## #9 Data Wrap

- Secara default, data JSON yang dibuat oleh Resource akan disimpan dalam attribute `"data"`
- Jika kita ingin menggantinya, kita bisa ubah attribute $wrap di Resource dengan nama attribute yang kita mau
- Secara default, jika dalam `toArray()` kita mengembalikan array yang terdapat attribute sama dengan `$wrap`, maka data JSON tidak akan di wrap

### Kode : Product Resource

```php
class ProductResource extends JsonResource
{
	public static $wrap = "value";

	public function toArray(Request $request): array
	{
		return [
			'id' => $this->id,
			'name' => $this->name,
			'category' => new CategorySimpleResource($this->category),
			'price' => $this->price,
			'created_at' => $this->created_at,
			'updated_at' => $this->updated_at,
		];
	}
}
```

### Kode: Product Seed

```php
public function run(): void
{
	Category::all()->each(function (Category $category) {
		for ($i = 0; $i < 5; i++) {
			$category->products()->create([
				'name' => "Product $i of $category->name",
				'price' => rand(100, 1000),
			]);
		}
	});
}
```

### Kode: Route

```php
Route::get("/products/{id}", function ($id) {
	$product = \App\Models\Product::find($id);
	return new \App\Http\Resources\ProductResource($product);
});
```

### Kode: Test Product Test

```php
public function testProductResource()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$product = Product::all();

	$this->get("/api/products/$product->id")
		->assertStatus(200)
		->assertJson([
			'value' => [
				'name' => $product->name,
				'price' => $product->price,
				'category' => [
					'id' => $product->category->id,
					'name' => $product->category->name,
				],
				'created_at' => $product->created_at->toJSON(),
				'updated_at' => $product->updated_at->toJSON(),
			]
		]);
}
```

## #10 Data Wrap Collection

- Khusus untuk mengubah attribute $wrap untuk Collection, kita tidak bisa menggunakan `NamaResource::collection()`, hal ini karena kode tersebut sebenarnya akan membuat object `AnonymousResourceCollection`, bukan menggunakan Resource yang kita buat
- <https://laravel.com/api/10.x/Illuminate/Http/Resources/Json/AnonymousResourceCollection.html>
- Jika hasil result JSON di `ResourceCollection.toArray()` mengandung attribute yang terdapat di `$wrap`, maka Laravel tidak akan melakukan wrap, namun jika tidak ada, maka akan melakukan wrap

### Kode : Product Collection

```php
class ProductCollection extends ResourceCollection
{
	public static $wrap = "data";

	public function toArray(Request $request): array
	{
		return [
			'data' => ProductResource::collection($this->collection),
		];
	}
}
```

### Kode: Route

```php
Route::get("/products", function () {
	$products = \App\Models\Product::all();
	return new \App\Http\Resources\ProductCollection($products);
});
```

### Kode: Product Test

```php
public function testProductCollection()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$response = $this->get('/api/products')
		->assertStatus(200);

	$names = $response->json("data.*.name");
	for ($i = 0; $i < 5; $i++) {
		self::assertContains("Product $i of Food", $names);
	}
	for ($i = 0; $i < 5; $i++) {
		self::assertContains("Product $i of Gadget", $names);
	}
}
```

## #11 Pagination

- Jika kita mengirim data Pagination ke dalam Resource Collection, secara otomatis Laravel akan menambahkan informasi link dan juga meta (paging) secara otomatis
- Attribute `links` berisi informasi link menuju page sebelum dan setelahnya
- Attribute `meta` berisi informasi paging

### Kode: Route

```php
Route::get("/products-paging", function (Request $request) {
	$page = $request->get('page', 1);
	$products = \App\Models\Product::paginate(perPage: 2, page: $page);
	return new \App\Http\Resources\ProductCollection($products);
});
```

### Kode: Paginate Test

```php
public function testProductPaging()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$response = $this->get('/api/products-paging')
		->assertStatus(200);

	self::assertNotNull($response->json('links'));
	self::assertNotNull($response->json('meta'));
	self::assertNotNull($response->json('data'));
}
```

## #12 Additional Metadata

- Kadang, kita ingin menambahkan attribute tambahan selain `"data"`
- Untuk attribute tambahan yang statis, kita bisa tambahkan di Resource dengan meng-override properties $additional

### Kode: Product Debug Resource

```php
class ProductDebugResource extends JsonResource
{
	public $additional = [
		"author" => "Programmer Zaman Now"
	];

	public function toArray(Request $request): array
	{
		return [
			'id' => $this->id,
			'name' => $this->name,
			'price' => $this->price,
		];
	}
}
```

### Kode: Route

```php
Route::get("/products-debug/{id}", function ($id) {
	$product = \App\Models\Product::find($id);
	return new \App\Http\Resources\ProductDebugResource($product);
});
```

### Kode: Product Test

```php
public function testAdditionalMetadata()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$product = Product::first();

	$this->get("/api/products-debug/$product->id")
		->assertStatus(200)
		->assertJson([
			'author' => 'Programmer Zaman Now',
			'data' => [
				'id' => $product->id,
				'name' => $product->name,
				'price' => $product->price,
			]
		]);
}
```

### Additional Parameter Dinamis

- Jika kita butuh tambahan additional parameter yang dinamis, kita bisa langsung saja buat di dalam `toArray()`
- Yang penting adalah ada attribute yang sama dengan `$wrap`

### Kode : Product Debug Resource

```php
class ProductDebugResource extends JsonResource
{
	public static $wrap = 'data';

	public function toArray(Request $request): array
	{
		return [
			'author' => 'Programmer Zaman Now',
			'server_time' => now()->toDateTimeString(),
			'data' => [
				'id' => $this->id,
				'name' => $this->name,
				'price' => $this->price,
			]
		];
	}
}
```

### Kode: Product Test

```php
public function testAdditionalMetadataDinamis()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$product = Product::first();

	$this->get("/api/products-debug/$product->id")
		->assertStatus(200)
		->assertJson([
			'author' => 'Programmer Zaman Now',
			'data' => [
				'id' => $product->id,
				'name' => $product->name,
				'price' => $product->price,
			]
		]);

	self::assertNotNull($response->json('server_time'));
}
```

## #13 Conditional Attributes

- Pada beberapa kasus, ketika kita mengakses relation pada model di Resource, secara otomatis Laravel akan melakukan query ke database
- Kadang hal ini berbahaya kalo ternyata relasinya sangat banyak, sehingga ketika mengubah menjadi JSON, akan sangat lambat
- Kita bisa melakukan pengecekan conditional attribute, bisa kita gunakan untuk pengecekan boolean ataupun relasi

### Conditional Method

- Untuk melakukan pengecekan kondisi, kita bisa gunakan method berikut di Resource
- `when(boolean, value, default)`
- `whenHas(attribute, default)`
- `whenNotNull(attribute)`
- `mergeWhen(boolean, array)`
- `whenLoaded(relation)`

### Kode: Product Resource

```php
class ProductResource extends JsonResource
{
	public static $wrap = "value";

	public function toArray(Request $request): array
	{
		return [
			'id' => $this->id,
			'name' => $this->name,
			'category' => new CategorySimpleResource($this->whenLoaded('category')),
			'price' => $this->price,
			'is_expensive' => $this->when($this->price > 1000, true, false),
			'created_at' => $this->created_at,
			'updated_at' => $this->updated_at,
		];
	}
}
```

### Kode: Route Product

```php
Route::get("/products/{id}", function ($id) {
	$products = \App\Models\Product::find($id);
	$category->load('category');
	return new \App\Http\Resources\ProductDebugResource($products);
});
```

### Kode: Product Test

```php
public function testProduct()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$product = Product::first();

	$this->get("/api/products/$product->id")
		->assertStatus(200)
		->assertJson([
			"value" => [
				"name" => $product->name,
				"category" => [
					"id" => $product->category->id,
					"name" => $product->category->name,
				],
				"price" => $product->price,
				"is_expensive" => $product->price > 1000,
				"created_at" => $product->created_at->toJSON(),
				"updated_at" => $product->updated_at->toJSON(),
			]
		]);
}
```

## #14 Resource Response

- Di method `toArray()` terdapat parameter Request, yang artinya kita bisa mengambil informasi pada HTTP Request jika dibutuhkan
- Resource juga memiliki method `withResponse()` yang bisa kita override untuk mengubah Http Response

### Kode : Product Collection Response

```php
class ProductCollection extends ResourceCollection
{
	public static $wrap = "data";

	public function toArray(Request $request): array
	{
		return [
			'data' => ProductResource::collection($this->collection),
		];
	}

	public function withResponse(Request $request, JsonResource $response): void
	{
		$response->header("X-Powered-By", "Programmer Zaman Now");
	}
}
```

### Kode: Product Collection Test

```php
public function testProducts()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$response = $this->get('/api/products')
		->assertStatus(200)
		->assertHeader("X-Powered-By", "Programmer Zaman Now");

	$names = $response->json("data.*.name");
	for ($i = 0; $i < 5; $i++) {
		self::assertContains("Product $i of Food", $names);
	}
	for ($i = 0; $i < 5; $i++) {
		self::assertContains("Product $i of Gadget", $names);
	}
}
```

### Response Method

- Atau saat membuat Resource object, terdapat method response() yang bisa kita gunakan juga untuk memanipulasi data response

### Kode: Route Product

```php
Route::get("/products/{id}", function ($id) {
	$products = \App\Models\Product::find($id);
	$category->load('category');
	return (new \App\Http\Resources\ProductDebugResource($products))
		->response()
		->header("X-Powered-By", "Programmer Zaman Now");;
});
```

### Kode: Product Test

```php
public function testProduct()
{
	$this->seed([CategorySeeder::class, ProductSeeder::class]);
	$product = Product::first();

	$this->get("/api/products/$product->id")
		->assertStatus(200)
		->assertHeader("X-Powered-By", "Programmer Zaman Now");
		->assertJson([
			"value" => [
				"name" => $product->name,
				"category" => [
					"id" => $product->category->id,
					"name" => $product->category->name,
				],
				"price" => $product->price,
				"is_expensive" => $product->price > 1000,
				"created_at" => $product->created_at->toJSON(),
				"updated_at" => $product->updated_at->toJSON(),
			]
		]);
}
```

## #15 Materi Selanjutnya

- Laravel RESTful API
