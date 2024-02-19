# Design Patterns

## #1 Apa itu Design Patterns?

- Design Patterns adalah solusi umum dari permasalahan-permasalahan umum yang sering terjadi dalam membuat Software.
- Design Patterns bukanlah hal yang bisa di translate ke dalam kode program secara langsung. Melainkan hanya template dan panduan cara menyelesaikan permasalahan.

### Siapa yang butuh belajar Design Patterns?

- Semua software developer yang ingin membuat desain software dengan lebih baik.

### Keuntungan menggunakan Design Patterns?

- Desain software lebih baik
- Software mudah untuk di maintain

## #2 Singleton

```java
// SingletonApp.java
package khannedy.designpattern.singleton;

public class SingletonApp {

	public static void main(String[] args) {
		OrderService orderService = new OrderService();
		orderService.save("001");

		OrderDetailService orderDetailService = new OrderDetailService();
		orderDetailService.save("001", "Indomie");
		orderDetailService.save("001", "Sabun");
		orderDetailService.save("001", "Pepsoden");
	}
}
```

### Kode: Tanpa Singleton Database Connection

```java
// OrderService.java
package khannedy.designpattern.singleton;

public class OrderService {

	public void main(String orderId) {
		Connection = connection = new Connection("localhost", "root", "root");
		connection.sql("INSERT INTO ORDER ...");
	}
}
```

```java
// OrderDetailService.java
package khannedy.designpattern.singleton;

public class OrderDetailService {

	public void save(String userId, String product) {
		Connection connection = new Connection("localhost", "root", "root");
		connection.sql("INSERT INTO ORDER_DETAIL ...");
	}
}
```

```java
// DatabaseHelper.java
package khannedy.designpattern.singleton;

public class DatabaseHelper {

	private static Connection connection;

	public static Connection getConnection() {
		if (connection == null)  {
			connection = new Connection("localhost", "root", "root");
		}
		return connection;
	}
}
```

### Kode: Dengan Singleton Database Connection

```java
// OrderService.java
package khannedy.designpattern.singleton;

public class OrderService {

	public void main(String orderId) {
		DatabaseHelper.getConnection().sql("INSERT INTO ORDER ...");
	}
}
```

```java
// OrderDetailService.java
package khannedy.designpattern.singleton;

public class OrderDetailService {

	public void save(String userId, String product) {
		DatabaseHelper.getConnection().sql("INSERT INTO ORDER_DETAIL ...");
	}
}
```

## #3 Builder

```php
<?php

class Customer
{
	private $id;
	private $fistName;
	private $lastName;
	private $email;
	private $phone;
	private $address;
	private $age;

	public function __construct($ud, $fistName, $lastName, $email, $phone, $address = "", $age = 0) {
		$this->id = $id;
		$this->firstName = $firstName;
		$this->lastName = $lastName;
		$this->email = $email;
		$this->phone = $phone;
		$this->address = $address;
		$this->age = $age;
	}

	public function getId()
	{
		return $this->id
	}

	// getter and setter
}
```

```php
<?php

$customer1 = new Customer("1", "Eko", "Kurniawan", "khannedy", "eko@example.com", "123");

$customer2 = new Customer("1", "Eko", "Kurniawan", "khannedy", "eko@example.com", "123", "Indonesia");

$customer4 = new Customer("1", "Eko", "Kurniawan", "khannedy", "eko@example.com", "123", "Indonesia", 20);
```

```php
<?php

class CustomerBuilder
{
	private $id;
	private $fistName;
	private $lastName = "";
	private $email;
	private $phone;
	private $address = "";
	private $age = 0;
	private $hobby = ";

	public function setId($id)
	{
		$this->id = $id;
		return $this;
	}

	// setter

	public function build(): Customer
	{
		return new Cusomer(
			$this->id,
			$this->fistName,
			$this->lastName,
			$this->email,
			$this->phone,
			$this->address,
			$this->age,
			$this->hobby,
		);
	}
}
```

```php
<?php

$customer1 = (new CustomerBuilder())
	->setFirstName("Eko")
	->setPhone("123")
	->setAge(20)
	->build();

	$customer1 = (new CustomerBuilder())
	->setFirstName("Eko")
	->setLastName("Kurniawan")
	->setPhone("123")
	->setAddress("Indonesia")
	->setAge(20)
	->build();

$customer3 = (new CustomerBuilder())
	->setFirstName("Eko")
	->setPhone("123")
	->setAge(20)
	->setHobby("Coding")
	->build();
```

