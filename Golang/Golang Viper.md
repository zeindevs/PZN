# Golang Viper

## Sebelum Belajar

- Go-Lang Dasar
- Go-Lang Modules
- Go-Lang Unit Test

## #1 Pengenalan Golang Viper

### Application Configuration

- Setiap kita membuat aplikasi, pasti kita tiadak akan meng-hardcode konfigurasi aplikasi di kode program kita. Misal konfigurasi koneksi ke database, atau yang sejenisnya
- Hal ini dikrenakan, tiap tempat untuk menjalankan aplikasi yang kita buat mungkin akan berbeda konfigurasi nya, misal di komputer kita, di server atau di komputer orang lain
- Artinya, aplikasi yang kita buat, perlu memiliki kemampuan untuk mengambil data konfigurasi dari luar aplikasi, agar isi konfigurasi bisa diubah-ubah secara dinamis

### Golang Viper

- Viper adalah salah satu library untuk Golang yang populer untuk melakukan manajemen konfigurasi
- Viiper mendukung banyak jenis file konfigurasi, seperti JSON, YAML, env file, properties, dan lain-lain
- <https://github.com/spf13/viper>

## #2 Membuat Project

- Buat folder belajar-golang-viper
- `go mod init belajar-golang-viper`

### Menambahkan Viper

- `go get github.com/spf13/viper`

### Menambahkan Testify

- `go get github.com/stretchr/testify`

## #3 Membuat Viper

- Untuk membuat Viper, caranya cukup mudah, kita bisa menggunakan function `viper.New()`
- Setelah membuat viper, kita bisa menentukan dari mana kita akan mengambil konfigurasi

### Kode: Membuat Viper

```go
func TestViper(t *testing.T) {
	var config *viper.Viper = viper.New()
	assert.NotNill(t, config)
}
```

## #4 JSON

- Viper bisa digunakan untuk membaca konfigurasi dari file JSON
- Sekarang kita akan coba membuat file JSON, lalu membaca isi konfigurasinya menggunakan Viper

### Kode: Config JSON

> config.json

```json
{
	"app": {
		"name": "belajar-golang-viper",
		"version": "1.0.0",
		"author": "Eko Khannedy"
	},
	"database": {
		"show_sql": true,
		"host": "localhost",
		"port": 3306
	}
}
```

### Kode: Membaca JSON

```go
func TestJSON(t *testing.T) {
	config := viper.New()
	config.SetConfigName("config")
	config.SetConfigType("json")
	config.AddConfigPath(".")

	// membace config
	err := config.ReadInConfig()
	assert.Nil(t, err)

}
```

### Mengambil Value

- Untuk mengambil value yang sudah dibaca dari file yang kita tentukan, kita bisa menggunakan method `GetXxxx`, sesuai dengan tipe datanya, misal `GetString`, `GetInt`, `GetBool` dan lain-lain
- Jika kita ingin mengakses nested object pada JSON, kita bisa menggunakan `.` (titik), misal: `app.name`, `database.host`

### Kode: Mengambil Value

```go
assert.Equal(t, "belajar-golang-viper", config.GetString("app.name"))
assert.Equal(t, "Eko Khannedy", config.GetString("app.author"))
assert.Equal(t, 3306, config.GetSInt("database.port"))
assert.True(t, config.GetBool("database.show_sql"))
```

## #5 YAML

- Selain JSON, Viper juga bisa digunakan untuk membaca file dengan format YAML
- Cara penggunaanya sama seperti ketika menggunakan JSON

### Kode: Config YAML

> config.yaml

```yaml
app:
	name: "belajar-golang-viper"
	version: "1.0.0"
	author: "Eko Khannedy"
database:
	show_sql: true
	host: "localhost"
	port: 3306
```

### Kode: Membaca YAML

```go
func TestYAML(t *testing.T) {
	config := viper.new()
	config.SetConfigFile("config.yaml")
	config.AddConfigPath(".")

	err := config.ReadInConfig()
	assert.Nil(t, err)

	assert.Equal(t, "belajar-golang-viper", config.GetString("app.name"))
	assert.Equal(t, "Eko Khannedy", config.GetString("app.author"))
	assert.Equal(t, 3306, config.GetSInt("database.port"))
	assert.True(t, config.GetBool("database.show_sql"))
}
```

## #6 ENV File

- Viper juga bisa digunakan untuk membaca file dengan format Env
- Di beberapa framework, kadang banyak yang menggunakan format ini

### Kode: Config ENV

> config.env

```env
APP_NAME=belajar-golang-viper
APP_VERSION=1.0.0
APP_AUTHOR=Eko Khannedy

DATABSE_HOST=locahost
DATABSE_PORT=3306
DATABSE_SQL_SHOW=true
```

### Kode: Membaca ENV File

```go
func TestENVFile(t *testing.T) {
	config := viper.new()
	config.SetConfigFile("config.env")
	config.AddConfigPath(".")

	err := config.ReadInConfig()
	assert.Nil(t, err)

	assert.Equal(t, "belajar-golang-viper", config.GetString("APP_NAME"))
	assert.Equal(t, "Eko Khannedy", config.GetString("APP_AUTHOR"))
	assert.Equal(t, 3306, config.GetSInt("DATABASE_PORT"))
	assert.True(t, config.GetBool("DATABASE_SHOW_SQL"))
```

## #7 Environment Variable

- Kadang saat menjalankan aplikasi, kita menyimpan konfigurasi menggunakan environment variable yang terdapat di sistem operasi yang kita gunakan
- Secara default, Viper tidak akan membaca data dari environment variable
- Namun jika kita mau, kita bisa menggunakan method `AutomaticEnv()` untuk membaca dari environment variable

### Kode: Membaca dari Environment variabe

```go
func TestENVFile(t *testing.T) {
	config := viper.new()
	config.SetConfigFile("config.env")
	config.AddConfigPath(".")
	config.AutomaticEnv()

	err := config.ReadInConfig()
	assert.Nil(t, err)

	assert.Equal(t, "belajar-golang-viper", config.GetString("APP_NAME"))
	assert.Equal(t, "Eko Khannedy", config.GetString("APP_AUTHOR"))
	assert.Equal(t, 3306, config.GetSInt("DATABASE_PORT"))
	assert.True(t, config.GetBool("DATABASE_SHOW_SQL"))

	assert.Equal(t, "Hello", config.GetString("FROM_ENV"))
```

## #8 Fitur Lainnya

### File Config Lainnya

- Sebenarnya Viper bisa digunakan untuk membaca dari jenis file konfigurasi yang lain, misal
- HCL (Hasicorp Configuration Language)
- Properties (Java Properties File)

### Remote Config

- Viper juga bisa digunakan untuk membaca konfigurasi dari remote/server yang terdapat di aplikasi
- Consul: <https://github.com/spf13/viper@consul>
- Etcd: <https://github.com/spf13/viper@etcd>

## #9 Penutup
