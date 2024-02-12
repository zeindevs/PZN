# Kotlin Generic

## Sebelum Belajar

- Kotlin Dasar
- Kotlin Object Oriented Programming
- <https://www.udemy.com/course/pemrograman-kotlin-pemula-sampai-mahir/?referralCode=98BE2E779EB8A0BEC230>

## #1 Pengenalan Generic

- Generic adalah kemampuan menambahkan parameter type saat membuat class atau function
- Berbeda dengan parameter type yang biasa kita gunakan di class di function, generic memungkinkan kita bisa mengubah-ubah bentuk type sesuai dengan yang kita mau.

### Manfaat Generic

- Pengecekan ketika proses kompilasi
- Tidak perlu manual menggunakan pengecekan tipe data dan konversi tipe data
- Memudahkan programmer membuat kode program yang generic sehingga bisa digunakan oleh berbagai tipe data

### Kode: Bukan Generic

```kt
class Data(val data: Any)

fun main() {
	val dataString = Data("Eko")
	val valueString: String = dataString.data as String

	val dataInt = Data(10)
	val valueInt: Int = dataInt.data as Int
}
```

### Kode: Generic

```kt
class Data<T>(val data: T)

fun main() {
	val dataString = Data<String>("Eko")
	val valueString: String = dataString.data

	val dataInt = Dat<Int>a(10)
	val valueInt: Int = dataInt.data
}
```

## #2 Generic Class

### Generic Type

- Generic type adalah class atau interface yang memiliki parameter type
- Tidak ada ketentuan dalam pembuatan generic parameter type, namun biasanya kebanyakan orang menggunakan 1 karakter sebagai generic parameter type
- Nama generic parameter type yang biasa digunakan adalah :
  - `E` - Element (biasa digunakan di collection atau struktur data)
  - `K` - Key
  - `N` - Number
  - `T` - Type
  - `V` - Value
  - `S,U,V` etc. - 2nd, 3rd, 4th types

### Kode: Generic Type

```kt
class MyData<T>(val firstData: T) {

	fun printData() {
		return println("Data is $fistData")
	}

	fun getData() {
		return firstData
	}
}

fun main() {
	val data1: MyData<String> = MyData<String>("Eko")
	val data2: MyData<String> = MyData("Eko")
	val data3 = MyData("Eko")

	data1.printData()
	data2.printData()
	data3.printData()
}
```

### Multiple Parameter Type

- Parameter type di Generic type boleh lebih dari satu
- Namun harus menggunakan nama type berbeda
- Ini sangat berguna ketika kita ingin membuat generic parameter type yang banyak

### Kode: Multiple Parameter Type

```kt
class MyData<T, U>(val firstData: T, val secondData: U) {

	fun printData() {
		return println("Data is $fistData, $secondData")
	}

	fun getSecond(): U {
		return secondData
	}

	fun getData(): T {
		return firstData
	}
}

fun main() {
	val data1: MyData<String, Int> = MyData<String, Int>("Eko", 30)
	val data2: MyData<String, Int> = MyData("Eko", 30)
	val data3 = MyData("Eko", 30)

	data1.printData()
	data2.printData()
	data3.printData()
}
```

## #3 Generic Function

- Generic parameter type tidak hanya bisa digunakan pada class atau interface
- Kita juga bisa menggunakan generic parameter type di function
- Generic parameter type yang kita deklarasikan di function, hanya bisa diakses di function tersebut, tidak bisa digunakan di luar function
- Ini cocok jika kita ingin membuat generic function, tanpa harus mengubah deklarasi class

### Kode: Program Generic Function

```kt
class Function(val name: String) {
	fun <T> sayHello(param: T) {
		println("Hello $param, my name is $name")
	}
}

func main() {
	val function = Function("Eko")

	function.sayHello<String>("Budi")
	function.sayHello("Budi")

	function.sayHello<Int>(10)
	function.sayHello(10)
}
```

## #4 Invariant

- Secara default, saat kita membuat generic parameter type, sifat parameter tersebut adalah invariant
- Invariant artinya tidak boleh di subtitusi dengan `subtype` (child) atau `supertype` (parent)
- Artinya saat kita membuat object `Contoh<String>`, maka tidak sama dengan `Contoh<Any>`, begitupun sebaliknya, saat membuat object `Contoh<Any>`, maka tidak sama dengan `Contoh<String>`

### Kode: Invariant

```kt
class Invariant<T>(val data: T)

fun main() {
	val data1: Invariant<String> = Invariant("Eko")
	val data2: Invariant<Any> = data1 // error
}
```

## #5 Covariant

- Covariant artinya kita bisa melakukan subtitusi `subtype` (child) dengan `supertype` (parent)
- Tidak semua jenis class generic yang mendukung covariant, hanya class generic yang menggunakan generic parameter type sebagai return type function
- Artinya saat kita membuat object `Contoh<String>`, maka bisa disubtitusi menjadi `Contoh<Any>`
- Untuk memberitahu bahwa generic parameter type tersebut adalah covariant, kita perlu menggunakan kata kunci `out`

### Kode: Covariant