## #4 Factory Method

```php
// Employee.php
<?php

class Employee
{
	private $name;
	private $title;
	private $salary;

	public function __construct($name, $title, $salary)
	{
		$this->name = $name;
		$this->title = $title;
		$this->salary = $salary;
	}

	public function getName()
	{
		return $this->name;
	}

	// getter
}
```

```php
// App.php

$manager1 = new Employee("Eko", "Manager", 10000000);

$manager2 = new Employee("Rudi", "Manager", 12000000);
```

```php
// EmployeeFactory

class EmployeeFactory
{
	static function createManager($name): Employee
	{
		return new Employee($name, "Manager", 1000000);
	}

	static function createStaff($name): Employee
	{
		return new Employee($name, "Staff", 5000000);
	}
}
```

```php
// App.php

$manager1 = EmployeeFactory::createManager("Eko");
$manager2 = EmployeeFactory::createManager("Rudi");
$staff1 = EmployeeFactory::createStaff("Mora");
$staff2 = EmployeeFactory::createStaff("Joko");
```

```php
interface Animal
{
	function speak()l
}

/**
 * @deprecated
 */
class Tiger implements Animal
{
	function speak()
	{
		echo "Roar!!";
	}
}

class Tiger2 implements Animal
{
	function speak()
	{
		echo "Roar!!";
	}
}

class Cat implements Animal
{
	function speak()
	{
		echo "Meow!!";
	}
}

class Dog implements Animal
{
	function speak()
	{
		echo "Gog gog!!";
	}
}
```

```php
// AnimalFactory.php

class AnimalFactory
{
	public static function create($type): Animal
	{
		if ($type == "tiger") {
			return new Tiger();
		} else if ($type == "cat") {
			return new Cat();
		} else if ($type == "Dog") {
			return new Dog();
		}
	}
}
```

```php
$tiger = AnimalFactory::create("tiger");
$cat = AnimalFactory::create("cat");
$dog = AnimalFactory::create("dog");
```

## #5 Abstract Factory

```php
// Level.php

interface Level
{
	function start();
}

class LevelEasy implements Level
{
	funciton start()
	{
		echo "Level Easy";
	}
}

class LevelMedium implements Level
{
	funciton start()
	{
		echo "Level Medium";
	}
}

class LevelHard implements Level
{
	funciton start()
	{
		echo "Level Hard";
	}
}
```

```php
// Arena.php

interface Arena
{
	function start();
}

class ArenaEasy implements Arena
{
	function start()
	{
		echo "Arena Easy";
	}
}

class ArenaMedium implements Arena
{
	function start()
	{
		echo "Arena Medium";
	}
}

class ArenaHard implements Arena
{
	function start()
	{
		echo "Arena Hard";
	}
}
```

```php
// Game.php

class Game
{
	private $level;

	private $arena;

	public function __construct(Level $level, Arena $arena)
	{
		$this->level = $level;
		$this->arena = $arena;
	}

	function start() {
		$this->level->start();
		$this->arena->start();
	}
}
```

```php
// App.php

$gameEasy = new Game(new LevelEasy(), new ArenaEasy());
$gameEasy->start();

$gameMedium = new Game(new LevelMedium(), new ArenaMedium());
$gameMedium->start();

$gameHard = new Game(new LevelHard(), new ArenaHard());
$gameHard->start();
```

```php
// Enemy.php

interface Enemy
{
	function start();
}

class EnemyEasy implements Enemy
{
	function start()
	{
		echo "Enemy Easy";
	}
}

class EnemyMedium implements Enemy
{
	function start()
	{
		echo "Enemy Medium";
	}
}

class EnemyHard implements Enemy
{
	function start()
	{
		echo "Enemy Hard";
	}
}
```

