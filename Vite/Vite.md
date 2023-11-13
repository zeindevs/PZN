# Vite

## Sebelum Belajar

- Kelas JavaScript dari Programmer Zaman Now
- Kelas NodeJS dari Programmer Zaman Now
- Kelas TypeScript dari Programmer Zaman Now

## #1 Pengenalan Vite

### Deployment Tool

- Saat kita membuat aplikasi menggunakan NodeJS, baik itu menggunakan JavaScript atau TypeScript
- Maka biasanya banyak hal-hal yang sering kita lakukan secara manual, misal menjalankan aplikasinya, lalu melakukan restart aplikasi ketika sudah melakukan perubahan
- Atau jika menggunakan TypeScript, kita harus melakukan compile terlebih dahulu, lalu menjalankan ulang aplikasinya

## Pengenalan Vite

- Vite (dibaca vit) adalah tool untuk mempercepat proses development aplikasi yang kita buat menggunakan NodeJS
- Vite memiliki fitur HMR (Hot Module Replacement), dimana bisa melakukan perubahan module secara cepat, tanpa kita harus melakukan reload / restart aplikasi yang sedang kita jalankan
- Vite juga memiliki fitur untuk melakukan bundling (membungkus) kode-kode yang kita buat menjadi sebuah kode yang dioptimasi dengan baik sebagai kode yang baik untuk mode production, menggunakan bantuan library Rollup :
- <https://rollupjs.org/>
- Website resmi Vite bisa kita buka di :
- <https://vitejs.dev/>
- Vite saat ini menjadi rekomendasi sebagai Development Tool untuk membuat aplikasi VueJS

## #2 Membuat Project

- Buat folder beajar-vite
- `npm init`
- Ubah type di `package.json` menjadi `module`

### Menginstall Vite Dependency

- `npm install --save-dev vite`

## #3 Menjalankan Aplikasi

- Vite bisa kita gunakan menjalankan aplikasi Web
- Vite menggunakan file index.html sebagai entry point dari aplikasi web yang kita buat
- Untuk menjalankan vite, kita bisa gunakan perintah :
- `npx vite`
- Terdapat banyak opsi yang bisa kita lihat ketika menjalankan Vite, kita bisa gunakan perintah :
- `npx vite --help`

### Kode: Index HTML

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<title>Belajar Vite</title>
	</head>
	<body>
		<script type="module">
			console.info("Hello Vite");
		</script>
	</body>
</html>
```

### Kode: Menjalankan Vite

```bash
npx vite
```

### Tugas

- Lakukan perubahan pada file index.html, dan perhatikan perubahan yang terjadi secara otomatis pada Web Browser tanpa melakukan reload atau restart

## #4 JavaScript

- Secara default, Vite mendukung JavaScript Module, sehingga kita bisa membuat kode JavaScript dengan menggunakan JavaScript Module

### Kode: Say Hello

```js
// say-hello.js
export const sayHello = (name) => {
	console.info(`Hello ${name}!`);
};
```

### Kode: Index HTML

```html
<body>
	<h1>Hello Vite</h1>
	<script type="module">
		import { sayHello } from "./say-hello.js";

		sayHello("Eko");
	</script>
</body>
```

## #5 TypeScript

- Vite juga mendukung kompilasi kode TypeScript secara realtime, sehingga kita bisa membuat menggunakan kode TypeScript, dan tidak perlu khawatir dengan Web Browser
- Karena Vite akan melakukan proses kompilasi kode TypeScript secara otomatis

### Kode: Menambahkan TypeScript

```bash
npm install --save-dev typescript
```

### Kode: Setup TypeScript

```bash
npx tsx --init
```

### Kode: Menambahkan TypeScript

```json
// tsconfig.json
{
	"module": "ES6"
}
```

### Kode: Say Goodbye TypeScript

```ts
// say-goodbye.ts
export const sayGoodbye = (name: string): void => {
	console.info(`Goodbye ${name}!`);
};
```

### Kode: Index HTML

```html
<body>
	<h1>Hello Vite</h1>
	<script type="module">
		import { sayHello } from "./say-hello.js";
		import { sayGoodbye } from "./say-goodbye.ts";

		sayHello("Eko");
		sayGoodbye("Budi");
	</script>
