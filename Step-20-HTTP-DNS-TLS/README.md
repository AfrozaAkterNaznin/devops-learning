# Step 20 — HTTP, DNS & TLS

## Documentation Part 1 — HTTP Fundamentals + Request/Response

## 1. Overview

Step 20 focused on understanding how web communication works from the application layer down to secure network communication.

The practical work was performed on the existing MERN Blog Application.

Main areas covered in this part:

- HTTP fundamentals
- HTTP request and response
- Express server
- HTTP port
- API routes
- Request method
- Response status
- Response body
- HTML vs JSON response
- Basic HTTP request flow
- Practical HTTP verification

Project used:

    Blog-App-using-MERN-stack

Backend:

    Express.js

Backend Port:

    5001

---

## 2. What is HTTP?

HTTP stands for HyperText Transfer Protocol.

It is an application-layer protocol used for communication between a client and a server.

A client sends an HTTP request and the server returns an HTTP response.

Basic flow:

    Client
       |
       | HTTP Request
       v
    Express Server
       |
       | HTTP Response
       v
    Client

Example:

    Client → GET /api/users → Express Server
    Client ← 200 OK + JSON ← Express Server

HTTP does not itself mean encryption.

For encrypted communication, HTTPS is used.

---

## 3. HTTP Request

An HTTP request is sent by a client to a server.

A request can contain:

- HTTP method
- URL/path
- Headers
- Query parameters
- Request body

Example:

    GET /api/users HTTP/1.1

The request tells the server:

    "I want to access /api/users using GET."

---

## 4. HTTP Response

The server sends an HTTP response back to the client.

A response can contain:

- HTTP status code
- Response headers
- Response body

Example:

    HTTP/1.1 200 OK

    Content-Type: application/json

    {"users":[]}

The status code tells the client whether the request was successful or not.

---

## 5. HTTP Request vs HTTP Response

| Feature | HTTP Request | HTTP Response |
|---|---|---|
| Direction | Client → Server | Server → Client |
| Contains | Method, URL, headers, body | Status, headers, body |
| Example | `GET /api/users` | `200 OK` |
| Created by | Client | Server |
| Purpose | Ask server to perform/access something | Return result to client |

---

## 6. Express Server in the Project

The project uses Express.js as the HTTP server.

The main server file is:

    server/server.js

The application creates an Express instance:

    const app = express();

The server listens on:

    5001

Therefore:

    http://localhost:5001

is the backend base URL.

---

## 7. Project HTTP Configuration

The project contains:

    app.use(cors());

This enables CORS middleware.

The project also uses Helmet:

    app.use(helmet(...));

Helmet adds security-related HTTP response headers.

JSON request parsing is enabled with:

    app.use(express.json());

This allows Express to parse incoming JSON request bodies.

---

## 8. HTTP Port

The backend listens on port:

    5001

The server configuration contains:

    app.listen(5001, () => console.log("app started at 5001..."));

Therefore:

    localhost = local machine
    5001     = application port

Full backend address:

    http://localhost:5001

---

## 9. HTTP Listening Port Verification

The project port was verified using:

    ss -lntp | grep -E ':3000|:5000|:8000'

Later the actual project port was verified using:

    ss -lntp | grep ':5001'

Observed:

    LISTEN ... *:5001 ...

This confirms that the Express application was actively listening on port 5001.

### Important `ss` Flags

| Flag | Meaning |
|---|---|
| `-l` | Show listening sockets |
| `-n` | Show numeric addresses/ports |
| `-t` | Show TCP sockets |
| `-p` | Show process information |

Therefore:

    ss -lntp

means:

    Show listening TCP sockets with numeric addresses and process information.

---

## 10. Project API Routes

The project contains two main route groups.

### User Routes

Base route:

    /api/users

Available routes include:

    GET  /api/users
    POST /api/users/signup
    POST /api/users/login

### Blog Routes

Base route:

    /api/blogs

Available routes include:

    GET    /api/blogs
    POST   /api/blogs/add
    PUT    /api/blogs/update/:id
    GET    /api/blogs/:id
    DELETE /api/blogs/:id
    GET    /api/blogs/user/:id

---

## 11. Route Structure

The Express server mounts the routers using:

    app.use("/api/users", userRouter);
    app.use("/api/blogs", blogRouter);

Therefore the route path is constructed from:

    Base Path + Router Path

Example:

    /api/users + /

becomes:

    /api/users

And:

    /api/users + /signup

becomes:

    /api/users/signup

---

## 12. HTTP Request Flow in This Project

Example:

    Client
       |
       | GET /api/users
       v
    Express Server :5001
       |
       v
    userRouter
       |
       v
    getAllUser controller
       |
       v
    MongoDB
       |
       v
    JSON Response
       |
       v
    Client

This demonstrates how HTTP connects the client with the backend application and database.

---

## 13. Practical HTTP Test

The project was tested using HTTP requests.

### Base API Test

Request:

    GET http://localhost:5001/api

Response:

    HTTP/1.1 200 OK

Response body:

    hello

This confirms that the Express server accepted the HTTP request and returned a successful response.

---

## 14. `/api` Response

The `/api` route is defined as:

    app.use("/api", (req, res, next) => {
        res.send("hello");
    });

Therefore a request to:

    GET /api

returns:

    HTTP/1.1 200 OK

and:

    hello

The response Content-Type was:

    text/html; charset=utf-8

Although the body is only the text `hello`, Express treats the response as a text/HTML response.

---

## 15. `/api/users` Response

The users endpoint was tested:

    GET http://localhost:5001/api/users

Observed status:

    HTTP/1.1 200 OK

The response contained JSON:

    {
        "statusCode": 200,
        "data": {
            "users": [...]
        },
        "message": "Users fetched successfully",
        "success": true
    }

The response Content-Type was:

    application/json; charset=utf-8

This confirms that the API endpoint is returning structured JSON data.

---

## 16. HTML/Text Response vs JSON Response

| `/api` | `/api/users` |
|---|---|
| `text/html` | `application/json` |
| `hello` | Structured user data |
| Simple response | API response |
| Uses `res.send()` | Uses `res.json()` |
| Human-readable text | Machine-readable JSON |

This difference is important in web applications because frontend applications normally consume API data as JSON.

---

## 17. HTTP Status Code Observation

During testing, the project returned different HTTP statuses.

Successful user request:

    HTTP/1.1 200 OK

Blog request when there were no blogs:

    HTTP/1.1 404 Not Found

Therefore HTTP status codes provide information about the result of the request.

Basic categories:

| Range | Meaning |
|---|---|
| `1xx` | Informational |
| `2xx` | Success |
| `3xx` | Redirection |
| `4xx` | Client-side/request problem |
| `5xx` | Server-side problem |

Important examples observed during Step 20:

| Status | Meaning |
|---|---|
| `200` | Request successful |
| `301` | Resource redirected permanently |
| `404` | Resource not found |

More detailed status-code work was covered in Step 20.2.

---

## 18. Successful Request vs Failed Resource Request

