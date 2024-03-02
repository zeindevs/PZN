# Spring Data JPA

## Sebelum Belajar

- Kelas Java dari Programmer Zaman Now
- Kelas MySQL dari Programmer Zaman Now
- Spring Web MVC

## #1 Pengenalan Spring Data JPA

- Spring Data JPA adalah fitur di Spring yang digunakan untuk mempermudah kita membuat aplikasi menggunakan JPA
- Dengan menggunakan Spring Data JPA, kita tidak perlu membuat banyak hal secara manual dilakukan seperti jika menggunakan JPA secara manual
- <https://spring.io/projects/spring-data-jpa>

### Spring Data

- Spring Data JPA sendiri merupakan bagian dari Spring Data Project
- Spring Data Project adalah project di Spring yang digunakan untuk mempermudah kita melakukan manipulasi data di database
- Konsep yang digunakan di Spring Data hampir sama di semua tipe project, oleh karena itu jika nanti kita sudah mengerti tentang Spring Data JPA, ketika akan menggunakan fitur Spring Data lainnya, maka tidak akan kesulitan, seperti Spring Data MongoDB, Spring Data Elasticsearch, Spring Data Redis, dan lain-lain
- <https://spring.io/projects/spring-data>

## #2 Membuat Project

- <https://start.spring.io/>
- Spring Web MVC
- Spring Data JPA
- Lombok
- MySQL
- Validation

### Membuat Database

```sql
CREATE DATABASE Belajar_spring_data_jpa;

USE Belajar_spring_data_jpa;
```

## #3 Data Source

- Salah satu keuntungan menggunakan Spring Data JPA dan Spring Boot adalah, semua upacara yang biasa kita lakukan ketika menggunakan JPA, sudah dilakukan oleh Spring Boot
- Jadi kita tidak perlu membuat DataSource secara manual, karena sudah otomatis dibuat oleh Spring Boot
- <https://github.com/spring-projects/spring-boot/blob/main/spring-boot-project/spring-boot-autoconfigure/src/main/java/org/springframework/boot/autoconfigure/jdbc/DataSourceAutoConfiguration.java>
- Untuk mengubah konfigurasi DataSource, kita cukup menggunakan application properties saja
- Kita bisa lihat semua konfigurasinya dengan prefix `spring.datasource.*`
- <https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.data>

### Kode: Konfigurasi DataSource

```
spring.datasource.driver-class-name=com.mysql.cj-jdbc.Driver
spring.datasource.username=root
spring.datasource.password=
spring.datasource.url=jdbc:mysql://localhost:3306/belajar_spring_data_jpt
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.maximum-pool-size=10
```

## #4 JPA Configuration

- Untuk melakukan konfigurasi JPA, kita juga tidak perlu melakukannya secara manual lagi di file `persistence.xml`
- Secara otomatis JPA akan menggunakan DataSource di Spring, dan jika kita butuh mengubah konfigurasi, kita bisa menggunakan properties dengan prefix `spring.jpa.*`
- <https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html>

### Kode: Konfigurasi JPA

```
spring.datasource.driver-class-name=com.mysql.cj-jdbc.Driver
spring.datasource.username=root
spring.datasource.password=
spring.datasource.url=jdbc:mysql://localhost:3306/belajar_spring_data_jpt
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.maximum-pool-size=10

spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.shoformat_sql=true
```

## #5 Entity Manager Factory

- Selain DataSource, Spring Boot juga secara otomatis membuatkan bean EntityMangerFactory, sehingga kita tidak perlu membuatnya secara manual
- Itu semua secara otomatis dibuat oleh Spring Boot
- <https://github.com/spring-projects/spring-boot/blob/main/spring-boot-project/spring-boot-autoconfigure/src/main/java/org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaAutoConfiguration.java>

### Kode: Entity Manager Factory

```java
@SpringBootTest
public class EntityManagerTest {

	@Autowired
	private EntityManagerFactory entityManagerFactory;

	@Test
	void testEntityManagerFactory() {
		assertNotNull(entityManagerFactory);

		EntityManager entityManager = entityManagerFactory.createEntityManager();
		assertNotNull(entityManager);

		EntityManager.close();
	}
}
```

## #6 Repository

- Saat kita menggunakan Spring Data JPA, kita akan jarang sekali membuat Entity Manager lagi
- Bahkan mungkin jarang menggunakan Entity Manager Factory
- Spring Data membawa konsep Repository (diambil dari buku Domain Driven Design)
- Dimana Repository merupakan layer yang digunakan untuk mengelola data (contohnya di database)

### Repository

