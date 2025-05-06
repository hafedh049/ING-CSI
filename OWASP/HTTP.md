### 🌐 The Complete HTTP Reference Guide

## 📋 HTTP Versions

```mermaid
graph LR;
    A["HTTP/0.9 (1991)"]-->B["HTTP/1.0 (1996)"]
    B-->C["HTTP/1.1 (1997)"]
    C-->D["HTTP/2 (2015)"]
    D-->E["HTTP/3 (2022)"]
```

| Version      | Year | Key Features                                                                                                |
| ------------ | ---- | ----------------------------------------------------------------------------------------------------------- |
| **HTTP/0.9** | 1991 | • Single-line protocol`<br>`• Only GET method`<br>`• Only HTML responses                                    |
| **HTTP/1.0** | 1996 | • Headers introduced`<br>`• Status codes`<br>`• Content-Type support`<br>`• New methods (POST, HEAD)        |
| **HTTP/1.1** | 1997 | • Persistent connections`<br>`• Pipelining`<br>`• Host header`<br>`• Additional methods (PUT, DELETE, etc.) |
| **HTTP/2**   | 2015 | • Multiplexed connections`<br>`• Header compression`<br>`• Server push`<br>`• Binary protocol               |
| **HTTP/3**   | 2022 | • QUIC transport protocol`<br>`• Improved performance`<br>`• Built-in encryption`<br>`• Reduced latency     |

## 🔢 HTTP Status Codes

### 1xx: Informational

| Code    | Name                | Description                                                                         |
| ------- | ------------------- | ----------------------------------------------------------------------------------- |
| **100** | Continue            | Server received the request headers and client should proceed with the request body |
| **101** | Switching Protocols | Server is switching protocols as requested by client                                |
| **102** | Processing          | Server has received and is processing the request, but no response is available yet |
| **103** | Early Hints         | Used to return some response headers before final HTTP message                      |

### 2xx: Success

| Code    | Name                          | Description                                                                 |
| ------- | ----------------------------- | --------------------------------------------------------------------------- |
| **200** | OK                            | Request succeeded                                                           |
| **201** | Created                       | Request succeeded and a new resource was created                            |
| **202** | Accepted                      | Request accepted for processing but not completed                           |
| **203** | Non-Authoritative Information | Request processed successfully but returned info may be from another source |
| **204** | No Content                    | Request succeeded but returns no content                                    |
| **205** | Reset Content                 | Request succeeded, client should reset the document view                    |
| **206** | Partial Content               | Server is delivering only part of the resource due to range header          |

### 3xx: Redirection

| Code    | Name               | Description                                                     |
| ------- | ------------------ | --------------------------------------------------------------- |
| **300** | Multiple Choices   | Multiple options for the resource                               |
| **301** | Moved Permanently  | Resource has been permanently moved to a new URL                |
| **302** | Found              | Resource temporarily located at a different URL                 |
| **303** | See Other          | Response to the request can be found at another URL using GET   |
| **304** | Not Modified       | Resource has not been modified since last requested             |
| **307** | Temporary Redirect | Resource temporarily moved, but use same method for new request |
| **308** | Permanent Redirect | Resource permanently moved, but use same method for new request |

### 4xx: Client Errors

