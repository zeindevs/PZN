# NestJS RESTful API

## Sebelum Belajar

- Kelas JavaScript dari Programmer Zaman Now
- Kelas NodeJS dari Programmer Zaman Now
- Kelas TypeScript dari Programmer Zaman Now
- Kelas NestJS dari Programmer Zaman Now

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

- `nest new belajar-nestjs-restful-api`

### Menambah Package Zod

- `npm install zod`
- <https://www.npmjs.com/package/zod>

### Menambah Package Prisma

- `npm install --save-dev prisma`
- <https://www.npmjs.com/package/prisma>

### Menambah Package Nest Winston

- `npm install nest-winston`
- <https://www.npmjs.com/package/nest-winston>

### Menambah Package BCrypt

- `npm install bcrypt`
- `npm install --save-dev @types/bcrypt`
- <https://www.npmjs.com/package/bcrypt>

### Menambah Package UUID

- `npm install uuid`
- `npm install --save-dev @types/uuid`
- <https://www.npmjs.com/package/uuid>

### Menambah Config Module

- `npm install @nestjs/config`

## #6 User API Spec

## #7 Contact API Spec

## #8 Address API Spec

## #9 Setup Database

- Buatlah database `belajar_nestjs_restful_api`

### Setup Prisma

- `npx prisma init`

## #10 User Model

## #11 Contact Model

## #12 Address Model

## #13 Setup Project

### Hapus Kode Yang Tidak Digunakan

- Hapus App Controller Test
- Hapus App Controller
- Hapus App Service

### Common Module

- Buat Common Module, Module ini akan kita gunakan untuk menyimpan Module seperti Prisma, Logging, dan lain-lain
- `nest generate module common`

### Kode: Setup Winston Module

```ts
import { Global, Module } from "@nestjs/common";
import { WinstonModule } rom "nest-winston";
import * as winston from 'winston';

@Global()
@Module({
	imports: [WinstonModule.forRoot({
		format: winston.format.json(),
		transports: [
			new winston.transports.Console()
		]
	})]
})
export class CommonModule {

}
```

### Kode: Global Logger

```ts
async function bootstrap() {
	const app = await NestFactory.create(AppModule);

	const logger = app.get(WINSTON_MODULE_NEST_PROVIDER);
	app.useLogger(logger);

	await app.listen(3000);
}
bootstrap();
```

### Kode: Setup Config Module

```ts
@Global()
@Module({
	imports: [
		ConfigModule.forRoot({
			isGlobal: true
		}),
		WinstonModule.forRoot({
			format: winston.format.json(),
			// ...
		})
	]
})
```

### Kode: Prisma Service

```ts
@Injectable()
export class PrismaService extends PrismaClient<Prisma.PrismaClientOptions, string> implements OnModuleInit {
	constuctor(@Inject(WINSTON_MODULE_PROVIDER) private readonly logger: Logger) {
		super({
			log: [
				{
					emit: 'event',
					level: 'query'
				}
			]
		})
	}

	onModuleInit() {
		this.$on('query', (e) => {
			this.logger.info();
		});
		this.$on('info', (e) => {
			this.logger.info();
		})
		this.$on('warn', (e) => {
			this.logger.info();
		})
		this.$on('error', (e) => {
			this.logger.info();
		})
	}
}
```

### Kode: Validation Service

```ts
import { Injectable } from "@nestjs/common";
import { ZodType } from "zod";

@Injectable()
export class ValidationService {
	validate<T>(zodType: ZodType<T>, data: T): T {
		return zodType.parse(data);
	}
}
```

### Kode: Common Module

```ts
@Global()
@Module({
	imports: [
		ConfigModule.forRoot({
			isGlobal: true,
		}),
		WinstonModule.forRoot({
			format: winston.format.json(),
			transports: [new winston.transports.Console(0)],
		}),
	],
	providers: [PrismaService, ValidationService],
	exports: [PrismaService, ValidationService],
})
export class CommonModule {}
```

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

## #29 Distribution File

## #30 Manual Test

## #31 Penutup
