# HTTP Request/Response Log

**Author:** Ankit
**Tool used:** `curl -i`
**API tested:** [JSONPlaceholder](https://jsonplaceholder.typicode.com) — a free, public, read-only fake REST API for testing.

This log captures five `curl -i` requests against a public JSON API, along with their full response headers and bodies. One request (Request 5) deliberately targets a resource that does not exist, to capture a `404 Not Found` response.

---

## Request 1 — GET a single post

```bash
curl -i https://jsonplaceholder.typicode.com/posts/1
```

**Response**

```http
HTTP/1.1 200 OK
Date: Sun, 16 Aug 2026 10:56:59 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 292
Connection: keep-alive
Cache-Control: max-age=43200
ETag: W/"124-yiKdLzqO5gfBrJFrcdJ8Yq0LGnU"
Server: cloudflare
X-Powered-By: Express
X-Ratelimit-Limit: 1000
X-Ratelimit-Remaining: 999

{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
```

**Note:** `200 OK` means the request succeeded and the server returned the requested resource. `Content-Type: application/json` tells the client the response body is JSON-formatted text.

---

## Request 2 — GET a single user

```bash
curl -i https://jsonplaceholder.typicode.com/users/1
```

**Response**

```http
HTTP/1.1 200 OK
Date: Sun, 16 Aug 2026 11:00:09 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 509
Connection: keep-alive
Cache-Control: max-age=43200
ETag: W/"1fd-+2Y3G3w049iSZtw5t1mzSnunngE"
Server: cloudflare
X-Powered-By: Express
X-Ratelimit-Limit: 1000
X-Ratelimit-Remaining: 937

{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "suite": "Apt. 556",
    "city": "Gwenborough",
    "zipcode": "92998-3874",
    "geo": { "lat": "-37.3159", "lng": "81.1496" }
  },
  "phone": "1-770-736-8031 x56442",
  "website": "hildegard.org",
  "company": {
    "name": "Romaguera-Crona",
    "catchPhrase": "Multi-layered client-server neural-net",
    "bs": "harness real-time e-markets"
  }
}
```

**Note:** `200 OK` confirms success; `Content-Type: application/json; charset=utf-8` specifies the payload is JSON text encoded in UTF-8.

---

## Request 3 — GET a single to-do item

```bash
curl -i https://jsonplaceholder.typicode.com/todos/1
```

**Response**

```http
HTTP/1.1 200 OK
Date: Sun, 16 Aug 2026 11:02:17 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 83
Connection: keep-alive
Cache-Control: max-age=43200
ETag: W/"53-hfEnumeNh6YirfjyjaujcOPPT+s"
Server: cloudflare
X-Powered-By: Express
X-Ratelimit-Limit: 1000
X-Ratelimit-Remaining: 999

{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

**Note:** `200 OK` again indicates a successful fetch; the small `Content-Length` (83 bytes) reflects the compact size of this resource.

---

## Request 4 — GET a nested/related resource (comments on a post)

```bash
curl -i https://jsonplaceholder.typicode.com/posts/1/comments
```

**Response**

```http
HTTP/1.1 200 OK
Date: Sun, 16 Aug 2026 11:03:41 GMT
Content-Type: application/json; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Cache-Control: max-age=43200
ETag: W/"5e6-4bSPS5tq8F8ZDeFJULWh6upjp7U"
Server: cloudflare
X-Powered-By: Express
X-Ratelimit-Limit: 1000
X-Ratelimit-Remaining: 999

[
  {
    "postId": 1,
    "id": 1,
    "name": "id labore ex et quam laborum",
    "email": "Eliseo@gardner.biz",
    "body": "laudantium enim quasi est quidem magnam voluptate ipsam eos..."
  },
  {
    "postId": 1,
    "id": 2,
    "name": "quo vero reiciendis velit similique earum",
    "email": "Jayne_Kuhic@sydney.com",
    "body": "est natus enim nihil est dolore omnis voluptatem numquam..."
  }
  // ...3 more comment objects omitted for brevity
]
```

**Note:** `200 OK` means success; `Transfer-Encoding: chunked` (instead of a fixed `Content-Length`) means the server streamed the response body in chunks because its total size wasn't known upfront.

---

## Request 5 — GET a non-existent resource (deliberate failure)

```bash
curl -i https://jsonplaceholder.typicode.com/posts/9999
```

**Response**

```http
HTTP/1.1 404 Not Found
Date: Sun, 16 Aug 2026 11:05:26 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 2
Connection: keep-alive
Cache-Control: max-age=43200
ETag: W/"2-vyGp6PvFo4RvsFtPoIWeCReyIC8"
Server: cloudflare
X-Powered-By: Express
X-Ratelimit-Limit: 1000
X-Ratelimit-Remaining: 999

{}
```

**Note:** `404 Not Found` means the server could not find any resource matching the requested ID (post `9999` does not exist); `Content-Type: application/json` shows the (empty) error body is still returned as JSON.

---

## Summary Table

| # | Endpoint | Status | Meaning | Content-Type |
|---|----------|--------|---------|---------------|
| 1 | `/posts/1` | 200 OK | Resource found and returned | `application/json` |
| 2 | `/users/1` | 200 OK | Resource found and returned | `application/json` |
| 3 | `/todos/1` | 200 OK | Resource found and returned | `application/json` |
| 4 | `/posts/1/comments` | 200 OK | Related resources found and returned | `application/json` |
| 5 | `/posts/9999` | **404 Not Found** | No resource exists with this ID | `application/json` |
