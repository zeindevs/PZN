# NodeJS Dasar

## Sebelum Belajar

- Sudah menyelesaikan Kelas Readmap JavaScript dari Programmer Zaman Now

## Agenda

- Pengenalan NodeJS
- Pengenalan Concurrency
- NodeJS Architecture
- Menginstall NodeJS
- NodeJS REPL
- Standard Library
- Dan lain-lain

## #1 Pengenalan NodeJS

- NodeJS diperkenalkan pertama kalo oleh Ryan Dahl pada tahun 2009
- NodeJS merupakan teknologi yang bisa digunakan untuk menjalankan kode JavaScript diluar Web Browser
- NodeJS dibuat dari V8 Engine, yaitu Engine untuk Google Chrome
- NodeJS merupakan project yang Free dan OpenSource
- <https://nodejs.org>

### Kenapa Belajar NodeJS

- NodeJS mempopulerkan pradigma JavaScript Everywhere, dimana dengan menggunakan NodeJS, kita bisa membuat aplikasi berbasis server side dengan bahasa pemrogramman JavaScript
- Hal ini membuar kita hanya butuh belajar bahasa pemrograman JavaScript untuk membuat aplikasi web misalnya, sehingga tidak butuh belejar bahasa pemrograman lain seperti PHP atau Java untuk server side wab nya
- Saat ini NodeJS sangat populer dan banyak sekali digunakan di perusahaan teknologi, terutama untuk membantu pengembangan Web Frontend

### Yang Tidak Bisa Dilakukan di NodeJS

- Pada Kelas JavaScript, kita sudah membahas banyak sekali fitur JavaScript yang berjalan di Browser
- Karena NodeJS tidak berhalan di Browser, jadi tidak semua fitur JavaScript bisa dilakukan di NodeJS
- Fitur seperti Document Object Model dan banyak Web API tidak bisa dilakukan di NodeJS, hal ini karena DOM dan beberapa Web API berjalan membutuhkan Browser

### Text Editor

- NodeJS menggunakan bahasa pemrograman JavaScript, oleh karena itu kita bisa menggunakan Text Editor apapun untuk membuat aplikasi menggunakan NodeJS misal:
- Visual Studio Code
- JetBrains WebStrom
- Sublime
- Atom
- NodePad++
- Dan lain-lain

## #2 Web Application

- Web Application adalah aplikasi yang berjalan di Server dan ditampilkan di Browser Client
- Saat kita membuat Web Application, biasanya akan dibagi menjadi 3 bagian, Client, Server dan Database

### Diagram Web Application

![Diagram Web Application](./images/nodejs-dasar-01.jpg)

### Client 

- Client merupakan user interface atau bagian frontend dari web application, yang digunakan oleh pengguna web application
- Client digunakan untuk berinteraksi dengan Server, baik itu mengirim data atau menerima data
- Frontend biasanya dibuat menggunakan HTML, CSS, dan JavaScript

### Server

- Server betanggung jawab untuk menerima request dari Client, mengerjakan request yang dikirim dan membalas request berupa response ke Client
- Server bertugas sebagai backend untuk web application, dimana semua logic aplikasi akan dilakukan di Server
- Biasanya Server dibuat menggunakan PHP, Python, Java, .NET dan banyak bahasa pemrogramman lainnya
- Dengan adanya NodeJS, sekarang kita bisa membuat Server menggunakan JavaScript

### Database

- Database adalah tempat untuk menyimpan data web application
- Data disimpan dan diambil oleh Server
- Client tidak bisa langsung mengambil atau menyimpan data ke Database secara langsung, oleh karena itu perlu penengah untuk melakukannya, yaitu Server
- Database biasanya menggunakan aplikasi sistem basis data seperti misalnya MySQL, PostgreSQL, MongoDB, dan lain-lain

## #3 Concurrency dan Parallel

### Sejarah

- Dahulu, komputer hanya menjalankan sebuah program pada satu waktu
- Karena hanya bisa menjalankan satu program pada satu waktu, hal ini tidak efisien dan memakan waktu lama karena hanya bisa mengerjakan satu tugas pada satu waktu
- Semakin kesini, sistem operasi untuk komputer semakin berkembang, sekarang sistem operasi bisa menjalankan program secara bersamaan dalam proses yang berbeda-beda, terisolasi dan saling independen antar program

