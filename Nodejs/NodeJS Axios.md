# NodeJS Axios

## Sebelum Belajar

- Kelas JavaScript dari Programmer Zaman Now
- Kelas NodeJS Dasar
- Kelas NodeJS NPM

## #1 Pengenalan Axios

### NodeJS HTTP Module

- Untuk melakukan HTTP Client, biasanya di NodeJS kita bisa menggunakan HTTP Module
- <https://nodejs.org/api/http.html>
- Namun, cara penggunaannya kurang terlalu developer friendly, dan masih menggunakan callback (tidak menggunakan promise)

### Pengenalan Axios

- Axios adalah alternatif lain sebagai library untuk HTTP Client
- Axios bisa berjalan di NodeJS, bahkan bisa digunakan di Web Browser
- API Axios menggunakan promise, sehingga mudah untuk digunakan dibandingkan NodeJS HTTP Module yang masih menggunakan callback
- <https://axios-http.com/>
- <https://github.com/axios/axios>

## #2 Membuat Project

- Buatlah folder dengan nama `belajar-nodejs-axios`
- `npm init`
- Ubah `type` di `package.json` menjadi `module`

### Menambah Package Jest

- `npm install --save-dev jest @types/jest`
- <https://www.npmjs.com/package/jest>

### Menambah Package Axios

- `npm install axios`
- <https://npmjs.com/package/axios>

### Menambah Library Babel

- `npm install --save-dev babel-jest @babel/preset-env`
- <https://babeljs.io/setup#installation>

## #3 HTTP Client

- Hal pertama yang perlu kita lakukan untuk menggunakan Axios adalah membuat HTTP Client
- Untuk membuat HTTP Client, kita bisa menggunakan object Axios di menggunakan create function di package `

### Kode: HTTP Client

```ts
import * as axios from "axios";

describe("HTTP Client", () => {
	it("should be supported by axios", async () => {
		const httpClient = axios.create();
		expect(httpClient).toBeDefined();
	});
});
```

### Request Config

- Saat kita membuat object Axios, kita bisa menggunakan default Request Config
- Default Request Config adalah konfigurasi default yang akan digunakan ketika membuat HTTP Request
- <https://axios-http.com/docs/req_config>

### Kode: Request Config

```ts
import * as axios from "axios";

