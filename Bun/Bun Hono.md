# Bun Hono

## #1 Sebelum Belajar

### Sebelum Belajar

- Kelas JavaScript
- Kelas NodeJS
- Kelas Bun Dasar

## #2 Pengenalan Hono

### Pengenalan Hono

- Hono adalah salah satu library yang bisa kita gunakan untuk mempermudah membuat web di Bun
- Hono adalah alternatif lain dari ExpressJS yang bisa kita gunakan di Bun
- Performa Hono sangat baik, bahkan lebih baik dibandingkan ExpressJS
- Hono menggunakan Web Standard, sehingga jika kita sudah terbiasa menggunakan class seperti `Request`, `Response`, URL, dan lain-lain di JavaScript, maka akan sangat mudah menggunakan Hono
- <https://hono.dev/>

### Kenapa Hono?

- Hono merupakan Web Framework yang saat ini paling populer di Bun
- Hono menggunakan standard library JavaScript, sehingga mempermudah ketika membuat program menggunakan Hono

### Dokumentasi Resmi

- Dokumentasi resmi untuk Hono sangat baik
- Kita bisa belajar langsung dari dokumentasinya
- Oleh karena itu, di kelas ini, kita akan belajar menggunakan dokumentasi resmi Hono
- Kita akan gunakan dokumentasi resminya sebagai referensi, dan langsung kita praktekan
- <https://hono.dev/docs/>

## #3 Membuat Project

### Membuat Project

- Terdapat dua cara membuat project Hono
- Pertama, membuat project baru, dengan menggunakan perintah :
- bun create hono belajar-bun-hono
- Kedua, menambahkan ke project bun yang sudah kita buat, menggunakan perintah :
- `bun add hono`
- <https://hono.dev/docs/getting-started/bun>

## #4 App

- Hono adalah object utama untuk membuat http server menggunakan Hono
- Atau, biasa kita sebut sebagai App
- Object Hono ini memiliki banyak sekali method yang bisa kita gunakan. Kita akan bahas secara bertahap di materi-materi selanjutnya
- <https://hono.dev/docs/api/hono>

### Bun HTTP Server

- Hono kompatibel dengan Bun HTTP Server
- Oleh karena itu, untuk menjalankan Hono Server, kita hanya perlu menggunakan perintah :
- `bun run nama-file.ts`
- Atau di dalam `package.json`, sudah disediakan script untuk run aplikasi Hono nya

## #5 Routing

### Routing

- Hono menyediakan method dengan nama sesuai dengan HTTP Method, seperti `get`, `post`, `put`, dan lain-lain
- Method ini digunakan untuk membuat routing
- <https://hono.dev/docs/api/routing>

## #6 Context

- Saat kita menambah route di Hono, terdapat parameter `Context` yang bisa kita gunakan untuk mengelola Request dan Response untuk Web
- <https://hono.dev/docs/api/context>

## #7 Request

### Request

- Di dalam Context, terdapat property bernama `req`
- Property req merupakan representasi dari Object Request
- <https://hono.dev/docs/api/request>

## #8 Response

### Response

- Context banyak sekali memiliki method yang bisa kita gunakan untuk mengubah HTTP Response
- <https://hono.dev/docs/api/context>
- <https://developer.mozilla.org/en-US/docs/Web/API/Response>

## #9 Exception

### Exception

- Hono menyediakan class `Error` yang bisa kita gunakan untuk secara otomatis menjadi HTTP Response
- Kita bisa menggunakan class `HTTPException`
- <https://hono.dev/docs/api/exception>

### Error Handler

- Hono juga memiliki fitur untuk menangkap Error yang terjadi, misal saja terjadi kondisi dimana kode yang kita buat Error, sehingga kita ingin ubah menjadi HTTP Response yang kita inginkan
- <https://hono.dev/docs/api/exception#handling-httpexception >

## #10 Middleware

### Membuat Middleware

- Untuk membuat Middleware, kita cukup membuat Handler seperti pada Routing, namun dengan tambahan parameter `next`
- Jika kita ingin melanjutkan Request ke Handler, kita bisa panggil function `next()`
- <https://hono.dev/docs/guides/middleware>

## #11 Built-in Middleware

### Built-in Middleware

- Hono sudah menyediakan banyak sekali Middleware (Built-in)
- Kita bisa pelajari Middleware yang sudah ada, dan bisa gunakan jika sesuai kebutuhan kita
- <https://hono.dev/docs/middleware/builtin/basic-auth>

## #12 Helper

### Helper

- Hono menyediakan function-function untuk membantu mempermudah ketika membuat web
- Ada banyak sekali function-function helper yang bisa kita gunakan
- <https://hono.dev/docs/guides/helpers>

## #13 JSX

### JSX

- Hono mendukung pembuatan kode HTML menggunakan JSX (JavaScript XML), dimana kita bisa menulis menggunakan kode html di dalam JavaScript
- Untuk membuat kode JSX, jika menggunakan JavaScript, maka kita akan buat dalam file `.jsx`, sedangkan jika menggunakan TypeScript, maka kita akan buat dalam file `.tsx`
- <https://legacy.reactjs.org/docs/introducing-jsx.html>
- <https://hono.dev/docs/guides/jsx>

## #14 Testing

### Testing

- Kita tahu bahwa Bun sudah memiliki unit testing sendiri mirip seperti `Jest`
- Untungnya, Hono juga menyediakan fitur untuk `unit test`, sehingga memudahkan kita ketika ingin membuat testing aplikasi Hono yang akan kita buat
- <https://hono.dev/docs/guides/testing>

## #15 Validation

### Validation

- Hono menyediakan fitur untuk melakukan validasi, baik itu secara manual, atau integrasi dengan library validation seperti `Zod`
- Namun sebenarnya sangat disarankan untuk menggunakan validation library seperti `Zod`, dibandingkan melakukan validasi secara manual
- <https://hono.dev/docs/guides/validation>

## #16 Best Practice

### Best Practice

- Salah satu kebiasaan kita saat membuat web adalah mengumpulkan beberapa route dalam sebuah controller
- Di Hono, hal itu tidak disarankan, karena akan banyak sekali kode TypeScript Generic yang harus dibuat
- Lantas bagaimana cara menangani jika memang route kita sangat banyak?
- Hono menyediakan best practice dengan cara membuat beberapa object Hono App
- <https://hono.dev/docs/guides/best-practices>

## #17 Static File

### Static File

- Kadang-kadang, kita ingin membuat route yang hanya menampilkan file static yang sudah ada, misal file `gambar`, `html`, `css`, `js`, dan lain-lain
- Kita tidak perlu manual membuat tiap route untuk tiap file, Hono sudah menyediakan fitur tersebut
- <https://hono.dev/docs/getting-started/bun#serve-static-files>

## #18 Materi Selanjutnya

- Membuat Restful API menggunakan Bun Hono
