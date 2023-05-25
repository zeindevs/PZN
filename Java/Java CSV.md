# Java CSV

## Sebelum Belajar

- Java Dasar
- Java OOP
- Java Generic, Collection & Stream
- Apache Maven
- Java Unit Test
- Java Input Output

## #1 Pengenalan CSV

- CSV singkatan dari comma separated value, yaitu format data tabular yang biasanya menggunakan comma sebagai pemisah tiap data kolom nya
- CSV merupakan format sederhana dari format Spreadsheet seperti Microsoft Excel
- Salah satu keuntungan menggunakan format CSV adalah, karena berbasis text, sehingga mudah untuk dibaca dan dimodifikasi dengan editor apapun

### Karakter Pemisah

- Walaupun secara default pemisah pada file CSV adalah , (koma), tapi tidak menjadi keharusan juga
- Beberapa format CSV kadang menggunakan pemisah ; (titik koma), karakter tab, atau yang lainnya

### Contoh File CSV

```csv
Eko, Kurniawan, Khannedy,
Budi, , Nugraha, 90
Joko, Morro, Anwar, 90
Rudi, , Awaludin, 85
```

### CSV Heade

- Pada beberapa kasus, kadang pembuat CSV menyisipkan baris pertama sebagai Header, untuk mempermudah yang membukanya mengerti isi dari file CSV

### Contoh File CSV dengan Header

```csv
Nama Depan, Nama Tengah, Nama Belakang, Nilai Ujian
Eko, Kurniawan, Khannedy, 100
Budi, , Nugraha, 90
Joko, Morro, Anwar, 90
Rudi, , Awaludin, 85
```

## #2 Penganalan Apache Commons CSV

- Java tidak memiliki fitur bawaannya untuk mengolah format file CSV, oleh karena itu kita perlu library untuk mengolah data file CSV
- Apache Commons CSV adalah salah satu library yang populer untuk mengolah data file dengan format CSV
- Apache Commons CSV adalah library yang opensource, sehingga kita bisa gunakan secara gratis
- <https://commons.apache.org/proper/commons-csv/>

## #3 Membuat Project

- <https://start.spring.io/>

### Menambah Dependency

```xml
<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-csv</artifactId>
	<version>1.10.0</version>
</dependency>
```

## #4 Membuat CSV

- Untuk membuat CSV, kita bisa menggunakan class CSVPrinter yang terdapat di library Commons CSV
- Saat membuat CSVPrinter, kita perlu tentukan output tujuan dari CSV
- Di class CSVPrinter, terdapat method printRecord() yang bisa kita gunakan untuk menambah data ke CSV
- <https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVPrinter.html>

### Kode : Membuat CSV

```java
@Test
void createCSV() throws IOExeption {
	StringWriter writer = new StringWriter();

	CSVPrinter printer = new CSVPrinter(writer, CSVFormat.DEFAULT);
	printer.printRecord("Eko", "Khannedy", 100);
	printer.printRecord("Budi", "Nugraha", 100);
	printer.flush();

	System.out.println(write.getBuffer().toString());
}

```

## #5 Membaca CSV

- Untuk membaca CSV, kita bisa menggunakan class CSVParser
- CSVParser adalah turunan dari Iterable, sehingga otomatis kita bisa melakukan iterasi datanya menggunakan perulangan foreach
- <https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVParser.html>
- Tiap perulangan, kita bisa ambil dataya dalam bentuk CSVRecord

### Kode : File CSV

> sample.csv

```csv
Eko,Khannedy,100
Budi,Nugraha,95
```

### Kode : Membaca CSV

```java
@Test
void readCSV() throws IOExeption {
	Path path = Path.of("sample.csv");
	Reader reader = Files.newBUfferedReader(path);

	CSVParser parser = new CSVParser(reader, CSVFormat.DEFAULT);
	for (CSVRecord record : parser) {
		System.out.println("First Name : " = record.get(0));
		System.out.println("Last Name : " = record.get(1));
		System.out.println("Value : " = record.get(2));
	}
}
```

## #6 CSV Header

- Seperti yang sempat dibahas di awal, kadang-kadang, saat kita membuat file CSV, biasanya kita menambahkan baris pertama sebagai Header
- Saat menggunakan Commons CSV, kita harus memberi tahu CSVFormat jika baris pertama adalah kolom, jadi kita bisa menggunakan method `setHeader()` untuk memberitahu bahwa baris pertama adalah header
- Keuntungan menggunakan header, kita bisa menggunakan nama header untuk mendapatkan nilai dari tiap kolom di CSV, jadi tidak perlu menggunakan nomor index lagi

### Kode : File CSV

> sample.csv

```csv
First Name,Last Name,Value
Eko,Khannedy,100
Budi,Nugraha,95
```

### Kode : Membaca CSV dengan Header

```java
@Test
void readCSVWithHeader() throws IOExeption {
	Path path = Path.of("sample.csv");
	Reader reader = Files.newBUfferedReader(path);

	CSVFormat format = CSVFormat.DEFAULT.builder().setHeader().build();
	CSVParser parser = new CSVParser(reader, format);
	for (CSVRecord record : parser) {
		System.out.println("First Name : " = record.get("First Name"));
		System.out.println("Last Name : " = record.get("Last Name"));
		System.out.println("Value : " = record.get("Value"));
	}
}
```

## #7 Menulis CSV Header

- CSVFormat juga bisa digunakan untuk menulis CSV dengan Header
- Kita cukup sebutkan saja nama-nama header dengan method `setHeader()`

### Kode : Menulis CSV dengan Header

```java
@Test
void createCSVWaitHeader() throws IOExeption {
	StringWriter writer = new StringWriter();

	CSVFormat format = CSVFormat.DEFAULT.builder()
	.setHeader("First Name", "Last Name", "Value").build();
	CSVPrinter printer = new CSVPrinter(writer, format);
	printer.printRecord("Eko", "Khannedy", 100);
	printer.printRecord("Budi", "Nugraha", 95);
	printer.flush();

	System.out.println(writer.getBuffer().toString());
}
```

## #8 CSV Format

- Secara default, format file CSV itu dipisahkan menggunakan , (koma)
- Tapi kadang ada beberapa format CSV lain yang mungkin menggunakan pemisah dengan karakter tab, titik koma, atau yang lainnya
- Bahkan beberapa Spreadsheet editor, memiliki aturan tertentu untuk membuat CSV file
- Untungnya, Commons CSV mendukung beberapa format Spreadsheet editor
- Kita bisa lihat di class CSVFormat, terdapat banyak sekali format yang didukung, selain DEFAULT
- <https://commons.apache.org/proper/commons-csv/apidocs/index.html>

### Kode : CSV Format TDF

```java
@Test
void createWithFormat() throws IOExeption {
	StringWriter writer = new StringWriter();

	CSVFormat format = CSVFormat.TOF.builder()
		.setHeader("First Name", "Last Name", "Value").build();
	CSVPrinter printer = new CSVPrinter(writer, format);
	printer.printRecord("Eko", "Khannedy", 100);
	printer.printRecord("Rudi", "Nugraha", 95);
	printer.flush();

	System.out.println(writer.getBuffer().toString());
}
```

## #9 Materi Lainnya

- Perbanyak studi kasus
- Belajar Java Library lain
