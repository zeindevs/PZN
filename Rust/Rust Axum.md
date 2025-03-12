# Rust Axum

## Sebelum Belajar

- Rust Dasar
- Rust Unit Test
- Rust Concurrency
- Rust Validation
- Rust Template

## #1 Pengenalan

### Axum

- Axum adalah salah satu library untuk web di Rust yang populer
- Axum terintegrasi baik dengan library Tokio, sehingga jika kita sudah terbiasa menggunakan Tokio, kita bisa menggunakan Axum dengan mudah
- Axum merupakan library minimalis, sehingga sangat mudah menggunakannya
- Axum menggunakan ekosistem library yang sudah ada, seperti Tokio, Tower dan Hyper, sehingga memudahkan ketika menggunakan library-library yang sudah terintegrasi dengan ekosistem tersebut dengan baik
- <https://github.com/tokio-rs/axum>
- <https://crates.io/crates/axum>

## #2 Membuat Project

### Membuat Project

- `cargo new belajar-rust-axum`

### Menambah Library

- `cargo add tokio --features full`
- `cargo add tower`
- `cargo add serde --features derive`
- `cargo add http`
- `cargo add axum`
- `cargo add axum-extra`
- `cargo add axum-test`
- `cargo add anyhow`

## #3 Setup

### Setup

- Untuk menggunakan Axum, pertama kita perlu membuat aplikasi Axum dalam bentuk Router
- <https://docs.rs/axum/latest/axum/struct.Router.html>
- Selanjutnya, kita perlu membuat Listener Axum untuk menentukan ip dan port web akan dijalankan
- <https://docs.rs/axum/latest/axum/serve/trait.Listener.html>
- Selanjutnya, kita bisa menjalankan aplikasi Axum menggunakan method serve
- <https://docs.rs/axum/latest/axum/serve/fn.serve.html>

### Kode: Setup

```rust
#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(|| async { "Hello, World!" }));
    let listener = TcpListener::bind("0.0.0.0:3000").await.unwrap();
    serve(listener, app).await.unwrap();
}
```

## #4 Test

### Test

- Saat kita membuat web menggunakan Axum, kadang agak menyulitkan jika kita harus melakukan test secara manual
- Untungnya, ada library untuk integration test Axum, sehingga kita tidak perlu menjalankan web Axum secara manual lagi
- <https://crates.io/crates/axum_test>

### Menggunakan Axum Test

- Untuk menggunakan Axum Test sangat mudah, kita bisa membuat `TestServer` dari Router yang sudah kita buat
- <https://docs.rs/axum-test/latest/axum_test/struct.TestServer.html>
- Selanjutnya kita bisa mengirim data berupa `TestRequest`, dan hasilnya berupa `TestResponse`
- <https://docs.rs/axum-test/latest/axum_test/struct.TestRequest.html>
- <https://docs.rs/axum-test/latest/axum_test/struct.TestResponse.html>

### Kode: Test

```rust
#[tokio::main]
async fn test_main() {
    let app = Router::new()
        .route("/", get(|| async { "Hello, World!" }));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/").await;

    response.assert_status_ok();
    response.assert_text("Hello, World!");
}
```

## #5 Router

### Router

- Struct Router merupakan objek utama dalam Axum yang digunakan untuk memetakan antara path dan kode yang dieksekusi ketika path tersebut diakses
- Untuk menambahkan pemetaan baru, kita bisa menggunakan method `route(path, method_router)`
- <https://docs.rs/axum/latest/axum/struct.Router.html#method.route>

### Path

- Path di Router merupakan tipe data &str yang berisikan lokasi Path di URL, misal contohnya :
  - `/`
  - `/path`
  - `/path/nested/path`
- Pembuatan path harus dipastikan unik, jika ada yang duplikat, maka akan terjadi error

### Method Router

- Method di Router merupakan implementasi dari Struct `MethodRouter`
- <https://docs.rs/axum/latest/axum/routing/method_routing/struct.MethodRouter.html>
- Untuk membuat `MethodRouter`, kita bisa menggunakan function helper yang sudah disediakan untuk mempermudah, yang berada di module `axum::routing`
- Nama-nama function disesuaikan dengan nama HTTP Method
- <https://docs.rs/axum/latest/axum/routing/method_routing/index.html>