### Thread

- Program biasanya berjalan dalam sebuah proses, dan proses akan memiliki resource yang independen dengan proses lain
- Sekarang sistem operasi tidak hanya bisa menjalankan multiple proses, namun dalam proses kita bisa menjalankan banyak pekerjaan sekaligus, atau bisa dibilang proses ringan atau lebih dikenal dengan nama Thread
- Thread membuat proses aplikasi bisa berjalan tidak harus selalu sequential, kita bisa membuat proses aplikasi berjalan menjadi asynchronous atau parallel

### Concurrency vs Parallel

- Kadang banyak yang bingung dengan concurrency dan parallel, sebenarnya kita tidak perlu terlalu memusingkan hal ini
- Karena saat ini, kita pasti akan menggunakan keduannya ketika membuat aplikasi
- Concurrency artinya mengerjakan beberapa pekerjaan satu persatu
- Parallel artinya mengerjakan beberapa pekerjaan sekaligus pada satu waktu

### Diagram Cocurrency

![Diagram Cocurrency](./images/nodejs-dasar-02.jpg)

### Diagram Parallel

![Diagram Parallel](./images/nodejs-dasar-03.jpg)

### Contoh Concurrency dan Paralel

- Browser adalah aplikasi yang concurrent dan parallel
- Browser akan melakukan proses concurrent ketika membuka web, browser akan melakukan http request, lalu mendownload semua file web (html, css, js) lalu merender dalam bentuk tampilan web
- Browser akan menlakukan proses parallel, ketika kita membuka beberapa tab web, dan juga sambil download beberapa file, dan menonton video dari web streaming

### Synchronous vs Asynchronous

- Saat membuat aplikasi yang concurrent atau parallel, kadang kita sering menemui istilah synchronous dan asynchronous
- Tidak perlu bingung dengan istilah tersebut, secara sederhana
- Synchronous adalah ketika kode program kita berjalan secara sequential, dan semua tahapan ditunggu sampai prosesnya selesai baru akan dieksekusi ke tahapan selanjutnya
- Sedangkan, Asynchronous artinya ketika kode program kita berjalan dan kita tidak perlu menunggu eksekusi kode tersebut selesai, bisa kita lanjutkan ke tahapan kode program selanjutnya

### Diagram Synchronus

![Diagram Synchronus](./images/nodejs-dasar-04.jpg)

### Diagram Asynchronus

![Diagram Asynchronus](./images/nodejs-dasar-05.jpg)

## #4 Threadpool Web Model

- Pada materi sebelumnya sudah dijelaskan bahwa thread adalah proses ringan yang bisa dibuat saat membuka aplikasi
- Walaupun bisa dibilang ringan, namun jika terlalu banyak membuat thread, maka tetap akan memberatkan sistem operasi kita
- Oleh karena itu, biasanya kita akan menggunakan threadpool untuk melakukan management thread
- Threadpool merupakan tempat dimana kita menyimpan thread, ketika kita butuk kita akan ambil dari threadpool, ketikda sudah selesai, kita akan mengembalikan thread nya ke threadpool
- Dengan threadpool, kita bisa memanfaatkan thread yang sama berkali-kali, tanpa harus membuat thread baru terus menerus

### Diagram Threadpool

![Diagram Threadpool](./images/nodejs-dasar-06.jpg)

### Threadpool Queue

- Apa yang terjadi ketika semua thread sendang bekerja? Bagaimana jika kita ingin meminta thread ke threadpool untuk mengerjakan sesuatu?
- Jika semua thread penuh, kita tidak bisa meminta lagi thread ke threadpool. Kita harus menunggu sampai ada thread yang tidak sibut
- Dimana kita harus menunggu sampai ada thread tersedia untuk digunakan?
- Biasanya threadpool memiliki tempat untuk menyimpan tugas yang belum dikerjakan oleh thread di tempat bernama queue (antrian)
- Ketika kita mengirim perintah ke threadpool, perintah tersebut akan dikirim ke queue, lalu perintah-perintah itu akan dieksekusi satu per satu oleh thread yang tersedia di threadpool

### Diagram Threadpool Queue

![Diagram Threadpool Queue](./images/nodejs-dasar-07.jpg)