```php
// GameFactory.php

interface GameFactory
{
	function createLevel(): Level;

	function createArena(): Arena;

	function createEnemy(): Enemy;
}

class GameFactoryEasy implements GameFactory
{
	function createLevel(): Level
	{
		return new LevelEasy();
	}

	function createArena(): Arena
	{
		return new ArenaEasy();
	}

	function createEnemy(): Enemy
	{
		return new EnemyEasy();
	}
}

class GameFactoryMedium implements GameFactory
{
	function createLevel(): Level
	{
		return new LevelMedium();
	}

	function createArena(): Arena
	{
		return new ArenaMedium();
	}

	function createEnemy(): Enemy
	{
		return new EnemyMedium();
	}
}

class GameFactoryHard implements GameFactory
{
	function createLevel(): Level
	{
		return new LevelHard();
	}

	function createArena(): Arena
	{
		return new ArenaHard();
	}

	function createEnemy(): Enemy
	{
		return new EnemyHard();
	}
}
```

```php
// Game.php

class Game
{
	private $level;

	private $arena;

	private $enemy;

	public function __construct(GameFactory $gameFactory)
	{
		$this->level = $gameFactory->createLevel();
		$this->arena = $gameFactory->createArena();
		$this->enemy = $gameFactory->createEnemy();
	}

	function start() {
		$this->level->start();
		$this->arena->start();
		$this->enemy->start();
	}
}
```

```php
// App.php

$gameEasy = new Game(new GameFactoryEasy());
$gameEasy->start();

$gameMedium = new Game(new GameFactoryMedium());
$gameMedium->start();

$gameHard = new Game(new GameFactoryHard());
$gameHard->start();
```

## #6 Prototype

```php
// Store.php
<?php

class Store
{
	private $name;

	private $city;

	private $country;

	private $category;

	public function __construct($name, $city, $country, $category)
	{
		$this->name = $name;
		$this->city = $city;
		$this->country = $country;
		$this->category = $category;
	}

	// getter dan setter

	public function clone(): Store
	{
		// return new Store(
		// 	$this->name,
		// 	$this->city,
		// 	$this->country,
		// 	$this->category,
		// );

		return clone $this;
	}
}
```

```php
// App.php
<?php

$store1 = new Store("Toko X", "Jakarta", "Indonesia", "Gadget");
$store2 = new Store("Toko Z", "Jakarta", "Indonesia", "Gadget");
$store3 = new Store("Toko Y", "Bandung", "Indonesia", "Gadget");
$store4 = new Store("Toko W", "Bandung", "Indonesia", "Fashion");
```

```php
// App.php
<?php

$store1 = new Store("Toko X", "Jakarta", "Indonesia", "Gadget");
$store2 = new Store("Toko Z", $store1->getCity(), $store1->getCountry(), $store1->getCategory());
$store3 = new Store("Toko Y", "Bandung", $store1->getCountry(), $store1->getCategory());
$store4 = new Store("Toko W", $store3->getCity(), $store3->getCountry(), "Fashion");
```

```php
// App.php
<?php

$store1 = new Store("Toko X", "Jakarta", "Indonesia", "Gadget");
$store2 = $store1->clone();
$store2->setName("Toko Z");

$store3 = $store1->clone();
$store3->setName("Toko X");
$store3->setCity("bandung");

$store4 = $store3->clone();
$store4->setName("Toko W");
$store4->setCategory("Fashion");
```

## #7 Object Pool

```java
// DatabasePool.java
package khannedy.designpattern.objectpool

import khannedy.designpattern.singleton.Connection

import java.util.ArrayList;
import java.util.List;

public class DatabasePool {

	private static List<Connection> pool = new ArrayList<>();

	static {
		for (int i = 0; i < 100; i++) {
			Connection connection = new Connection("localhost", "root", "root");
			pool.add(connection);
		}
	}

	public static Connection getConnection() {
		if (pool.isEmpty()) {
			throw new RuntimeException("Koneksi nya abis");
		} else {
			Connection connection = pool.remove(0);
			return connection;
		}
	}

	public static void close(Connection connection) {
		pool.add(connection);
	}
}
```

```java
// OrderDetailService.java
package khannedy.designpattern.singleton;

public class OrderDetailService {

	public void save(String userId, String product) {
		Connection connection = DatabasePool.getConnection()
		connection.sql("INSERT INTO ORDER_DETAIL ...");
		DatabasePool.close(connection);
	}
}
```

```java
// OrderService.java
package khannedy.designpattern.singleton;

public class OrderService {

	public void main(String orderId) {
		Connection connection = DatabasePool.getConnection()
		connection.sql("INSERT INTO ORDER ...");
		DatabasePool.close(connection);
	}
}
```

## #8 Adapter

