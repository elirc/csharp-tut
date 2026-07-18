# 09 - HTTP Servers and Routing Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `http.createServer(handler)` | Creates HTTP server. |
| 2 | `server.listen(3000)` | Starts listening on port. |
| 3 | `req.method` | HTTP method. |
| 4 | `req.url` | Request URL path and query. |
| 5 | `req.headers` | Request headers. |
| 6 | `res.statusCode = 404` | Sets response status. |
| 7 | `res.setHeader("Content-Type", "application/json")` | Sets response header. |
| 8 | `res.end(body)` | Sends and ends response. |
| 9 | `JSON.stringify(data)` | Converts response to JSON. |
| 10 | `new URL(req.url, base)` | Parses request URL. |
| 11 | `url.searchParams.get("page")` | Reads query parameter. |
| 12 | `for await (const chunk of req)` | Reads request body chunks. |
| 13 | `POST` | Usually creates data. |
| 14 | `GET` | Usually reads data. |
| 15 | `PUT` | Usually replaces data. |
| 16 | `PATCH` | Usually partially updates data. |
| 17 | `DELETE` | Usually removes data. |
| 18 | `404` | Not found. |
| 19 | `500` | Server error. |
| 20 | `request and response are streams` | HTTP bodies are stream-based. |
| 21 | `http.STATUS_CODES[404]` | Gets standard reason phrase for status code. |
| 22 | `req.socket.remoteAddress` | Reads client address information. |
| 23 | `server.close()` | Stops accepting new connections. |
| 24 | `res.write(chunk)` | Streams part of a response body. |
| 25 | `req.on("aborted", handler)` | Detects aborted client request. |
| 26 | `Content-Length` | Header that declares body size. |
| 27 | `Transfer-Encoding: chunked` | Response body sent in chunks. |
| 28 | `405` | Method not allowed. |
| 29 | `201` | Created. |
| 30 | `204` | No content. |

## Interview Questions

**Q1. What are the core pieces of an HTTP request?**  
Method, URL, headers, and optionally a body.

**Q2. What is the difference between `GET` and `POST`?**  
`GET` usually retrieves data and should be safe/idempotent. `POST` usually creates or triggers a server-side action.

**Q3. Why are request and response bodies stream-based in Node?**  
HTTP bodies may be large or arrive over time, so streams let Node process them incrementally.

**Q4. What should an API return for an unknown route?**  
Usually `404 Not Found`.

**Q5. Why do many teams use frameworks instead of only `node:http`?**  
Frameworks provide routing, middleware, body parsing, error handling patterns, and ecosystem conventions.