### Threadpool Web Model

- Dahulu pembuatan web application sangat populer menggunakan threadpool model
- Setiap request yang masuk ke web server akan diproses oleh satu buah thread
- Dengan demikian ketika banyak request masuk, semua bisa diproses secara paralel karena akan ditangani oleh thread masing-masing
- Namun threadpool model ini memiliki kekurangan, ketika thread sedang sibuk semua, secara otomatis request selanjutnya harus menunggu sampai ada thread yang selesai melakukan tugas sebelumnya
- Contoh web server yang menggunakan threadpool model, seperti Apache HTTPD, Apache Tomcat, dan lain-lain

## #5 Blocking dan Non-Blocking

### Blocking

- Saat kita membuat kode program, secara default kode program akan berjalan secara blocking atau synchronous
- Artinya kita harus menunggu sebuah kode selesai sebelum kode selanjutnya dieksekusi
- Contoh ketika kita membuat kode program untuk membaca file, jika kode kita blocking, maka kita harus menunggu program selesai membace file, baru kita bisa melanjutkan kode program selanjutnya

### Non-Blocking

- Non-Blocking berbeda dengan Blocking, kode program Non-Blocking akan dieskekusi tanpa harus menunggu kode program tersebut selesai
- Non-Blocking akan dijalankan secara asynchronus
- Ketika memanggil kode Non-Blocking, biasanya kita perlu mengirimkan callback utnuk dipanggil oleh kode Non-Blocking tersebut ketika kodenya sudah selesai
- Contoh-contoh Non-Blocking, sudah kita bahasa di Kelas JavaScipt Async, seperti AJAX, Fetch API dan lain-lain
- Di NodeJS, hampir semua fiturnya mendukung kode Non-Blocking

## #6 NodeJS Architecture

![NodeJS Architecture](./images/nodejs-dasar-08.jpg)

### Event-Loop

- Event-Loop merupakan single thread proses yang digunakan untuk mengeksekusi kode Non-Blocking
- Karena Event-Loop hanya menggunakan single thread, maka kita harus berhati-hati ketika membuat blocking code, karena bisa memperlambat proses eksekusi kode kita
- Event-Loop sendiri sebenarnya tugasnya hanya menerima dan mengirim eksekusi kode ke C++ Threadpool, oleh karena itu selalu usahakan menggunakan kode nonblocking agar proses blocking-nya dikerjakan di C++ threadpool
- Event-Loop akan menerima response dari C++ threadpool yang di kirim via callback

### C++ Threadpool

- NodeJS Menggunakan C++ Threadpool untuk workernya, yaitu threadpool untuk melakukan pekerjaan
- Libuv adalah library yang digunakan di NodeJS, dimana secara default libuv menggunaakan 4 thread, di dalam threadpool nya, hal ini manjadikan kita bisa melakukan 4 pekerjaan blocking sekaligus dalam satu waktu
- Jika terlalu banyak pekerjaan blocking, kita bisa mengubah jumlah thread di libuv dengan pengaturan environment variable UV_THREADPOOL_SIZE
- <https://docs.libuv.org/en/v1.x/threadpool.html>

## #7 Menginstall NodeJS

### Menginstall NodeJS Manual

- Download versi NodeJS LTS (Long Term Suppoer)
- <https://nodejs.org/en/download>

### Menginstall NodeJS dengan Package Manager

- <https://github.com/nvm-sh/nvm>
- <https://community.chocholatey.org/packages/nodejs>
- <https://formulae.brow.sh/formula/node>

### Setting PATH NodeJS

- Setelah menginstall NodeJS, disarankan melakukan setting PATH NodeJS pada sistem operasi kita
- Hal ini agar mudah ketika kita mengekses program NodeJS menggunakan terminal/command prompt

### Kode Mengecek NodeJS

```bash
node --version
```

## #8 Hello World

### Kode: Hello World

```js
console.info("Hello World")
```

### Menjalankan Kode JavaScript

- Karena NodeJS tidak memerlukan Web Browser, jadi kita bisa langsung menjalankan program JavaScript kita menggunakan apliaksi NodeJS lewat terminal/command prompt, dengan perintah:
- `node namafile.js`

### Kode: Menjalankan Hello World

```bash
node hello-world.js
```