- Setiap Entity yang kita buat di JPA, maka kita biasanya akan buatkan Repository nya
- Repository berbentuk Interface, yang secara otomatis diimplementasikan oleh Spring menggunakan AOP
- Untuk membuat Repository, kita cukup membuat interface turunan dari `JPARepository<T, ID>`
- <https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html>
- Dan kita juga bisa tambahkan annotation `@Repository` (walaupun tidak wajib)
- <https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Repository.html>

### Kode: Create Table Category

```sql
CREATE TABLE categories
(
	id BIGINT NOT NULL AUTO_INCREMENT,
	name VARCHAR(100) NOT NULL,
	PRIMARY KEY (id)
) ENGINE InnoDB;
```

### Kode: Category Entity

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name = "categories")
public class Category {

	@Id
	@GeneretedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	private String name;
}
```

### Kode: Category Repository

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import programmerzamannow.jpa.entity.Category;

@Repository
public interface CategoryRepository extends JpaRepository<Category, Long> {

}
```

## #7 Crud Repository

### Category Repository

- JpaRepository adalah turunan dari interface `CrudRepository` dan `ListCrudRepository`, dimana di interface tersebut banyak method yang bisa digunakan untuk melakukan operasi CRUD
- Kita tidak perlu lagi menggunakan Entity Manager untuk melakukan operasi CRUD, cukup gunakan JpaRepository
- Ada yang perlu diperhatikan di JpaRepository, method untuk CREATE dan UPDATE digabung dalam satu method `save()`, yang artinya method `save()` adalah `CREATE` or `UPDATE`
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html>
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/ListCrudRepository.html>

### Kode: Test Create Category Repository

```java
@SpringBootTest
class CategoryRepositoryTest {

	@Autowired
	private CategoryRepository categoryRepository;

	@Test
	void testCreate() {
		Category category = new Category();
		category.setName("GADGET");

		categoryRepository.save(category);

		assertNotNull(category.getId());
	}
}
```

### Kode: Test Update Category Repository

```java
@Test
void tetsUpdate(){
	Category category = categoryRepository.findById(1L).orElse(null);
	assertNotNull(category);

	category.setName("GADGET MURAH");
	categoryRepository.save(category);

	category - categoryRepository.findById(1L).orElse(null);
	assertNotNull(category);
	assertEquals("GADGET MURAH", category.getName());
}
```

## #8 Declarative Transaction

- Saat kita menggunakan JPA secara manual, kita harus melakukan management transaction secara manual menggunakan `EntityManager`
- Spring menyediakan fitur Declarative Transaction, yaitu management transaction secara declarative, yaitu dengan menggunakan annotation `@Transactional`
- Annotation ini secara otomatis dibaca oleh Spring AOP, dan akan menjalankan transaction secara otomatis ketika memanggil method yang terdapat annotation `@Transactional` nya
- <https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html>

### Yang Perlu Diperhatikan

- Saat membuat method dengan annotation `@Transactional`, karena dia dibungkus oleh Spring AOP, jadi untuk menjalankannya, kita harus memanggil method tersebut dari luar object
- Misal kita memiliki `CategoryService.create()` dengan annotation `@Transactional`, jika kita panggil dari `CategoryController`, maka Spring AOP akan berjalan, namun jika dipanggil di `CategoryService.test()` misalnya, maka Spring AOP tidak akan berjalan

### Kode: Category Service

```java
@Service
public class CategoryService {

	@Autowired
	private CategoryRepository categoryRepository;

	@Transactional
	public void create() {
		for (int i = 0 i < 5; i++) {
			Category category = new Category();
			category.setName("Category " + i);
			categoryRepository.save(category);
		}
		throw new RuntimeException("Ups rollback please");
	}

	public void test() {
		create();
	}
}
```

### Kode: Test Category Service

```java
@Test
void success() {
	assertThrows(RuntimeException.class, () -> {
		categoryService.create();
	});
}

@Test
void failed() {
	assertThrows(RuntimeException.class, () -> {
		categoryService.test();
	});
}
```

### Transaction Propagation

- Saat kita membuat method dengan annotation `@Transactional`, kita mungkin didalamnya memanggil method `@Transactional` lainnya
- Pada kasus seperti itu, ada baiknya kita mengerti tentang attribute propagation pada `@Transactional`
- Kita bisa memilih nilai apa yang ingin kita gunakan
- <https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Propagation.html>

## #9 Programmatic Transaction

- Fitur Declarative Transaction sangat mudah untuk digunakan, karena hanya butuh menggunakan annotation
- Namun pada beberapa kasus, misal kode yang kita buat butuh jalan secara async misal nya, maka Declarative Transaction tidak akan berjalan, mau tidak mau biasanya kita akan melakukan manual transaction management lagi
- Kita bisa gunakan cara lama menggunakan Entity Manager, atau kita bisa menggunakan fitur Spring untuk melakukan management transaction secara manual
- Ada beberapa cara untuk melakukan programmatic transaction di Spring