```java
// Book.java
package khannedy.designpattern.adapter;

public class Book {

	private String title;

	private String author;

	public Book(String title, String author) {
		this.title = title;
		this.author = author;
	}

	// getter and setter
}
```

```java
// AdapterApp.java
package khannedy.designpattern.adapter;

import java.util.ArrayList;
import java.util.List;

public class AdapterApp {

	public static void main(String[] args) {
		List<Object> list = new ArrayList<>();

		list.add(new Book("Pemrograman Java", "Eko"));
		list.add(new Book("Pemrograman PHP", "Kurniawan"));
		list.add(new Book("Pemrograman Python", "Khannedy"));

		list.add(new Screencast("Web Laravel", "Joko", 100L));
		list.add(new Screencast("Web Rails", "Budi", 200L));
		list.add(new Screencast("Flask Web", "Ardi", 150L));

		list.forEach(item -> {
			if (item instanceof Book) {
				Book book = (Book) item;
				System.out.println(book.getTitle() + " by " + book.getAuthor());
			}	else if (item instanceof Screencast) {
				Screencast screencast = (Screencast) item;
				System.out.println(screencast.getTitle() + " by " + screencast.getAuthor());
			}
		});
	}
}
```

```java
// CatalogAdapter.java
package khannedy.designpattern.adapter;

public interface CatalogAdapter {

	String getCatalogTitle();
}
```

```java
// BookCatalogAdapter.java
package khannedy.designpattern.adapter;

public class BookCatalogAdapter implements CatalogAdapter {

	private Book book;


	public BookCatalogAdapter(Book book) {
		this.book = book;
	}

	@Override
	public String getCatalogTitle() {
		return book.getTitle() + " by " + book.getAuthor();
	}
}
```

```java
// ScreencastCatalogAdapter.java
package khannedy.designpattern.adapter;

public class ScreencastCatalogAdapter implements CatalogAdapter {

	private Screencast screencast;


	public ScreencastCatalogAdapter(Screencast screencast) {
		this.screencast = screencast;
	}

	@Override
	public String getCatalogTitle() {
		return screencast.getTitle() + " by " + screencast.getAuthor();
	}
}
```

```java
// AdapterApp.java
package khannedy.designpattern.adapter;

import java.util.ArrayList;
import java.util.List;

public class AdapterApp {

	public static void main(String[] args) {
		List<CatalogAdapter> list = new ArrayList<>();

		list.add(new BookCatalogAdapter(new Book("Pemrograman Java", "Eko"));
		list.add(new BookCatalogAdapter(new Book("Pemrograman PHP", "Kurniawan")));
		list.add(new BookCatalogAdapter(new Book("Pemrograman Python", "Khannedy")));

		list.add(new BookCatalogAdapter(new Screencast("Web Laravel", "Joko", 100L)));
		list.add(new BookCatalogAdapter(new Screencast("Web Rails", "Budi", 200L)));
		list.add(new BookCatalogAdapter(new Screencast("Flask Web", "Ardi", 150L)));

		list.forEach(item -> {
			System.out.println(item.getCatalogTitle());
		});
	}
}
```

## #9 Repository

```java
// Product.java
package khannedy.designpattern.repository;

public class Product {

	private String id;

	private String name;

	private Integer price;

	// getter and setter
}
```

```java
package khannedy.designpattern.repository;

public class RepositoryApp {

	public static void main(String[] args) {
		Product product = new Product();
		product.setId("1");
		product.setName("Contoh 1");
		product.setPrice(1000);

		DatabasePool.getConnection().sql("INSERT INTO products(id, name, price) VALUES (?, ?, ?)", product.getId(), product.getName(), product.getPrice());

		DatabasePool.getConnection().sql("UPDATE products set name = ?, price = ? WHERE id = ?", product.getName(), product.getPrice(), product.getId());
	}
}
```

```java
// ProductRepository.java
package khannedy.designpattern.repository;

import java.util.List;

public class ProductRepository {

	public void insert(Product product) {
		DatabasePool.getConnection().sql("INSERT INTO products(id, name, price) VALUES (?, ?, ?)", product.getId(), product.getName(), product.getPrice());
	}

	public void update(Product product) {
		DatabasePool.getConnection().sql("UPDATE products set name = ?, price = ? WHERE id = ?", product.getName(), product.getPrice(), product.getId());
	}

	public void delete(String id) {
		DatabasePool.getConnection().sql("DELETE products WHERE id = ?", product.getId());
	}

	public List<Product> selectAll() {
		List<Object[]> select = DatabasePool.getConnection().sql("SELECT * FROM products");
		List<Product> products = new ArrayList<>();
		for (Object[] objects: select) {
				Product product = new Product();
				product.setId((String) objects[0]);
				product.setName((String) objects[1]);
				product.setPrice((Integer) objects[2]);
				products.add(product);
		}
		return product;
	}
}
```