During practical testing:

    GET /api/users

returned:

    200 OK

while:

    GET /api/blogs

returned:

    404 Not Found

The `/api/blogs` response was:

    {
        "statusCode": 404,
        "data": null,
        "success": false,
        "errors": []
    }

The 404 occurred because the application currently had no blog documents.

This is different from the server being completely down.

### Important distinction

| Situation | Result |
|---|---|
| Server is running and route/data request succeeds | `200` |
| Server is running but requested resource is unavailable | `404` |
| Server is not running | Connection failure |
| Application encounters server-side error | Usually `5xx` |

---

## 19. HTTP Request/Response Workflow

The practical HTTP workflow can be summarized as:

    1. Client creates HTTP request
           |
           v
    2. Request reaches server IP + port
           |
           v
    3. Express receives request
           |
           v
    4. Express matches route
           |
           v
    5. Controller performs required operation
           |
           v
    6. Server creates HTTP response
           |
           v
    7. Response contains status + headers + body
           |
           v
    8. Client receives response

For the project:

    Client
       |
       | GET /api/users
       v
    localhost:5001
       |
       v
    Express
       |
       v
    User Router
       |
       v
    User Controller
       |
       v
    MongoDB
       |
       v
    JSON Response
       |
       v
    Client

---

## 20. HTTP vs HTTPS

| HTTP | HTTPS |
|---|---|
| HyperText Transfer Protocol | HTTP Secure |
| No TLS encryption | Uses TLS |
| Data is not protected by TLS | Data is encrypted through TLS |
| Example: `http://` | Example: `https://` |
| Port commonly `80` | Port commonly `443` |

The project backend was tested over HTTP on:

    http://localhost:5001

HTTPS/TLS verification was separately performed using Google during Step 20.6 and Step 20.7.

---

## 21. Important Concepts Learned

| Concept | Step 20 Implementation |
|---|---|
| HTTP | Communication protocol |
| Client | Sends HTTP requests |
| Server | Receives requests and sends responses |
| Express | Project HTTP server |
| Port | `5001` |
| Route | `/api`, `/api/users`, `/api/blogs` |
| Request | Client → Server |
| Response | Server → Client |
| Status Code | Indicates request result |
| Response Body | Returned data |
| JSON | API data format |
| Content-Type | Identifies response data type |

---

## 22. Practical Verification Summary

The following HTTP behavior was successfully verified:

    Express server running
    ↓
    Port 5001 listening
    ↓
    GET /api → 200 OK
    ↓
    GET /api/users → 200 OK + JSON
    ↓
    GET /api/blogs → 404 when no blogs exist

This confirms that the project's HTTP layer is functioning correctly.

---

## 23. Key Takeaways

1. HTTP provides the communication mechanism between client and server.

2. An HTTP request contains information such as method and path.

3. An HTTP response contains status, headers and body.

4. Express.js provides the HTTP server for this MERN project.

5. The backend listens on port `5001`.

6. Express routes determine which controller handles a request.

7. `/api` returned a text/HTML response.

8. `/api/users` returned a JSON API response.

9. `200` means the request was successfully processed.

10. `404` means the requested resource was not found.

11. A `404` response does not mean the entire server is down.

12. HTTP and HTTPS are different; HTTPS adds TLS security.

---

## 24. Part 1 Completion

Step 20 Documentation Part 1 covered:

    HTTP Fundamentals
    ↓
    HTTP Request
    ↓
    HTTP Response
    ↓
    Express Server
    ↓
    Port 5001
    ↓
    Project Routes
    ↓
    Status Codes
    ↓
    HTML vs JSON
    ↓
    Request/Response Workflow
    ↓
    Practical HTTP Verification



## Documentation Part 2 — HTTP Methods, Status Codes, Headers & Content-Type

## 1. Overview

This part covers the practical HTTP methods, status codes, response headers, Content-Type, CORS and security headers observed in the MERN project.

Practical work covered:

    20.2 → HTTP Methods + Status Codes
    20.3 → HTTP Headers + Content-Type + CORS + Security Headers

Main project:

    Blog-App-using-MERN-stack

Backend:

    Express.js

Backend Port:

    5001

---

## 2. HTTP Methods

HTTP methods describe what the client wants to do with a resource.

Main methods relevant to this project:

    GET
    POST
    PUT
    DELETE

---

## 3. HTTP Methods Used in the Project

| Method | Purpose | Project Example |
|---|---|---|
| GET | Read/retrieve data | `/api/users` |
| POST | Create/send new data | `/api/users/signup` |
| PUT | Update existing data | `/api/blogs/update/:id` |
| DELETE | Remove data | `/api/blogs/:id` |

The project therefore follows the common CRUD HTTP method pattern.

    GET    → Read
    POST   → Create
    PUT    → Update
    DELETE → Delete

---

## 4. GET

GET is used to retrieve information from the server.

Project examples:

    GET /api/users
    GET /api/blogs
    GET /api/blogs/:id
    GET /api/blogs/user/:id

Practical test:

    GET /api/users

Observed:

    HTTP/1.1 200 OK

The server returned the existing user as JSON.

Important observation:

GET normally retrieves data rather than creating or modifying the resource.

---

## 5. POST

POST is used to send data to the server, commonly to create a new resource or perform an operation.

Project examples:

    POST /api/users/signup
    POST /api/users/login
    POST /api/blogs/add

Example:

    POST /api/users/login

The request contains JSON data such as:

    email
    password

The server processes the request and returns a response.

POST was already practically tested during the authentication work.

---

## 6. PUT

PUT is used to update an existing resource.

Project route:

    PUT /api/blogs/update/:id

The `:id` identifies the blog that should be updated.

Example structure:

    PUT /api/blogs/update/RESOURCE_ID

The request can contain updated fields such as:

    title
    desc

---

## 7. DELETE

DELETE is used to remove a resource.

Project route:

    DELETE /api/blogs/:id

Example:

    DELETE /api/blogs/RESOURCE_ID

The server identifies the blog using the ID and removes it.

---

## 8. CRUD Mapping

| CRUD Operation | HTTP Method | Project Example |
|---|---|---|
| Create | POST | `/api/users/signup` |
| Read | GET | `/api/users` |
| Update | PUT | `/api/blogs/update/:id` |
| Delete | DELETE | `/api/blogs/:id` |

This mapping is one of the most important practical relationships between REST APIs and HTTP.

---

## 9. HTTP Method vs HTTP Status Code

These are different concepts.

| HTTP Method | HTTP Status Code |
|---|---|
| Describes what the client wants to do | Describes the result |
| Sent as part of the request | Returned as part of the response |
| Examples: GET, POST, PUT, DELETE | Examples: 200, 404, 500 |
| Client-side request information | Server response information |

Example:

    GET /api/users

The:

    GET

is the request method.

The response:

    HTTP/1.1 200 OK

contains the status code.

Therefore:

    GET ≠ 200

GET describes the operation.

200 describes the result.

---

## 10. Status Codes

