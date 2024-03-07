# TypeScript RESTful API

## Sebelum Belajar

- Kelas JavaScript dari Programmer Zaman Now
- Kelas MySQL dari Programmer Zaman Now
- Kelas NodeJS dari Programmer Zaman Now
- Kelas TypeScript dari Programmer Zaman Now

## #1 Requirement

- Pada kelas ini, kita akan coba membuat RESTful API untuk Contact Management, dimana RESTful API yang akan kita buat memiliki fitur sebagai berikut :
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

### Contact Management Requirement

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

## #3 Address Management Requirement

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

## #4 Membuat Project

- Buat folder `belajar-typescript-restful-api`
- `npm init`

### Menambah Package Zod

- `npm install zod`

### Menambah Package ExpressJS

- `npm install express`
- `npm install --save-dev @types/express`
- <https://www.npmjs.com/package/express>

### Menambah Package Prisma

- `npm install --save-dev prisma`
- <https://www.prisma.io/>

### Menambah Package Winston

- `npm install winston`
- <https://www.npmjs.com/package/winston>

### Menambah Package BCrypt

- `npm install bcrypt`
- `npm install --save-dev @types/bcrypt`
- <https://www.npmjs.com/package/bcrypt>

### Menambah Library Jest untuk Unit Test

- `npm install --save-dev jest @types/jest`
- <https://www.npmjs.com/package/jest>

### Menambah Library Babel

- `npm install --save-dev babel-jest @babel/preset-env`
- <https://babeljs.io/setup#installation>

### Setup TypeScipt Untuk Jest

- `npm install --save-dev @babel/preset-typescript`
- `npm install --save-dev @jest/globals`
- <https://jestjs.io/docs/getting-started#using-typescript>

### Menambah Library Supertest

- `npm install --save-dev supertest @types/supertest`
- <https://www.npmjs.com/package/supertest>

### Menambah TypeScript

- `npm install --save-dev typescript`
- <https://www.npmjs.com/package/typescript>

### Setup TypeScript Project

- `npx tsc --init`
- Semua konfigurasi akan dibuat di file `tsconfig.json`
- Ubah `"module"` menjadi `"commonjs"`
- Ubah `"moduleResolution"` menjadi `"Node"`
- Tambahkan `include src/**/*`
- Ubah outDir menjadi `"./dist"`

## #5 User API Spec

## #6 Contact API Spec

## #7 Address API Spec

## #8 Setup Database

- Buatlah database `belajar_typescript_restful_api`

## Setup Prisma

- `npx prisma init`

## #9 User Model

## #10 Contact Model

## #11 Address Model

## #12 Setup Project

### Kode: Setup Prisma

```ts
export const prismaClient = new PrismaClient();
```

### Kode: Setup Winston

```ts
export const logger = winston.createLogger({
	level: "info",
	format: winston.format.json(),
	transports: [new winston.transports.Console({})],
});
```

### Kode: Setup Express

```ts
export const web = exress();

web.use(express.json());
```

### Setup Prisma Log

- <https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/logging>

## #14 Register User API

## #15 Login User API

### Menambah Package UUID

- `npm install uuid`
- `npm install --save-dev @types/uuid`
- <https://www.npmjs.com/package/uuid>

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

## #29 List Address API

## #30 Distribution File

## #31 Manual Test

## #32 Penutup