</body>
```

## #6 Build

- Ketika aplikasi kita sudah selesai, dan siap untuk di deploy ke production, maka kita bisa melakukan build menggunakan Vite
- Vite akan melakukan Dependency Pre-Bundling, dimana Vite akan menggabungkan banyak module yang kita gunakan menjadi satu, hal ini akan mempercepat performa load halaman web kita
- Selain itu, Vite bisa mendeteksi module-module yang kita gunakan, dan hanya melakukan build pada module-module yang kita gunakan saja, sehingga ukuran file build akan lebih optimal karena berisikan kode yang dibutuhkan saja

### Perintah Build

- Untuk melakukan build, kita bisa gunakan perintah :
- `npx vite build`
- Untuk melihat opsi apa saja yang bisa kita ubah, kita bisa gunakan perintah :
- `npx vite build --help`
- Secara default, hasil build akan disimpan di folder dist

### Kode: Build

```bash
npx vite build
```

## #7 Node Dependency

- Vite juga bisa secara otomatis melakukan build dari dependency yang kita install menggunakan NodeJS Packages
- Namun perlu diperhatikan, tidak semua NodeJS Package itu compatible dengan Web Browser, jadi kita harus memastikan NodeJS Package yang kita gunakan itu bisa dijalankan di Web Browser
- Contoh jika kita memasukkan dependency ExpressJS, ini mungkin tidak bisa berjalan di Web Browser, karena membutuhkan NodeJS HTTP Server, sedangkan di Web Browser, hal itu tidak ada

### Kode: Menambah Dependency UUID

```bash
npm install uuid

# and

npm install --save-dev @types/uuid
```

### Kode: Index HTML

```html
<body>
	<h1>Hello Vite</h1>
	<script type="module">
		import { sayHello } from "./say-hello.js";
		import { sayGoodbye } from "./say-goodbye.ts";
		import { v4 } from "uuid";

		sayHello("Eko");
		sayGoodbye("Budi");

		console.info(v4());
	</script>
</body>
```

## #8 CSS

- Kode CSS yang terdapat di halaman HTML, secara otomatis juga akan di bundle oleh Vite, dengan begitu kode CSS nya akan menjadi lebih optimal
- Vite juga mendukung CSS `@import`

### Kode: CSS

```css
/*style.css*/
h1 {
	color: red;
}
```

### Kode: Index HTML

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<title>Belajar Vite</title>
		<style>
			@import url("style.css");
		</style>
	</head>
	<body>
		<h1>Hello Vite</h1>
		<script type="module">
			import { sayHello } from "./say-hello.js";
			import { sayGoodbye } from "./say-goodbye.ts";
			import { v4 } from "uuid";

			sayHello("Eko");
			sayGoodbye("Budi");

			console.info(v4());
		</script>
	</body>
</html>
```

## #9 CSS Pre-processors

- Saat ini, banyak sekali CSS Pre-processor framework yang digunakan untuk mempermudah pembuatan kode CSS
- Contohnya SASS, LESS, dan STYLUS
- Vite juga mendukung CSS Pre-processors, sehingga kita tidak perlu melakukan kompilasi secara manual agar menjadi kode CSS yang dimengerti oleh Web Browser

### Kode: Menginstall Sass

```bash
npm install --save-dev sass
```

### Kode: Sass

```sass
<!-- sample.sass -->
h2
	color: red
```

### Kode: Index HTML

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<title>Belajar Vite</title>
		<style>
			@import url("style.css");
			@import url("sample.sass");
		</style>
	</head>
	<body>
		<h1>Hello Vite</h1>
		<h1>Support Sass</h1>
		<script type="module">
			import { sayHello } from "./say-hello.js";
			import { sayGoodbye } from "./say-goodbye.ts";
			import { v4 } from "uuid";

			sayHello("Eko");
			sayGoodbye("Budi");

			console.info(v4());
		</script>
	</body>
</html>
```

## #10 JSON

- Vite juga mendukung bundle file JSON, sehingga kita bisa dengan mudah menyimpan data dalam bentuk JSON, dan mengambil datanya dari JavaScript dengan cara menggunakan JavaScript Module

### Kode: JSON

```json
// person.json
{
	"name": "Eko",
	"web": "www.programmerzamannow.com",
	"instagram": "programmerzamannow"
}
```

### Kode: Index HTML

```html
<body>
	<h1>Hello Vite</h1>
	<h1>Support Sass</h1>
	<script type="module">
		import { sayHello } from "./say-hello.js";
		import { sayGoodbye } from "./say-goodbye.ts";
		import { v4 } from "uuid";
		import * as person from "./person.json";

		console.log(person);
		console.log(person.name);
		console.log(person.web);
		console.log(person.instagram);
	</script>
</body>
```

## #11 Static Assets

- Selain file JavaScript, TypeScript, CSS, JSON, kadang kita juga menggunakan Static Assets ketika membuat Web, seperti Image, Video, File Text, dan lain-lain
- Vite juga secara otomatis akan melakukan bundle ketika di Web kita menggunakan file Static Assets
- Selain itu, kita juga bisa melakukan import untuk Static Assets di JavaScript sebagai Text, pada kasus berupa Text File

### Kode: Image

```html
<body>
	<h1>Hello Vite</h1>
	<h1>Support Sass</h1>
	<img src="img.png" style="height: 100px; width: 100px" alt="Vite Logo" />
	<script type="module">
		import { sayHello } from "./say-hello.js";
		import { sayGoodbye } from "./say-goodbye.ts";
		import { v4 } from "uuid";
		import * as person from "./person.json";

		console.log(person);
		console.log(person.name);
		console.log(person.web);
		console.log(person.instagram);
	</script>