### Transaction Operations

- Pada kasus yang sederhana, kita bisa menggunakan `TransactionOperations`
- Kita bisa menggunakan bean `TransactionOperations` yang sudah secara otomatis dibuat oleh Spring Boot
- <https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/support/TransactionOperations.html>

### Kode: Transaction Operations

```java
@Service
public class CategoryService {

	@Autowired
	private TransactionOperations transactionOperations;

	public void error() {
		throw new RuntimeException("Ups");
	}

	public void createCategories() {
		transactionOperations.executeWithoutResult(transctionStatus -> {
			for (int i = 0 i < 5; i++) {
				Category category = new Category();
				category.setName("Category " + i);
				categoryRepository.save(category);
			}
			error();
		});
	}
}
```

### Platform Transaction Manager

- Jika kita butuh melakukan management transaction secara low level, maka sebenarnya kita bisa menggunakan Entity Manager, namun hal itu tidak disarankan
- Kita bisa menggunakan Platform Transaction Manager yang sudah disediakan oleh Spring
- <https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html>
- Penggunaan ini sangat manual, sehingga kita bisa atur semuanya secara manual

### Kode: Platform Transaction Manager

```java
@Service
public class CategoryService {

	// ...
	@Autowired
	private PlatformTransactionManager transactionManager;

	public void manual() {
		DefaultTransactionDefinition definition = new DefaultTransactionDefinition();
		definition.setTimeout(10);
		definition.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);

		TransactionStatus transaction = transactionManager.getTransaction(definition);
		try {
			for (int i = 0 i < 5; i++) {
				Category category = new Category();
				category.setName("Category Manual " + i);
				categoryRepository.save(category);
			}
			error();
			transactionManager.commit(transaction);
		} catch (Throwable throwable) {
			transactionManager.rollback(transaction);
			throw throwable;
		}
	}

}
```

## #10 Query Method

- Saat kita menggunakan EntityManager, kita bisa membuat query menggunakan JPA QL, namun bagaimana jika menggunakan Repository?
- Spring Data menyediakan fitur Query Method, yaitu membuat query menggunakan nama method secara otomatis
- Spring Data akan melakukan penerjemahan secara otomatis dari nama method menjadi JPA QL

### Format Query Method

- Untuk melakukan query yang mengembalikan data lebih dari satu, kita bisa gunakan prefix `findAll…`
- Untuk melakukan query yang mengembalikan data pertama, kita bisa gunakan prefix `findFirst…`
- Selanjutnya diikuti dengan kata By dan diikuti dengan operator query nya
  Untuk operator query yang didukung, kita bisa lihat di halaman ini
- <https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation>

### Kode: Category Repository

```java
@Repository
public interface CategoryRepository extends JpaRepository<Category, Long> {

	Optinal<Category> findFirstByNameEquals(String name)

	List<Category> findAllByNameLike(String name);
}
```

### Kode: Test Category Repository

```java
@Test
void testQueryMethod() {
	Category category = categoryRepository.findFirstByNameEquals("GADGET MURAH").orElse(null);
	assertNotNull(category)
	assertEquals("GADGET MURAH", category.getName());

	List<Category> categories = categoryRepository.findAllByNameLike("%GADGET%");
	assertEquals(1, categories.size());
	assertEquals("GADGET MURAH", categories.get(0).getName());
}
```

## #11 Query Relation

- Saat kita belajar JPA, kita bisa melakukan query ke relasi Entity atau Embedded field secara otomatis menggunakan tanda `.` (titik)
- Di Spring Data Repository, kita bisa gunakan `_` (garis bawah) untuk menyebutkan bahwa itu adalah tanda `.` (titik) nya
- Misal `ProductRepository.findAllByCategory_Name(String)`

### Kode: Create table Product

```sql
CREATE TABLE products
(
	id BIGINT NOT NULL AUTO_INCREMENT,
	name VARCHAR(100) NOT NULL,
	price BIGINT NOT NULL,
	category_id BIGINT NOT NULL,
	PRIMARY KEY (id),
	FOREIGN KEY fk_products_categories (category_id) REFERENCES categories (id)
) ENGINE InnoDB;
```

### Kode: Product Entity

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name = "products")
public class Product {

	@Id
	@GeneretedValue(strategy  =GenerationType.IDENTITY)
	private Long id;

	private String name;

	private Long price;

	@ManyToOne
	@JoinColumn(name = "category_id", referencedColumnName = "id")
	private Category category;
}

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name = "categories")
public class Category {

	@Id
	@GeneretedValue(strategy  =GenerationType.IDENTITY)
	private Long id;

	private String name;

	@OneToMany
	private List<Product> products;
}
```

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	List<Product> findAllByCategory_Name(String name);
}
```

### Kode: Test Product Repository