```kt
class Covariant<out T>(val data: T) {
	fun data() {
		return data
	}
}

fun main() {
	val data1: Covariant<String> = Covariant("Eko")
	val data2: Covariant<Any> = data1

	println(data2.data())
}
```

## #6 Contravariant

- Contravariant artinya kita bisa melakukan subtitusi supertype (parent) dengan subtype (child)
- Tidak semua jenis class generic yang mendukung contravariant, hanya class generic yang menggunakan generic parameter type sebagai parameter function
- Artinya saat kita membuat object `Contoh<Any>`, maka bisa disubtitusi menjadi `Contoh<String>`
- Untuk memberitahu bahwa generic parameter type tersebut adalah covariant, kita perlu menggunakan kata kunci `in`

### Kode: Contravariant

```kt
class Contravariant<in T> {
	fun sayHello(name: T) {
		return println("Hello $name")
	}
}

fun main() {
	val data1: Contravariant<Any> = Contravariant()
	val data2: Contravariant<String> = data1

	data2.sayHello("Eko")
}
```

## #7 Generic Constraints

- Kadang kita ingin membatasi data yang boleh digunakan di generic parameter type
- Kita bisa menambahkan constraint di generic parameter type dengan menyebutkan tipe yang diperbolehkan
- Secara otomatis, type data yang bisa digunakan adalah type yang sudah kita sebutkan, atau class-class turunannya
- Secara default, constraint type untuk generic parameter type adalah `Any`, sehingga semua tipe data bisa digunakan

### Kode: Generic Constraint

```kt
open class Employee

class Managet : Employee()

class VicePresident : Employee()

class Company<T : Employee>(val employee: T)

fun main() {
	val data1 = Company(Manager())
	val data2 = Company(VicePresident())
}
```

### Where Keyword

- Kadang kita ingin membatasi tipe data dengan beberapa jenis tipe data di generic parameter type
- Secara default, hanya satu tipe data yang bisa digunakan untuk membatasi generic parameter type
- Jika kita ingin menggunakan lebih dari satu tipe data, kita bisa menggunakan kata kunci `where`

### Kode: Where Keyword

```kt
interface CanSayHello {
	fun sayHello(name: String)
}

open class Employee

class Manager : Employee{}

class VicePresident : Employee(), CanSayHello {
	override fun sayHello(name: String) {
		println("Hello $name")
	}
}

class Company<T>(val employee: T) where T : Employee, T : CanSayHello

fun main() {
	val data1 = Company(Manager()) // error
	val data2 = Company(VicePresident()) // error
	val data3 = Company("String") // error
}
```

## #8 Type Projection

- Kadang agak sulit untuk membuat class generic type yang harus covariant atau contravariant, misal karena memang di class generic tersebut terdapat input dan output generic parameter type
- Namun jika kita membuat function untuk memanipulasi data invariant sangat lah sulit, karena generic parameter type nya harus selalu sama
- Kita bisa melakukan type projection, yaitu menambahkan informasi covariant atau contravariant di parameter function, ini memaksa isi function untuk melakukan pengecekan
- Jika covariant, kita tidak boleh mengubah data generic di object
- Jika contravariant, kita tidak boleh ngambil data generic object

### Kode: Type Projection

```kt
class Container<T>(var data: T)

fun copy(from: Container<out Any>, to: Container<Any>) {
	to.data = from.data
}

fun main() {
	val data1 = Container("Data 1")
	val data2 = Container<Any> = Container("Data 2")
	copy(data1, data2)
}
```

## #9 Star Projection

- Kadang ada kasus kita tidak peduli dengan generic parameter type pada object
- Misal kita hanya ingin mengambil panjang data `Array<T>`, dan kita tidak peduli dengan isi data `T` nya
- Jika kita mengalami kasus seperti ini, kita bisa menggunakan Star Projection
- Star projection bisa dibuat dengan mengganti generic parameter type dengan karakter `*` (star, bintang)

### Kode: Star Projection

```kt
fun displayLength(array: Array<*>) {
	println("Length Array is ${array.size}")
}

fun main() {
	val arrayInt = arrayOf(1, 2, 3, 4, 5, 6)
	val arrayString = arrayOf("Eko", "Kurniawan", "Khannedy")
	displayLength(arrayInt)
	displayLength(arrayString)
}
```

## #10 Type Erasure

- Type erasure adalah proses pengecekan generic pada saat compile time, dan menghiraukan pengecekan pada saat runtime
- Type erasure menjadikan informasi generic yang kita buat akan hilang ketika kode program kita telah di compile menjadi binary file
- Compiler akan mengubah generic parameter type menjadi tipe `Any` (atau Object di Java)

### Kode: Type Erasure

```kt
package belajar.generic.app

class TypeErasure<T>(param: T) {
	private val data: T = param
	fun getData(): T = data
}

public final class TypeErasure {
	private final Object data;

	public final Object getData() { return data }

	public TypeErasure(Object param)
}

fun main() {
	val data = TypeErasure("Eko")
	val name = data.getData()
}
```

### Problem Type Erasure