HTTP status codes are grouped into five major categories.

| Range | Category | Meaning |
|---|---|---|
| 1xx | Informational | Request is being processed |
| 2xx | Success | Request succeeded |
| 3xx | Redirection | Client should follow another location/action |
| 4xx | Client Error | Request/resource problem |
| 5xx | Server Error | Server-side failure |

---

## 11. Important Status Codes

| Status | Meaning | Example |
|---|---|---|
| 200 | OK | Successful GET |
| 201 | Created | Successful resource creation |
| 301 | Moved Permanently | Permanent redirect |
| 400 | Bad Request | Invalid request |
| 401 | Unauthorized | Authentication required/failed |
| 403 | Forbidden | Access denied |
| 404 | Not Found | Resource not available |
| 500 | Internal Server Error | Server-side failure |

---

## 12. Practical 200 Response

The users API returned:

    HTTP/1.1 200 OK

This means:

    Request received
    ↓
    Route matched
    ↓
    Controller executed
    ↓
    Data returned
    ↓
    Successful response

The API response contained:

    "statusCode": 200
    "success": true

Therefore both the HTTP status and application response indicated success.

---

## 13. Practical 404 Response

The blogs endpoint returned:

    HTTP/1.1 404 Not Found

Response:

    {
        "statusCode": 404,
        "data": null,
        "success": false,
        "errors": []
    }

In the current project state, there were no blog documents.

Important observation:

    Server running
    +
    Route accessible
    +
    No available blog resource
    =
    404 response

This is different from a connection failure.

---

## 14. 200 vs 404 vs Connection Failure

| Situation | Result |
|---|---|
| Server running + resource available | `200 OK` |
| Server running + resource unavailable | `404 Not Found` |
| Server not running | Connection refused/failure |
| Server running + application exception | Usually `500` |

This distinction is important during troubleshooting.

---

## 15. Unknown Route Observation

During testing, an unknown-looking route under `/api` returned:

    HTTP/1.1 200 OK

with:

    hello

This happened because the application contains a general `/api` middleware:

    app.use("/api", (req, res, next) => {
        res.send("hello");
    });

Therefore not every unmatched API path automatically produces a 404.

The application configuration determines how the request is handled.

Important lesson:

    HTTP status behavior depends on server routing logic.

---

## 16. HTTP Headers

Headers provide additional information about an HTTP request or response.

They are metadata associated with HTTP communication.

Examples observed in the project:

    Content-Type
    Content-Length
    Access-Control-Allow-Origin
    X-Content-Type-Options
    X-Frame-Options
    Strict-Transport-Security
    Referrer-Policy

---

## 17. Response Headers Observed

The project returned headers including:

    Access-Control-Allow-Origin: *

    Content-Type: text/html; charset=utf-8

    Content-Length: 5

    X-Content-Type-Options: nosniff

    X-Frame-Options: SAMEORIGIN

    Strict-Transport-Security: max-age=31536000; includeSubDomains

These headers came from Express, CORS configuration and Helmet/security middleware.

---

## 18. Content-Type

Content-Type tells the client what kind of data is contained in the response body.

Two Content-Type values were observed during testing.

For `/api`:

    Content-Type: text/html; charset=utf-8

For `/api/users`:

    Content-Type: application/json; charset=utf-8

---

## 19. Content-Type Difference

| Content-Type | Meaning | Project Example |
|---|---|---|
| `text/html` | HTML/text-based response | `/api` |
| `application/json` | JSON API response | `/api/users` |
| `text/plain` | Plain text | Simple text response |
| `application/octet-stream` | Generic binary data | File/binary response |

For APIs, `application/json` is commonly used because frontend applications can directly parse the returned structured data.

---

## 20. `res.send()` vs `res.json()`

The project demonstrates two different response styles.

The `/api` route uses:

    res.send("hello");

The user controller returns structured API data using JSON response handling.

Conceptual difference:

| `res.send()` | `res.json()` |
|---|---|
| Sends a response body | Sends JSON response |
| Can send text/HTML/data | Designed for JSON |
| `/api` uses this style | API controllers use JSON |
| Example body: `hello` | Example body: `{ "users": [] }` |

---

## 21. CORS

CORS stands for:

    Cross-Origin Resource Sharing

The project enables CORS using:

    app.use(cors());

The practical response contained:

    Access-Control-Allow-Origin: *

This means the server allows requests from any origin under this configuration.

---

## 22. CORS Observation

Observed header:

    Access-Control-Allow-Origin: *

Meaning:

    Browser
       |
       | Cross-Origin Request
       v
    Express Server
       |
       | CORS allows origin
       v
    Response

Important:

CORS is primarily a browser security mechanism.

It does not encrypt HTTP traffic.

Encryption is handled by TLS when HTTPS is used.

---

## 23. CORS vs TLS

| CORS | TLS |
|---|---|
| Controls browser cross-origin access | Secures network communication |
| Browser security mechanism | Cryptographic security protocol |
| Uses HTTP headers | Uses TLS handshake/encryption |
| Example: `Access-Control-Allow-Origin` | Example: `TLSv1.3` |
| Does not encrypt data | Encrypts data in transit |

---

## 24. Helmet Security Headers

The project uses Helmet:

    const helmet = require("helmet");

and:

    app.use(helmet(...));

Helmet adds security-related HTTP response headers.

Observed examples:

    X-Content-Type-Options: nosniff

    X-Frame-Options: SAMEORIGIN

    Strict-Transport-Security: max-age=31536000; includeSubDomains

---

## 25. Important Security Headers

### X-Content-Type-Options

Observed:

    X-Content-Type-Options: nosniff

Purpose:

Helps prevent browsers from trying to interpret content as a different MIME type.

---

### X-Frame-Options

Observed:

    X-Frame-Options: SAMEORIGIN

Purpose:

Controls whether the page can be loaded inside a frame from another origin.

---

### Strict-Transport-Security

Observed:

    Strict-Transport-Security: max-age=31536000; includeSubDomains

Purpose:

Tells compatible browsers to use HTTPS for the specified period.

This header is related to HTTPS/TLS and is discussed further in the TLS/HTTPS documentation.

---

## 26. CORS vs Security Headers

| Feature | CORS | Helmet Security Headers |
|---|---|---|
| Main purpose | Control cross-origin browser access | Improve HTTP security |
| Mechanism | CORS response headers | Multiple security headers |
| Project configuration | `cors()` | `helmet()` |
| Example | `Access-Control-Allow-Origin` | `X-Frame-Options` |
| Encrypts traffic | No | No |
| Replaces TLS | No | No |

---

## 27. Cookies

Cookies are small pieces of data stored by the browser and associated with a website.

They can be used for:

    Session management
    Authentication
    User preferences
    Tracking

Typical cookie-related request/response headers are:

    Set-Cookie

and:

    Cookie

Important distinction:

Cookies are not the same as headers, but cookies are transported through HTTP headers.

---

## 28. Cookies vs Headers