| Code    | Name                          | Description                                                     |
| ------- | ----------------------------- | --------------------------------------------------------------- |
| **400** | Bad Request                   | Server cannot process the request due to client error           |
| **401** | Unauthorized                  | Authentication is required and has failed or not been provided  |
| **402** | Payment Required              | Reserved for future use                                         |
| **403** | Forbidden                     | Server understood request but refuses to authorize it           |
| **404** | Not Found                     | Requested resource could not be found                           |
| **405** | Method Not Allowed            | Request method not supported for the requested resource         |
| **406** | Not Acceptable                | Resource cannot generate content according to Accept headers    |
| **407** | Proxy Authentication Required | Authentication with proxy required                              |
| **408** | Request Timeout               | Server timed out waiting for the request                        |
| **409** | Conflict                      | Request conflicts with current state of the server              |
| **410** | Gone                          | Resource is no longer available and will not be available again |
| **411** | Length Required               | Content-Length header is required                               |
| **412** | Precondition Failed           | Server does not meet preconditions in request headers           |
| **413** | Payload Too Large             | Request entity is larger than limits defined by server          |
| **414** | URI Too Long                  | URI requested by client is longer than server can interpret     |
| **415** | Unsupported Media Type        | Media format of the requested data not supported                |
| **416** | Range Not Satisfiable         | Range in Range header cannot be satisfied                       |
| **417** | Expectation Failed            | Expectation in Expect header cannot be met                      |
| **418** | I'm a teapot                  | Server refuses to brew coffee with a teapot (April Fools' joke) |
| **422** | Unprocessable Entity          | Request well-formed but semantically erroneous                  |
| **429** | Too Many Requests             | User has sent too many requests in a given time                 |
### 5xx: Server Errors

| Code    | Name                            | Description                                                  |
| ------- | ------------------------------- | ------------------------------------------------------------ |
| **500** | Internal Server Error           | Generic server error message                                 |
| **501** | Not Implemented                 | Server does not recognize method or lacks ability to fulfill |
| **502** | Bad Gateway                     | Server acting as gateway received invalid response           |
| **503** | Service Unavailable             | Server currently unavailable (overloaded/maintenance)        |
| **504** | Gateway Timeout                 | Server acting as gateway did not get response in time        |
| **505** | HTTP Version Not Supported      | HTTP version used in the request not supported               |
| **506** | Variant Also Negotiates         | Server has an internal configuration error                   |
| **507** | Insufficient Storage            | Server unable to store the representation                    |
| **508** | Loop Detected                   | Server detected an infinite loop while processing            |
| **510** | Not Extended                    | Further extensions to the request are required               |
| **511** | Network Authentication Required | Client needs to authenticate to gain network access          |
## 🔄 HTTP Methods

```mermaid
graph TD;
    A["HTTP Methods"]-->B["GET: Retrieve data"]
    A-->C["POST: Submit data"]
    A-->D["PUT: Update resource"]
    A-->E["DELETE: Remove resource"]
    A-->F["PATCH: Partial update"]
    A-->G["HEAD: Get headers only"]
    A-->H["OPTIONS: Get supported methods"]
    A-->I["TRACE: Diagnostic"]
    A-->J["CONNECT: Tunnel"]
```

### GET

Used to request data from a specified resource.

**Request Example:**

```bash
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
```

**Response Example:**

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 82

{
  "users": [
    {"id": 1, "name": "John"},
    {"id": 2, "name": "Jane"}
  ]
}
```

### POST

Used to submit data to be processed to a specified resource.

**Request Example:**

```bash
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 29

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

**Response Example:**

```bash
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 45
Location: /api/users/3

{
  "id": 3,
  "name": "Alice",
  "email": "alice@example.com"
}
```

### PUT

Used to update a resource or create it if it doesn't exist.

**Request Example:**

```bash
PUT /api/users/3 HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 45

{
  "id": 3,
  "name": "Alice Smith",
  "email": "alice.smith@example.com"
}
```

**Response Example:**

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 45

{
  "id": 3,
  "name": "Alice Smith",
  "email": "alice.smith@example.com"
}
```

### DELETE

Used to delete a specified resource.

**Request Example:**

```bash
DELETE /api/users/3 HTTP/1.1
Host: example.com
```

**Response Example:**

```bash
HTTP/1.1 204 No Content
```

### PATCH

Used to apply partial modifications to a resource.

**Request Example:**

```bash
PATCH /api/users/2 HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 30

{
  "email": "jane.new@example.com"
}
```

**Response Example:**

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 52

{
  "id": 2,
  "name": "Jane",
  "email": "jane.new@example.com"
}
```

### HEAD

Similar to GET but returns only HTTP headers, not the body.

**Request Example:**