```java
@Test
void createProduct() {
	Category category = categoryRepository.findById(1L).orElse(null);
	assertNotNull(category);

	{
		Product product = new Product();
		product.setName("Apple Iphone 14 Pro Max");
		product.setPrice(25_000_000L);
		product.setCategory(category);
		productRepository.save(product);
	}

	{
		Product product = new Product();
		product.setName("Apple Iphone 13 Pro Max");
		product.setPrice(28_000_000L);
		product.setCategory(category);
		productRepository.save(product);
	}
}

@Test
void findProducts() {
	List<Product> products = productRepository.findAllByCategory_Name("GADGET MURAH");

	assertEquals(2, products.size());
	assertEquals("Apple Iphone 14 Pro Max", products.get(0).getName());
	assertEquals("Apple Iphone 13 Pro Max", products.get(1).getName());
}
```

## #12 Sorting

- Spring Data Repository juga memiliki fitur untuk melakukan Sorting, caranya kita bisa tambahkan parameter Sort pada posisi parameter terakhir
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/Sort.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	List<Product> findAllByCategory_Name(String name);

	List<Product> findAllByCategory_Name(String name, Sort sort);
}
```

### Kode: Test Product Repository

```java
@Test
void testProductsSort() {
	Sort sort = Sort.by(Sort.Order.desc("id"));
	List<Product> products = productRepository.findAllByCategory_Name("GADGET MURAH", sort);

	assertEquals(2, products.size());
	assertEquals("Apple Iphone 13 Pro Max", products.get(0).getName());
	assertEquals("Apple Iphone 14 Pro Max", products.get(1).getName());
}
```

## #13 Paging

- Selain Sort, Spring Data Repository juga mendukung paging seperti di EntityManager
- Caranya kita bisa tambahkan parameter `Pageable` di posisi terakhir parameter
- Pageable adalah sebuah interface, biasanya kita akan menggunakan `PageRequest` sebagai class implementasinya
- Dan jika sudah menggunakan Pageable, kita tidak perlu lagi menggunakan `Sort`, karena sudah bisa dihandle oleh Pageable
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/PageRequest.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	List<Product> findAllByCategory_Name(String name);

	List<Product> findAllByCategory_Name(String name, Sort sort);

	List<Product> findAllByCategory_Name(String name, Pageable pageable);
}
```

### Kode: Test Product Repository

```java
@Test
void testFindProductsWithPageable() {
	// Page 0
	Pageable pageable = PageRequest.of(0, 1, Sort.by(Sort.Order.desc("id")));
	List<Product> products = productRepository.findAllByCategory_Name("GADGET MURAH", pageable);

	assertEquals(1, products.size());
	assertEquals("Apple Iphone 13 Pro Max", products.get(0).getName());

	// Page 1
	Pageable pageable = PageRequest.of(1, 1, Sort.by(Sort.Order.desc("id")));
	products = productRepository.findAllByCategory_Name("GADGET MURAH", pageable);

	assertEquals(1, products.size());
	assertEquals("Apple Iphone 14 Pro Max", products.get(0).getName());
}
```

## #14 Page Result

- Saat kita menggunakan Paging, kadang kita ingin tahu seperti jumlah total data hasil query, dan juga total page nya
- Hal ini biasanya kita akan lakukan dengan cara manual dengan cara menghitung count dari hasil total hasil query tanpa paging
- Untungnya, Spring Data JPA menyediakan return value berupa `Page<T>`, dimana secara otomatis akan diambil informasi total data dan total page nya
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/Page.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	List<Product> findAllByCategory_Name(String name);

	List<Product> findAllByCategory_Name(String name, Sort sort);

	List<Product> findAllByCategory_Name(String name, Pageable pageable);
}
```

### Kode: Test Product Repository

```java
@Test
void testFindProductsWithPageableResult() {
	// Page 0
	Pageable pageable = PageRequest.of(0, 1, Sort.by(Sort.Order.desc("id")));
	List<Product> products = productRepository.findAllByCategory_Name("GADGET MURAH", pageable);

	assertEquals(1, products.getContent().size());
	assertEquals(0, products.getNumber());
	assertEquals(2, products.getTotalElements());
	assertEquals(2, products.getTotalPages());
	assertEquals("Apple Iphone 13 Pro Max", products.get(0).getName());

	// Page 1
	Pageable pageable = PageRequest.of(1, 1, Sort.by(Sort.Order.desc("id")));
	products = productRepository.findAllByCategory_Name("GADGET MURAH", pageable);

	assertEquals(1, products.size());
	assertEquals(1, products.getNumber());
	assertEquals(2, products.getTotalElements());
	assertEquals(2, products.getTotalPages());
	assertEquals("Apple Iphone 14 Pro Max", products.get(0).getName());
}
```

## #15 Count Query Method

- JPA Repository juga bisa digunakan untuk membuat count query method
- Cukup gunakan prefix method `countBy…`
- Selebihnya kita bisa membuat format seperti Query Method biasanya

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	Long countByCategory_Name(String name);
	// ...
}
```