| Cookies | Headers |
|---|---|
| Store small client-side state | Carry HTTP metadata |
| Can maintain sessions | Describe request/response properties |
| Sent using `Cookie` header | Many different header fields exist |
| Server can issue them using `Set-Cookie` | Headers are directly part of HTTP messages |
| Common in authentication/session systems | Used in almost every HTTP request/response |

No application cookie was required for the Step 20 practical verification. Cookies were covered as an HTTP concept so their role can be distinguished from headers, CORS and authentication.

---

## 29. Request Headers vs Response Headers

| Request Headers | Response Headers |
|---|---|
| Client → Server | Server → Client |
| Describe client request | Describe server response |
| Example: `Content-Type` | Example: `Content-Type` |
| Can contain `Cookie` | Can contain `Set-Cookie` |
| Sent before server processes request | Returned with server response |

---

## 30. HTTP Header Verification

Headers were inspected using:

    curl -I http://localhost:5001/api

The `-I` option requests response headers without downloading the normal response body.

Important `curl` flag:

    -I

Meaning:

    Fetch headers only.

This is useful when troubleshooting:

    Status code
    Content-Type
    CORS
    Security headers
    Redirects

---

## 31. Header Verification Workflow

The practical workflow was:

    Client
       |
       | HTTP Request
       v
    Express
       |
       | Route processing
       v
    Response
       |
       +--> Status Code
       |
       +--> Headers
       |
       +--> Body
       |
       v
    Client

Headers provide information about how the response should be interpreted or handled.

---

## 32. HTTP Methods + Headers + Status

These three concepts must not be confused.

| Concept | Question it answers |
|---|---|
| HTTP Method | What does the client want to do? |
| Status Code | What happened to the request? |
| Header | What metadata/instructions accompany the message? |
| Body | What actual data is being transferred? |

Example:

    GET /api/users

    HTTP/1.1 200 OK

    Content-Type: application/json

    {"users":[...]}

Here:

    GET
    ↓
    Method

    200 OK
    ↓
    Status

    Content-Type: application/json
    ↓
    Header

    {"users":[...]}
    ↓
    Body

---

## 33. Complete HTTP Message Structure

Conceptually:

    Request
    ┌──────────────────────────────┐
    │ Method + Path                │
    │ Headers                      │
    │ Body                         │
    └──────────────────────────────┘
                  |
                  v
              Server
                  |
                  v
    Response
    ┌──────────────────────────────┐
    │ Status Code                  │
    │ Headers                      │
    │ Body                         │
    └──────────────────────────────┘

---

## 34. Practical Observation Summary

The following behavior was verified during Step 20.2 and Step 20.3:

    GET /api/users
    → 200 OK
    → application/json

    GET /api/blogs
    → 404 Not Found when no blogs exist

    GET /api
    → 200 OK
    → text/html

Response headers included:

    Access-Control-Allow-Origin: *
    X-Content-Type-Options: nosniff
    X-Frame-Options: SAMEORIGIN
    Strict-Transport-Security: ...

This confirmed that the Express application is not only returning API data but also applying CORS and security-related HTTP headers.

---

## 35. Important Differences

### GET vs POST vs PUT vs DELETE

| GET | POST | PUT | DELETE |
|---|---|---|---|
| Read | Create/send | Update | Delete |
| Usually retrieves data | Usually creates/submits | Updates resource | Removes resource |
| `/api/users` | `/api/users/signup` | `/api/blogs/update/:id` | `/api/blogs/:id` |

### 200 vs 201 vs 301 vs 404 vs 500

| Code | Meaning |
|---|---|
| 200 | Successful request |
| 201 | Resource created |
| 301 | Permanent redirect |
| 404 | Resource not found |
| 500 | Server-side error |

### HTTP vs HTTPS

| HTTP | HTTPS |
|---|---|
| Plain HTTP communication | HTTP secured with TLS |
| No TLS encryption | TLS encryption |
| `http://` | `https://` |
| Common port 80 | Common port 443 |

---

## 36. Part 2 Key Takeaways

1. HTTP methods describe the intended operation.

2. GET is mainly used to retrieve data.

3. POST is commonly used to create/send data.

4. PUT is used to update resources.

5. DELETE is used to remove resources.

6. Status codes describe the result of the request.

7. Headers carry metadata and instructions.

8. Content-Type tells the client what type of data is being returned.

9. CORS controls browser cross-origin access.

10. CORS does not provide encryption.

11. Helmet adds security-related HTTP headers.

12. Cookies provide a mechanism for client-side state/session data.

13. TLS is responsible for secure encrypted communication.

14. HTTP method, status code, header and body are separate concepts.

---

## 37. Part 2 Completion

Step 20 Documentation Part 2 covered:

    HTTP Methods
    ↓
    GET / POST / PUT / DELETE
    ↓
    CRUD Mapping
    ↓
    Status Codes
    ↓
    HTTP Headers
    ↓
    Content-Type
    ↓
    CORS
    ↓
    Helmet Security Headers
    ↓
    Cookies
    ↓
    Request vs Response Headers
    ↓
    Practical Header Verification
    ↓
    HTTP Message Structure
    ↓
    Important Difference Tables

## Documentation Part 3 — DNS + TLS/HTTPS

## 1. Overview

This part covers the network components behind web communication:

    20.4 → DNS Fundamentals + Resolution
    20.5 → DNS Tools + Troubleshooting
    20.6 → TLS/SSL Fundamentals
    20.7 → HTTPS + Certificate Verification

The practical work verified:

    Domain → IP resolution
    DNS resolver
    DNS server
    IPv4 / IPv6
    DNS troubleshooting tools
    TLS connection
    TLS version
    TLS cipher
    Certificate subject
    Certificate issuer
    Certificate validity
    HTTPS response
    Certificate verification

---

## 2. What is DNS?

DNS stands for:

    Domain Name System

DNS translates human-readable domain names into IP addresses.

Example:

    google.com
          ↓
    142.251.222.174

IPv6 resolution can also return:

    2404:6800:4007:838::200e

Without DNS, users would need to remember IP addresses instead of domain names.

---

## 3. Basic DNS Workflow

    User enters:
        google.com
             |
             v
        DNS Resolver
             |
             v
        DNS Server
             |
             v
        IP Address
             |
             v
        Client connects to IP

Example from the practical work:

    google.com
        ↓
    142.251.222.174

and:

    google.com
        ↓
    2404:6800:4007:838::200e

---

## 4. Domain Name vs IP Address

| Domain Name | IP Address |
|---|---|
| Human-readable | Machine/network address |
| Example: `google.com` | Example: `142.251.222.174` |
| Easier to remember | Used for network communication |
| Resolved through DNS | Destination address |

---

## 5. IPv4 vs IPv6

The DNS tests returned both IPv4 and IPv6 addresses.

| IPv4 | IPv6 |
|---|---|
| 32-bit address | 128-bit address |
| Example: `142.251.222.174` | `2404:6800:4007:838::200e` |
| Shorter format | Longer hexadecimal format |
| Address space is smaller | Very large address space |