## #9 NodeJS REPL

### REPL (Read Eval Print Loop)

- REPL singkatan Read Eval Print Loop
- Yaitu mekanisme dimana program bisa membaca langsung kode program yang diketikan, lalu mengeksekusinya, menampilkan hasilnya, lalu mengulangi dari awal lagi
- NodeJS mendukung REPL, sehingga lebih mudah ketika belajar
- Namun tetap, saya menyarankan menyimpan kode program di file JavaScript, agar lebih mudah diubah ketika terjadi masalah
- Untuk menggunaakn NodeJS REPL, cukup jalankan aplikasi node saja

### Kode: REPL

```bash
node
```

![Kode: REPL](./images/nodejs-dasar-09.jpg)

## #10 NodeJS Standard Library

- Saat kita belajar JavaScript, di Web Browser, terdapat fitur-fitur yang bernama Web API
- <https://developer.mozilla.org/en-US/docs/Web/API>
- Kebanyakan fitur Web API hanya berjalan di Web Browser, sehingga tidak bisa jalan di NodeJS
- NodeJS sendiri hanya menggunakan bahasa pemrograman JavaScript nya, namum tidak mengadopsi fitur Web API nya, karena itu hanya berjalan di Web Browser
- NodeJS sendiri memiliki standard library yang bisa kita gunakan untuk mempermudah pembuatan aplikasi
- <https://nodejs.org/dist/latest-v16.x/docs/api>

## #11 Modules

- Standard Library yang terdapat di NodeJS bisa kita gunakan seperti layaknya JavaScript Modules
- Jika belum mengerti tentang JavaScript Modules, silahkan pelajari kelas sayan tentang JavaScript Modules
- Karena NodeJS menggunakan Modules, jika kita ingin menggunakan Modules, kita juga perlu memberi tahu bahwa file JavaScript kita menggunakan Modules, caranya dengan mengubah nama file dari .js menjadi .mjs

### Kode: Contoh Standard Library

> standard-library.mjs

```js
import os from os

console.info(os.platform());
console.table(os.cpus());
```

## #12 Require Function

- Awal ketika NodeJS rilis, fitur JavaScript Modules belum rilis, namun sekarang JavaScript sudah banyak menggunakan JavaScript Modules
- NodeJS pun awalnya tidak menggunakan JavaScript Modules, namun sekarang NodeJS sudah bisa menggunakan JavaScript Modules, dan sangat direkomendasikan menggunakannya
- Namun awal sebelum Modules, NodeJS menggunakan function require() untuk melakukan import file
- Di materi ni saya sengaja bahas, agar tidak bingun ketika kita melihat tutorial yang masih menggunakan function require

### Kode: Function Require

```js
const os = require("ps")

console.info(os.platform());
console.table(os.cpus());
```

## #13 Global Async di Module

### Global Async

- Saat kita belajar JavaScript, untuk menggunakan Async Await, biasanya kita perlu membuat dahulu function yang kita tandai sebagai async
- Saat kita menggunakan Module, secara default, global code adalah Async, oleh karena itu kita bisa menggunakan Async Await
- Kecuali jika ktia membuat function, maka function tersebut harus kita tandai sebagai Async jika ingin menggunakan Async Await

### Kode: JavaScript

> sample.js

```js
function samplePromise() {
	return Promise.resolve("Eko")
}

const data = await samplePromise() // error
console.info(data)
```

### Kode: JavaScript Module

> sample.mjs

```js
function samplePromise() {
	return Promise.resolve("Eko")
}

const data = await samplePromise()
console.info(data)
```

## #14 OS

- OS merupakan standard library yang bisa digunakan untuk mendapatkan informasi tentang sistem operasi yang digunakan
- <https://nodejs.org/dist/latest-v16.x/docs/api/os.html>

### Kode: OS

```js
import os from "os"

console.info(os.platform())
console.info(os.arch())
console.table(os.cpus())
console.info(os.uptime())
console.info(os.totalmem())
console.info(os.freemem())
console.table(os.networkInterfaces())
```

## #15 Path

- Path merupakan standard library yang bisa kita gunakan untuk bekerja dengan loksi file dan directory/folder
- <https://nodejs.org/dist/latest-v16.x/docs/api/path.html>

### Kode: Path