```java
package khannedy.designpattern.repository;

import java.util.List;

public class RepositoryApp {

	public static void main(String[] args) {
		Product product = new Product();
		product.setId("1");
		product.setName("Contoh 1");
		product.setPrice(1000);

		ProductRepository repository = new ProductRepository();
		repository.insert(product)

		product.setPrice(2000);

		repository.update(product);

		repository.delete(product.getId());

		List<Product> products = repository.selectAll();
	}
}
```

## #10 Facade

```java
// Account.java
package khannedy.designpattern.facade.entity;

public class Account {

	private String no;

	private Long balance;

	public Account(String no, Long balance) {
		this.no = no;
		this.balance = balance;
	}

	// getter and setter
}
```

```java
// Address.java
package khannedy.designpattern.facade.entity;

public class Address {

	private String id;

	private String street;

	private String country;

	public Address(String id, String street, String country) {
		this.id = id;
		this.street = street;
		this.country = country;
	}

	// getter and setter
}
```

```java
// Customer.java
package khannedy.designpattern.facade.entity;

import java.util.ArrayList;
import java.util.List;

public class Customer {

	private String id;

	private String name;

	private List<Address> addresses = new ArrayList<>();

	public Customer(String id, String name) {
		this.id = id;
		this.name = name;
	}

	// getter and setter
}
```

```java
// AccountRepository.java
package khannedy.designpattern.facade.repository;

import khannedy.designpattern.facade.entity.Account;

public class AccountRepository {

	public Account findById(String no) {
		return null;
	}

	public void update(Account account) {

	}
}
```

```java
// AddressRepository.java
package khannedy.designpattern.facade.repository;

import khannedy.designpattern.facade.entity.Address;

public class AddressRepository {

	public void save(Address address) {

	}
}
```

```java
// CustomerRepository.java
package khannedy.designpattern.facade.repository;

import khannedy.designpattern.facade.entity.Customer;

public class CustomerRepository {

	public void save(Customer customer) {

	}
}
```

```java
// FacadeApp.java

public class FacadeApp {

	public static void main(String[] args) {
		CustomerRepository customerRepository = new CustomerRepository();
		AddressRepository addressRepository = new AddressRepository();

		Customer customer = new Customer("1", "Eko");

		Address address1 = new Address("1", "Subang", "Indonesia");
		Address address1 = new Address("2", "Jakarta", "Indonesia");

		customer.getAddresses().add(address1);
		customer.getAddresses().add(address2);

		customerRepository.save(customer);
		addressRepository.save(address1);
		addressRepository.save(address2);

		// ===================================

		AccountRepository accountRepository = new AccountRepository();

		Account account1 = accountRepository.findById("001");
		Account account2 = accountRepository.findById("002");

		// transfer 1.000.000 from account1 to account2
		account1.setBalance(account1.getBalance() - 1000000);
		account2.setBalance(account2.getBalance() + 1000000);

		accountRepository.update(account1);
		accountRepository.update(account2);
	}
}
```

```java
// CustomerFacade.java
package khannedy.designpattern.facade.facade;

import khannedy.designpattern.facade.entity.Address;
import khannedy.designpattern.facade.entity.Cusomer;
import khannedy.designpattern.facade.repository.AddressRepository;
import khannedy.designpattern.facade.repository.CustomerRepository;

public class CustomerFacade {

	private CustomerRepository customerRepository = new CustomerRepository();

	private AddressRepository addressRepository = new AddressRepository();

	public void save(Customer customer) {
		customerRepository.save(customer);

		for (Address address : customer.getAddresses()) {
			addressRepository.save(address);
		}
	}
}
```