### Kode: Test Product Repository

```java
@Test
void testCount() {
	Long count = productRepository.count();
	assertEquals(2L, count);

	count = productRepository.countByCategory_Name("GADGET MURAH");
	assertEquals(2L, count);
}
```

## #16 Exists Query Method

- Selain Count, kita juga bisa membuat Exists method di Query Method
- Method ini sebenarnya sederhana, return value nya adalah boolean, untuk mengecek apakah ada data sesuai dengan Query Method atau tidak
- Untuk membuatnya kita bisa gunakan prefix `existsBy…`

### Kode: Product Category

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	Long existsByName(String name);
	// ...
}
```

### Kode: Test Product Category

```java
@Test
void testExists() {
	boolean exists = productRepository.existsByName("Apple Iphone 14 Pro Max");
	assertTrue(exists);

	// test not exists
	exists = productRepository.existsByName("Apple Iphone 14 Pro Max 2");
	assertFalse(exists);
}
```

## #17 Delete Query Method

- Kita juga bisa membuat delete Query Method dengan prefix `deleteBy`
- Untuk delete, kita bisa return `int` sebagai penanda jumlah record yang berhasil di hapus
- Untuk membuat delete query method, kita bisa gunakan prefix `deleteBy…`

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	int deleteByName(String name);
}
```

### Kode: Test Product Repository

```java
@Test
void testDelete() {
	transactionOperations.executeWithoutResult(transctionStatus -> {
		Category category = categoryRepository.findById(1L).orElse(null);
		assertNotNull(category);

		Product product = new Product();
		product.setName("Samsung Galaxy S9");
		product.setPrice(10_000_000L);
		product.setCategory(category);
		productRepository.save(product);

		int delete = productRepository.deleteByName("Samsung Galaxy S9");
		assertEquals(1, delete);

		// test not exists
		delete = productRepository.deleteByName("Samsung Galaxy S9");
		assertEquals(0, delete);
	});
}
```

## #18 Repository Transaction

- Secara default, saat kita membuat Repository interface, Spring akan membuat sebagai instance turunan dari `SimpleJpaRepository`
- Oleh karena itu, saat kita melakukan CRUD, kita tidak perlu melakukan didalam Transaction, hal ini karena sudah ditambahkan annotation di class `SimpleJpaRepository`
- Class `SimpleJpaRepository` terdapat annontatio `@Transactional(readOnly=true)`, oleh karena itu saat kita buat Query Method di Repository, maka secara default akan menjalankan transaction read only
- <https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/support/SimpleJpaRepository.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	@Transactional
	int deleteByName(String name);
	// ...
}
```

### Kode: Test Product Repository

```java
@Test
void testDelete() {
	Category category = categoryRepository.findById(1L).orElse(null);
	assertNotNull(category);

	Product product = new Product();
	product.setName("Samsung Galaxy S9");
	product.setPrice(10_000_000L);
	product.setCategory(category);
	productRepository.save(product);

	int delete = productRepository.deleteByName("Samsung Galaxy S9");
	assertEquals(1, delete);

	// test not exists
	delete = productRepository.deleteByName("Samsung Galaxy S9");
	assertEquals(0, delete);
}
```

## #19 Named Query

- Saat kita menggunakan JPA, kita sering sekali menggunakan Named Query
- Lantas bagaimana jika kita menggunakan Spring Data JPA Repository?
- Untuk menggunakan Named Query di Repository, kita cukup buat nama method sesuai degan nama Named Query, misal jika kita memiliki Named Query dengan nama `Product.searchProductUsingName`, maka kita bisa membuat method `ProductRepository.searchProductUsingName()`
- Secara otomatis itu akan menggunakan Named Query tersebut

### Kode: Product Entity

```java
@Entity
@Table(name = "products")
@NamedQueries({
	@NamedQuery(name = "Product.searchProductUsingName",
					query = "SELECT p FROM Product p WHERE p.name = :name"),
})
public class Product {

}
```

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	List<Product> searchProductUsingName(@Param("name") String name);
	// ...
}
```

### Kode: Test Product Repository

```java
@Test
void searchProduct() {
	List<Product> products = productRepository.searchProductUsingName("Apple Iphone 14 Pro Max");

	assertEquals(1, products.save());
	assertEquals("Apple Iphone 14 Pro Max", products.get(0).getName());
}
```

### Sorting dan Paging

- Named Query di Repository tidak mendukung Sort
- Namun mendukung Pageable (tanpa Sort), oleh karena itu kita harus menambahkan Sorting secara manual di Named Query nya

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	List<Product> searchProductUsingName(@Param("name") String name, Pageable pageable);
	// ...
}