</body>
```

### Kode: Text File

```txt
<!-- hello.txt -->
Hello, this is text file

```

### Kode: Index HTML

```html
<body>
	<h1>Hello Vite</h1>
	<h1>Support Sass</h1>
	<img src="img.png" style="height: 100px; width: 100px" alt="Vite Logo" />
	<script type="module">
		import { sayHello } from "./say-hello.js";
		import { sayGoodbye } from "./say-goodbye.ts";
		import { v4 } from "uuid";
		import * as person from "./person.json";
		import textFile from "./hello.txt?raw";

		console.log(textFile);
		console.log(person);
		console.log(person.name);
		console.log(person.web);
		console.log(person.instagram);
	</script>
</body>
```

### Public Directory

- Kadang, kita juga butuh Static Assets yang ditampilkan secara dinamis, sehingga kemungkinan lokasinya tidak terdapat di halaman Web HTML
- Atau Static Assets yang nama file nya tidak boleh berubah, misal `favicon.ico` atau `s`, dan lain-lain
- Vite mendukung public directory, dimana kita bisa menyimpan semua Static Assets yang tidak akan di optimize pada folder tersebut

## #12 Preview

- Setelah kita melakukan build menggunakan Vite, semua file distribution terdapat di folder dist
- Kadang kita ingin melihat hasilnya terlebih dahulu, sebelum melakukan deployment ke server production
- Kita bisa menggunakan perintah :
- `npx vite preview`
- Ingat ini hanya untuk preview di proses development, jangan lakukan hal ini di server production

## #13 Configuration

- Secara default, Vite sudah memiliki default configuration, sehingga kita tidak perlu melakukan pengaturan apapun
- Namun, jika kita ingin melakukan pengaturan, Vite juga mendukungnya dengan membuat file vite.config.js
- Semua konfigurasi yang bisa kita lakukan di Vite, bisa kita baca disini
- <https://vitejs.dev/config>

### Kode: Configuration

```js
// vite.config.js
import {defineConfig} from "vite"

export default defineConfig {
	build: {
		outDir: "production"
	},
	server: {
		port: 3000
	}
}
```

## #14 Multi-Page Application

- Saat kita membuat halaman web, kadang kita membuat halaman web lebih dari satu halaman HTML
- Vite juga mendukung proses build untuk lebih dari satu halaman HTML
- Namun, khusus untuk Multi-Page Application, kita harus daftarkan file-file mana saja yang akan di build pada Vite Configuration

### Kode: Configuration

```js
// vite.config.js
import {defineConfig} from "vite"

export default defineConfig {
	build: {
		outDir: "production",
		rollupOptions: {
			input: {
				index: "index.html",
				blog: "blog.html",
				contact: "other/contact.html"
			}
		}
	},
	server: {
		port: 3000
	}
}
```

### Kode: Blog HTML

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<title>Blog</title>
	</head>
	<body>
		<h1>Blog</h1>
		<script type="module">
			import { sayHello } from "./say-hello.js";

			sayHello("Blog");
		</script>
	</body>
</html>
```

### Kode: Contact HTML

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<title>Contact</title>
	</head>
	<body>
		<h1>Contact</h1>
		<script type="module">
			import { sayHello } from "./say-hello.js";

			sayHello("Contact");
		</script>
	</body>
</html>
```

## #15 Plugins

- Saat kita membuat web yang hanya membutuhkan HTML, CSS dan JavaScript, Vite sudah sangat baik mendukung pekerjaan kita
- Namun, kadang kita menggunakan framework tambahan seperti VueJS, ReactJS, Svelte, dan lain-lain
- Vite juga memiliki banyak sekali Plugins tambahan untuk mendukung nya
- Kita bisa melihat daftar plugin yang didukung oleh Vite dihalaman ini
- <https://vitejs.dev/plugins/>

## #16 Templates

- Diawal, kita membuat project Vite secara manual. Hal ini saya bahas agar kita tahu bagaimana cara membuat project Vite
- Namun sebenarnya, Vite memiliki template yang bisa kita gunakan untuk membuat project
- Untuk membuat project baru menggunakan template yang sudah disediakan Vite, kita bisa gunakan perintah : npm create vite@latest

## #17 Materi Selanjutnya

- Vitest
- VueJS