```js
import path from "path"

const file = "/Users/khannedy/contoh.html"

console.info(path.sep)
console.info(path.dirname(file))
console.info(path.basename(file))
console.info(path.extname(file))
console.info(path.parse(file))
```

## #16 File System

- File System merupakan standard library yang bisa digunakan untuk memanipulasi file system
- Dalam File System, terdapat 3 jenis library
- Pertama library yang bersifat blocking atau synchronous
- Kedua library yang bersifat non-blocking atau asynchronous menggunakan callback
- Ketika library yang bersifat non-blocking atau asynchronous tiap menggunakan promise
- <https://nodejs.org/dust/latest-v16.x/docs/api/fs.html>

### Kode: File System

```js
import fs from "fs"

const buffer = fs.readFileSync("file-system.mjs")

console.info(buffer.toString())

fs.writeFileSync("temp.txt", "Hello World")
```

## #17 Debugger

- NodeJS memiliki fitur debugger, dimana kita bisa mengikuti tahapan eksekusi program di NodeJS
- Hal ini sangat cocok ketika kita melakukan proses debugging, mencari sebab masalah yang terjadi di aplikasi kita
- <https://nodejs.org/dist/latest-v16.x/docs/api/debugger.html>

### Breakpoint

- Dalam debugging, terdapat istilah breakpoint, yaitu lokasi dimana kita ingin menghentikan sementara eksekusi kode program
- Biasanya ini dilakukan untuk mengawasi data-data di sekitar lokasi berhentinya tersebut
- Untuk menambahkan breakpoint, kita bisa menggunakan kata kunci debugger

### Menjalankan Mode Debug

- Jika kita menjalankan file JavaScript hanya dengan menggunakan perintah node namafile.js, maka secara default dia tidak akan jalan dalam mode debug
- Agar jalan dalam mode debug, kita harus menambahkan perintah inspect:
- `node inspect namafile.js`

### Perintah Debuggger

- Saat masuk ke mode debug, ada beberapa perintah yang bisa kita gunakan dalam melakukan debugging
- cont, c: Continue execution
- next, n: Step next
- step, s: Step in
- out, o: Step out
- pause: Pause running code

### Kode: Debugger

```js
function sayHello(name) {
	debugger;
	return `Hello ${name}`
}

const firstName = "Eko"

console.info(sayHello(firstName))
```

## #18 DNS

- DNS merupakan standard library yang bida digunakan untuk bekerja dengan DNS (domain name server)
- <https://nodejs.org/dist/latest-v16.x/docs/api/dns.html>

### Kode: DNS

```js
import dns from "dns"

function callback(error, ip) {
	console.info(ip)
}

dns.lookup("www.programmerzamannow.com", callback)
```

### Kode: DNS Promise

```js
import dns from "dns/promises"

const lookup = await dns.lookup("www.programmerzamannow.com")

console.info(lookup.family)
console.info(lookup.address)
```

## #19 Events

- Events adalah standard library di NodeJS yang bisa digunakan sebagai implementasi Event Listener
- Di dalam Events, terdapat sebuah class bernama EventEmiter yang bisa digunakan untuk menampung data listener per jenis event
- Lalu kita bisa melakukan emmit untuk mentrigger jenis event dan mengirim data ke event tersebut
- <https://nodejs.org/dist/latest-v16.x/docs/api/events.html>

### Kode: Events

```js
import {EventEmitter} from "events"

const emitter = new EventEmitter()
emitter.addListener("hello", (name) => {
	console.info(`Hello ${name}`)
})
emitter.addListener("hello", (name) => {
	console.info(`Halo ${name}`)
})

emitter.emit("hello", "Eko")
```

## #20 Globals

- Di dalam NodeJS, terdapat library berupa variable atau function yang secara global bisa diakses dimana saja, tanpa harus melakukan import
- Kita bisa melihat detail apa saja fitur yang terdapat secara global di halaman dokumentasinya
- <https://nodejs.org/dist/latest-v16.x/docs/api/globals.html>

### Kode: Globals

```js
setTimeout(() => {
	console.info("Hello World")
}, 3000)
```

## #21 Process