@Test
void searchProduct() {
	Pageable pageable = Pageable.of(0, 1);
	List<Product> products = productRepository.searchProductUsingName("Apple Iphone 14 Product Max", pageable);

	assertEquals(1, products.save());
	assertEquals("Apple Iphone 14 Pro Max", products.get(0).getName());
}
```

## #20 Query Annotation

- Query Method cocok untuk kasus membuat jenis query yang tidak terlalu kompleks. Saat query terlalu kompleks dan parameter banyak, maka nama method bisa terlalu panjang jika menggunakan Query Method
- Untungnya Spring Data JPA menyediakan membuat query menggunakan annotation Query, dimana kita bisa buat JPA QL atau Native Query
- <https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Query.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	@Query(value = "SELECT p FROM Product p WHERE p.name LIKE :name or p.category.name LIKE :name")
	List<Product> searchProduct(@Param("name") String name);
}
```

### Kode: Test Product Repository

```java
@Test
void searchProductLike() {
	List<Product> products = productRepository.searchProduct("%Iphone");
	assertEquals(2, products.size());

	product = productRepository.searchProduct("%GADGET%");
	assertEquals(2, products.size());
}
```

### Sort dan Paging

- Query Annotation mendukung Sort dan Paging
- Jadi kita bisa menggunakan parameter Sort atau Pageable pada Query Annotation

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	@Query(value = "SELECT p FROM Product p WHERE p.name LIKE :name or p.category.name LIKE :name")
	List<Product> searchProduct(@Param("name") String name, Pageable pageable);
}
```

### Kode: Test Product Repository

```java
@Test
void testProductsSearch() {
	Pageable pageable  = PageRequest(0, 1, Sort.by(Sort.Order.desc("id")));

	List<Product> products = productRepository.searchProduct("%Iphone%", pageable);
	assertEquals(1, products.size());

	products = productRepository.searchProduct("%GADGET%", pageable);
	assertEquals(1, products.size());
}
```

### Page Result

- Sebelumnya kita sempat bahas tentang Page Result ketika menggunakan Paging
- Pada kasus jika kita ingin return dari Query Method nya adalah `Page<T>`, bukan `List<T>`, maka kita harus memberitahu Spring Data JPA bagaimana cara melakukan count query nya
- Kita bisa tambahkan query count nya pada attribute countQuery di Query Annotation

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	@Query(
		value = "SELECT p FRM Product p WHERE p.name LIKE :name or p.category.name LIKE :name",
		countQuery = "SELECT COUNT(p) FROM Product p where p.name LIKE :name or p.category.name LIKE :name"
	)
	Page<Product> searchProduct(@Param("name") String name, Pageable pageable);
}
```

## Kode: Test Product Repository

```java
@Test
void testProductsSearchPage() {
	Pageable pageable = PageRequest.of(0, 1, Sort.by(Sort.Order.desc("id")));

	Page<Product> products = productRepository.searchProduct("%Iphone%", pageable);
	assertEquals(1, products.getContent().size());
}
```

## #21 Modifying

- Query Annotation juga bisa digunakan untuk membuat JPQ QL atau Native Query untuk perintah Update atau Delete, caranya kita perlu menambahkan annotation `@Modifying` untuk memberitahu bahwa ini bukan Query Select
- <https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Modifying.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	@Modifying
	@Query("DELETE FROM Product p WHERE p.name = :name")
	int deleteProductUsingName(@Param("name") String name);

	@Modifying
	@Query("UPDATE Product p SET p.price = 0 WHERE p.id = :id")
	int updateProductPriceToZero(@Param("id") Long id);
}
```

### Kode: Test Product Repository

```java
@Test
void testModifying() {
	transactionOperations.executeWithoutResult(transctionStatus -> {
		int total = productRepository.deleteProductUsingName("Wrong");
		assertEquals(0, total);

		total = productRepository.updateProductPriceToZero(1L);
		assertEquals(1, total);

		Product product = productRepository.findById(1L).orElse(null);
		assertNotNull(product);
		assertEquals(0L, product.getPrice());
	});
}
```

## #22 Stream

- Saat kita menggunakan `List<T>` dan Query Method `findAll…`, maka secara otomatis seluruh data hasil dari database akan di load ke memory
- Pada kasus data yang sangat banyak, hal ini sangat berbahaya karena bisa terjadi error `OutOfMemory`
- Spring Data JPA bisa menggunakan fitur database cursor, untuk mengambil data sedikit demi sedikit ketika diperlukan menggunakan Java Stream
- Kita bisa membuat Query Method dengan prefix `streamAll…` dan return value `Stream<T>`

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	Stream<Product> streamAllByCategory(Category category);
}
```