```java
// FacadeApp.java
package khannedy.designpattern.facade;

public class FacadeApp {

	public static void main(String[] args) {
		Customer customer = new Customer("1", "Eko");

		Address address1 = new Address("1", "Subang", "Indonesia");
		Address address1 = new Address("2", "Jakarta", "Indonesia");

		customer.getAddresses().add(address1);
		customer.getAddresses().add(address2);

		CustomerFacade customerFacade = new CustomerFacade();
		customerFacade.save(customer);

		// ===================================

		AccountRepository accountRepository = new AccountRepository();

		Account account1 = accountRepository.findById("001");
		Account account2 = accountRepository.findById("002");

		// transfer 1.000.000 from account1 to account2
		account1.setBalance(account1.getBalance() - 1000000);
		account2.setBalance(account2.getBalance() + 1000000);

		accountRepository.update(account1);
		accountRepository.update(account2);
	}
}
```

```java
// BankFacade.java
package khannedy.designpattern.facade.facade;

public class BankFacade {

	private AccountRepository accountRepository = new AccountRepository();

	public void transfer(String fromAccountNo, String toAccountNo, Long amount) {
		Account account1 = accountRepository.findById(fromAccountNo);
		Account account2 = accountRepository.findById(toAccountNo);

		// transfer 1.000.000 from account1 to account2
		account1.setBalance(account1.getBalance() - amount);
		account2.setBalance(account2.getBalance() + amount);

		accountRepository.update(account1);
		accountRepository.update(account2);
	}
}
```

```java
// FacadeApp.java
package khannedy.designpattern.facade;

public class FacadeApp {

	public static void main(String[] args) {
		Customer customer = new Customer("1", "Eko");

		Address address1 = new Address("1", "Subang", "Indonesia");
		Address address1 = new Address("2", "Jakarta", "Indonesia");

		customer.getAddresses().add(address1);
		customer.getAddresses().add(address2);

		CustomerFacade customerFacade = new CustomerFacade();
		customerFacade.save(customer);

		// ===================================

		BankFacade bankFacade = new BankFacade();
		BankFacade.transfer("001", "002", 1000000);
		BankFacade.transfer("002", "003", 500000);
	}
}
```

```php
// App.php
$customer = new Customer("1", "Eko", "echo.khannedy@gmail.com");

$customerArray = [
	"id" => $customer->getId(),
	"name" => $customer->getName(),
	"email" => $customer->getEmail(),
];
$json = json_encode($customerArray, JSON_PRETTY_PRINT);
```

```php
// CustomerFacade.php

class CustomerFacade
{
	function toJson(Customer $customer): string {
		$customerArray = [
			"id" => $customer->getId(),
			"name" => $customer->getName(),
			"email" => $customer->getEmail(),
		];
		$json = json_encode($customerArray, JSON_PRETTY_PRINT);
		return $json;
	}
}
```

```php
$customer = new Customer("1", "Eko", "echo.khannedy@gmail.com");

$facade = new CustomerFacade();

$json = $facade->toJson($customer);
```

## #11 Template Method

```java
// BlockGame.java
package khannedy.designpattern.template;

public class BlockGame {

	public void start() {
		System.out.println("BLOCK GAME")	;

		for (int i = 0; i < 10; i++) {
			for (int j = 0; j < 10; j++) {
				System.out.print("O");
			}
			System.out.print("\n\r");
		}
		System.out.println("FINISH BLOCK GAME");
	}
}
```

```java
// TemplateApp.java
package khannedy.designpattern.template;

public class TemplateApp {

	public static void main(String[] args) {
		BlockGame blockGame = new BlockGame();
		blockGame.start();
	}
}
```

```java
// BlockGame2.java
package khannedy.designpattern.template;

public class BlockGame2 {

	public void start() {
		System.out.println("BLOCK GAME 2")	;

		for (int i = 0; i < 20; i++) {
			for (int j = 0; j < 20; j++) {
				System.out.print("O");
			}
			System.out.print("\n\r");
		}
		System.out.println("FINISH BLOCK GAME 2");
	}
}
```

```java
// BlockGame3.java
package khannedy.designpattern.template;

public class BlockGame3 {

	public void start() {
		System.out.println("BLOCK GAME 3")	;

		for (int i = 0; i < 30; i++) {
			for (int j = 0; j < 30; j++) {
				System.out.print("O");
			}
			System.out.print("\n\r");
		}
		System.out.println("FINISH BLOCK GAME 3");
	}
}
```