Both can represent the network address of the same domain.

---

## 6. Localhost DNS Resolution

The localhost test returned:

    ::1    localhost

`::1` is the IPv6 loopback address.

It refers back to the local machine.

Comparison:

| Address | Meaning |
|---|---|
| `127.0.0.1` | IPv4 loopback |
| `::1` | IPv6 loopback |
| `localhost` | Hostname commonly mapped to loopback |

Therefore:

    localhost
        ↓
    local machine

---

## 7. System DNS Resolver

The system resolver was observed through:

    resolvectl status

Observed DNS servers included:

    40.40.40.40
    8.8.8.8
    fd17:625c:f037:2::3

The resolver configuration determines which DNS servers the system can use for name resolution.

---

## 8. DNS Resolver vs DNS Server

These terms should not be confused.

| DNS Resolver | DNS Server |
|---|---|
| Handles DNS lookup process for the client | Provides DNS information/answers |
| Client/system commonly communicates with resolver | Stores or obtains DNS records |
| May forward queries | Returns DNS answers |
| Example local resolver: `127.0.0.53` | Example: `8.8.8.8` |

In the practical output:

    Server: 127.0.0.53
    Address: 127.0.0.53#53

This represents the local system resolver.

A separate test explicitly queried:

    8.8.8.8

---

## 9. DNS Resolution Test

The domain was resolved using:

    getent hosts google.com

Observed result included:

    2404:6800:4007:833::200e google.com

This confirms that the operating system could resolve the hostname to an IP address.

---

## 10. `getent`

`getent` queries information through the system's configured name-service mechanisms.

Command used:

    getent hosts google.com

Purpose:

    Verify hostname resolution using the system resolver configuration.

Important point:

`getent` is useful when testing what the operating system itself can resolve.

---

## 11. `host`

The `host` command was used to inspect DNS information for the domain.

Observed information included:

    google.com has address 142.251.222.174

    google.com has IPv6 address 2404:6800:4007:838::200e

It also returned other DNS-related records such as:

    google.com mail is handled by 10 smtp.google.com.

and:

    google.com has HTTP service bindings ...

This demonstrates that DNS can provide different types of records, not only IP addresses.

---

## 12. `nslookup`

The command:

    nslookup google.com

returned:

    Server: 127.0.0.53
    Address: 127.0.0.53#53

and a non-authoritative answer containing IPv4 and IPv6 addresses.

A second test explicitly used:

    nslookup google.com 8.8.8.8

This allowed the DNS lookup to be tested against Google's public DNS server.

---

## 13. `nslookup` Default vs Specific DNS Server

| Command | DNS Server Used |
|---|---|
| `nslookup google.com` | System-configured resolver |
| `nslookup google.com 8.8.8.8` | Explicitly uses `8.8.8.8` |

This distinction is useful for troubleshooting.

If the default resolver fails but an external DNS server works, the problem may be related to local DNS configuration.

---

## 14. DNS Troubleshooting Workflow

A simple DNS troubleshooting flow:

    Domain
       |
       v
    getent
       |
       v
    nslookup
       |
       v
    Test specific DNS server
       |
       v
    Compare results

Example:

    getent hosts google.com

If this fails:

    nslookup google.com

If the default resolver has problems:

    nslookup google.com 8.8.8.8

This helps determine whether the issue is:

    Local resolution
        or
    DNS server resolution

---

## 15. DNS Tool Comparison

| Tool | Main Purpose | Practical Use |
|---|---|---|
| `getent` | System-level name resolution | Check what the OS resolves |
| `host` | Quick DNS lookup | Inspect DNS records |
| `nslookup` | DNS query/debugging | Test resolver/server |
| `resolvectl` | System DNS configuration/status | Inspect configured DNS servers |

---

## 16. Important DNS Observation

The same domain can resolve to different IPv4 addresses at different times.

For example, practical tests returned:

    142.251.222.174

and another test returned:

    142.251.221.206

This is not automatically a DNS failure.

Large services may use:

    Load balancing
    Multiple servers
    Geographic routing
    Dynamic DNS responses

Therefore the exact IP returned for a large service can change.

---

## 17. What is TLS?

TLS stands for:

    Transport Layer Security

TLS provides secure communication over a network.

It helps provide:

    Encryption
    Authentication
    Integrity

HTTPS uses HTTP over TLS.

Conceptually:

    HTTP
      +
    TLS
      =
    HTTPS

---

## 18. SSL vs TLS

SSL is the older security protocol family.

TLS is the modern successor.

| SSL | TLS |
|---|---|
| Older protocol family | Modern protocol family |
| Deprecated/insecure versions exist | Modern versions are used today |
| Historical name still commonly used | Current standard |
| Often called "SSL certificate" colloquially | Technically certificates are used with TLS |

The practical connection used:

    TLSv1.3

Therefore the actual secure protocol observed was TLS, not an old SSL protocol.

---

## 19. TLS Connection Test

TLS was tested using:

    openssl s_client -connect google.com:443 -servername google.com

The output showed:

    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384

and:

    Protocol: TLSv1.3

This confirms that the connection negotiated TLS 1.3.

---

## 20. TLS Version

Observed:

    TLSv1.3

TLS 1.3 is a modern TLS version designed to provide secure communication with improved handshake performance and security compared with older versions.

The important observation for this practical:

    Client
       ↓
    TLS negotiation
       ↓
    TLSv1.3
       ↓
    Secure connection established

---

## 21. TLS Cipher

Observed:

    TLS_AES_256_GCM_SHA384

This identifies the cryptographic algorithms used for the TLS connection.

The practical goal was not to memorize every cryptographic component, but to understand that TLS negotiates a cryptographic configuration for the secure connection.

---

## 22. Certificate

A TLS certificate helps establish the identity of the server.

The practical certificate subject was:

    subject=CN=*.google.com

This indicates that the certificate covers Google domains matching the wildcard pattern.

---

## 23. Certificate Subject

The certificate subject identifies the entity/domain information represented by the certificate.

Observed:

    CN=*.google.com

`CN` means:

    Common Name

The certificate therefore represents a Google domain.

---

## 24. Certificate Issuer

Observed issuer:

    issuer=C=US, O=Google Trust Services, CN=WE2

The issuer is the certificate authority/service that issued the certificate.

Therefore:

    Google domain certificate
            ↓
    Issued by Google Trust Services

---

## 25. Certificate Validity

The certificate contained:

    notBefore=Jul 20 18:06:16 2026 GMT
    notAfter=Oct 12 18:06:15 2026 GMT

This represents the period during which the certificate is valid.

Conceptually:

    notBefore
        |
        |---- Certificate valid ----|
                                  notAfter

A certificate outside its validity period should not be considered valid.

---

## 26. Certificate Verification

The most important verification result was:

    Verify return code: 0 (ok)

This indicates that OpenSSL successfully verified the certificate chain for the tested connection.

Therefore:

    TLS connection = successful
    Certificate verification = successful

---

## 27. TLS vs Certificate

