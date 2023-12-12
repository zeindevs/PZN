# Golang JSON

## Sebelum Balajar

- Go-Lang Dasar
- Go-Lang Modules
- Go-Lang Unit Test

## Agenda

- Pengenalan Package json
- Encode dan Decode JSON
- Stream Encoder dan Decoder

## #1 Pengenalan Package json

### Pengenalan JSON

- JSON singkatan dari JavaScript Object Notation, merupakan struktur format data yang bentuknya seperti Object di JavaScript
- JSON merupakan struktur format data yang paling banyak digunakan saat kita membuat RESTful API
- dan pada kelas ini kita akan menggunakan JSON juga
- <https://www.json.org/json-en.html>

### Kode: Contoh JSON

```go
{
	"firstName": "Eko",
	"middleName": "Kurniawan",
	"lastName": "Khannedy",
	"website": {
		"title": "Programmer Zaman Now",
		"url": "https://www.programmerzamannow.com"
	},
	"hobbies": [
		"Coding",
		"Reading",
		"Gaming"
	]
}
```

### Package json

- Go-Lang sudah menyediakan package json, dimana kita bisa menggunakan package ini untuk melakukan konversi data ke JSON (encode) atau sebaliknya (decode)
- <https://pkg.go.dev/encoding/json>

## #2 Encode JSON

- Go-Lang telah menyediakan function untuk melakukan konversi data ke JSON, yaitu menggunakan function `json.Marshal(interface{})`
- Karena parameter nya dalah `interface{}`, maka kita bisa memasukan tipe data apapun ke dalam function Marshal

### Kode: Encode JSON

```go
func logJson(data interface{}) {
	bytes, err := json.Marshal(data)
	if err != nil {
		panic(err)
	}
	fmt.Println(string(bytes))
}

func TestMarshal(t *testing.T) {
	logJson("Eko")
	logJson(1)
	logJson(true)
	logJson([]string{"Eko", "Kurniawan", "khannedy"})
}
```

## #3 JSON Object

- Pada materi sebelumnya kita melakukan encode data seperti string, number, boolean dan tipe data primitif lainnya
- Walaupun memang bisa dilakukan, karena sesuai dengan kontrak `interface{}`, namun tidak sesuai dengan kontrak JSON
- Jika mengikuti kontrak json.org, data JSON bentuknya adalah Object dan Array
- Sedangkan value nya baru berupa tipe data primitif

### Kode: Contoh Object JSON

```go
{
	"firstName": "Eko",
	"middleName": "Kurniawan",
	"lastName": "Khannedy"
}
```

### Struct

- JSON Object di Go-Lang direpresentasikan dengan tipe data Struct
- Dimana tiap attibute di JSON Object merupakan attribute di Struct

### Kode: Encode Struct ke JSON (1)

```go
type Customer struct {
	FirstName  string
	MiddleName string
	LastName   string
}
```

### Kode: Encode Struct ke JSON (2)

```go
customer := Customer{
	FirstName:  "Eko",
	MiddleName: "Kurniawan",
	LastName:   "Khannedy",
}

bytes, _, := json.Marshal(customer)
fmt.Println(string(bytes))
```

## #4 Decode JSON

- Sekarang kita sudah tahu bagaimana caranya melakukan encode dari time data di Go-Lang ke JSON
- Namun bagaimana jika kebalikannya?
- Untuk melakukan konversi dari JSON ke tipe data di Go-Lang (Decode), kita bisa menggunakan function `json.Unmarshal(byte[], interface{})`
- Dimana `byte[]` adalah data JSON nya, sedangkan `interface{}` adalah tempat menyimpan hasil konversi, biasa berupa pointer

### Kode: Decode JSON

```go
jsonRequest := `{"FirstName":"Eko","MiddleName":"Kurniawan","LastName":"Khannedy"}`
jsonBytes := []byte(jsonRequest)

customer := &Customer{}
json.Unmarshal(jsonBytes, customer)

fmt.Println(customer)
```

## #5 JSON Array

- Selain tipe data dalam bentuk Object, biasanya dalam JSON, kita kadang menggunakan tipe data Array
- Array di JSON mirip dengan Array di JavaScript, dia bisa berisikan tipe data primitif, atau tipe data kompleks (Object atau Array)
- Di Go-Lang, JSON Array direpresentasikan dalam bentuk slice
- Konversi dari JSON atau ke JSON dilakukan secara otomatis oleh package json menggunakan tipe data slice

### Kode: JSON Array Primitive (1)

```go
type Customer struct {
	FirstName  string
	MiddleName string
	LastName   string
	Hobbies    []string
}
```

### Kode: JSON Array Primitive (2)

```go
customer := Customer{
	FirstName:  "Eko",
	MiddleName: "Kurniawan",
	LastName:   "Khannedy",
	Hobbies:    []string{"Gaming", "Reading", "Coding"},
}

bytes, _, := json.Marshal(customer)
fmt.Println(string(bytes))
```

### Kode: JSON Array Complex (1)

```go
type Address struct {
	Street 		 string
	Country 	 string
	PostalCode string
}

type Customer struct {
	FirstName  string
	MiddleName string
	LastName 	 string
	Hobbies 	 []string
	Addresses  []Address
}
```