```java
// TemplateApp.java
package khannedy.designpattern.template;

public class TemplateApp {

	public static void main(String[] args) {
		BlockGame blockGame = new BlockGame();
		blockGame.start();

		BlockGame2 blockGame2 = new BlockGame2();
		blockGame2.start();

		BlockGame3 blockGame3 = new BlockGame3();
		blockGame3.start();
	}
}
```

```java
// BlockTemplate.java
package khannedy.designpattern.template;

public abstract class BlockTemplate {

	public final void start() {
		System.out.println(getTitle())	;

		for (int i = 0; i < getheight(); i++) {
			for (int j = 0; j < getWidth(); j++) {
				System.out.print(getCharacter());
			}
			System.out.print("\n\r");
		}
		System.out.println(getEndTitle());
	}

	public abstract String getTitle();

	public abstract String getEndTitle();

	public abstract String getCharacter();

	public abstract Integer getHeight();

	public abstract Integer getWidth();
}
```

```java
// BlockGame.java
package khannedy.designpattern.template;

public class BlockGame extends BlockTemplate {

	@Override
	public String getTitle() {
		return "BLOCK GAME";
	}

	@Override
	public String getEndTitle() {
		return "FINISH BLOCK GAME";
	}

	@Override
	public String getCharacter() {
		return "O";
	}

	@Override
	public Integer getHeight() {
		return 10;
	}

	@Override
	public Integer getWidth() {
		return 10;
	}
}
```

```java
// TemplateApp.java
package khannedy.designpattern.template;

public class TemplateApp {

	public static void main(String[] args) {
		BlockGame blockGame = new BlockGame();
		blockGame.start();

		BlockGame2 blockGame2 = new BlockGame2();
		blockGame2.start();

		BlockGame3 blockGame3 = new BlockGame3();
		blockGame3.start();
	}
}
```

## #12 Bridge

```java
// Binatang.java
package khannedy.designpattern.bridge;

public interface Binatang {

	String getNama();

	boolean hidupAir();

	boolean hidupDiDarat();
}
```

```java
// Anjing.java
package khannedy.designpattern.bridge;

public class Anjing implements Binatang {

	@Override
	public String getNama() { return "Anjing"; }

	@Override
	pubilc boolean hidupDiAir() { return false; }

	@Override
	pubilc boolean hidupDiDarat() { return true; }
}
```

```java
// Kucing.java
package khannedy.designpattern.bridge;

public class Kucing implements Binatang {

	@Override
	public String getNama() { return "Kucing"; }

	@Override
	pubilc boolean hidupDiAir() { return false; }

	@Override
	pubilc boolean hidupDiDarat() { return true; }
}
```

```java
// BinatangApp.java
package khannedy.designpattern.bridge;

public class BinatangApp {

	publc static void main(String[] args) {
		List<Binatang> binatangs = Arrays.asList(
			new Anjing(),
			new Kucing(),
			new Kambing(),
			new Koi(),
			new Koi(),
			new Hiu()
		);

		binatangs.forEach(binatang -> {
			if (binatang.hidupDiAir()) {
				System.out.println(binatang.getNama() + " hidup di air");
			} else if (binatang.hidupDiDarat()) {
				System.out.println(binatang.getNama() + " hidup di darat");
			}
		});
	}
}
```

```java
// Anjing.java
package khannedy.designpattern.bridge;

public class Anjing implements Binatang {

	@Override
	public String getNama() { return "Anjing"; }

	@Override
	pubilc boolean hidupDiAir() { return false; }

	@Override
	pubilc boolean hidupDiDarat() { return true; }

	public Integer getJumlahKaki() {
		return 4;
	}
}
```

```java
// BinatangApp.java
package khannedy.designpattern.bridge;

public class BinatangApp {

	publc static void main(String[] args) {
		List<Binatang> binatangs = Arrays.asList(
			new Anjing(),
			new Kucing(),
			new Kambing(),
			new Koi(),
			new Koi(),
			new Hiu()
		);

		binatangs.forEach(binatang -> {
			if (binatang.hidupDiAir()) {
				System.out.println(binatang.getNama() + " hidup di air");
			} else if (binatang.hidupDiDarat()) {
				if (binatang instanceof Kambing) {
					Kambing kambing (Kambing) binatang;
					System.out.println(binatang.getNama() + " hidup di darat dengan kaki " + kambing.getJumlahKaki());
				} else if (binatang instanceof Anjing) {
					Anjing anjing    (Anjing) binatang;
					System.out.println(binatang.getNama() + " hidup di darat dengan kaki " + anjing   .getJumlahKaki());
				}
			}
		});
	}
}
```