TLS and certificates are related but not identical.

| TLS | Certificate |
|---|---|
| Security protocol | Digital identity document |
| Establishes secure communication | Helps authenticate server identity |
| Provides encrypted communication | Contains identity/issuer/validity information |
| Example: `TLSv1.3` | Example: `CN=*.google.com` |

A useful mental model:

    TLS = Secure communication mechanism

    Certificate = Server identity proof used during that process

---

## 28. HTTPS

HTTPS means:

    HyperText Transfer Protocol Secure

It is HTTP communication protected by TLS.

Basic flow:

    Client
       |
       | HTTPS connection
       v
    TLS handshake
       |
       v
    Certificate verification
       |
       v
    Encrypted HTTP communication
       |
       v
    Server

---

## 29. HTTPS Verification

The HTTPS request was tested using:

    curl -I https://google.com

Observed:

    HTTP/2 301

and:

    location: https://www.google.com/

This means the HTTPS connection itself succeeded, but Google redirected the request to:

    https://www.google.com/

The `301` is a redirect, not a TLS failure.

---

## 30. HTTPS Response

Observed:

    HTTP/2 301
    location: https://www.google.com/
    content-type: text/html; charset=UTF-8

Important observations:

    HTTP/2
        ↓
    HTTPS request successfully reached server

    301
        ↓
    Permanent redirect

    location
        ↓
    New destination

---

## 31. 301 Redirect vs TLS Verification

These are separate events.

| `301` | `Verify return code: 0` |
|---|---|
| HTTP response status | TLS certificate verification result |
| Indicates redirect | Indicates successful certificate verification |
| Application/web-server layer | TLS/security layer |
| Does not mean certificate failure | Does not mean HTTP response is 200 |

Therefore the practical result:

    TLS = successful
    Certificate = verified
    HTTPS = successful
    HTTP response = 301 redirect

---

## 32. HTTPS vs HTTP

| HTTP | HTTPS |
|---|---|
| Application protocol | HTTP + TLS |
| No TLS encryption | Encrypted through TLS |
| `http://` | `https://` |
| Common port `80` | Common port `443` |
| No certificate verification at TLS layer | Certificate is verified |
| Suitable for non-sensitive local testing | Required for secure production communication |

---

## 33. DNS → TCP → TLS → HTTP

A complete web connection can be understood as layers.

    1. DNS
       google.com → IP address

             ↓

    2. TCP
       Connect to server port

             ↓

    3. TLS
       Establish secure encrypted connection

             ↓

    4. HTTP
       Send request and receive response

This is one of the most important workflows from Step 20.

---

## 34. Practical Step 20 Network Flow

For the Google HTTPS test:

    google.com
        |
        v
    DNS resolution
        |
        v
    Google IP address
        |
        v
    TCP connection to port 443
        |
        v
    TLS handshake
        |
        v
    Certificate verification
        |
        v
    TLSv1.3 secure connection
        |
        v
    HTTPS request
        |
        v
    HTTP/2 301 response

---

## 35. DNS vs TLS vs HTTP

| DNS | TLS | HTTP |
|---|---|---|
| Finds IP address | Secures connection | Transfers application data |
| Domain → IP | Secure channel | Request/response |
| Example: `google.com → IP` | `TLSv1.3` | `GET /` |
| Uses DNS infrastructure | Uses certificates/cryptography | Uses HTTP methods/status codes |

---

## 36. DNS vs HTTPS

| DNS | HTTPS |
|---|---|
| Resolves domain names | Transfers web data securely |
| Produces IP addresses | Produces HTTP responses |
| Happens before connection to destination | Happens after secure connection is established |
| Example: `google.com → IP` | Example: `HTTPS → HTTP/2 301` |

---

## 37. Useful Command and Purpose Reference

| Command | Purpose |
|---|---|
| `getent hosts google.com` | Test system hostname resolution |
| `host google.com` | Inspect DNS information |
| `nslookup google.com` | Query configured DNS resolver |
| `nslookup google.com 8.8.8.8` | Query a specific DNS server |
| `resolvectl status` | View system DNS configuration |
| `openssl s_client -connect google.com:443 -servername google.com` | Inspect TLS connection/certificate |
| `curl -I https://google.com` | Inspect HTTPS response headers/status |

---

## 38. Important Command Flags

### `curl -I`

    -I

Means:

    Fetch response headers only.

Useful for:

    Status code
    Redirect
    Content-Type
    HTTP headers

---

### `openssl s_client`

The important arguments used were:

    -connect google.com:443

Meaning:

    Connect to Google on port 443.

And:

    -servername google.com

This supplies the Server Name Indication (SNI) value during TLS negotiation.

SNI is important because a server can host multiple TLS-enabled domains on the same IP address.

---

### `nslookup`

The second argument can specify the DNS server:

    nslookup google.com 8.8.8.8

Meaning:

    Ask DNS server 8.8.8.8 to resolve google.com.

---

## 39. DNS Troubleshooting Decision Table

| Observation | Possible Meaning |
|---|---|
| `getent` resolves domain | System DNS working |
| `getent` fails | Local/system DNS problem possible |
| `nslookup` works with `8.8.8.8` | External DNS server reachable |
| Default resolver fails but `8.8.8.8` works | Local resolver/configuration problem possible |
| No IP returned | DNS resolution problem |
| IP works but HTTPS fails | Check TCP/TLS |
| TLS verification fails | Check certificate/CA/domain/time |
| HTTPS returns `301` | Redirect, not necessarily an error |

---

## 40. TLS Troubleshooting Decision Table

| Observation | Meaning |
|---|---|
| Connection cannot reach port 443 | Network/port problem |
| TLS handshake fails | TLS/network/server configuration problem |
| Wrong certificate domain | Certificate identity problem |
| Certificate expired | Certificate validity problem |
| Verify code not `0` | Certificate verification problem |
| `TLSv1.3` negotiated | Modern TLS connection established |
| Verify code `0 (ok)` | Certificate verification successful |

---

## 41. Actual Practical Results

### DNS

    google.com → IPv4 address
    google.com → IPv6 address

### DNS Resolver

    127.0.0.53

### External DNS Server

    8.8.8.8

### TLS

    Protocol: TLSv1.3

### Cipher

    TLS_AES_256_GCM_SHA384

### Certificate

    subject=CN=*.google.com

### Certificate Issuer

    Google Trust Services

### Certificate Verification

    Verify return code: 0 (ok)

### HTTPS

    HTTP/2 301

### Redirect

    https://www.google.com/

All required DNS and TLS/HTTPS checks were successful.

---

## 42. Important Mental Model

When opening:

    https://google.com

think:

    Domain name
        ↓
    DNS
        ↓
    IP address
        ↓
    TCP connection
        ↓
    TLS handshake
        ↓
    Certificate verification
        ↓
    Secure connection
        ↓
    HTTP/2 request
        ↓
    HTTP response

This connects the concepts learned throughout Step 20.

---

## 43. Part 3 Key Takeaways

1. DNS translates domain names into IP addresses.