- Process merupakan standard library yang digunakan untuk mendapatkan informasi proses NodeJS yang sedang berjalan
- Process juga merupakan instance dari EventListener, sehingga kita bisa menambahkan listener kedalam Process
- <https://nodejs.org/dist/latest-v16.x/docs/api/process.html>

### Kode: Process

```js
import process from "process"

process.addListener("exit", (exitCode) => {
	console.info(`NodeJS exit with code: ${exitCode}`)
})

console.info(process.version)
console.info(process.argv)
console.info(process.report)
console.info(process.env)

process.exit(1)

process.info("Not Printed because already exit")
```

## #22 Readline

- Readline merupakan standard library yang digunakan untuk membaca input
- Namun pada ssat dibuat video ini, Readline hanya mendukung versi callback di versi NodeJS LTS 16
- Di NodeJS 17 sudah mendukung Promise sehingga lebih mudah digunakan, namun itupun masih dalam tahap experimental
- <https://nodejs.org/dist/latest-v16.x/docs/api/readline.html>

### Kode: Readline

```js
import process from "process"
import readline from "readline"

const input = readline.createInterface({
	input: process.stdin,
	output: process.stdout,
})

input.question("Siapa nama anda? : ", (nama) => {
	console.info(`Hello ${nama}`)
	input.close()
})
```

## #23 Report

- Report merupakan fitur yang terdapat di NodeJS untuk membuat laporan secara otomatis dalam file ketika sesuatu terjadi pada aplikasi NodeJS kita
- <https://nodejs.org/dist/latest-v16.x/docs/api/report.html>

### Kode: Error pada Aplikasi NodeJS

```js
import process from "process"

process.report.reportOnFatalError = true
process.report.reportOnUncaughtException = true
process.report.reportOnSignal = true
process.report.filename = "report.json"

function sampleError() {
	throw new Error("ups")
}

sampleError()
```

## #24 Buffer

- Buffer merupakan object yang berisikan urutan byte dengan panjang tetap
- Buffer merupakan turunan dari tipe data Uint8Array
- <https://nodejs.org/dist/latest-v16.x/docs/api/buffer.html>

### Kode: Buffer

```js
const buffer = Buffer.from("Eko")
console.info(buffer.toString())

buffer.reverse()
console.info(buffer.toString())
```

### Buffer Encoding

- Buffer juga bisa digunakan untuk melakukan encoding dari satu encoding ke encoding yang lain
- Ada banyak encoding yang didukung oleh Buffer, misal utf8, ascii, hex, base64, base64url, dan lain-lain

### Kode: Buffer Encoding

```js
const buffer = Buffer.from("Eko Kurniawan Khannedy", "utf8")

console.info(buffer.toString('base64'))
console.info(buffer.toString('hex'))

const buffer2 = Buffer.from("RWtvIEt1cm5pYXdhbiBLaGFubmVkeQ==", "base64url")

console.info(buffer2.toString('utf8'))
```

## #25 Stream

- Stream adalah standard libray untuk kontrak aliran data di NodeJS
- Ada banyak sekali Stream object di NodeJS
- Stream bisa jadi object yang bisa dibaca, atau bisa di tulis, dan Stream adalah turunan dari EventEmitter
- <https://nodejs.org/dist/latest-v16.x/docs/api/stream.html>

### Kode: Stream

```js
import fs from "fs"

const writer = fs.createWriteStream("target.log")
writer.write("Eko")
writer.write("Kurniawan")
writer.write("Khannedy")
writer.close()

const reader = fs.createReadStream("target.log")
reader.on('data', (data) => {
	console.info(data.toString())
	reader.close()
})
```

## #26 Timer

- Timer merupakan standard library untuk melakukan scheduling
- Function di Timer terdapat di globals, sehingga kita bisa menggunakannya tanpa melakukan import, namun semua function Timer menggunakan Callback
- Jika kita ingin menggunakan Timer versi Promise, kita bisa meng-import dari module timer/promise
- <https://nodejs.org/dist/latest-v16.x/docs/api/timers.html>

### Kode: Timer

```js
setTimeout(() => {
	console.info(`Timer at ${new Date()}`)
}, 1000)
```

### Kode: Timer Promise

```js
import timers from "timers/promises"

for await (const startTime of timers.setInterval(1000, new Date())) {
	console.info(`Start Timer at ${startTime}`)
}
```

## #27 Net

