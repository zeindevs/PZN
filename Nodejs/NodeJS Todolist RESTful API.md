# NodeJS Todolist RESTful API

## Sebelum Belajar

- NodeJS Dasar
- RESTful API
- Open API

## Agenda

- Membuat Aplikasi Todolist RESTful API menggunakan NodeJS

## #1 API Spec

```json
{
	"openapi": "3.0.3",
	"info": {
		"title": "NodeJS Todolist API",
		"description": "NodeJS Todolist API",
		"version": "1.0.0"
	},
	"servers": [
		{
			"url": "http://localhost:3000"
		}
	],
	"paths": {
		"/": {
			"get": {
				"responses": {
					"200": {
						"description": "Get all todolist",
						"content": {
							"application/json": {
								"schema": {
									"$ref": "#/components/schemas/Response"
								}
							}
						}
					}
				}
			},
			"post": {
				"requestBody": {
					"content": {
						"application/json": {
							"schema": {
								"type": "object",
								"properties": {
									"todo": {
										"type": "string"
									}
								}
							}
						}
					}
				},
				"responses": {
					"200": {
						"description": "Create new todo",
						"content": {
							"application/json": {
								"schema": {
									"$ref": "#/components/schemas/Response"
								}
							}
						}
					}
				}
			},
			"delete": {
				"requestBody": {
					"content": {
						"application/json": {
							"schema": {
								"type": "object",
								"properties": {
									"id": {
										"type": "number"
									}
								}
							}
						}
					}
				},
				"responses": {
					"200": {
						"description": "Delete todo",
						"content": {
							"application/json": {
								"schema": {
									"$ref": "#/components/schemas/Response"
								}
							}
						}
					}
				}
			},
			"put": {
				"requestBody": {
					"content": {
						"application/json": {
							"schema": {
								"type": "object",
								"properties": {
									"id": {
										"type": "number"
									},
									"todo": {
										"type": "string"
									}
								}
							}
						}
					}
				},
				"responses": {
					"200": {
						"description": "Update todo",
						"content": {
							"application/json": {
								"schema": {
									"$ref": "#/components/schemas/Response"
								}
							}
						}
					}
				}
			}
		}
	},
	"components": {
		"schemas": {
			"Response": {
				"type": "object",
				"properties": {
					"code": {
						"type": "number"
					},
					"status": {
						"type": "string"
					},
					"data": {
						"type": "array",
						"items": {
							"type": "object",
							"properties": {
								"id": {
									"type": "number"
								},
								"todo": {
									"type": "string"
								}
							}
						}
					}
				}
			}
		}
	}
}
```

## #2 Membuat HTTP Server

```js
// app.mjs
import http from "http";
const server = http.createServer((request, response) => {
	response.setHeader("Content-Type", "application/json");
});

server.listen(3000);
```

## #3 API Get Todolist

```js
// todolist-service.mjs
export class TodolistService {
	todolist = ["Programmer", "Zaman", "Now"];

	getJsonTodoList() {
		return JSON.stringify({
			code: 200,
			status: "OK",
			data: this.todolist.map((value, index) => {
				return {
					id: index,
					todo: value,
				};
			}),
		});
	}

	getTodoList(request, response) {
		response.write(this.getJsonTodoList());
		response.end();
	}
}
```

```js
// app.mjs
import http from "http";
import { TodolistService } from "./todolist-service.mjs";

const service = new TodolistService();
const server = http.createServer((request, response) => {
	response.setHeader("Content-Type", "application/json");

	if (request.method === "GET") {
		service.getTodoList(request, response);
	}
});

server.listen(3000);
```

## #4 API Create Todo

```js
// todolist-service.mjs
export class TodolistService {

    todolist = ["Programmer", "Zaman", "Now"];

    getJsonTodoList() {
        return JSON.stringify({
            code: 200,
            status: "OK",
            data: this.todolist.map((value, index) => {
                return {
                    id: index,
                    todo: value
                }
            })
        });
    }

    ...

    createTodo(request, response) {
        request.addListener("data", (data) => {
            const body = JSON.parse(data.toString());
            this.todolist.push(body.todo);

            response.write(this.getJsonTodoList());
            response.end();
        })
    }
}
```

```js
// app.mjs
import http from "http";
import { TodolistService } from "./todolist-service.mjs";

const service = new TodolistService();
const server = http.createServer((request, response) => {
	response.setHeader("Content-Type", "application/json");

	if (request.method === "GET") {
		service.getTodoList(request, response);
	} else if (request.method === "POST") {
		service.createTodo(request, response);
	}
});

server.listen(3000);
```

## #5 API Update Todo

```js
// todolist-service.mjs
export class TodolistService {

    todolist = ["Programmer", "Zaman", "Now"];

    getJsonTodoList() {
        return JSON.stringify({
            code: 200,
            status: "OK",
            data: this.todolist.map((value, index) => {
                return {
                    id: index,
                    todo: value
                }
            })
        });
    }

    ...

    updateTodo(request, response) {
        request.addListener("data", (data) => {
            const body = JSON.parse(data.toString());
            if (this.todolist[body.id]) {
                this.todolist[body.id] = body.todo;
            }

            response.write(this.getJsonTodoList());
            response.end();
        })
    }
}
```

```js
// app.mjs
import http from "http";
import { TodolistService } from "./todolist-service.mjs";

const service = new TodolistService();
const server = http.createServer((request, response) => {
	response.setHeader("Content-Type", "application/json");

	if (request.method === "GET") {
		service.getTodoList(request, response);
	} else if (request.method === "POST") {
		service.createTodo(request, response);
	} else if (request.method === "PUT") {
		service.updateTodo(request, response);
	}
});

server.listen(3000);
```

## #6 API Delete Todo

```js
// todolist-service.mjs
export class TodolistService {

    todolist = ["Programmer", "Zaman", "Now"];

    getJsonTodoList() {
        return JSON.stringify({
            code: 200,
            status: "OK",
            data: this.todolist.map((value, index) => {
                return {
                    id: index,
                    todo: value
                }
            })
        });
    }

    ...

    deleteTodo(request, response){
        request.addListener("data", (data) => {
            const body = JSON.parse(data.toString());
            if (this.todolist[body.id]) {
                this.todolist.splice(body.id, 1);
            }

            response.write(this.getJsonTodoList());
            response.end();
        })
    }
}
```

```js
// app.mjs
import http from "http";
import { TodolistService } from "./todolist-service.mjs";

const service = new TodolistService();
const server = http.createServer((request, response) => {
	response.setHeader("Content-Type", "application/json");

	if (request.method === "GET") {
		service.getTodoList(request, response);
	} else if (request.method === "POST") {
		service.createTodo(request, response);
	} else if (request.method === "PUT") {
		service.updateTodo(request, response);
	} else if (request.method === "DELETE") {
		service.deleteTodo(request, response);
	}
});

server.listen(3000);
```

## #7 Test API

```http
GET http://localhost:3000

###

POST http://localhost:3000
Content-Type: application/json
Accept: application/json

{
  "todo": "Belajar NodeJS"
}

###

PUT http://localhost:3000
Content-Type: application/json
Accept: application/json

{
  "id" : 4,
  "todo" : "Pemrograman JavaScript"
}

###

DELETE http://localhost:3000
Content-Type: application/json
Accept: application/json

{
  "id" : 2
}
```

## #8 Materi Selanjutnya

- NPM (Node Package Manager)
- NodeJS Unit Test
- ExpressJS
- NodeJS Database
- Dan lain-lain