2. A domain can resolve to both IPv4 and IPv6 addresses.

3. `getent`, `host`, `nslookup` and `resolvectl` serve different DNS troubleshooting purposes.

4. `127.0.0.53` was observed as the local system resolver.

5. `8.8.8.8` was tested as an external DNS server.

6. DNS resolution must generally happen before connecting to a domain.

7. TLS provides secure communication.

8. TLS 1.3 was successfully negotiated.

9. The certificate subject was `CN=*.google.com`.

10. The certificate issuer was Google Trust Services.

11. Certificate verification returned `0 (ok)`.

12. HTTPS is HTTP running through TLS.

13. `HTTP/2 301` indicates a successful HTTPS connection followed by a redirect.

14. DNS, TLS and HTTP solve different problems.

15. A useful web troubleshooting flow is:

    DNS
      ↓
    TCP/Port
      ↓
    TLS
      ↓
    HTTP/HTTPS

---

## 44. Part 3 Completion

Step 20 Documentation Part 3 covered:

    DNS Fundamentals
    ↓
    DNS Resolution
    ↓
    IPv4 / IPv6
    ↓
    System DNS Resolver
    ↓
    getent
    ↓
    host
    ↓
    nslookup
    ↓
    DNS Troubleshooting
    ↓
    TLS Fundamentals
    ↓
    TLS vs SSL
    ↓
    TLSv1.3
    ↓
    Certificate
    ↓
    Certificate Subject
    ↓
    Certificate Issuer
    ↓
    Certificate Validity
    ↓
    Certificate Verification
    ↓
    HTTPS
    ↓
    HTTP/2
    ↓
    301 Redirect
    ↓
    DNS → TCP → TLS → HTTP Workflow

## Documentation Part 4 — Troubleshooting + Final Project Verification

## 1. Overview

This final part covers:

    20.8 → HTTP / DNS / TLS Troubleshooting
    20.9 → Final Project Verification

The purpose was to verify that the complete communication chain works correctly and to understand how to identify which layer is responsible when something fails.

Main troubleshooting layers:

    HTTP
    DNS
    TCP / Port
    TLS
    HTTPS

---

## 2. Why Layer-Based Troubleshooting Matters

A web request can fail for different reasons.

For example:

    Domain does not resolve
        ↓
    DNS problem

    Domain resolves but port is unreachable
        ↓
    Network / Port problem

    Port works but TLS handshake fails
        ↓
    TLS problem

    HTTPS works but endpoint returns 404
        ↓
    HTTP / Application routing problem

Therefore, instead of assuming that every web problem is an application problem, each layer should be checked separately.

---

## 3. Troubleshooting Workflow

The practical troubleshooting workflow used in Step 20 was:

    DNS
      ↓
    IP Address
      ↓
    TCP / Port
      ↓
    TLS
      ↓
    Certificate Verification
      ↓
    HTTPS
      ↓
    HTTP Response
      ↓
    Application API

This allows the failure point to be isolated.

---

## 4. HTTP Health Check

The project HTTP endpoint was tested with:

    curl -s -o /dev/null -w "HTTP Status: %{http_code}\nTime: %{time_total}s\n" http://localhost:5001/api

Observed:

    HTTP Status: 200
    Time: 0.001834s

This confirmed that:

    Express server is running
    Port 5001 is reachable
    /api route responds
    HTTP request is successful

The response time was approximately:

    0.001834 seconds

---

## 5. Meaning of the curl Options

The HTTP troubleshooting command used several curl options.

| Option | Purpose |
|---|---|
| `-s` | Silent mode |
| `-o /dev/null` | Discard response body |
| `-w` | Display custom output |
| `%{http_code}` | Show HTTP status code |
| `%{time_total}` | Show total request time |

Therefore the command was designed to quickly verify:

    HTTP status
    Request time

without printing the complete response body.

---

## 6. HTTP Header Check

Headers were checked using:

    curl -I http://localhost:5001/api

Observed:

    HTTP/1.1 200 OK

    Access-Control-Allow-Origin: *

    Cross-Origin-Opener-Policy: same-origin

    Cross-Origin-Resource-Policy: same-origin

    Origin-Agent-Cluster: ?1

This confirmed that the HTTP endpoint was responding and that the expected response headers were present.

---

## 7. DNS Health Check

DNS resolution was tested using:

    getent hosts google.com

Observed:

    2404:6800:4007:833::200e google.com

This confirms that the system was able to resolve:

    google.com
        ↓
    IPv6 address

Therefore basic DNS resolution was working.

---

## 8. External DNS Server Check

The external DNS resolver was tested with:

    nslookup google.com 8.8.8.8

Observed:

    Server: 8.8.8.8
    Address: 8.8.8.8#53

and:

    Address: 142.251.222.174

    Address: 2404:6800:4007:838::200e

This confirmed that Google DNS could successfully resolve the domain.

---

## 9. System DNS vs External DNS

| Test | Result |
|---|---|
| System resolver | Successful |
| External resolver `8.8.8.8` | Successful |
| IPv4 resolution | Successful |
| IPv6 resolution | Successful |

This gives stronger confidence that the DNS problem was not present.

---

## 10. TLS Health Check

TLS was tested using OpenSSL:

    openssl s_client -connect google.com:443 -servername google.com

Important output:

    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384

    Protocol: TLSv1.3

    Verify return code: 0 (ok)

This confirms that the TLS connection was successfully established.

---

## 11. TLS Troubleshooting Interpretation

The output can be interpreted as:

    Connection to port 443
        ↓
    TLS handshake
        ↓
    TLSv1.3 negotiated
        ↓
    Cipher selected
        ↓
    Certificate verified
        ↓
    Verify return code: 0
        ↓
    TLS healthy

---

## 12. Certificate Verification

The certificate verification result was:

    Verify return code: 0 (ok)

This means OpenSSL successfully verified the certificate chain for the tested connection.

The certificate subject was:

    subject=CN=*.google.com

The issuer was:

    Google Trust Services

Therefore the certificate identity and trust verification were successful during the test.

---

## 13. HTTPS Health Check

HTTPS was tested using:

    curl -I https://google.com

Observed:

    HTTP/2 301

    location: https://www.google.com/

    content-type: text/html; charset=UTF-8

The HTTPS connection itself was successful.

The server returned a redirect from:

    https://google.com

to:

    https://www.google.com/

---

## 14. Why 301 Is Not a TLS Error

The result:

    HTTP/2 301

does not mean that HTTPS failed.

The sequence was:

    DNS successful
        ↓
    TCP connection successful
        ↓
    TLS handshake successful
        ↓
    Certificate verified
        ↓
    HTTPS request successful
        ↓
    Server returned 301 redirect

Therefore:

    TLS = successful
    HTTPS = successful
    HTTP response = redirect

---

## 15. Local Project Port Verification

The project backend port was checked using:

    ss -lntp | grep ':5001'

Observed:

    LISTEN 0 511 *:5001 *:* users:(("MainThread",pid=16177,fd=24))