- Net merupakan standard library yang bisa digunakan untuk membuat network client dan server berbasis TCP
- Net Server dan Client merupakan object Stream, sehingga kita bisa baca datanya, tulis datanya dan juga menembahkan listener
- <https://nodejs.org/dist/latest-v16.x/docs/api/net.html>

### Kode: Net Server

```js
import net from "net"

const server = net.createServer((client) => {
	console.info("Client connected")
	client.on('data', (data) => {
		console.info(`Receive data from client : ${data.toString()}`)
		client.write(`Hello ${data.toString()}\r\n`)
	})
})

server.listen(3000, "localhost")
```

### Kode: Net Client

```js
import net from "net"

const connection = net.createConnection({port: 3000, host: 'localhost'})

setInterval(() => {
	connection.write("Ekp\r\n")
}, 2000)

connection.addListener('data', (data) => {
	console.info(`Receive data from server : ${data.toString()}`)
})
```

## #28 URL

- URL merupakan standard library untuk bekerja dengan URL
- <https://nodejs.org/dist/latest-v16.x/docs/api/url.html>

### Kode: URL

```js
import { URL } from "url"

const pzn = new URL("https://www.programmerzamannow.com/belajar?helas=nodejs")

console.info(pzn.toString())
console.info(pzn.protocol)
console.info(pzn.host)
console.info(pzn.pathname)
console.info(pzn.searchParams)
```

### Kode: Mengubah URL

```js
import { URL } from "url"

const pzn = new URL("https://www.programmerzamannow.com/belajar?helas=nodejs")

pzn.host = "www.khannedy.com"
const searchParams = pzn.searchParams
searchParams.append('status', "premium")

console.info(pzn.toString())
```

## #29 Util

- Util adalah standard library yang berisikan utility-utility yang bisa kita gunakan untuk mempermudah pembuatan kode program di NodeJS
- <https://nodejs.org/dist/latest-v16.x/docs/api/util.html>

### Kode: Util

```js
import util from "util"

console.info(util.format("Nama : %s", "Eko"))

const person = {firstName: "Eko", lastName: "Khannedy"}
console.info(util.format("Person: %j", person))
```

## #30 Zlib

- Zlib adalah standard library yang digunakan untuk melakukan kompresi menggunakan Gzip
- <https://nodejs.org/dist/latest-v16.x/docs/api/zlib.html>

### Kode: Zlib Compress

```js
import zlib from "zlib"
import fs from "fs"

const source = fs.readFileSync("zlib.mjs")
const result = zlib.gzipSync(source)

fs.writeFileSync('zlib.mjs.txt', result)
```

### Kode: Zlib Decompress

```js
import zlib from "zlib"
import fs from "fs"

const source = fs.readFileSync("zlib.mjs.txt")
const result = zlib.unzipSync(source)
console.info(result.toString())
```

## #31 Console

- Console adalah standard library yang sudah sering kita gunakan
- Secara global, object console bisa digunakan tanpa harus melakukan import module, dan console melakukan print text nya ke stdout
- Namun jika kita juga bisa membuat object Console sendiri jika kita mau
- <https://nodejs.org/dist/latest-v16.x/docs/api/console.html>

### Kode: Console

```js
import { Console } from "console"
import fs from "fs"

const logFile = fs.createWriteStream("application.log")

const log = new Console({
	stdout: logFile,
	stderr: logFile
})

log.info("Hello world")
log.erro("Ups")
```

## #32 Worker Threads

- Worker Threads adalah standard library yang bisa kita gunakan untuk menggunakan thread ketika mengeksekusi JavaScript secara paralel
- Worker Threads sangat cocok ketika kita membuat kode program yang buth jalan secara paralel, dan biasanya kasusnya adalah ketika kode program kita membutuhkan proses yang CPU intensive, seperti misalnya enkripsi atau kompresi
- Cara kerja Worker Threads mirip dengan Web Worker di JavaScript Web API
- <https://nodejs.org/dist/latest-v16.x/docs/api/worker_threads.html>

### Kode: Main Thread

```js
import { threadId, Worker } from "worker_threads"

const worker1 = new Worker("./worker.mjs")
const worker2 = new Worker("./worker.mjs")

worker1.addListener("message", (message) => {
	console.info(`thread-${threadId} receive message : ${message}`)
})
worker2.addListener("message", (message) => {
	console.info(`thread-${threadId} receive message : ${message}`)
})

worker1.postMessage(10)
worker2.postMessage(10)
```