### Kode: Test Product Repository

```java
@Test
void stream() {
	transactionOperations.executeWithoutResult(transctionStatus -> {
		Category category = categoryRepository.findById(1L).orElse(null);
		assertNotNull(category);

		Stream<Product> stream = productRepository.streamAllByCategory(category);
		stream.forEach(product -> System.out.println(product.getId() + " : " + product.getName()));
	});
}
```

## #23 Slice

- Saat kita mengembalikan data dalam bentuk `Page<T>`, maka kita hanya akan dapat data untuk nomor page yang dipilih
- Kita bisa menggunakan `Slice<T>`, yang bisa mengembalikan informasi apakah ada next page dan previous page
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/Slice.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	Slice<Product> findAllByCategory(Category category, Pageable pageable);
}
```

### Kode: Test Product Repository

```java
@Test
void slice() {
	Pageable firstPage = PageRequest.of(0, 1);

	Category category = categoryRepository.findById(1L).orElse(null);
	assertNotNull(category);

	Slice<Product> slice = productRepository.findAllByCategory(category, firstPage);

	// do with content
	while (slice.hasNext()) {
		slice = productRepository.findAllByCategory(category, slice.nextPageable());
		// do with content
	}
}
```

## #24 Locking

- Di kelas JPA, kita sudah bahas melakukan Pessimistic Locking
- Karena di Spring Data JPA, kita tidak perlu lagi menggunakan Entity Manager, bagaimana jika kita butuh melakukan Pessimistic Locking?
- Kita bisa membuat Query Method dengan menambahkan annotation `@Lock`
- <https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Lock.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

	@Lock(LockModeType.PESSIMISTIC_WRITE)
	Optional<Product> findFirstByIdEquals(Long id);
}
```

### Kode: Test Product Repository

```java
@Test
void lock1() {
	transactionOperations.executeWithoutResult(transctionStatus -> {
		try {
			Product product = productRepository.findFirstByIdEquals(1L).orElse(null);
			assertNotNull(product);
			product.setPrice(30_000_000L);

			Thread.sleep(20_000L);
			productRepository.save(product);
		} catch (InterruptedException exception) {
			throw new RuntimeException(exception);
		}
	});
}

@Test
void lock2() {
	transactionOperations.executeWithoutResult(transctionStatus -> {
		Product product = productRepository.findFirstByIdEquals(1L).orElse(null);
		assertNotNull(product);
		product.setPrice(10_000_000L);
		productRepository.save(product);
	});
}
```

## #25 Auditing

- Saat kita membuat table, sering sekali kita menambahkan informasi audit seperti createdAt dan updatedAt
- Spring Data JPA mendukung mengubahan data audit secara otomatis ketika proses save
- Kita cukup gunakan annotation `@CreatedDate` dan `@LastModifiedDate`, dan menggunakan `EntityListener` `AuditingEntityListener`
- Kita bisa menggunakan tipe data `Date`, `Timestamp`, `Instance` atau `Long` (milis) untuk field audit nya
- Secara default, fitur ini tidak aktif, untuk mengaktifkannya, kita harus menambahkan annotation `@EnableJpaAuditing`

### Kode: Jpa Auditing

```java
@SpringBootApplication
@EnableJpaAuditing
public class BelajarSpringDataJpaApplication {

	public static void main(String[] args) {
		SpringBootApplication.run(BelajarSpringDataJpaApplication.class, args);
	}
}
```

### Kode: Alter Table Category

```sql
ALTER TABLE categories
	ADD COLUMN created_date TIMESTAMP;

ALTER TABLE categories
	ADD COLUMN last_modified_date TIMESTAMP;
```

### Kode: Class Category

```java
@Table(name = "categories")
@EntityListeners({AuditingEntityListener.class})
public class Category {

	@CreatedDate
	@Column(name = "created_date")
	priavte Instant createdDate;

	@LastModifiedDate
	@Column(name = "last_modified_date")
	priavte Instant lstModifiedDate;
}
```

### Kode: Test Category Repository

```java
@Test
void audit() {
	Category category = new Category();
	category.setName("Sample Audit");
	categoryRepository.save(category);

	assertNotNull(category.getId());
	assertNotNull(category.getCreatedDate());
	assertNotNull(category.getModifiedDate());
}
```

## #26 Example

- Spring Data JPA memiliki fitur Query by Example, dimana kita bisa membuat data object Entity, lalu meminta Spring Data JPA untuk membuat Query berdasarkan data example Entity yang kita buat
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/Example.html>

### Example Repository

- JpaRepository memiliki parent interface `QueryByExampleExecutor `
- Dimana sudah disediakan banyak method yang bisa kita gunakan dengan parameter Example untuk mencari data
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/query/QueryByExampleExecutor.html>