This confirms that the application was actively listening on:

    port 5001

Therefore the local backend was reachable.

---

## 16. HTTP Troubleshooting Decision Flow

If the application does not respond:

    Check port
        ↓
    Is port listening?
        |
        +-- No → Check application/service
        |
        +-- Yes
             ↓
         Test HTTP
             |
             +-- 200 → HTTP working
             |
             +-- 404 → Check route/resource
             |
             +-- 4xx → Check request/client
             |
             +-- 5xx → Check server/application

---

## 17. DNS Troubleshooting Decision Flow

If a domain cannot be reached:

    Check system resolution
        ↓
    getent hosts domain
        |
        +-- Works → DNS probably working
        |
        +-- Fails
             ↓
         nslookup domain
             |
             +-- Works → Check local resolver
             |
             +-- Fails
                  ↓
              Test external DNS
                  |
                  ↓
              nslookup domain 8.8.8.8

This separates local DNS configuration problems from broader DNS problems.

---

## 18. TLS Troubleshooting Decision Flow

If HTTPS fails:

    Check port 443
        ↓
    Test TLS handshake
        ↓
    Check TLS version
        ↓
    Check certificate
        ↓
    Check certificate verification
        ↓
    Test HTTPS with curl

Important outputs:

    TLSv1.3
    Verify return code: 0 (ok)

indicate a healthy TLS connection.

---

## 19. Common Failure Interpretation

| Symptom | Likely Layer |
|---|---|
| Domain cannot resolve | DNS |
| DNS works but port unreachable | Network / Port |
| Port works but TLS handshake fails | TLS |
| Certificate verification fails | Certificate / Trust |
| HTTPS connects but returns 404 | HTTP / Application |
| HTTPS connects but returns 500 | Application / Server |
| HTTP works but browser blocks cross-origin request | CORS |
| Response has unexpected data type | Content-Type / Application |

---

## 20. Complete Troubleshooting Comparison

| Layer | What It Does | Test Used |
|---|---|---|
| DNS | Domain to IP | `getent`, `host`, `nslookup` |
| TCP / Port | Establishes network connection | `ss` |
| TLS | Secures connection | `openssl s_client` |
| HTTPS | Secure HTTP communication | `curl -I https://...` |
| HTTP | Application request/response | `curl` |
| Express | Handles project routes | `/api`, `/api/users` |

---

## 21. Final Verification

Step 20.9 performed the final project verification.

### Backend Port

Observed:

    LISTEN 0 511 *:5001 *:*

Result:

    Project backend listening successfully.

---

## 22. Final HTTP Verification

Project HTTP test:

    curl -s -o /dev/null -w "HTTP Status: %{http_code}\n" http://localhost:5001/api

Observed:

    HTTP Status: 200

Result:

    Project HTTP layer is working.

---

## 23. Final API Verification

Users API test:

    curl -s -o /dev/null -w "Users API Status: %{http_code}\n" http://localhost:5001/api/users

Observed:

    Users API Status: 200

Result:

    Actual project API is responding successfully.

This is more meaningful than only checking the root route because it verifies an actual application endpoint.

---

## 24. Final DNS Verification

Observed:

    google.com → IPv6 address

External DNS server:

    8.8.8.8

Observed IPv4 and IPv6 results.

Result:

    DNS resolution successful.

---

## 25. Final TLS Verification

Observed:

    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384

    Protocol: TLSv1.3

    Verify return code: 0 (ok)

Result:

    TLS connection successful.
    Certificate verification successful.

---

## 26. Final HTTPS Verification

Observed:

    HTTP/2 301

    location: https://www.google.com/

Result:

    HTTPS connection successful.

The 301 response indicates a redirect to the canonical www address.

---

## 27. Final Verification Results

| Component | Result | Evidence |
|---|---|---|
| Backend Port | PASS | `*:5001` listening |
| Project HTTP | PASS | `HTTP Status: 200` |
| Users API | PASS | `Users API Status: 200` |
| DNS | PASS | Domain resolved |
| External DNS | PASS | `8.8.8.8` returned addresses |
| TLS | PASS | `TLSv1.3` |
| Certificate | PASS | `Verify return code: 0` |
| HTTPS | PASS | `HTTP/2 301` |
| Step 20 | PASS | All required verification completed |

---

## 28. Complete Step 20 Network Workflow

The practical work demonstrated the following complete flow:

    User / Client
          |
          v
    Domain Name
          |
          v
    DNS Resolution
          |
          v
    IP Address
          |
          v
    TCP Connection
          |
          v
    Port 443 for HTTPS
          |
          v
    TLS Handshake
          |
          v
    Certificate Verification
          |
          v
    Secure TLS Connection
          |
          v
    HTTPS Request
          |
          v
    HTTP Response

For the local MERN application:

    Client
       |
       v
    localhost
       |
       v
    Port 5001
       |
       v
    Express
       |
       v
    API Route
       |
       v
    Controller
       |
       v
    Database / Redis
       |
       v
    HTTP Response

---

## 29. Step 20 Practical Coverage

| Substep | Focus | Result |
|---|---|---|
| 20.1 | HTTP Fundamentals + Request/Response | Complete |
| 20.2 | HTTP Methods + Status Codes | Complete |
| 20.3 | Headers, Cookies + Content-Type | Complete |
| 20.4 | DNS Fundamentals + Resolution | Complete |
| 20.5 | DNS Tools + Troubleshooting | Complete |
| 20.6 | TLS/SSL Fundamentals | Complete |
| 20.7 | HTTPS + Certificate Verification | Complete |
| 20.8 | HTTP/DNS/TLS Troubleshooting | Complete |
| 20.9 | Final Project Verification | Complete |

---

## 30. What Step 20 Demonstrated

Step 20 connected several layers that are often studied separately.

    DNS
    ↓
    Finds the destination IP

    TCP / Port
    ↓
    Establishes network connectivity

    TLS
    ↓
    Secures the connection

    HTTPS
    ↓
    Carries HTTP securely

    HTTP
    ↓
    Handles application requests and responses

    Express
    ↓
    Handles project API routes

This provides the foundation for understanding how real web applications communicate over networks.

---

## 31. Final Key Takeaways

1. DNS resolves domain names to IP addresses.

2. TCP connectivity must exist before higher-level communication can work.

3. TLS secures communication between client and server.

4. Certificates help verify server identity.

5. HTTPS is HTTP over TLS.

6. HTTP status codes describe the result of requests.

7. Headers provide metadata and instructions.

8. CORS controls browser cross-origin access.

9. Express handles the project's HTTP API.

10. The project backend listens on port 5001.

11. DNS, TCP, TLS and HTTP represent different layers of the communication process.

12. Troubleshooting should proceed layer by layer instead of assuming every failure is an application problem.

---

## 32. Final Step 20 Verification

The final verification produced:

    HTTP: OK
    DNS: OK
    TLS: OK
    HTTPS: OK
    Project Port: OK

Therefore:

    Step 20 — COMPLETE