```java
// BinatangDarat.java
package khannedy.designpattern.bridge;

public abstract class BinatangDarat implements Binatang {

	@Override
	public boolean hidupDiAir() {
		return false;
	}

	@Override
	public boolean hidupDiDarat() {
		return true;
	}

	public abstract Integer getJumlahKaki();
}
```

```java
// BinatangAir.java
package khannedy.designpattern.bridge;

public abstract class BinatangAir implements Binatang {

	@Override
	public boolean hidupDiAir() {
		return true;
	}

	@Override
	public boolean hidupDiDarat() {
		return false;
	}
}
```

```java
// Anjing.java
package khannedy.designpattern.bridge;

public class Anjing extends BinatangDarat {

	@Override
	public String getNama() { return "Anjing"; }

	@Override
	public Integer getJumlahKaki() {
		return 4;
	}
}
```

```java
// Hiu.java
package khannedy.designpattern.bridge;

public class Hiu extends BinatangAir {

	@Override
	public String getNama() { return "Hiu"; }
}
```

```java
// BinatangApp.java
package khannedy.designpattern.bridge;

public class BinatangApp {

	publc static void main(String[] args) {
		List<Binatang> binatangs = Arrays.asList(
			new Anjing(),
			new Kucing(),
			new Kambing(),
			new Koi(),
			new Koi(),
			new Hiu()
		);

		binatangs.forEach(binatang -> {
			if (binatang instanceof BinatangAir) {
				BinatangAir binatangAir = (BinatangAir) binatang;
				System.out.println(binatangAir.getNama() + " hidup di air");
			} else if (binatang instanceof BinatangDarat) {
				BinatangDarat binatangDarat = (BinatangDarat) binatang;
				System.out.println(binatangDarat.getNama() + " hidup di darat dengan kaki " + binatangDarat.getJumlahKaki());
			}
		});
	}
}
```

## #13 Composite

```java
// Category.java
package khannedy.designpattern.composite;

public class Category {

	public String name;

	public Category(String name) {
		this.name = name;
	}

	public String getName(): { return name; }

	public void setName(String name) { this.name = name; }
}
```

```java
// CompositeApp.java
package khannedy.designpattern.composite;

public class CompositeApp {

	public static void main(String[] args) {
		List<Category> categories = Arrays.asList(
			new Category("Handphone"),
			new Category("Computer"),
			new Category("Fashion")
		);

		categories.forEach(category -> {
			printCategory(category);
		});
	}

	public static void printCategory(Category category) {
		System.out.println(category.getName());
	}
}
```

```java
// CompositeCategory.java
package khannedy.designpattern.composite;

import java.util.ArrayList;
import java.util.List;

public class CompositeCategory extends Category {

	private List<Category> categories = new ArrayList<>();

	public CompositeCategory(String name, List<Category> categories) {
		super(name);
		this.categories = categories;
	}

	public List<Category> getCategories() {
		return categories;
	}
}
```

```java
// CompositeApp.java
package khannedy.designpattern.composite;

public class CompositeApp {

	public static void main(String[] args) {
		List<Category> categories = Arrays.asList(
			new CompositeCategory("Handphone", Arrays.asList(
				new Category("Android"),
				new Category("IOS")
			)),
			new CompositeCategory("Computer", Arrays.asList(
				new Category("Laptop"),
				new Category("PC")
			)),
			new CompositeCategory("Fashion", Arrays.asList(
				new CompositeCategory("Fashion Pria", Arrays.asList(
					new Category("Celana Pria"),
					new Category("Jeans Pria"),
					new CompositeCategory("Baju Pria", Arrays.asList(
						new Category("Kaos Hitam"),
						new Category("Jas Hitam")
					)),
				)),
				new Category("Fashion Wanita")
			))
		);

		categories.forEach(category -> {
			printCategory(category);
		});
	}

	public static void printCategory(Category category) {
		System.out.println(category.getName());

		if (category instanceof CompositeCategory) {
			CompositeCategory compositeCategory = (CompositeCategory) category;
			compositeCategory.getCategories().forEach(value -> {
				printCategory(value);
			});
		}
	}
}
```
