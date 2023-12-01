# NodeJS RESTful API

## Sebelum balajar

- Kelas JavaScript dari Programmer Zaman Now
- Kelas MySQL dari Programmer Zaman Now
- NodeJS ExpressJS
- NodeJS Validation
- NodeJS Database

## #1 Requirement

- Pada kelas ini, kita akan coba membuat RESTful API untuk Contact Management, dimana RESTful API yang akan kita buat membuat fitur sebagai berikut:
- User Management
- Contact Management
- Address Management

## #2 User Management Requirement

### User Data

- Username
- Password
- Name

### User API

- Register User
- Login User
- Update User
- Get User
- Logout User

## #3 Contact Management Requirement

### Contact Data

- First Name
- Last Name
- Email
- Phone

### Contact API

- Create Contact
- Update Contact
- Get Contact
- Search Contact
- Remove Contact

## #4 Address Management Requirement

### Contact Address Data

- Street
- City
- Province
- Country
- Postal Code

### Address API

- Create Address
- Update Address
- Get Address
- List Address
- Remove Address

## #5 Membuat Project

- Buat folder belajar-nodejs-restful-api
- `npm init`
- Buka package.json, dan tambah type module

### Menambah Package Joi

- `npm install joi`
- <https://www.npmjs.com/package/joi>

### Menambah Package ExpressJS

- `npm install express`
- `npm install --save-dev @types/express`
- <https://www.npmjs.com/package/express>

### Menambah Package Prisma

- `npm install --save-dev prisma`
- <https://www.prisma.io>

### Menambah Package Winston

- `npm install winston`
- <https://www.npmjs.com/package/winston>

### Menambah Package BCrypt

- `npm install bcrypt`
- `npm install --save-dev @types/bcrypt`
- <https://www.npmjs.com/package/bcrypt>

### Menambah Package UUID

- `npm install uuid`
- `npm install --save-dev @types/uuid`
- <https://www.npmjs.com/package/uuid>

### Menambah Library Jest untuk Unit Test

- `npm install --save-dev jest @types/jest`
- <https://www.npmjs.com/package/jest>

### Menambah Library Babel

- `npm install --save-dev babel-jest @babel/preset-env`
- <https://babeljs.io/setup#installation>

```json
// package.json
"jest": {
	"transform": {
		"^.+\\.|t|j|sx?$": "babel-jest"
	}
}
```

```json
// babel.config.json
{
	"presets": ["@babel/preset-env"]
}
```

### Menambah Library Supertest

- `npm install --save-dev supertest @types/supertest`
- <https://www.npmjs.com/package/supertest>

## #6 User API Spec

## #7 Contact API Spec

## #8 Address API Spec

## #9 Setup Database

## #10 User Model

## #11 Contact Model

## #12 Address Model

## #13 Setup Project

### Kode: Setup Prisma

```js
export const prismaClient = new PrismaClient();
```

### Kode: Setup Winston

```js
export const logger = winston.createLogger({
	level 'info',
	format: winston.format.json(),
	transports: [
		new winston.transports.Console({})
	]
})
```

### Kode: Setup Express

```js
export const web = express();
we.use(express.json());
```

### Setup Prisma Log

- <https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/logging>

## #14 Register User API

## #15 Login User API

## #16 Get User API

## #17 Update User API

## #18 Logout User API

## #19 Create Contact API

## #20 Get Contact API

## #21 Update Contact API

## #22 Remove Contact API

## #23 Search Contact API

## #24 Create Address API

## #25 Get Address API

## #26 Update Address API

## #27 Remove Address API

## #28 List Address API

## #29 Manual Test

## #30 Materi Selanjutnya

- Belajar Library/Package Lainnya
- NodeJS MongoDB
- Dan lain-lain