```bash
HEAD /api/users HTTP/1.1
Host: example.com
```

**Response Example:**

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 82
Last-Modified: Wed, 21 Oct 2023 07:28:00 GMT
```

### OPTIONS

Used to describe the communication options for the target resource.

**Request Example:**

```bash
OPTIONS /api/users HTTP/1.1
Host: example.com
```

**Response Example:**

```bash
HTTP/1.1 200 OK
Allow: GET, POST, HEAD, OPTIONS
Access-Control-Allow-Methods: GET, POST, HEAD, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

### TRACE

Used for diagnostic purposes to see what is being received at the other end.

**Request Example:**

```bash
TRACE /api/debug HTTP/1.1
Host: example.com
X-Custom-Header: test
```

**Response Example:**

```bash
HTTP/1.1 200 OK
Content-Type: message/http
Content-Length: 57

TRACE /api/debug HTTP/1.1
Host: example.com
X-Custom-Header: test
```

### CONNECT

Used to establish a tunnel to the server identified by the target resource.

**Request Example:**

```bash
CONNECT example.com:443 HTTP/1.1
Host: example.com
```

**Response Example:**

```bash
HTTP/1.1 200 Connection Established
Proxy-Agent: MyProxy/1.1
```

## 🔗 URL Syntax and URI

### URL Syntax

```bash
scheme://[userinfo@]host[:port][/path][?query][#fragment]
```

**Example:**

```bash
https://user:password@www.example.com:8080/path/to/resource?name=value&name2=value2#section
```

| Component    | Description               | Example                    |
| ------------ | ------------------------- | -------------------------- |
| **Scheme**   | Protocol to use           | `https:`                   |
| **Userinfo** | Username and password     | `user:password@`           |
| **Host**     | Domain name or IP address | `www.example.com`          |
| **Port**     | Port number               | `:8080`                    |
| **Path**     | Path to resource          | `/path/to/resource`        |
| **Query**    | Query parameters          | `?name=value&name2=value2` |
| **Fragment** | Fragment identifier       | `#section`                 |

### URI vs URL

```mermaid
graph TD;
    A["URI (Uniform Resource Identifier)"]-->B["URL (Uniform Resource Locator)"]
    A-->C["URN (Uniform Resource Name)"]
```

**URI (Uniform Resource Identifier):**

- A string of characters that identifies a resource
- Includes both URLs and URNs
- Example: `https://example.com/page` or `urn:isbn:0451450523`


**URL (Uniform Resource Locator):**

- A type of URI that specifies where a resource is located and how to access it
- Includes the protocol (scheme)
- Example: `https://example.com/page`


**URN (Uniform Resource Name):**

- A type of URI that identifies a resource by name in a particular namespace
- Does not specify location or how to access the resource
- Example: `urn:isbn:0451450523`


**Key Differences:**

1. All URLs are URIs, but not all URIs are URLs
2. URLs provide a method to access the resource (like https, ftp)
3. URNs identify resources by name without specifying how to access them
4. URLs can change if the resource moves, URNs are designed to remain globally unique and persistent


```bash 
project="http-request-demo"
...
https://kzmoqred1hazsuka1z5j.lite.vusercontent.net
```

I've created an interactive HTTP request tester above that you can use to experiment with different HTTP methods and see the responses in real-time!

This comprehensive guide covers all the essential aspects of HTTP:

- **HTTP Versions**: From the original HTTP/0.9 to the latest HTTP/3, with key features of each version
- **Status Codes**: All HTTP status codes organized by category with detailed explanations
- **HTTP Methods**: All standard HTTP methods with request and response examples
- **URL Syntax**: Detailed breakdown of URL components with examples
- **URI vs URL**: Clear explanation of the differences between URI and URL


The interactive demo allows you to:

1. Select different HTTP methods
2. Enter any URL to send requests to
3. Add request bodies for POST, PUT, and PATCH requests
4. View response status codes, headers, and body content
5. Copy response data to clipboard


Feel free to experiment with the demo to see different status codes and responses in action!