### Kode: Worker Thread

```js
import { parentPart, threadId } from 'worker_threads'

parentPart.addListener("message", (message) => {
	for (let i = 0; i < message; i++) {
		console.info(`thread-${threadId} send message ${i}`)
		parentPart.postMessage(i)
	}
	parentPart.close()
})
```

## #33 HTTP Client

- NodeJS juga memiliki standard library untuk HTTP
- Salah satu ditur di module HTTP adalah HTTP Client, dimana kita bisa melakukan simulasi HTTP Request menggunakan NodeJS
- Terdapat 2 jenis module HTTP di NodeJS, HTTP dan HTTPS
- <https://nodejs.org/dist/latest-v16.x/docs/api/http.html>
- <https://nodejs.org/dist/latest-v16.x/docs/api/https.html>

### Kode: HTTP Client

```js
import https from "https"

const url = "https://hookb.in/"
const request = https.request(url, {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json',
		'Accept': 'application/json'
	}
}, (response) => {
	response.addListener('data', (data) => {
		console.info(`Receive : ${data.toString()}`)
	})
})

const body = JSON.stringify({
	firstName: "Eko",
	lastName: "Khannedy"
})

request.write(body)
request.end()
```

## #34 HTTP Server

- Standard Library HTTP juga tidak hanya bisa digunakan untuk membuat HTTP Client, tapi juga bisa digunakan untuk membuat HTTP Server
- Untuk kasus sederhana, cocok sekali jika ingin membuat HTTP Server menggunakan standard library NodeJS, namun untuk kasus yang lebih kompleks, direkomendasikan menggunakan library atau framework yang lebih mudah penggunaanya
- <https://nodejs.org/dist/latest-v16.x/docs/api/http.html>

### Kode: Simple HTTP Server

```js
import http from "http"

const server = http.createServer((request, response) => {
	response.write("Hello World")
	response.end()
})

server.listen(3000)
```

### Kode: Request Respons HTTP Server

```js
import http from "http"

const server = http.createServer((request, response) => {
	if (request.method === "POST") {
		request.addListener("data", (data) => {
			response.setHeader("Content-Type", "application/json")
			response.write(data)
			response.end()
		})
	} else {
		response.write("Hello World")
		response.end()
	}
})

server.listen(3000)
```

## #35 Cluster

- Seperti yang dijelaskan di awal, bahwa NodeJS itu secara default dia berjalan single thread, kecuali jika kita membuat thread manual menggunakan worker thread, tapi tetap dalam satu process
- NodeJS memiliki standard library bernama Cluster, dimana kita bisa menjalankan beberapa process NodeJS secara sekaligus
- Ini sanget cocok ketika kita menggunakan CPU yang multicore, sehingga semua core bis kita utilisasi dengan baik, misal kita jalankan process NodeJS sejumlaj CPU core
- <https://nodejs.org/dist/latest-v16.x/docs/api/cluster.html>

### Cluster Primary dan Worker

- Di dalam Cluster, terdapat 2 jenis aplikasi, Primary dan Worker
- Primary biasanya digunakan sebagai koordinator atau manager untuk para Worker
- Sedangkan Worker sendiri adalah aplikasi uang menjalankan tugas nya

### Kode: Cluster Primary

```js
import cluster from "cluster"
import http from "http"
import os from "os"
import process from "process"

if (cluster.isPrimary) {
	for (let i = 0; i < os.cpus().length; i++) {
		cluster.fork()
	}

	cluster.addListener("exit", (worker) => {
		console.info(`Worker ${worker.id} is exited`)
	})
}
```

### Kode: Cluster Worker

```js
if (cluster.isWorker) {
	const server = http.createServer((request, response) => {
		response.write(`Response from process ${process.pid}`)
		response.end()
		process.exit()
	})

	server.listen(3000)
	console.info(`Start cluster worker ${process.pid}`)
}
```

## #36 Materi Salanjutnya

- NPM (Node Package Manager)
- NodeJS Unit Test
- ExpressJS
- NodeJS Database
- Dan lain-lain