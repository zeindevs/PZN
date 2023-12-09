# Golang Modules

## Sebelum Belajar Materi Ini

- Golang Dasar

## Agenda

- Mengenal Go Modules
- Membuat Module
- Rilis Module
- Menambah Dependency
- Upgrade Module
- Upgrade Dependency

## #1 Pengenalan Go Module

### Sebelum Ada Go Modules

- Saat kita membuat aplikasi, biasanya kita akan menggunakan library atau dependency dari project lain.
- Sebelum ada Go Modules, management untuk dependency sangat sulit dilakukan di Go-Lang.
- Biasanya kita akan meng-copy semua source code library atau dependency lain ke project kita, hal ini membuat project kita menjadi bengkak karena penuh dengan library orang lain.
- Atau biasanya library orang lain akan kita save di GOPATH directory, namun hal ini akan sulit jika ternyata beberapa aplikasi buth library yang sama dengan versi yang berbeda

### Go-Modules

- Go-Modules mulai dikenalkan di Go-Lang 1.11 and 1.12
- Dengan Go-Modules, kita bisa membuat project dengan mudah dan dependency management yang sanga mudah

### Membuat Module

- Untuk membuat module baru, kita bisa menggunakan perintah : 
- `go mod init nama-module`
- Go-Lang akan secara otomatis membuat file `go.mod` yang berisikan informati nama module dan juga versi Go-lang yang kita gunakan

### Rilis Module

- Go-lang terintegrasi baik dengan Git
- Untuk merilis module, kita hanya perlu membuat Tag di Git

## #2 Membuat Project

```sh
git mod init github.com/<username>/<module-name>
```

```sh
# git init
git init
```

```sh
# git commit
git commit -m "initial"
```

```sh
# git push
git push origin master
```

```sh
# create tag
git tag v1.0.0
```

```sh
# push tag
git push v1.0.0
```

```sh
# go.mod
module github.com/<username>/<module-name>

go 1.15
```

## #3 Menambah Dependency

- `go get nama-module`

```sh
git get github.com/<username>/<module-name>
```

> go.mod

```mod
module github.com/<username>/<module-name>

go 1.15

require "github.com/username/module-name" v1.0.0
```

```go
package main

import (
	"fmt"
	say_hello "github.com/username/module-name"
)

func main() {
	fmt.Println(say_hello.SayHello())
}
```

## #4 Upgrade Module

- Untuk melakukan upgrade module, kita hanya cukup membuat tag baru di Git

> create new tag

`go tag v1.0.1`

> push tag

`go push v1.0.1`

## #5 Upgrade Dependency

- Untuk upgrade dependency ke versi terbaru, kita bisa mengubah isi go.mod, lalu mengubah tag nya menjadi tag terbaru
- Untuk mendownload versi terbaru, gunakan perintah: `go get`

```mod
module github.com/<username>/<module-name>

go 1.15

require "github.com/username/module-name" v1.0.5
```

## #6 Major Upgrade

- Major upgrade biasanya terjadi dikarenakan ada perbahan pada isi kode program kita, sehingga membuat tidak backward compatible
- Sebaiknya hal ini sebisa mungkin dihindari
- Namun jika tidak bisa dihindari, strategy terbaik adalah merubah nama module

```mod
module github.com/<username>/<module-name>/v2

go 1.15
```

`go get github.com/<username>/<module-name>/v2`

## #7 Materi Selanjutnya

- Go-Lang Unit Test
- Go-Lang Concurrency