### Kode: Test Category Repository

```java
@Test
void example() {
	Category category = new Category();
	category.setName("GADGET MURAH");

	Example<Category> example = Example.of(category);

	List<Category> categories = categoryRepository.findAll(example);
	assertEquals(1, categories.size());
}
```

### Example Matcher

- Example memiliki fitur Matcher, dimana kita bisa atur cara Example melakukan query
- <https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/ExampleMatcher.html>

### Kode: Test Category Repository

```java
@Test
void exampleMatcher() {
	Category category = new Category();
	category.setName("gadget murah");

	ExampleMatcher matcher = ExampleMatcher.matching().withIgnoreNullValues().withIgnoreCase();

	Example<Category> example = Example.of(category, matcher);

	List<Category> categories = categoryRepository.findAll(example);
	assertEquals(1, categories.size());
}
```

## #27 Specification

### Specification Executor

- Di JPA, terdapat fitur Criteria untuk membuat Query secara dinamis
- Hal ini bisa kita gunakan fitur Specification di Spring Data JPA
- Untuk mendukung fitur ini, Repository yang kita buat harus extends `JpaSpeficitaionExecutor`, dimana terdapat banyak sekali method dengan parameter Specification
- <https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaSpecificationExecutor.html>

### Specification

- Specification adalah lambda yang bisa kita buat dengan mengembalikan data JPA Predicate seperti yang perah kita pelajari di kelas JPA
- Kita bisa mendapatkan detail dari `Root`, `CriteriaQuery` dan `CriteriaBuilder` di method `toPredicate()` milik Specification
- <https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/domain/Specification.html>

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long, JpaSpeficitaionExecutor<Product> {

}
```

### Kode: Test Product Repository

```java
@Test
void specification() {
	Specification<Product> specification = (root, criteria, builder) -> {
		return criteria.where(
				builder.or(
					builder.equal(root.get("name"), "Apple Iphone 14 Pro Max"),
					builder.equal(root.get("name"), "Apple Iphone 13 Pro Max"),
				)
			).getRestriction();
	};

	List<Product> products = productRepository.findAll(specification);
	assertEquals(2, products.size());
}
```

## #28 Projection

- Saat kita belajar JPA, kita tahu terdapat fitur di JPA QL untuk memanggil constructor sebuah class, sehingga return hasil query bisa dalam bentuk class bukan Entity
- Di Spring, terdapat fitur bernama Projection, yang mirip namun lebih mudah
- Caranya di Repository, kita bisa buat Query Method dengan return Interface yang kita inginkan, secara otomatis nanti Spring Data akan melakukan mapping sesuai dengan field hasil Query dengan Interface return nya
- Yup, tidak salah mengetik, jadi kita harus buat dalam bentuk Interface, bukan Class
- Hal ini agar Spring Data tahu bahwa itu adalah projection

### Kode: Simple Product

```java
public interface SimpleProduct {

	Long getId();

	String getName();
}
```

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> JpaSpeficitaionExecutor<Product> {

	List<SimpleProduct> findAllByNameLike(String name);
}
```

### Kode: Test Product Repository

```java
@Test
void projection() {
	List<SimpleProduct> simpleProducts = productRepository.findAllByNameLike("%Apple%");
	assertEquals(2, simpleProducts.size());
}
```

### Java Record

- Atau, jika sudah menggunakan versi Java 17, ada baiknya kita buat Projection dalam bentuk Java Record
- Bedanya dengan interface, saat menggunakan interface, maka Spring Data akan menggunakan Proxy (Reflection)
- Sedangkan ketika menggunakan Java Record, akan dibuat instance nya secara otomatis

### Kode: Simple Product

```java
public record SimpleProduct(Long id, String name) {

}
```

### Dynamic Projection

- Kadang kita mungkin ingin membuat beberapa jenis Projection Interface / Record
- Pada kasus ini, kita bisa menggunakan Generic di Query Method nya, dan juga menambahkan parameter Class di parameter terakhir Query Method nya

### Kode: Product Price

```java
public record ProductPrice(Long id, Long price) {

}
```

### Kode: Product Repository

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long>, JpaSpeficitaionExecutor<Product> {

	<T> List<T> findAllByNameLike(String naem, Class<T> tClass);
}
```

### Kode: Test Product Repository

```java
@Test
void testProjection() {
	List<SimpleProduct> simpleProducts = productRepository.findAllByNameLike("%Apple%", SimpleProduct.class);
	assertEquals(2, simpleProducts.size());

	List<ProductPrice> productPrices = productRepository.findAllByNameLike("%Apple%", ProductPrice.class);
	assertEquals(2, productPrices.size());
}
```

## #29 Materi Selanjutnya

- Spring RESTful API
- Spring Async
- Dan lain-lain