### Kode: Routing

```rust
#[tokio::main]
async fn test_method_routing() {
    async fn hello_world() -> String {
        "Hello, hello_world!".to_string();
    }

    let app = Router::new()
        .route("/get", get(hello_world))
        .route("/post", post(hello_world));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Hello, World!");

    let response = server.post("/post").await;
    response.assert_status_ok();
    response.assert_text("Hello, World!");
}
```

## #6 Request

### Request

- Saat kita membuat routing (pemetaan path dan kode), kadang kita ingin mendapatkan informasi Request yang dikirim oleh pengguna
- Data Request di Axum direpresentasikan dalam bentuk `axum::extract::Request`, dimana ini sebenarnya adalah alias untuk `http::Request`
- <https://docs.rs/axum/latest/axum/extract/type.Request.html>
- <https://docs.rs/http/latest/http/request/struct.Request.html>
- Dari objek Request, kita bisa mendapatkan seluruh informasi HTTP Request yang kita butuhkan

### Kode: Request

```rust
#[tokio::main]
async fn test_method_routing() {
    async fn hello_world(request: Request) -> String {
        format!("Hello {}", request.method());
    }

    let app = Router::new()
        .route("/get", get(hello_world))
        .route("/post", post(hello_world));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Hello GET");
}
```

## #7 Extractor

### Extractor

- Walaupun mendapatkan informasi HTTP Request dari objek Request sudah lengkap, namun, kadang banyak hal yang harus kita lakukan secara manual untuk mendapatkan informasinya
- Misal jika ingin mendapatkan informasi `Query` `Param`, `Header`,` Cookie`, `Body`, dan lain-lain
- Axum menyediakan fitur bernama` Extractor`, yaitu fitur yang digunakan untuk mempermudah mengambil informasi dari `Request`
- Extractor merupakan implementasi dari Trait `FromRequest` dan `FromRequestParts`
- <https://docs.rs/axum/latest/axum/extract/trait.FromRequest.html>
- <https://docs.rs/axum/latest/axum/extract/trait.FromRequestParts.html>

### Http Extractor

- Axum menggunakan standard library http, sehingga kita bisa dengan mudah menggunakan banyak sekali ekosistem library `http` yang sudah banyak diadopsi di komunitas Rust
- Salah satunya kita bisa gunakan `Extractor` untuk data-data di Http Library
- <https://crates.io/crates/http>
- Sebelumnya, kita sudah menggunakan Request pada routing function yang kita buat, selain itu, kita juga bisa gunakan banyak sekali http object yang tersedia, seperti `Uri` dan `Method`

### Kode: Http Extractor

```rust
#[tokio::main]
async fn test_uri() {
    async fn route(uri: Uri, method: Method) -> String {
        format!("Hello {} {}", method.as_str(), uri.path());
    }

    let app = Router::new().route("/uri", get(hello_world))

    let server = TestServer::new(app).unwrap();
    let response = server.get("/uri").await;
    response.assert_status_ok();
    response.assert_text("Hello GET /uri");
}
```

## #8 Common Extractor

### Common Extractor

- Ada banyak sekali extractor yang disediakan di Axum selain Http Extractor, kita bisa melihat semua daftar extractor yang tersedia pada module `axum::extrac`
- <https://docs.rs/axum/latest/axum/extract/index.html>

### Query Extractor

- Struct Query merupakan extractor yang bisa kita gunakan untuk mendapatkan informasi Query Parameter
- <https://docs.rs/axum/latest/axum/extract/struct.Query.html>

### Kode: Query Extractor

```rust
#[tokio::main]
async fn test_query() {
    async fn route(Query(params): Query<HashMap<String, String>>) -> String {
        format!("Hello {}", params.get("name").unwrap());
    }

    let app = Router::new()
        .route("/query", get(route));

    let server = TestServer::new(app).unwrap();
    let respnse = server.get("/query").add_query_param("name", "Eko").await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
}
```

### Header Extractor

- Struct `HeaderMap` bisa digunakan untuk mendapatkan informasi Request Header
- <https://docs.rs/http/latest/http/header/struct.HeaderMap.html>

### Kode: Header Extractor