### Kode: JSON Array Complex (2)

```go
customer := Customer{
	FirstName:  "Eko",
	MiddleName: "Kurniawan",
	LastName:   "Khannedy",
	Hobbies:    []string{
		"Gaming", "Reading", "Coding",
	},
	Addresses: []Address{
		{
			Street:     "Jalan Belum Jadi",
			Country:    "Indonesia",
			PostalCode: "41111",
		},
	},
}

bytes, _, := json.Marshal(customer)
fmt.Println(string(bytes))
```

### Decode JSON Array

- Selain menggunakan Array pada attribute di Object
- Kita juga bisa melakukan encode atau decode langsung JSON Array nya
- Encode dan Decode JSON Array bisa menggunakan tipe data Slice

### Kode: Decode JSON Array

```go
jsonRequest := `{"Street":"Jalan Belum Jadi","Country":"Indonesia","PostalCode":"41111"}`
jsonBytes := []byte(jsonRequest)

addresses := &[]Address{}
json.Unmarshal(jsonBytes, addresses)

fmt.Println(addresses)
```

## #6 JSON Tag

- Secara default atribut yang terdapat di Struct dan JSON akan di mapping sesuai dengan nama atribut yang sama (case sensitive)
- Kadang ada style yang berbeda antara penamaan atribute di Struct dan di JSON, mial di JSON kita ingin mengunakan `snake_case`, tapi di Struct, kita ingin menggunakan PascalCase
- Untungnya, package json mendukung Tag Reflection
- Kita bisa menamnahkan tag reflection dengan nama json, lalu diikuti dengan atribut yang kita inginkan ketika konversi dari atau ke JSON

### Kode: JSON Tag

```go
type Product struct {
	Id 			 string `json:"id"`
	Name 		 string `json:"name"`
	Price 	 int64  `json:"price"`
	ImageURL string `json:"image_url"`
}
```

## #7 Map

- Saat menggunakan JSON, kadang mungkin kita menemukan kasus data JSON nya dynamic
- Artinya atribut nya tidak menentu, bisa bertambah, bisa berkurang, dan tidak tetap
- Pada kasus seperti itu, menggunakan Struct akan menyulitkan, karena pada Struct, kita harus menentukan semua atribut nya
- Untuk kasus seperti ini, kita bisa menggunakan tipe data `map[string]interface{}`
- Secara otomatis, atribut akan menjadi key di map, dan value menjadi valur di map
- Namun karena vaue berupa `interface{}`, maka kita harus lakukan konversi secara manual jika ingin mengamnil value nya
- Data tipe data Map tidak mendukung JSON Tag lagi

### Kode: Map

```go
jsonRequest := `{"id":"12345","name":"Apple iPhone","price":20000000}`
jsonBytes := []byte(jsonRequest)

var result map[string]interface{}
_ = json.Unmarshal(jsonBytes, &result)

fmt.Println(result)
fmt.Println(result["id"])
fmt.Println(result["name"])
fmt.Println(result["price"])
```

## #8 Streaming Decoder

- Sebelumnya kita belajar package json dengan melakukan konversi data JSON yang sudah dalam bentuk variable dan data `string` atau `[]byte`
- Pada kenyataanya, kadang data JSON nya berasal dari input berupa `io.Reader` (File, Network, Request Body)
- Kita bisa saja membaca semua datanya terlebih dahulu, lalu simpan di variable, baru lakukan konversi dari JSON, namun hal ini sebenarnya tidak perlu dilakukan, karena package json memiliki fitur untuk membaca data dari Stream

### json.Decoder

- Untuk membuat json Decoder, kita bisa menggunakan function `json.NewDecoder(reader)`
- Selanjutnya untuk membaca isi input reader dan konversikan secara langsung ke data di Go-Lang cukup gunakan function `Decode(interface{})`

### Kode: Streaming Decoder

```go
reader, _ := os.Open("dample.json")
decoder := json.NewDecoder(reader)

customer := Customer{}
_ = decoder.Decode(&customer)

fmt.Println(customer)
```

## #9 Streaming Encoder

- Selain decoder, pacakge json juga mendukung membuat Encoder yang bisa digunakan untuk menulis langsung JSON nya ke `io.Writer`
- Dengan begitu, kita tidak perlu menyimpan JSON datanya telebih dahulu ke dalam variable `string` atau `[]byte`, kita bisa langsung tulis ke `io.Writer`

### json.Encoder

- Untuk membuat Encoder, kita bisa menggunakan function `json.NewEncoder(writer)`
- Dan untuk menulis data sebagai JSON langsung ke writer, kita bisa gunakan function `Encode(interface{})`

### Kode: Streaming Encoder

```go
writer, _ := os.Create("sample_output.json")
encoder := json.NewEncoder(writer)

customer := Customer{
	FirstName:  "Eko",
	MiddleName: "Kurniawan",
	LastName:   "Khannedy",
}
_ = encoder.Encode(customer)

fmt.Println(customer)
```

## #10 Materi Selanjutnya

- Go-Lang RESTful API