describe("HTTP Client", () => {
	it("should be supported by axios", async () => {
		const httpClient = axios.create({
			baseURL: "https://eo83131939.x.pipedream.net/",
			timeout: 5000,
		});
		expect(httpClient).toBeDefined();
	});
});
```

## #4 HTTP Method

- Axios adalah library untuk HTTP Client, oleh karena itu pastinya kita gunakan untuk melakukan HTTP Request
- Ketika melakukan HTTP Request, kita pasti akan menentukan HTTP Method yang akan kita gunakan
- Axios menggunakan nama method sebagai HTTP Method nya
- Kita bisa menggunakan nama-nama method sesuai dengan jenis HTTP Method dalam `lowercase` pada object Axios
- <https://axios-http.com/docs/instance>

### Kode: HTTP Get

```ts
describe("HTTP Client", () => {
	const httpClient = axios.create({
		baseURL: "https://eni83131939.x.pipedream.net/",
		timeout: 5000,
	});
	it("should support GET Method", async () => {
		const response = await httpClient.get("/");
		expect(response.status).toBe(200);
	});
});
```

## #5 HTTP Request

- Seperti yang dijelaskan di materi sebelumnya, untuk melakukan HTTP Request, kita bisa gunakan nama method yang sesuai dengan jenis HTTP Method yang ingin kita gunakan
- Selain itu, kita bisa menggunakan `Request Config` pada parameter di method yang kita gunakan
- <https://axios-http.com/docs/instance>
- <https://axios-http.com/docs/req_config>

### Kode: HTTP Request

```ts
it("should support GET Method", async () => {
	const response = await httpClient.get("/", {
		params: {
			name: "Eko",
		},
		headers: {
			Accept: "application/json",
		},
	});
	expect(response.status).toBe(200);
});
```

## #6 HTTP Response

- Setiap HTTP Request yang kita lakukan, maka akan mengembalikan `Promise<Response>`
- Response merupakan representasi dari HTTP Response
- <https://axios-http.com/docs/res_schema>

### Kode: HTTP Response

```ts
it("should support GET Method", async () => {
	const response = await httpClient.get("/", {
		params: {
			name: "Eko",
		},
		headers: {
			Accept: "application/json",
		},
	});
	expect(response.status).toBe(200);
	expect(response.statusText).toBe("OK");
	expect(response.data.success).toBe(true);
});
```

## #7 Request Body

- Beberapa jenis HTTP Method seperti `POST`, `PUT` dan `PATCH` bisa memiliki Request Body
- Untuk informasi Request Body yang kita kirim, kita bisa kirim dalam parameter data di object Axios
- <https://axios-http.com/docs/instance>

### Kode: JSON Request Body

```ts
it("should support PORT method with JSON request body", async () => {
	const json = {
		username: "khannedy",
		password: "rahasia",
	};
	const response = await httpClient.post("/", json, {
		headers: {
			"Content-Type": "application/json",
			Accept: "application/json",
		},
	});
	expect(response.status).toBe(200);
});
```

### Kode: Text Request Body

```ts
it("should support PORT method with TEXT request body", async () => {
	const text = "Eko Kurniawan khannedy";
	const response = await httpClient.post("/", text, {
		headers: {
			"Content-Type": "text/plain",
			Accept: "application/json",
		},
	});
	expect(response.status).toBe(200);
});
```

### Kode: Form Request Body

```ts
it("should support PORT method with FORM request body", async () => {
	const form = {
		username: "khannedy",
		password: "rahasia",
	};
	const response = await httpClient.post("/", form, {
		headers: {
			"Content-Type": "application/x-www-form-urlencoded",
		},
	});
	expect(response.status).toBe(200);
});
```

### Kode: Multipart Request Body

```ts
it("should support PORT method with Multipart request body", async () => {
	const form = new FormData();
	form.append("username", "khannedy");
	form.append("password", "rahasia");

	const data = fs.readFileSync("image.png");
	form.append("file", new Blob(data), "image.png");

	const response = await httpClient.post("/", form, {
		headers: {
			"Content-Type": "application/form-data",
		},
	});
	expect(response.status).toBe(200);
});
```

## #8 Request Interceptor

- Kadang, kita ingin melakukan sesuatu sebelum HTTP Request di kirim ke Server
- Axios memiliki fitur bernama Request Interceptor
- Ini adalah fitur yang bisa kita gunakan untuk melakukan sesuatu sebelum HTTP Request dikirim
- Kita bisa tentukan sebelum request dikirim, bahkan ketika error terjadi tapi request belum dikirim

### Kode: Request Interceptor

```ts
httpClient.interceptors.request.use(
	async (config) => {
		console.info(`Send request to ${config.baseURL}${config.url}`);
		return config;
	},
	async (error) => {
		console.error(`Request error: ${error.message}`);
		return Promise.reject(error);
	},
	{
		synchronous: false,
	},
);
```

## #9 Response Interceptor

- Selain Request Interceptor, kita juga bisa tambahkan Response Interceptor
- Cara kerjanya sama, bedanya Interceptor ini digunakan setelah menerima HTTP Response

### Kode: Response Interceptor

```ts
httpClient.interceptors.response.use(
	async (response) => {
		const fullUrl = (response.config.baseURL = response.config.url);
		console.info(
			`Receive response from ${fullUrl} with body ${JSON.stringify(response.data)}`,
		);
		return response;
	},
	async (error) => {
		console.error(`Response error: ${error.message}`);
		return Promise.reject(error);
	},
	{
		synchronous: false,
	},
);
```

## #10 Error Handler

- Secara default, ketika terjadi error di HTTP Request atau HTTP Response, Axios akan mengembalikan Promise Reject, oleh karena itu untuk menangani `Error` di Axios, cukup mudah
- Bisa menggunakan catch di Promise, atau gunakan `try catch` jika menggunakan async await
- Secara default, jika terjadi error, Axios akan mengembalikan object error `AxiosError`
- <https://github.com/axios/axios/blob/v1.x/index.d.ts#L399>

### Kode: Axios Error

```ts
describe("Error Handler", () => {
	const httpClient = axios.create({
		baseURL: "https:/www.programmerzamannow.com",
		timeout: 5000,
	});

	it("should error it 404 not found", async () => {
		try {
			await httpClient.get("/not-found");
		} catch (error) {
			console.error(error);
			expect(error.response.status).toBe(404);
		}
	});
});
```

### Menentukan Error Status Code

- Secara default, jika HTTP Response status code bukan `2xx`, maka Axios akan menganggap itu error
- Jika kita ingin mengubah penentuan status code mana yang dianggap error, maka kita perlu menambahkan validateStatus di `RequestConfig`

### Kode: Validate Status

```ts
describe("Error Handler", () => {
	const httpClient = axios.create({
		baseURL: "https:/www.programmerzamannow.com",
		timeout: 5000,
		validateStatus: (status) => {
			return status < 500;
		},
	});

	it("should error it 404 not found", async () => {
		const response = await httpClient.get("/not-found");
		expect(response.status).toBe(404);
	});
});
```

## #11 Penutup