```rust
#[tokio::main]
async fn test_headers() {
    async fn route(headers: HeaderMap) -> String {
        format!("Hello {}", headers["name"].to_str().unwrap());
    }

    let app = Router::new()
        .route("/headers", get(route));

    let server = TestServer::new(app).unwrap();
    let respnse = server.get("/headers").add_header("name", "Eko").await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
}
```

## #9 Path Parameter Extractor

### Path Parameter

- Saat kita membuat Path dalam Routing, kadang ada kasus kita butuh Path yang sifatnya dinamis, contoh misal /products/123, dimana 123 adalah id product, yang artinya nilainya bisa berubah-ubah
- Ini kita sebut dengan nama Path Parameter. Kita bisa membuat Path Parameter dengan menggunakan tanda { diikuti dengan nama parameter dan ditutup dengan `}`
- Misal `/products/{id}`, artinya id adalah Path Parameter
- Path Parameter bisa lebih dari satu, misal `/products/{id}/categories/{id_category}`

### Path Parameter Extractor

- Untuk mendapatkan nilai pada Path Parameter tersebut, kita bisa menggungkan Path Parameter Extractor yaitu Struct `Path`
- <https://docs.rs/axum/latest/axum/extract/struct.Path.html>
- Saat menggunakan Struct Path, kita perlu membuat nama attribute sesuai dengan nama Path Parameter nya

### Kode: Path Extractor

```rust
#[tokio::main]
async fn test_path_parameter() {
    async fn route(Path((id, id_category)): Path<String, String>) -> String {
        format!("Product {}, Category {}", id, id_category);
    }

    let app = Router::new().route("/products/{id}/categories/{id_category}", get(route));

    let server = TestServer::new(app).unwrap();
    let respnse = server.get("/products/1/categories/2").await;
    response.assert_status_ok();
    response.assert_text("Product 1, Category 2");
}
```

## #10 Body Extractor

### Body Extractor

- Saat membuat Web, kita sering mengirim data melalui HTTP Body
- Axum menyediakan banyak Extractor untuk HTTP Body, sehingga kita bisa mudah ketika akan mengambil data di HTTP Body

### String Body Extractor

- Axum menyediakan Extractor untuk mengambil data di Request Body dalam tipe data String
- Kita bisa menggunakan tipe `String` pada routing function, yang secara otomatis data String tersebut akan diambil dari Request Body

### Kode: String Body Extractor

```rust
#[tokio::main]
async fn test_body_string() {
    async fn route(body: String) -> String {
        format!("Body {}", body);
    }

    let app = Router::new().route("/post", post(route));

    let server = TestServer::new(app).unwrap();
    let respnse = server.post("/post").text("This is body").await;
    response.assert_status_ok();
    response.assert_text("Body This is body");
}
```

### Json Body Extractor

- Selain tipe data String, kadang kita ingin request body dalam bentuk JSON. Jika menggunakan String, kita harus lakukan konversi manual dari String JSON menjadi tipe Object yang kita mau
- Axum menyediakan Extractor untuk Request Body tipe JSON, yaitu dengan menggunakan Struct `Json`, sehingga kita bisa konversi otomatis menjadi tipe yang kita inginkan
- Fitur ini menggunakan Serde untuk melakukan konversi tipe data JSON ke tipe object yang kita mau
- <https://docs.rs/axum/latest/axum/struct.Json.html>

### Kode: Login Struct

```rust
#[derive(Debug, Serialize, Deserialize)]
struct LoginRequest {
    username: String,
    password: String,
}
```

### Kode: Json Extractor

```rust
#[tokio::main]
async fn test_body_json() {
    async fn route(Json(request): Json<LoginRequest>) -> String {
        format!("Hello {}", request.username);
    }

    let app = Router::new().route("/post", post(route));

    let request = LoginRequest {
        username: "Eko".to_string(),
        password: "rahasia".to_string(),
    };
    let server = TestServer::new(app).unwrap();
    let response = server.post("/post").json(&request).await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
}
```

### Json Error

- Bagaimana jika format Json yang dikirim di Request Body bukan format yang benar?
- Kita bisa bungkus request body dalam bentuk `Result<Json<T>, JsonRejection>`
- <https://docs.rs/axum/latest/axum/extract/rejection/enum.JsonRejection.html>

### Kode: Json Rejection

```rust
#[tokio::main]
async fn test_body_json_error() {
    async fn route(payload: Result<Json<LoginRequest>, JsonRejection>) -> String {
        match payload {
            Ok(request) => {
                format!("Hello {}", request.username);
            }
            Err(error) => {
                format!("Error {:?}", error);
            }
        }
    }
}
```

## #11 Response

### Response

- Sebelumnya, kita hanya mengembalikan data String pada routing function yang kita buat
- Tipe data apapun yang merupakan implementasi dari `IntoResponse` bisa digunakan sebagai return value di routing function
- <https://docs.rs/axum/latest/axum/response/trait.IntoResponse.html>
- Dimana ada banyak sekali `IntoResponse` yang sudah diimplementasikan oleh Axum
- <https://docs.rs/axum/latest/axum/response/index.html>

### Kode: Response

```rust
#[tokio::main]
async fn test_response() {
    async fn route(request: Request) -> Response {
        Response::builder()
            .status(StatusCode::OK)
            .header("X-Owner", "Eko")
            .body(Body::from(format!("Hello {}", request.method())))
            .unwrap();
    }

    let app = Router::new().route("/get", get(route));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/").await;
    response.assert_status_ok();
    response.assert_text("Hello GET");
    response.assert_header("X-Owner", "Eko");
}
```

### Json Response

- Struct `Json`, selain bisa digunakan sebagai Request Body, bisa juga digunakan sebagai Response
- Struct Json akan secara otomatis mengubah object menjadi format Json menggunakan Serde
- Hal ini mempermudah kita tanpa harus melakukan konversi ke format Json secara manual lagi
- Kita bisa langsung gunakan return `Json<T>`

### Kode: Json Response

```rust
#[tokio::main]
async fn test_response_json() {
    async fn route() -> Json<LoginResponse> {
        Json(LoginResponse {
            token: "TOKEN".to_string(),
        });
    }

    let app = Router::new().route("/get", get(route));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/").await;
    response.assert_status_ok();
    response.assert_text_contains("TOKEN");
}
```

### Tuple IntoResponse

- Jika kita perhatikan, implementasi IntoResponse juga banyak sekali dilakukan di tuple
- <https://docs.rs/axum/latest/axum/response/trait.IntoResponse.html>
- <https://docs.rs/axum/latest/axum/response/trait.IntoResponseParts.html>

### Kode: Tuple IntoResponse

```rust
#[tokio::main]
async fn test_response_tuple() {
    async fn route() -> (Response<()>, Json<LoginResponse>) {
        (

            Response::builder()
                .status(StatusCode::OK)
                .header("X-Owner", "Eko")
                .body(()).unwrap(),
            Json(LoginResponse {
                token: "TOKEN".to_string(),
            }),
        )
    }

    let app = Router::new().route("/get", get(route));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/").await;
    response.assert_status_ok();
    response.assert_text_contains("TOKEN");
    response.assert_header("X-Owner", "Eko");
}
```

## #12 Form Request

### Form Request

- Saat kita membuat Web, kadang kita membuat aksi Form Request
- Axum juga menyediakan Extractor untuk Form Request, dimana kita bisa menyimpan hasil Request Body Form tersebut ke dalam object Struct yang kita perlukan
- Kita bisa menggunakan Struct `Form`
- <https://docs.rs/axum/latest/axum/struct.Form.html>
- Axum menggunakan `Serialize` untuk mengkonversi Request Body Form menjadi object Struct yang kita inginkan

### Kode: Form Request

```rust
#[tokio::main]
async fn test_form_request() {
    async fn route(Form(form): Form<LoginRequest>) -> String {
        format!("Hello {}", from.username);
    }

    let app = Router::new().route("/post", post(route));

    let server = TestServer::new(app).unwrap();
    let response = server.post("/post").form(&LoginRequest{
        username: "Eko".to_string(),
        password: "rahasia".to_string(),
    }).await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
}
```

## #13 Multipart Request

### Multipart Request

- Selain Form, Axum juga menyediakan Extractor untuk menangani Multipart Request menggunakan struct `Multipart`
- <https://docs.rs/axum/latest/axum/extract/struct.Multipart.html>
- Karena Multipart Request memerlukan membaca seluruh data Request Body, maka untuk Multipart Request, kita perlu tambahkan sebagai parameter di bagian paling akhir jika menggunakan lebih dari satu Extractor
- Multipart di Axum memerlukan features multipart, kita bisa tambahkan `--features` multipart, ketika menambah library axum

### Kode: Multipart Request

```rust
#[tokio::main]
async fn test_multipart_request() {
    async fn route(mut payload: Mulipart) -> String {
        let mut profile: Bytes = Bytes::new();
        let mut username: String = "".to_string();
        while let Some(field) = payload.next_field().await.unwrap() {
            if field.name().unwrap_or("") == "profile" {
                profile = field.bytes().await.unwrap();
            } else if field.name().unwrap_or("") == "username" {
                username = field.text().await.unwrap();
            }
        }
        assert!(profile.len() > 0) // make sure profile is not empty
        format!("Hello {}", username)
    }

    let app = Router::new().route("/post", post(route));

    let request = MulipartForm::new()
        .add_text("username", "Eko")
        .add_text("password", "rahasia")
        .add_text("profile", Part::bytes(Bytes::from("profile")));
    let server = TestServer::new(app).unwrap();
    let resposne = server.post("/post").multipart(request).await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
}
```

## #14 Cookie

### Cookie

- Saat kita membuat Web, kadang kita butuh membuat Cookie
- Sebenarnya cara membuat Cookie sendiri kita bisa lakukan secara manual menggunakan Response Header `Set-Cookie`
- Namun, kita bisa gunakan library axum-extra untuk membantu melakukan manajemen Cookie secara mudah
- <https://crates.io/crates/axum-extra>

### Menambah Axum Extra

- `cargo add axum-extra --features cookie`

### Cookie jar

- Untuk menambah fitur Cookie, kita bisa menggunakan Struct `CookieJar`
- <https://docs.rs/axum-extra/latest/axum_extra/extract/cookie/struct.CookieJar.html>
- CookieJar secara otomatis akan mengambil Cookie yang ada di Request, karena CookieJar mengimplementasikan `FromRequest`
- Jika kita ingin membuat Cookie, kita bisa kembalikan CookieJar dalam Response, karena CookieJar juga mengimplementasikan `IntoResponse`

### Kode: Cookie Response

```rust
#[tokio::main]
async fn test_cookie_response() {
    async fn route(query: Query<HashMap<String, String>>) -> (CookieJar, String) {
        let name = query.get("name").unwrap();
        (
            CookieJar::new().add(Cookie::new("name", name.clone()));
            format!("Hello {}", name.clone());
        )
    }

    let app = Router::new().route("/query", get(route));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/query").add_query_param("name", "eko").await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
    response.assert_contains_header("Set-Cookie");
    response.assert_header("Set-Cookie", "name=Eko");
}
```

### Kode: Cookie Request

```rust
#[tokio::main]
async fn test_cookie_request() {
    async fn route(cookies: CookieJar) -> String {
        format!("Hello {}", cookies.get("name").unwrap().value());
    }

    let app = Router::new().route("/cookies", get(route));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/cookies").add_header("Cookie", "name=Eko").await;
    response.assert_status_ok();
    response.assert_text("Hello Eko");
}
```

## #15 Middleware

### Middleware

- Middleware adalah fitur dimana kita bisa melakukan sesuatu sebelum routing function di eksekusi
- Banyak yang menggunakan Middleware untuk melakukan manipulasi request sebelum dikirim ke routing function atau manipulasi response sebelum dikembalikan ke pengguna
- Axum tidak memiliki fitur Middleware sendiri, Axum menggunakan library Tower untuk menambah fitur Middleware. Keuntungannya adalah ekosistem Tower bisa digunakan di Axum
- <https://crates.io/crates/tower-http>

### Axum Middleware

- Untuk menambahkan Middleware di Axum nya, kita bisa gunakan di level Router menggunakan `Router::layer` / `Router::route_layer`
- Jika kita tambahkan di Router, artinya semua routing akan menggunakan Middleware tersebut
- Atau kita bisa tambahkan di `MethodRouter::layer` / `MethodRouter::route_layer`
- Jika kita tambahkan di `MethodRouter`, artinya hanya routing tersebut yang akan menggunakan Middleware tersebut

### Membuat Middleware

- Untuk membuat Middleware, kita bisa membuat object dari Layer
- <https://docs.rs/tower/latest/tower/trait.Layer.html>
- Namun, karena terlalu manual, untungnya Axum menyediakan helper untuk membuat Middleware menjadi lebih mudah
- Axum menyediakan banyak function yang bisa kita gunakan untuk membuat Middleware
- <https://docs.rs/axum/latest/axum/middleware/index.html#functions>

### Kode: Log Middleware

```rust
async fn log_middleware(request: Request, next: Next) -> Response {
    println!("Receive request {} {}", request.method(), request.uri());
    let response = next.run(request).await;
    println!("Send respnse {}", response.status());
    response
}
```

### Kode: Request ID Middleware

```rust
async fn request_id_middleware<T>(mut request: Request<T>) -> Request<T> {
    let request_id = "random-id"; // create random id, example from uuid
    request
        .headers_mut()
        .insert("X-Request-Id", request_id.parse().unwrap());
    request
}
```

### Kode: Menggunakan Middleware

```rust
#[tokio::main]
async fn test_middleware() {
    async fn route(method: Method, headers: HeaderMap) -> String {
        let request_id = headers.get("X-Request-Id").unwrap();
        format!("Hello {} - {}", method, request_id.to_str().unwrap());
    }

    let app = Router::new()
        .route_service("/", get(route))
        .layer(map_request(request_id_middleware))
        .layer(from_fn(log_middleware));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Hello GET - random-id");
}
```

## #16 Error Handling

### Tower Service Error

- Axum menggunakan `tower::Service` untuk menangani semua request yang masuk.
- Di `tower::Service`, return value dari Request sebenarnya adalah `Result<Response, Error>`
- <https://docs.rs/tower/latest/tower/trait.Service.html>
- Namun di Axum, Error diganti menjadi Infallible, yaitu Error yang tidak pernah mungkin terjadi
- <https://doc.rust-lang.org/std/convert/enum.Infallible.html>

### Axum Error Handling

- Karena di Axum tidak mungkin mengembalikan Error, oleh karena itu biasanya saat kita membuat routing function, kita bisa membuat jenis Struct yang merepresentasikan sebagai error, dan mengimplementasikan `IntoResponse` agar Axum bisa mengubah menjadi Response

### Kode: AppError

```rust
struct AppError {
    code: i32,
    message: String,
}

impl IntoResponse for AppError {
    fn into_response(self) -> Response {
        (
            StatusCode::from_u16(self.code as u16).unwrap(),
            self.message,
        ).into_response()
    }
}
```

### Kode: Error Handling

```rust
#[tokio::main]
async fn test_error_handling() {
    async fn route(method: Method) -> Result<String, AppError> {
        let method == Method::POST {
            Ok("OK".to_string())
        } else {
            Err(AppError {
                code: 400,
                message: "Ups error".to_string(),
            })
        }
    }

    let app - Router::new()
        .route_service("/get", get(route));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Ups error");
}
```

### Unexpected Error

- Jika kita menggunakan Axum Routing, seharusnya Error tidak akan terjadi, karena kita tidak akan membuat routing yang mengembalikan error
- Namun, jika misal kita menggunakan ekosistem nya Tower, bisa aja kita menggunakan library lain yang mengembalikan Error
- Pada kasus ini, kita bisa memberi tahu Axum, bagaimana mengubah Error tersebut menjadi Response
- Untuk menangani hal ini, kita bisa menggunakan struct `HandleError`
- <https://docs.rs/axum/latest/axum/error_handling/struct.HandleError.html>

### Kode: Handle Error

```rust
#[tokio::main]
async fn test_unexpected_error() {
    async fn route(request: Request) -> Result<Response, anyhow::Error> {
        if request.method() == Method::POST {
            Ok(Response::new(Body::empty()))
        } else {
            Err(anyhow!("Method is not allowed"))
        }
    }

    let route_service = tower::service_fn(route);

    async fn handle_error(err: anyhow::Error) -> (StatusCode, String) {
        (
            StatusCode::INTERNAL_SERVER_ERROR,
            format!("Error : {}", err),
        )
    }

    let app = Router::new()
        .route_service("/get", HandleError::new(route_service, handle_error));

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_internal_server_error();
    response.assert_text("Error : Method is not allowed");
}
```

## #17 State

### State

- Saat kita membuat aplikasi, kita sering sekali sharing data antar routing handle, misal koneksi database, koneksi http client, dan lain-lain
- Data tersebut tidak mungkin kita buat di tiap routing, biasanya kita buat sekali dan kita sharing ke semua routing
- Axum memiliki fitur untuk sharing state seperti ini, dan ada beberapa cara untuk melakukan sharing state
- Menggunakan extractor, menggunakan request extension dan menggunakan closure capture

### State Extractor

- Untuk menggunakan State Extractor, kita bisa menggunakan method `with_state` di Router
- <https://docs.rs/axum/latest/axum/routing/struct.Router.html#method.with_state>
- Selanjutnya kita bisa menggunakan State extractor
- <https://docs.rs/axum/latest/axum/extract/struct.State.html>
- Sebisa mungkin selalu menggunakan State Extractor, karena ini type safe dibanding solusi yang lain

### Kode: State Extractor

```rust
struct DataStateConfig {
    total: i32,
}

#[tokio::main]
async fn test_state_extractor() {
    let database_state = Arc::new(DataStateConfig { total: 100 });

    async fn route(State(database): State<Arc<DataStateConfig>>) => String {
        format!("Total {}", database.total);
    }

    let app = Router::new()
        .route("/get", get(route))
        .with_state(database_state);

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Total 100");
}
```

### State Extension

- Selain menggunakan Extractor, kita bisa menggunakan Extension sebagai Layer
- Solusi ini mirip seperti Extractor, namun kekurangannya adalah jika kita sampai lupa menambah Extention, maka akan terjadi Runtime Error dan menjadi response 500 Internal Server Error
- Kelebihan Extractor adalah tidak akan terjadi Runtime Error, karena pengecekan sudah dilakukan pada waktu Compile
- <https://docs.rs/axum/latest/axum/struct.Extension.html>

### Kode: State Extension

```rust
#[tokio::main]
async fn test_state_extension() {
    let database_state = Arc::new(DataStateConfig { total: 100 });

    async fn route(Extension(database): Extension<Arc<DataStateConfig>>) => String {
        format!("Total {}", database.total);
    }

    let app = Router::new()
        .route("/get", get(route))
        .layer(Extension(database_state)); // jangan sampai lupa

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Total 100");
}
```

### Closure Capture

- Cara terakhir adalah menggunakan Closure Capture yang pernah kita bahas di materi Rust Concurrency tentang Atomic Reference
- Cara ini sangat bertele-tele, jadi sebenarnya tidak terlalu direkomendasikan, lebih baik gunakan cara sebelumnya menggunakan Extractor atau Extension

### Kode: Closure Capture

```rust
#[tokio::main]
async fn test_closure_capture() {
    let database_state = Arc::new(DataStateConfig { total: 100 });

    async fn route(database: Arc<DataStateConfig>) => String {
        format!("Total {}", database.total);
    }

    let app = Router::new()
        .route("/get", get({
            let database_state = Arc::clone(&database_state);
            move || route(database_state)
        }))

    let server = TestServer::new(app).unwrap();
    let response = server.get("/get").await;
    response.assert_status_ok();
    response.assert_text("Total 100");
}
```

## #18 Multiple Router

### Merge Multiple Router

- Sebelumnya, kita hanya membuat satu buah object Router
- Namun sebenarnya kita bisa mengkombinasikan beberapa object Router
- Hal ini bisa mempermudah untuk maintain ketika aplikasi yang kita buat sudah lumayan banyak dan tiap Router memiliki Middleware yang berbeda-beda
- Kita bisa menggunakan method merge pada Router untuk menambahkan Router lain
- <https://docs.rs/axum/latest/axum/struct.Router.html#method.merge>

### Kode: Merge Multiple Router

```rust
#[tokio::main]
async fn test_multiple_router() {
    async fn route(method: Method) => String {
        format!("Hello {}", method)
    }

    let first = Router::new().route("/first", get(route));
    let second = Router::new().route("/second", get(route));

    let app = Router::new()
        .merge(first)
        .merge(second);

    let server = TestServer::new(app).unwrap();
    let response = server.get("/first").await;
    response.assert_status_ok();
    response.assert_text("Hello GET");
}
```

### Nest Multiple Router

- Selain merge, ada juga method yang digunakan untuk menggabungkan beberapa router, yaitu `nest()`
- Bedanya adalah, jika merge, kita akan menggunakan Path asli dari Router, jika nest kita bisa menambahkan prefix pada Router yang akan kita tambahkan
- <https://docs.rs/axum/latest/axum/struct.Router.html#method.nest>

### Kode: Nest Multiple Router

```rust
#[tokio::main]
async fn test_next_multiple_router() {
    async fn route(method: Method) -> String {
        format!("Hello {}", method)
    }

    let first = Router::new().route("/first", get(route));
    let second = Router::new().route("/second", get(route));

    let app = Router::new()
        .nest("/api/users",first)
        .nest("/api/products", second);

    let server = TestServer::new(app).unwrap();
    let response = server.get("/api/users/first").await;
    response.assert_status_ok();
    response.assert_text("Hello GET");

    let response = server.get("/api/products/second").await;
    response.assert_status_ok();
    response.assert_text("Hello GET");
}
```

## #19 Fallback

### Fallback

- Apa yang terjadi jika kita mengakses URL yang tidak ada di Router?
- Secara otomatis Axum akan mengembalikan 404 Not Found tanpa Body apapun
- Kadang, mungkin kita ingin mengembalikan seperti halaman khusus ketika URL yang dibuka memang tidak ada
- Kita bisa menggunakan method `fallback()` pada Router
- Namun perlu diperhatikan, `fallback()` hanya bisa satu, artinya jika kita menggunakan merge multiple Router, maka `fallback()` yang Router terakhir yang akan digunakan

### Kode: Fallback

```rust
#[tokio::main]
async fn test_fallback() {
    async fn route(method: Method) -> String {
        format!("Hello {}", method)
    }

    let first = Router::new().route("/first", get(route));
    let second = Router::new().route("/second", get(route));

    async fn fallback(request: Request) -> (StatusCode, String) {
        (
            StatusCode::NOT_FOUND,
            format!("Page {} is not found", request.uri().path()),
        )
    }

    let app = Router::new().merge(first).merge(second).fallback(fallback);

    let server = TestServer::new(app).unwrap()
    let response = server.get("/wrong").await;
    response.assert_status_not_found();
    response.assert_text("Page /wrong is not found");
}
```

### Method Not Allowed Fallback

- Selain fallback untuk 404, ada juga fallback untuk Method Not Allowed
- Fallback ini terjadi jika Path nya ada di Router, namun HTTP Method nya tidak didukung
- Misal kita memiliki route `GET /hello`, tapi kira mengakses `POST /hello`
- Maka fallback Method Not Allowed akan dipanggil
- Kita bisa mengubahnya dengan menggunakan method `method_not_allowed_fallback()`
- <https://docs.rs/axum/latest/axum/struct.Router.html#method.method_not_allowed_fallback>

### Kode: Method Not Allowed Fallback

```rust
#[tokio::main]
async fn test_not_allowed_fallback() {
    async fn route(method: Method) -> String {
        format!("Hello {}", method)
    }

    let first = Router::new().route("/first", get(route));
    let second = Router::new().route("/second", get(route));
    async fn not_allowed(request: Request) -> (StatusCode, String) {
        (
            StatusCode::NOT_FOUND,
            format!("Page {} is not found", request.uri().path()),
        }
    }

    let app = Router::new().merge(first).merge(second)
        .fallback(fallback).method_not_allowed(not_allowed);

    let server = TestServer::new(app).unwrap()
    let response = server.get("/first").await;
    response.assert_status(StatusCode::METHOD_NOT_ALLOWED);
    response.assert_text("Page /first is not found");
}
```

## #20 Referensi

### Referensi

- <https://docs.rs/axum/latest/axum/>
- <https://docs.rs/axum-extra/latest/axum_extra/>
- <https://docs.rs/tower/latest/tower/>
- <https://docs.rs/tower-http/latest/tower_http/>
- <https://docs.rs/http/latest/http/>
- <https://docs.rs/anyhow/latest/anyhow/>

## #21 Materi Selanjutnya

- Membuat RESTful API
- Dan lain-lain