- Karena informasi generic hilang ketika sudah menjadi binary file
- Oleh karena itu, konversi tipe data generic akan berbahaya jika dilakukan secara tidak bijak

### Kode: Problem Type Erasure

```kt
fun main() {
	val data = TypeErasure("Eko")
	val name = data.getData()

	val eko = data as TypeErasure<Int>
	val number = data.getData() // error runtime

	println(name)
}
```

## #11 Comparable Interface

- Sebelumnya kita sudah tahu bahwa operator perbandingan `==` dan `!=` akan menggunakan metode equals sebagai implementasinya.
- Bagaimana dengan operator perbandingan lainnya? Seperti `>` `>=` `<` `<=` ?
- Operator perbandingan tersebut bisa kita lakukan, jika object kita mewariskan interface generic Comparable

### Kode: Comparable

```kt
class Fruit(val name: String, val quantity: Int) :
	Comparable<Fruit> {

		override fun compareTo(other: Fruit): Int {
			return quantity.compareTo(other.quantity)
		}
	}
}

fun main() {
	val fruit1 = Fruit("Mango", 10)
	val fruit2 = Fruit("Mango", 100)

	println(fruit1 > fruit2)
	println(fruit1 < fruit2)
}
```

## #12 ReadOnlyProperty Interface

- Sebelumnya kita sudah belajar tentang delegate di Kotlin
- Di Kotlin, ada sebuah interface generic yang bisa digunakan sebagai delegate property yang sifatnya readonly, alias `val` (immutable), namanya `ReadOnlyProperty`
- ReadOnlyProperty bisa digunakan sebagai delegate, sehingga sebelum data kita kembalikan, kita bisa melakukan sesuatu, atau bahkan mengubah value si property

### Kode: ReadOnlyProperty

```kt
class NameWithLog(param: String) {
	val name: String by LogReadOnlyProperties(param)
}

class LogReadOnlyProperties(val data: String) : ReadOnlyProperty<Any, String> {
	override fun getValue(thisRef: Any, property: KProperty<*>): String {
		println("Access property ${property.name} with value $data")
		return data.toUpperCase()
	}
}

fun main() {
	val name = NameWithLog("Eko")
	println(name.name)
}
```

## #13 ReadWriteProperty Interface

- Selain ReadOnlyProperty, kita juga menggunakan interface generic `ReadWriteProperty` sebagai delegate
- ReadWriteProperty bisa digunakan untuk variable `var` (mutable)

### Kode: ReadWriteProperty

```kt
class StringLogReadWriteProperty(var data: String) : ReadWriteProperty<Any, String> {

	override fun getValue(thisRef: Any, property: KProperty<*>): String {
		println("You get data ${property.name} is $data")
		return data.toUpperCase()
	}

	override fun setValue(thisRef: Any, property: KProperty<*>, value: String) {
		println("You set data ${property.name} with value $data to $value")
		data = value
	}
}

class Person(param: String) {
	var name: String by StringLogReadWriteProperty(param)
}

fun main() {
	val person = Person('Eko')
	person.name = "Budi"
	println(person.name)
}
```

## #14 ObservableProperty Class

- Generic interface delegate yang sebelumnya kita gunakan (`ReadOnlyProperty `dan `ReadWriteProperty`) kita perlu mengatur value datanya secara manual
- Kadang kita hanya butuh melakukan sesuatu sebelum dan setelah data nya diubah
- Untuk kasus seperti ini, kita bisa menggunakan generic class `ObservableProperty`

### Kode: ObservableProperty

```kt
class LogObservableProperty<T>(data: T) : ObservableProperty<T>(data) {

	override fun beforeChange(property: KProperty<*>, oldValue: T, newValue: T): Boolean {
		println("Before change value from $oldValue to $newValue")
		return true
	}

	override fun afterChange(property: KProperty<*>, oldValue: T, newValue: T){
		println("After change value from $oldValue to $newValue")
	}
}

class Car(brand: String) {

	var brand: String by LogObservableProperty<String>(brand)
}

fun main() {
	val car = Car("Toyota")
	car.brand = "Wuling"
	println(car.brand)
}
```

### Object Delegates

| Function                                   | Keterangan                                                               |
| ------------------------------------------ | ------------------------------------------------------------------------ |
| `Delegates.notNull()`                      | ReadWriteProperty yang nilai awal bisa null, namun error jika masih null |
| `Delegates.veotable(value, beforeChange)`  | ObservableProperty dengan beforeChange                                   |
| `Delegates.observable(value, afterChange)` | ObservableProperty dengan afterChange                                    |

## #15 Generic Extension Function

- Generic juga bisa digunakan pada extension function
- Dengan begitu kita bisa memilih jenis generic parameter type apa yang bisa menggunakan extension function tersebut

### Kode: Generic Extension Function

```kt
class Data<T>(val data: T)

fun Data<String>.print() {
	val string = this.data
	println("String value is $string")
}

fun main() {
	val data1: Data<Int> = Data(1)
	val data2: Data<String> = Data("Eko")

	data1.print() // error
	data2.print()
}
```

## #16 Materi Selanjutnya

- Kotlin Collection
- Kotlin Coroutine
