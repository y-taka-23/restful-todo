# RESTful Todo Manager

[![Build Status](https://travis-ci.org/y-taka-23/restful-todo.svg?branch=master)](https://travis-ci.org/y-taka-23/restful-todo)

This is a simple task management application with JSON-based REST API.  The project is written in [Frege](https://github.com/Frege/frege) language and relevant stuffs, so you would be able to use it as a basic example of web application construction:

* [Chinook](https://github.com/fregelab/chinook): Lightweight web application framework
* [Sirocco](https://github.com/fregelab/sirocco): JDBC wrapper for Frege
* [frege.data.JSON](http://www.frege-lang.org/doc/frege/data/JSON): Standard library for JSON (de)serialization

I hope that this work helps you to enjoy heppy Frege coding!

## How to Run

The project is commited with Gradle wrapper in the repository, so what you have to do is execute:

```
git clone https://github.com/y-taka-23/restful-todo.git
cd restful-todo
./gradlew run
```

Then check `http://localhost:4567/api/v1/tasks`, and you should see the empty JSON `[]`.  Let's start with `POST /api/v1/tasks` in the following.

Note that if you terminate the process (by for example Ctrl + C), the stored data will be lost.  It is bacause that the application uses the in-memory mode of H2 database by default.  To make your data persistent, revise `src/main/frege/restfultodo/dataaccess/Connection.fr`from:

```
private dbUrl = "jdbc:h2:mem:restfultodo"
```

to:

```
private dbUrl = "jdbc:h2:PATH/TO/DUMPFILE"
```

## API Endpoints

### GET /api/v1/tasks

Shows a list of registered tasks. You can also filter (un)completed tasks by the query parameter `completed={true,false}`.

#### Request

```
curl -X GET http://localhost:4567/api/v1/tasks
```

```
curl -X GET http://localhost:4567/api/v1/tasks?completed=false
```

#### Response

```
HTTP/1.1 200 OK
Date: Sat, 07 May 2016 13:05:33 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Server: Jetty(9.3.2.v20150730)

[
  {
    "id": 1,
    "task": {
      "title": "Iron the dishes",
      "completed": true
    }
  },
  {
    "id": 2,
    "task": {
      "title": "Dust the dog",
      "completed": false
    }
  },
  {
    "id": 3,
    "task": {
      "title": "Take salad out of the oven",
      "completed": false
    }
  }
]
```

### POST /api/v1/tasks

Creates a new task. Both of `title` and `completed` fields are required.

#### Request

```
curl -X POST -H "Content-type: application/json" -d "{ \"title\": \"Iron the dishes\", \"completed\": false }" http://localhost:4567/api/v1/tasks
```

#### Response

```
HTTP/1.1 201 Created
Date: Sat, 07 May 2016 13:03:57 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Server: Jetty(9.3.2.v20150730)

{
  "id": 1,
  "task": {
    "title": "Iron the dishes",
    "completed": false
  }
}
```

### GET /api/v1/tasks/:id

Shows the task of specified `id`.

#### Request

```
curl -X GET http://localhost:4567/api/v1/tasks/1
```

#### Response

```
HTTP/1.1 200 OK
Date: Sat, 07 May 2016 12:59:21 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Server: Jetty(9.3.2.v20150730)

{
  "id": 1,
  "task": {
    "title": "Iron the dishes",
    "completed": false
  }
}
```

### PUT /api/v1/tasks/:id

Updates the task of specified `id`. The JSON format in request bodies is same as `POST /api/v1/tasks`.

#### Request

```
curl -X PUT -H "Content-type: application/json" -d "{ \"title\": \"Iron the dishes\", \"completed\": true }" http://localhost:4567/api/v1/tasks/1
```

#### Response

Same as `GET /api/v1/tasks/:id`.

### DELETE /api/v1/tasks/:id

Deletes the task of specified `id`.

#### Request

```
curl -X DELETE http://localhost:4567/api/v1/tasks/1
```

#### Response

```
HTTP/1.1 204 No Content
Date: Sat, 07 May 2016 13:07:42 GMT
Content-Type: application/json
Server: Jetty(9.3.2.v20150730)
```

## License

This project is released under the MIT license. For more details, see [LICENSE](LICENSE) file.
