# Step 21 — Nginx

## Part 1 — Fundamentals, Architecture and Configuration Concepts

---

## 1. Objective

The objective of Step 21 is to understand and practically configure Nginx as a web server and reverse proxy in front of the existing MERN backend application.

This step was performed on the main Phase 1 project:

```
/home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack
```

The documentation is maintained separately in:

```
/home/afroza/Projects/devops-learning/Step-21-Nginx/
```

The main practical objectives were:

| Objective                          | Practical Result                                                            |
| ---------------------------------- | --------------------------------------------------------------------------- |
| Install and inspect Nginx          | Nginx 1.28.3 was already installed                                          |
| Understand Nginx service           | nginx.service verified as active                                            |
| Understand configuration structure | nginx.conf, sites-available and sites-enabled inspected                     |
| Understand server blocks           | A custom blog-app server block was created                                  |
| Serve static content               | Static content was served through Nginx                                     |
| Understand reverse proxy           | Nginx was configured to forward requests to Express                         |
| Integrate with Express             | Nginx :80 was connected to Express :5001                                    |
| Understand proxy headers           | Forwarding headers were configured                                          |
| Understand logs                    | Access and error logs were inspected                                        |
| Troubleshoot Nginx                 | Configuration, ports, service, logs and upstream connectivity were verified |
| Verify complete request flow       | blog.local was resolved locally and tested from the browser                 |

---

## 2. Environment

| Component        | Actual Environment |
| ---------------- | ------------------ |
| Operating System | Ubuntu             |
| Nginx            | 1.28.3             |
| Node.js          | v24.19.0           |
| npm              | 11.17.0            |
| Backend          | Node.js + Express  |
| Backend Port     | 5001               |
| Nginx HTTP Port  | 80                 |
| Local Hostname   | blog.local         |
| Database         | MongoDB            |
| Cache            | Redis              |

---

## 3. Project Paths

The DevOps learning project uses separate paths for documentation, main project practice, general Linux practice and supplementary labs.

| Purpose                    | Path                                                                 | Usage                                              |
| -------------------------- | -------------------------------------------------------------------- | -------------------------------------------------- |
| Documentation Repository   | /home/afroza/Projects/devops-learning                                | Professional documentation and Git repository      |
| Phase 1 Main Project       | /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack | Real MERN project used for applicable DevOps steps |
| General Linux Practice     | /home/afroza/Linux-Practice                                          | General Linux practice                             |
| Phase 2 Supplementary Labs | /home/afroza/Projects/devops-supplementary-labs                      | Supplementary Step 16, 17, 26 and 27 labs          |

Step 21 uses the Phase 1 main project because Nginx naturally integrates with the existing Express backend.

---

## 4. What is Nginx?

Nginx is a high-performance web server that can also operate as a reverse proxy.

In this project, Nginx is placed in front of the Express backend.

The simplified architecture is:

```
Client
   |
   v
Nginx :80
   |
   v
Express :5001
```

Nginx receives the HTTP request and can either serve the requested content itself or forward the request to another application server.

---

## 5. Why Nginx is Used

Nginx is not required for Express to work.

The application can operate directly as:

```
Client
   |
   v
Express :5001
```

However, a reverse proxy provides a separate public-facing web server layer.

In the project architecture:

```
Client
   |
   v
Nginx :80
   |
   v
Express :5001
```

The responsibilities are separated.

| Nginx                                       | Express                           |
| ------------------------------------------- | --------------------------------- |
| Receives public HTTP requests               | Runs application logic            |
| Acts as reverse proxy                       | Handles API routes                |
| Can serve static files                      | Handles controllers               |
| Provides a public entry point               | Communicates with databases       |
| Can forward request information             | Processes application requests    |
| Can handle TLS in a production architecture | Handles backend application logic |

Nginx therefore does not replace Express.

Nginx sits in front of Express and controls how external requests reach the application.

---

## 6. Nginx vs Express

| Feature             | Nginx                                          | Express                                                 |
| ------------------- | ---------------------------------------------- | ------------------------------------------------------- |
| Type                | Web server / reverse proxy                     | Backend web framework                                   |
| Main role           | Receive and route HTTP requests                | Build and run backend application                       |
| Application logic   | No                                             | Yes                                                     |
| API routes          | Does not normally implement application routes | Yes                                                     |
| Static file serving | Yes                                            | Possible, but not its primary role in this architecture |
| Reverse proxy       | Yes                                            | No                                                      |
| Database logic      | No                                             | Yes                                                     |
| Public entry point  | Yes                                            | Usually behind Nginx in production                      |
| Project port        | 80                                             | 5001                                                    |

---

## 7. Port 80 vs Port 5001

The project uses two different ports for two different responsibilities.

| Port | Component | Role                    |
| ---- | --------- | ----------------------- |
| 80   | Nginx     | Public HTTP entry point |
| 5001 | Express   | Backend application     |

The final request flow is:

```
Browser
   |
   | HTTP :80
   v
Nginx
   |
   | proxy_pass
   v
Express :5001
```

Port 80 belongs to Nginx.

Port 5001 belongs to the Express application.

This separation allows the public request layer and application layer to have different responsibilities.

---

## 8. Reverse Proxy

A reverse proxy receives a request from the client and forwards that request to an internal backend server.

In this project:

```
Client
   |
   | Request
   v
Nginx :80
   |
   | proxy_pass
   v
Express :5001
```

The client communicates with Nginx.

Nginx communicates with Express.

The client does not need to directly access port 5001.

---

## 9. Why Reverse Proxy is Useful

| Without Reverse Proxy                              | With Reverse Proxy                                  |
| -------------------------------------------------- | --------------------------------------------------- |
| Client directly accesses Express                   | Client accesses Nginx                               |
| Express port may be directly exposed               | Nginx provides the public entry point               |
| Application handles direct external requests       | Nginx handles the external HTTP layer               |
| Static files may be handled by application         | Nginx can serve static files                        |
| No proxy layer                                     | Backend can be placed behind a proxy                |
| Less separation between web and application layers | Clear separation between web and application layers |

Nginx becomes especially useful when additional production requirements are introduced, such as HTTPS termination, static file serving, multiple backend instances or load balancing.

Those advanced configurations were not implemented in this step.

---

## 10. Static Server vs Reverse Proxy

Nginx was first used as a static file server during the practical lab.

Static serving flow:

```
Client
   |
   v
Nginx :8080
   |
   v
/var/www/blog-app/index.html
```

The static server configuration used a document root and served the requested file directly.

The reverse proxy configuration uses a backend upstream:

```
Client
   |
   v
Nginx :80
   |
   v
Express :5001
```

| Static Server                     | Reverse Proxy                              |
| --------------------------------- | ------------------------------------------ |
| Nginx serves a file               | Nginx forwards the request                 |
| Uses root                         | Uses proxy_pass                            |
| Example: /var/www/blog-app        | Example: 127.0.0.1:5001                    |
| Nginx generates the file response | Express generates the application response |
| Useful for static content         | Useful for backend applications            |

---

## 11. Nginx Configuration Structure

The main Nginx configuration file in this environment is:

```
/etc/nginx/nginx.conf
```

The Nginx build configuration confirmed:

```
--conf-path=/etc/nginx/nginx.conf
```

The main configuration includes additional configuration files.

Important structure:

```
/etc/nginx/
|
+-- nginx.conf
|
+-- conf.d/
|
+-- sites-available/
|
+-- sites-enabled/
|
+-- snippets/
|
+-- mime.types
|
+-- proxy_params
```

The main configuration contains:

```
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```

Therefore, configurations inside enabled locations become part of the effective Nginx configuration.

---

## 12. Important Nginx Configuration Locations

| Location                    | Purpose                                      |
| --------------------------- | -------------------------------------------- |
| /etc/nginx/nginx.conf       | Main Nginx configuration                     |
| /etc/nginx/conf.d/          | Additional Nginx configuration files         |
| /etc/nginx/sites-available/ | Available virtual host/server configurations |
| /etc/nginx/sites-enabled/   | Enabled server configurations                |
| /etc/nginx/snippets/        | Reusable configuration fragments             |
| /var/log/nginx/access.log   | Request access log                           |
| /var/log/nginx/error.log    | Error and warning log                        |

---

## 13. sites-available vs sites-enabled

Ubuntu's Nginx layout commonly separates available configurations from enabled configurations.

The configuration created for the project is:

```
/etc/nginx/sites-available/blog-app
```

It was enabled through:

```
/etc/nginx/sites-enabled/blog-app
```

The enabled entry is a symbolic link:

```
/etc/nginx/sites-enabled/blog-app
    |
    +----> /etc/nginx/sites-available/blog-app
```

| Directory       | Purpose                                              |
| --------------- | ---------------------------------------------------- |
| sites-available | Stores available server configurations               |
| sites-enabled   | Contains configurations that Nginx should load       |
| Symbolic link   | Connects an enabled configuration to its source file |

This separation allows a server configuration to exist without necessarily being active.

---

## 14. Server Block

A server block defines how Nginx should handle a particular set of requests.

The final project server block contains:

```
server {
    listen 80;
    listen [::]:80;

    server_name blog.local;

    location / {
        proxy_pass http://127.0.0.1:5001;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Important directives:

| Directive        | Purpose                                          |
| ---------------- | ------------------------------------------------ |
| server           | Defines a server block                           |
| listen           | Defines the port/interface for requests          |
| server_name      | Defines the hostname handled by the server block |
| location         | Defines how matching request paths are handled   |
| proxy_pass       | Sends requests to the backend                    |
| proxy_set_header | Controls headers forwarded to the backend        |

---

## 15. listen

The final configuration uses:

```
listen 80;
listen [::]:80;
```

The first line handles IPv4 HTTP traffic.

The second line handles IPv6 HTTP traffic.

| Configuration  | Meaning                                 |
| -------------- | --------------------------------------- |
| listen 80      | Listen for IPv4 HTTP traffic on port 80 |
| listen [::]:80 | Listen for IPv6 HTTP traffic on port 80 |

The initial learning configuration used port 8080 to avoid interfering with the default Nginx site.

After the reverse proxy was verified, the project configuration was moved to port 80.

---

## 16. server_name

The project uses:

```
server_name blog.local;
```

This allows Nginx to select the server block when the HTTP Host header is:

```
blog.local
```

The hostname was mapped locally using:

```
127.0.0.1 blog.local
```

in:

```
/etc/hosts
```

The final local request therefore becomes:

```
Browser
   |
   | blog.local
   v
/etc/hosts
   |
   | 127.0.0.1
   v
Nginx :80
```

---

## 17. location

The final configuration uses:

```
location / {
    proxy_pass http://127.0.0.1:5001;
}
```

The `/` location matches requests beginning from the root path and forwards them to the Express backend.

For example:

```
http://blog.local/api/blogs/
```

matches:

```
location /
```

and is forwarded to:

```
http://127.0.0.1:5001/api/blogs/
```

---

## 18. proxy_pass

The most important reverse proxy directive is:

```
proxy_pass http://127.0.0.1:5001;
```

Its purpose is to forward the incoming request to the Express backend.

Request flow:

```
Client
   |
   | GET /api/blogs/
   v
Nginx :80
   |
   | proxy_pass
   v
Express :5001
   |
   v
Application response
```

The backend port remains 5001, while the client communicates with Nginx on port 80.

---

## 19. Proxy Headers

The project configuration forwards important request information to Express.

```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

| Header            | Purpose                                              |
| ----------------- | ---------------------------------------------------- |
| Host              | Preserves the requested hostname                     |
| X-Real-IP         | Provides the client IP observed by Nginx             |
| X-Forwarded-For   | Preserves client IP information through proxy chains |
| X-Forwarded-Proto | Indicates the original request protocol              |

These headers become useful when the backend needs information about the original client request.

---

## 20. Nginx and Express Request Flow

The final project request flow is:

```
Browser
   |
   | http://blog.local
   v
/etc/hosts
   |
   | 127.0.0.1
   v
Nginx :80
   |
   | server_name blog.local
   |
   | location /
   |
   | proxy_pass
   v
Express :5001
   |
   +----> MongoDB
   |
   +----> Redis
   |
   v
Response
   |
   v
Nginx
   |
   v
Browser
```

This separates the public HTTP layer from the backend application layer.

---

## 21. Nginx Service and Process Architecture

Nginx is managed through systemd.

The service is:

```
nginx.service
```

The running process architecture observed during the lab was:

```
nginx master process
    |
    +-- nginx worker process
    |
    +-- nginx worker process
```

| Component             | Role                                               |
| --------------------- | -------------------------------------------------- |
| systemd nginx.service | Starts, stops, reloads and manages Nginx           |
| Master process        | Controls Nginx workers and configuration lifecycle |
| Worker process        | Handles client connections and requests            |

The Nginx service was verified as:

```
active (running)
```

and configured as enabled through systemd.

---

## 22. Static File Serving Practical Flow

Before configuring the reverse proxy, Nginx was tested as a static server.

A document root was created:

```
/var/www/blog-app
```

An index file was created:

```
/var/www/blog-app/index.html
```

The temporary server block listened on port 8080.

The request flow was:

```
Client
   |
   v
Nginx :8080
   |
   v
/var/www/blog-app/index.html
   |
   v
HTTP 200 OK
```

The response body confirmed:

```
Nginx Step 21.3 static server
```

This verified that the custom server block could serve static content successfully.

---

## 23. Reverse Proxy Practical Flow

After static serving was verified, the same custom server configuration was changed to use:

```
proxy_pass http://127.0.0.1:5001;
```

The temporary reverse proxy flow was:

```
Client
   |
   v
Nginx :8080
   |
   v
Express :5001
```

The direct Express request and Nginx-proxied request produced the same application response.

Direct request:

```
curl http://127.0.0.1:5001/api/blogs/
```

Reverse proxy request:

```
curl -H "Host: blog.local" http://127.0.0.1:8080/api/blogs/
```

Both returned the same application-level 404 response.

The identical application response confirmed that Nginx was forwarding the request to Express.

---

## 24. Why the 404 Response Was Not an Nginx Failure

During verification, the `/api/blogs/` request returned:

```
HTTP/1.1 404 Not Found
```

with:

```
{"statusCode":404,"data":null,"success":false,"errors":[]}
```

The same response was received from direct Express access.

Therefore the result proved:

```
Direct Express :5001
    |
    +----> HTTP 404

Nginx :80
    |
    +----> Express :5001
                |
                +----> HTTP 404
```

The 404 was application behavior rather than an Nginx connectivity failure.

The purpose of the test was to verify that the same request reached Express through Nginx.

---

## 25. Final Step 21 Architecture

The completed architecture is:

```
Browser
   |
   | http://blog.local
   v
/etc/hosts
   |
   | 127.0.0.1
   v
Nginx :80
   |
   | Reverse Proxy
   |
   | proxy_pass
   v
Express :5001
   |
   +----------------+
   |                |
   v                v
MongoDB           Redis
```

Nginx is the public HTTP entry point.

Express remains responsible for backend application logic.

MongoDB remains responsible for persistent application data.

Redis remains responsible for the application's Redis functionality.

---

## 26. Part 1 Summary

| Topic           | What Was Learned               | Practical Status |
| --------------- | ------------------------------ | ---------------- |
| Nginx           | Web server and reverse proxy   | Completed        |
| Nginx Service   | Managed by systemd             | Completed        |
| Master/Worker   | Nginx process architecture     | Observed         |
| Configuration   | nginx.conf and includes        | Inspected        |
| Server Block    | Virtual server configuration   | Created          |
| Static Serving  | Nginx serving files            | Verified         |
| Reverse Proxy   | Nginx forwarding to Express    | Verified         |
| proxy_pass      | Backend forwarding directive   | Used             |
| Proxy Headers   | Request information forwarding | Configured       |
| Port 80         | Public HTTP entry              | Configured       |
| Port 5001       | Express backend                | Verified         |
| blog.local      | Local hostname                 | Configured       |
| Request Flow    | Browser to Nginx to Express    | Verified         |
| Logs            | Access and error logs          | Inspected        |
| Troubleshooting | Nginx diagnostic workflow      | Practiced        |

Part 1 establishes the conceptual foundation and architecture of the Nginx setup. Command details, flags, configuration operations, troubleshooting commands and the complete practical command record are covered separately in Part 2.


## Part 2 — Commands, Configuration and Practical Operations

---

## 1. Nginx Version and Environment Commands

The first practical task was to inspect the existing Nginx environment before making any changes.

### 1.1 Check Nginx Version

```
nginx -v 2>&1
```

Purpose:

This command verifies whether Nginx is installed and displays its version.

Actual result:

```
nginx version: nginx/1.28.3 (Ubuntu)
```

| Command  | Purpose               | Important Output |
| -------- | --------------------- | ---------------- |
| nginx -v | Display Nginx version | nginx/1.28.3     |

The version check confirmed that Nginx was already installed, so reinstallation was unnecessary.

---

### 1.2 Inspect Nginx Build and Configuration Paths

```
nginx -V 2>&1
```

The output was filtered to identify the configuration and installation paths.

```
nginx -V 2>&1 | tr ' ' '\n' | grep -E 'conf-path|prefix'
```

Important output:

```
--prefix=/usr/share/nginx
--conf-path=/etc/nginx/nginx.conf
```

| Option      | Purpose                                     |
| ----------- | ------------------------------------------- |
| -V          | Display detailed build information          |
| 2>&1        | Combine standard error with standard output |
| tr ' ' '\n' | Convert spaces into line-separated output   |
| grep -E     | Search using an extended regular expression |

The main Nginx configuration file was confirmed as:

```
/etc/nginx/nginx.conf
```

---

### 1.3 Display Complete Effective Configuration

```
sudo nginx -T
```

Purpose:

This command tests and displays the complete configuration that Nginx is currently loading.

It is particularly useful when multiple configuration files are included through nginx.conf.

Difference:

| Command  | Purpose                                               |
| -------- | ----------------------------------------------------- |
| nginx -t | Test configuration syntax                             |
| nginx -T | Test and display the complete effective configuration |

During troubleshooting, nginx -T was used to confirm that the custom blog-app server block was actually loaded.

---

## 2. Nginx Service Management

Nginx is managed by systemd through:

```
nginx.service
```

### 2.1 Check Nginx Service

```
systemctl status nginx --no-pager
```

Purpose:

To inspect the current service state, process information and recent service messages.

Important output observed:

```
Active: active (running)
```

| Command                | Purpose                                |
| ---------------------- | -------------------------------------- |
| systemctl status nginx | Show detailed service status           |
| --no-pager             | Prevent output from opening in a pager |

The service was already running, so restarting or reinstalling Nginx was unnecessary.

---

### 2.2 Check Service State Only

```
systemctl is-active nginx
```

Purpose:

Returns a concise service state.

Actual final result:

```
active
```

This is useful for scripts and quick verification.

---

### 2.3 Reload Nginx

```
sudo systemctl reload nginx
```

Purpose:

Loads updated configuration without completely stopping the Nginx service.

Reload was used after changing the server configuration.

Difference:

| Operation | Effect                                                        |
| --------- | ------------------------------------------------------------- |
| reload    | Apply configuration changes while keeping the service running |
| restart   | Stop and start the service again                              |
| stop      | Stop Nginx                                                    |
| start     | Start Nginx                                                   |

For normal configuration changes, reload is preferred because it avoids an unnecessary complete service restart.

---

## 3. Nginx Process Inspection

The Nginx process architecture was inspected using:

```
ps aux | grep '[n]ginx'
```

The output showed:

```
nginx: master process
nginx: worker process
nginx: worker process
```

### Important ps Options

| Option | Meaning                                          |
| ------ | ------------------------------------------------ |
| a      | Show processes for all users                     |
| u      | User-oriented output format                      |
| x      | Include processes without a controlling terminal |

The master process was running as root, while the worker processes were running as www-data.

| Process        | Role                                                 |
| -------------- | ---------------------------------------------------- |
| Master process | Controls workers and manages Nginx process lifecycle |
| Worker process | Handles client connections and requests              |

---

## 4. Port Inspection

The listening ports were inspected using:

```
sudo ss -lntp
```

Important final ports:

```
0.0.0.0:80
[::]:80
*:5001
```

The temporary port 8080 was used during the learning phase and was removed from the final configuration.

### ss Flags

| Flag | Meaning                           |
| ---- | --------------------------------- |
| -l   | Show listening sockets            |
| -n   | Show numeric addresses and ports  |
| -t   | Show TCP sockets                  |
| -p   | Show the process using the socket |

The final architecture was confirmed as:

```
Nginx -> :80
Express -> :5001
```

---

## 5. Nginx Configuration Inspection

### 5.1 Find Configuration Files

```
sudo find /etc/nginx -maxdepth 2 -type f -print | sort
```

Purpose:

To inspect the available Nginx configuration files.

Important files found included:

```
/etc/nginx/nginx.conf
/etc/nginx/sites-available/default
/etc/nginx/proxy_params
/etc/nginx/mime.types
```

---

### 5.2 List Available Sites

```
sudo ls -la /etc/nginx/sites-available/
```

Purpose:

To inspect server configurations that are available but not necessarily enabled.

Initially the default configuration existed:

```
/etc/nginx/sites-available/default
```

Later the project configuration was created:

```
/etc/nginx/sites-available/blog-app
```

---

### 5.3 List Enabled Sites

```
sudo ls -la /etc/nginx/sites-enabled/
```

Purpose:

To identify which site configurations are enabled.

Initially:

```
default -> /etc/nginx/sites-available/default
```

Later:

```
blog-app -> /etc/nginx/sites-available/blog-app
```

---

### 5.4 Inspect a Configuration File

```
sudo cat /etc/nginx/sites-available/blog-app
```

Purpose:

To read the complete project server block.

The final configuration was:

```
server {
    listen 80;
    listen [::]:80;

    server_name blog.local;

    location / {
        proxy_pass http://127.0.0.1:5001;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

### 5.5 Inspect a Specific Range of Lines

```
sudo sed -n '1,220p' /etc/nginx/nginx.conf
```

Purpose:

Display a selected range of lines without opening an editor.

| Command  | Purpose                   |
| -------- | ------------------------- |
| sed -n   | Selectively print output  |
| '1,220p' | Print lines 1 through 220 |

The command was used to inspect nginx.conf without modifying it.

---

## 6. Nginx Configuration Testing

The configuration was tested repeatedly before and after changes.

```
sudo nginx -t
```

Successful output:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Meaning:

| Output             | Meaning                                         |
| ------------------ | ----------------------------------------------- |
| syntax is ok       | Nginx can parse the configuration syntax        |
| test is successful | Configuration validation completed successfully |

The configuration was never reloaded after a change until nginx -t confirmed that it was valid.

---

## 7. Initial Default Nginx Server

Before creating the project configuration, the default server block was inspected.

The default configuration contained:

```
listen 80 default_server;
listen [::]:80 default_server;

root /var/www/html;

index index.html index.htm index.nginx-debian.html;

server_name _;

location / {
    try_files $uri $uri/ =404;
}
```

This configuration served:

```
/var/www/html/index.nginx-debian.html
```

The default page returned:

```
HTTP/1.1 200 OK
```

This established the baseline that Nginx was already functioning before project integration.

---

## 8. Static Server Practical Configuration

A temporary static web root was created:

```
sudo mkdir -p /var/www/blog-app
```

A test HTML file was created:

```
echo "Nginx Step 21.3 static server" | sudo tee /var/www/blog-app/index.html > /dev/null
```

A temporary server block was created for port 8080.

Configuration:

```
server {
    listen 8080;
    listen [::]:8080;

    server_name blog.local;

    root /var/www/blog-app;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Purpose of each directive:

| Directive              | Purpose                                              |
| ---------------------- | ---------------------------------------------------- |
| listen 8080            | Listen for HTTP requests on port 8080                |
| server_name blog.local | Match requests for blog.local                        |
| root                   | Define static document root                          |
| index                  | Define default index file                            |
| location /             | Match requests under the root path                   |
| try_files              | Check whether the requested file or directory exists |

The configuration was enabled using:

```
sudo ln -s /etc/nginx/sites-available/blog-app /etc/nginx/sites-enabled/blog-app
```

The configuration was then tested:

```
sudo nginx -t
```

And reloaded:

```
sudo systemctl reload nginx
```

Static serving was verified with:

```
curl -i -H "Host: blog.local" http://127.0.0.1:8080/
```

The response returned:

```
HTTP/1.1 200 OK
```

and:

```
Nginx Step 21.3 static server
```

This confirmed that the custom server block successfully served static content.

---

## 9. Express Backend Verification

Before configuring reverse proxying, the existing Express backend was inspected.

Project backend path:

```
/home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server
```

The backend port was confirmed in server.js:

```
app.listen(5001, () => console.log("app started at 5001..."));
```

The backend was started using:

```
npm start
```

The startup output confirmed:

```
app started at 5001...
connected!
```

The process was then verified using:

```
sudo ss -lntp | grep ':5001'
```

The result showed that port 5001 was listening.

---

## 10. Direct Express Verification

The direct backend was tested using:

```
curl -i http://127.0.0.1:5001/
```

The response was:

```
HTTP/1.1 404 Not Found
```

with:

```
Cannot GET /
```

This did not indicate that Express was broken.

It proved that:

1. The Express server was running.
2. Port 5001 was reachable.
3. Express received the request.
4. The requested / route was not defined.

---

## 11. Reverse Proxy Configuration

The static server configuration was replaced with a reverse proxy configuration.

The final configuration was:

```
server {
    listen 80;
    listen [::]:80;

    server_name blog.local;

    location / {
        proxy_pass http://127.0.0.1:5001;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

The configuration was tested with:

```
sudo nginx -t
```

Then applied with:

```
sudo systemctl reload nginx
```

---

## 12. Reverse Proxy Verification

The reverse proxy was first tested through port 8080.

```
curl -i \
-H "Host: blog.local" \
http://127.0.0.1:8080/
```

The response changed from the static Nginx page to the Express response:

```
HTTP/1.1 404 Not Found

Cannot GET /
```

This proved that the request was no longer being served by the static file configuration.

The request flow became:

```
Client
   |
   v
Nginx :8080
   |
   | proxy_pass
   v
Express :5001
```

---

## 13. Project API Verification

The project route structure was inspected before selecting an API endpoint.

The project contains:

```
/api/users
```

and:

```
/api/blogs
```

The blog router contains:

```
blogRouter.get("/", getAllBlogs);
```

The direct API request was tested:

```
curl -i http://127.0.0.1:5001/api/blogs/
```

The same endpoint was then tested through Nginx:

```
curl -i \
-H "Host: blog.local" \
http://127.0.0.1:8080/api/blogs/
```

Both returned the same application response:

```
{"statusCode":404,"data":null,"success":false,"errors":[]}
```

The identical result confirmed that Nginx was forwarding the API request to Express.

---

## 14. Proxy Header Configuration

The following headers were configured:

```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

| Configuration                                               | Purpose                             |
| ----------------------------------------------------------- | ----------------------------------- |
| proxy_set_header Host $host                                 | Forward requested hostname          |
| proxy_set_header X-Real-IP $remote_addr                     | Forward client IP observed by Nginx |
| proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for | Preserve forwarded client IP chain  |
| proxy_set_header X-Forwarded-Proto $scheme                  | Forward original HTTP protocol      |

A custom request header was also used during testing:

```
curl -i \
-H "Host: blog.local" \
-H "X-Debug-Client: step21" \
http://127.0.0.1:8080/api/blogs/
```

The request successfully reached the backend.

The X-Debug-Client header was not expected to appear in the response because the Express application did not explicitly return incoming request headers.

---

## 15. Nginx Access Log

The Nginx access log was inspected using:

```
sudo tail -n 10 /var/log/nginx/access.log
```

The log contained requests such as:

```
127.0.0.1 - - [14/Aug/2026:18:38:16 +0600] "GET /api/blogs/ HTTP/1.1" 404 58 "-" "curl/8.18.0"
```

Important fields:

| Field       | Meaning               |
| ----------- | --------------------- |
| 127.0.0.1   | Source IP             |
| GET         | HTTP method           |
| /api/blogs/ | Requested path        |
| HTTP/1.1    | HTTP protocol version |
| 404         | Response status       |
| 58          | Response body size    |
| curl/8.18.0 | Client user agent     |

The access log proved that Nginx received the request.

---

## 16. Nginx Error Log

The error log was inspected using:

```
sudo tail -n 20 /var/log/nginx/error.log
```

No recent error was produced by the successful reverse proxy requests.

This was a positive result.

An empty error log during a successful request does not mean logging is disabled. It means no Nginx error was generated for those requests.

---

## 17. Nginx Upstream Connectivity Test

The backend was tested directly:

```
curl -sS -o /dev/null -w "Express :5001 -> HTTP %{http_code}\n" \
http://127.0.0.1:5001/api/blogs/
```

The result was:

```
Express :5001 -> HTTP 404
```

The same request was tested through Nginx:

```
curl -sS -o /dev/null -w "Nginx :8080 -> HTTP %{http_code}\n" \
-H "Host: blog.local" \
http://127.0.0.1:8080/api/blogs/
```

The result was:

```
Nginx :8080 -> HTTP 404
```

This comparison verified that both the direct backend and reverse proxy path were reachable.

---

## 18. Important curl Options

| Option | Meaning                                    |
| ------ | ------------------------------------------ |
| -i     | Include response headers                   |
| -H     | Add an HTTP request header                 |
| -s     | Silent mode                                |
| -S     | Show errors even in silent mode            |
| -o     | Write response body to a file or /dev/null |
| -w     | Print custom output after the request      |
| -I     | Send a HEAD request and show headers       |

Examples used during the lab:

```
curl -i http://127.0.0.1:5001/

curl -i -H "Host: blog.local" http://127.0.0.1:8080/

curl -sS -o /dev/null -w "%{http_code}\n" http://127.0.0.1:5001/api/blogs/
```

---

## 19. Important Shell Commands Used

| Command  | Purpose                                                 |
| -------- | ------------------------------------------------------- |
| cd       | Change working directory                                |
| pwd      | Display current directory                               |
| ls -la   | Show directory contents including hidden files          |
| find     | Search for files and directories                        |
| cat      | Display complete file contents                          |
| sed -n   | Display selected lines                                  |
| grep     | Search text                                             |
| tail -n  | Display the last N lines                                |
| ps       | Inspect processes                                       |
| ss       | Inspect network sockets                                 |
| curl     | Send HTTP requests                                      |
| getent   | Query system databases including hostname resolution    |
| ln -s    | Create a symbolic link                                  |
| cp       | Create a backup copy                                    |
| rm       | Remove a file or symbolic link                          |
| tee      | Write command output to a file                          |
| mkdir -p | Create directories including missing parent directories |

---

## 20. Important Nginx Directives

| Directive        | Actual Purpose in Step 21                 |
| ---------------- | ----------------------------------------- |
| listen           | Define the Nginx listening port           |
| server_name      | Match blog.local requests                 |
| server           | Define a virtual server configuration     |
| location         | Match incoming request paths              |
| root             | Define static content directory           |
| index            | Define default static index file          |
| try_files        | Check static file and directory existence |
| proxy_pass       | Forward requests to Express               |
| proxy_set_header | Forward important request information     |
| include          | Load additional configuration files       |
| error_log        | Define Nginx error log                    |
| access_log       | Define Nginx access log                   |

---

## 21. Hosts File Configuration

The browser initially returned a server resolution error for:

```
http://blog.local
```

The problem was that the hostname did not yet resolve to the local machine.

The existing hosts file was checked:

```
grep -nE 'blog\.local|127\.0\.0\.1' /etc/hosts
```

The hostname was then added:

```
echo '127.0.0.1 blog.local' | sudo tee -a /etc/hosts > /dev/null
```

The hostname was verified:

```
getent hosts blog.local
```

Actual result:

```
127.0.0.1 blog.local
```

The hostname could then be resolved locally.

---

## 22. Hostname-Based Nginx Verification

After configuring /etc/hosts, the request was tested using:

```
curl -i http://blog.local/api/blogs/
```

The response contained:

```
Server: nginx/1.28.3 (Ubuntu)
```

and the Express application response:

```
{"statusCode":404,"data":null,"success":false,"errors":[]}
```

This confirmed the complete local hostname path:

```
blog.local
    |
    v
127.0.0.1
    |
    v
Nginx :80
    |
    v
Express :5001
```

---

## 23. Final Port 80 Integration

The temporary 8080 configuration was used only during the learning phase.

A backup of the configuration was created:

```
sudo cp /etc/nginx/sites-available/blog-app /etc/nginx/sites-available/blog-app.port8080
```

The default Nginx site was disabled:

```
sudo rm /etc/nginx/sites-enabled/default
```

The project server block was changed from 8080 to 80.

The final configuration became:

```
listen 80;
listen [::]:80;
```

The configuration was tested:

```
sudo nginx -t
```

Then Nginx was reloaded:

```
sudo systemctl reload nginx
```

The final listening ports showed:

```
0.0.0.0:80
[::]:80
*:5001
```

Port 8080 was no longer used by the final configuration.

---

## 24. Final End-to-End Verification

The final direct Express verification:

```
curl -sS -o /dev/null -w "Express :5001 -> HTTP %{http_code}\n" \
http://127.0.0.1:5001/api/blogs/
```

Result:

```
Express :5001 -> HTTP 404
```

The final Nginx verification:

```
curl -i \
-H "Host: blog.local" \
http://127.0.0.1/api/blogs/
```

Result:

```
HTTP/1.1 404 Not Found
```

with the Express application response:

```
{"statusCode":404,"data":null,"success":false,"errors":[]}
```

The configuration was finally tested again:

```
sudo nginx -t
```

Result:

```
syntax is ok
test is successful
```

---

## 25. Browser Verification

After configuring:

```
127.0.0.1 blog.local
```

the browser was opened at:

```
http://blog.local
```

The browser displayed:

```
Cannot GET /
```

This was expected because the Express application does not define a GET / route.

The browser result nevertheless proved the complete network path:

```
Browser
   |
   | http://blog.local
   v
/etc/hosts
   |
   | 127.0.0.1
   v
Nginx :80
   |
   | proxy_pass
   v
Express :5001
   |
   v
Cannot GET /
```

The browser was therefore able to reach Express through Nginx.

---

## 26. Common Troubleshooting Workflow

The following workflow was established during Step 21.

| Problem                               | Command / Check           | What It Determines              |
| ------------------------------------- | ------------------------- | ------------------------------- |
| Nginx is not running                  | systemctl status nginx    | Service state                   |
| Need a quick service check            | systemctl is-active nginx | Active/inactive state           |
| Configuration may be invalid          | sudo nginx -t             | Configuration syntax            |
| Need complete effective configuration | sudo nginx -T             | Loaded configuration            |
| Need to inspect processes             | ps aux                    | Nginx master and workers        |
| Port is not responding                | sudo ss -lntp             | Listening sockets and processes |
| Backend may be down                   | curl :5001                | Express availability            |
| Reverse proxy may be failing          | curl :80                  | Nginx proxy path                |
| Need request evidence                 | access.log                | Whether Nginx received request  |
| Nginx may have an error               | error.log                 | Nginx errors and warnings       |
| Hostname does not resolve             | getent hosts blog.local   | Local hostname mapping          |
| Configuration file may be wrong       | cat / sed                 | Actual configuration contents   |

---

## 27. Troubleshooting Logic

The most important diagnostic distinction is between Nginx and the upstream application.

### Case 1: Nginx is down

```
systemctl is-active nginx
```

If the result is not active, the problem is with the Nginx service.

### Case 2: Configuration is invalid

```
sudo nginx -t
```

If the test fails, the configuration must be corrected before reload.

### Case 3: Express is down

```
curl http://127.0.0.1:5001/
```

If the connection fails, Nginx cannot successfully proxy requests to Express.

### Case 4: Express works but proxy fails

If:

```
curl http://127.0.0.1:5001/
```

works but:

```
curl http://127.0.0.1/
```

fails,

then Nginx configuration, server block selection, port listening or upstream configuration should be investigated.

### Case 5: Hostname does not resolve

```
getent hosts blog.local
```

If no address is returned, /etc/hosts or DNS resolution must be checked.

### Case 6: Request is received but returns 404

Check access.log and compare the same request directly against Express.

If both return the same application response, the 404 may be generated by Express rather than Nginx.

---

## 28. Important Command and Flag Reference

### Nginx

| Command  | Purpose                                  |
| -------- | ---------------------------------------- |
| nginx -v | Show version                             |
| nginx -V | Show build and configuration information |
| nginx -t | Test configuration                       |
| nginx -T | Test and display effective configuration |

### systemctl

| Command                   | Purpose                      |
| ------------------------- | ---------------------------- |
| systemctl status nginx    | Detailed service information |
| systemctl is-active nginx | Quick service state          |
| systemctl reload nginx    | Reload configuration         |

### ss

| Command  | Purpose                                  |
| -------- | ---------------------------------------- |
| ss -lntp | Show listening TCP sockets and processes |

### ps

| Command        | Purpose                                                         |
| -------------- | --------------------------------------------------------------- |
| ps aux         | Show running processes                                          |
| grep '[n]ginx' | Filter Nginx processes without matching the grep command itself |

### curl

| Command                     | Purpose                              |
| --------------------------- | ------------------------------------ |
| curl -i URL                 | Show headers and body                |
| curl -H "Header: value" URL | Send custom HTTP header              |
| curl -sS URL                | Quiet output while preserving errors |
| curl -o /dev/null URL       | Discard response body                |
| curl -w "%{http_code}" URL  | Print HTTP status code               |
| curl -I URL                 | Send HEAD request                    |

---

## 29. Operational Sequence Used in Step 21

The practical operational sequence was:

```
1. Inspect Nginx
2. Inspect Nginx service
3. Inspect Nginx processes
4. Inspect listening ports
5. Inspect configuration structure
6. Inspect default server block
7. Verify default static page
8. Create project static server
9. Test configuration
10. Reload Nginx
11. Verify static serving
12. Verify Express backend
13. Configure reverse proxy
14. Test configuration
15. Reload Nginx
16. Verify Nginx to Express
17. Inspect proxy headers
18. Inspect access and error logs
19. Verify upstream connectivity
20. Move project server to port 80
21. Disable default site
22. Configure blog.local
23. Verify hostname resolution
24. Verify browser request
```

This sequence provides a reusable Nginx troubleshooting and deployment workflow.

---

## 30. Part 2 Summary

| Area                 | Commands / Operations         | Result                                  |
| -------------------- | ----------------------------- | --------------------------------------- |
| Environment          | nginx -v, nginx -V            | Nginx environment identified            |
| Service              | systemctl                     | Service verified                        |
| Processes            | ps                            | Master and workers observed             |
| Ports                | ss                            | Nginx and Express ports verified        |
| Configuration        | find, ls, cat, sed, nginx -T  | Configuration structure understood      |
| Validation           | nginx -t                      | Configuration validated                 |
| Static server        | server block, root, try_files | Static content served                   |
| Reverse proxy        | proxy_pass                    | Express reached through Nginx           |
| Headers              | proxy_set_header              | Request metadata forwarding configured  |
| Logs                 | tail, access.log, error.log   | Request and error diagnostics practiced |
| Hostname             | /etc/hosts, getent            | blog.local resolved locally             |
| Project integration  | Nginx :80 to Express :5001    | Successfully completed                  |
| Browser verification | http://blog.local             | Express reached through Nginx           |

Part 2 records the actual commands, configuration directives, command flags and operational workflow used during the Step 21 practical lab.


## Part 3 — Real Lab, MERN Integration and Final Record

---

## 1. Real Lab Summary

Step 21 was completed by integrating Nginx with the existing Phase 1 MERN project.

The practical work was performed on the existing project rather than creating a separate application.

The final architecture is:

    Browser
       |
       | HTTP
       v
    Nginx :80
       |
       | Reverse Proxy
       v
    Express :5001
       |
       +------> MongoDB
       |
       +------> Redis

The main purpose of the practical lab was to understand how Nginx works as the web-facing layer while Express remains responsible for backend application logic.

---

## 2. Environment Information

| Item | Actual Value |
|---|---|
| Operating System | Ubuntu |
| Nginx | 1.28.3 (Ubuntu) |
| Node.js | v24.19.0 |
| npm | 11.17.0 |
| Backend | Node.js + Express |
| Backend Port | 5001 |
| Nginx HTTP Port | 80 |
| Local Hostname | blog.local |
| Database | MongoDB |
| Cache | Redis |

---

## 3. Project Information

The main Phase 1 project used for Step 21 is:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack

The backend is located inside:

    /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server

The professional DevOps documentation repository is:

    /home/afroza/Projects/devops-learning

Step 21 documentation is stored in:

    /home/afroza/Projects/devops-learning/Step-21-Nginx/

| Path | Purpose |
|---|---|
| /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack | Real MERN project used for Phase 1 DevOps practice |
| /home/afroza/Projects/mern-devops-practice/Blog-App-using-MERN-stack/server | Express backend |
| /home/afroza/Projects/devops-learning | Professional DevOps documentation repository |
| /home/afroza/Projects/devops-learning/Step-21-Nginx | Step 21 documentation |

The Nginx configuration was not stored inside the MERN project because system-level Nginx configuration belongs under /etc/nginx.

---

## 4. Existing MERN Backend State

Before Nginx integration, the Express backend was already configured to listen on port 5001.

The server contained:

    app.listen(5001, () => console.log("app started at 5001..."));

The backend was started using:

    npm start

The application output confirmed:

    app started at 5001...
    connected!

The listening port was verified using:

    sudo ss -lntp | grep ':5001'

The result showed the Express process listening on port 5001.

Therefore, Nginx was added in front of an already working backend instead of rebuilding the backend.

---

## 5. Nginx Configuration Files

The project server block was created at:

    /etc/nginx/sites-available/blog-app

The configuration was enabled through:

    /etc/nginx/sites-enabled/blog-app

The enabled configuration is represented by a symbolic link.

| File / Location | Purpose |
|---|---|
| /etc/nginx/nginx.conf | Main Nginx configuration |
| /etc/nginx/sites-available/blog-app | Project server configuration |
| /etc/nginx/sites-enabled/blog-app | Enabled project configuration |
| /etc/nginx/sites-available/default | Ubuntu default Nginx server configuration |
| /etc/nginx/sites-enabled/default | Default site before it was disabled |
| /var/log/nginx/access.log | Nginx request log |
| /var/log/nginx/error.log | Nginx error log |

---

## 6. Why a Separate Server Block Was Created

The default Nginx configuration was designed to serve:

    /var/www/html

It was not configured to forward requests to the MERN backend.

Therefore, a dedicated server block was created for the project.

The project server block uses:

    server_name blog.local;

This allows the Nginx configuration to specifically handle requests intended for the Blog application.

---

## 7. Initial Default Nginx Verification

Before modifying the default configuration, the default Nginx page was verified.

The default document root was:

    /var/www/html

The default index file was:

    /var/www/html/index.nginx-debian.html

The request returned:

    HTTP/1.1 200 OK

with:

    Content-Length: 615

and the standard Nginx welcome page.

This established that Nginx itself was working before project configuration began.

---

## 8. Static Serving Lab

A temporary static web root was created:

    /var/www/blog-app

A test index file was created:

    /var/www/blog-app/index.html

The content used for verification was:

    Nginx Step 21.3 static server

A temporary server block listened on port 8080.

The request flow was:

    Client
       |
       v
    Nginx :8080
       |
       v
    /var/www/blog-app/index.html

The verification returned:

    HTTP/1.1 200 OK

and:

    Nginx Step 21.3 static server

This demonstrated that Nginx can serve static content without involving Express.

---

## 9. Static Server vs Application Server

The static serving experiment demonstrated an important difference.

| Static Serving | Application Serving |
|---|---|
| Nginx reads a file | Express executes application logic |
| Uses document root | Uses application routes |
| Example: /var/www/blog-app | Example: Express :5001 |
| No backend request required | Backend application processes request |
| Nginx can directly return the file | Express generates the application response |

This distinction is important when designing production web architectures.

---

## 10. Reverse Proxy Lab

After static serving was verified, Nginx was configured as a reverse proxy.

The upstream application was:

    http://127.0.0.1:5001

The reverse proxy flow became:

    Client
       |
       v
    Nginx :8080
       |
       | proxy_pass
       v
    Express :5001

The request was tested through Nginx and reached Express successfully.

The application returned:

    HTTP/1.1 404 Not Found

with:

    Cannot GET /

This was an Express response, proving that Nginx had successfully forwarded the request to the backend.

---

## 11. Why the 404 Was Expected

The Express application does not define a GET / route.

Therefore:

    GET /

returns:

    Cannot GET /

This does not mean that the reverse proxy failed.

The important comparison was:

    Direct Express:
    http://127.0.0.1:5001/

    Result:
    HTTP 404

and:

    Through Nginx:
    http://127.0.0.1:8080/

    Result:
    HTTP 404

Because the same application response was returned through both paths, the proxy connection was working.

---

## 12. API Route Verification

The Express project contains the following route mounts:

    app.use("/api/users", userRouter);
    app.use("/api/blogs", blogRouter);

The blog router contains:

    blogRouter.get("/", getAllBlogs);

The API was tested directly:

    curl -i http://127.0.0.1:5001/api/blogs/

The response was:

    HTTP/1.1 404 Not Found

    {"statusCode":404,"data":null,"success":false,"errors":[]}

The same API was tested through Nginx.

The result was again:

    HTTP/1.1 404 Not Found

    {"statusCode":404,"data":null,"success":false,"errors":[]}

This comparison established that the Nginx reverse proxy was passing the request to the Express application.

---

## 13. Final Nginx Server Configuration

The final Blog application server block is:

    server {
        listen 80;
        listen [::]:80;

        server_name blog.local;

        location / {
            proxy_pass http://127.0.0.1:5001;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

---

## 14. Final Configuration Explanation

| Configuration | Actual Role |
|---|---|
| listen 80 | Nginx accepts IPv4 HTTP requests on port 80 |
| listen [::]:80 | Nginx accepts IPv6 HTTP requests on port 80 |
| server_name blog.local | Selects this server block for blog.local |
| location / | Matches requests under the root path |
| proxy_pass http://127.0.0.1:5001 | Sends requests to Express |
| proxy_set_header Host $host | Passes the original hostname |
| proxy_set_header X-Real-IP $remote_addr | Passes client IP information |
| proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for | Maintains forwarded client IP information |
| proxy_set_header X-Forwarded-Proto $scheme | Passes the original protocol |

---

## 15. Default Site Handling

The Ubuntu default Nginx site was initially enabled.

The default site listened on port 80 and used:

    root /var/www/html;

For the final project configuration, the default enabled site was removed from:

    /etc/nginx/sites-enabled/

The Blog application configuration became the active project server block.

The reason was to prevent the default server from handling the same HTTP entry point when the goal was to use the Blog application server configuration.

The available default configuration was retained under:

    /etc/nginx/sites-available/default

This preserves the original configuration as a reference.

---

## 16. Port Transition

The practical configuration intentionally used port 8080 first.

Reason:

The default Nginx server was already using port 80.

The learning sequence was therefore:

    Nginx :8080
        |
        v
    Static Server

then:

    Nginx :8080
        |
        v
    Express :5001

After the configuration was verified, the project server was moved to:

    Nginx :80

The final architecture became:

    Nginx :80
        |
        v
    Express :5001

This provided a controlled progression instead of changing the public HTTP configuration before the reverse proxy had been tested.

---

## 17. Proxy Header Integration

The final server block includes:

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

These settings make the proxy more informative to the backend.

| Header | Information Forwarded |
|---|---|
| Host | Requested hostname |
| X-Real-IP | Client IP observed by Nginx |
| X-Forwarded-For | Forwarded client IP chain |
| X-Forwarded-Proto | Original request protocol |

Nginx therefore acts as an intermediary without completely losing the original request context.

---

## 18. Local Hostname Configuration

The browser initially failed to resolve:

    http://blog.local

The local hosts file was inspected:

    grep -nE 'blog\.local|127\.0\.0\.1' /etc/hosts

The required entry was added:

    127.0.0.1 blog.local

The hostname was then verified using:

    getent hosts blog.local

Actual result:

    127.0.0.1 blog.local

This established local hostname resolution without requiring an external DNS server.

---

## 19. Local Hostname vs Real DNS

| /etc/hosts | DNS |
|---|---|
| Local machine mapping | Network-wide name resolution system |
| Manually configured | Usually managed by DNS servers |
| Useful for local development | Used for real domains and production systems |
| blog.local -> 127.0.0.1 | Example: real domain -> public IP |
| No external DNS server required | DNS infrastructure required |

The Step 21 lab used /etc/hosts because the Nginx server was running locally inside the Ubuntu environment.

---

## 20. Complete Request Flow

The final request flow can be represented as:

| Stage | Component | What Happens |
|---|---|---|
| 1 | Browser | Requests http://blog.local |
| 2 | /etc/hosts | Resolves blog.local to 127.0.0.1 |
| 3 | Nginx | Receives HTTP request on port 80 |
| 4 | Server Block | blog.local matches server_name |
| 5 | location / | Request matches the configured location |
| 6 | proxy_pass | Request is forwarded to 127.0.0.1:5001 |
| 7 | Express | Backend receives and processes request |
| 8 | MongoDB / Redis | Backend can communicate with its existing services when required |
| 9 | Express | Generates application response |
| 10 | Nginx | Receives upstream response |
| 11 | Browser | Receives the final HTTP response |

---

## 21. Complete Project Architecture

    Browser
       |
       | http://blog.local
       v
    /etc/hosts
       |
       | 127.0.0.1
       v
    Nginx :80
       |
       | proxy_pass
       v
    Express :5001
       |
       +------------------+
       |                  |
       v                  v
    MongoDB              Redis

The important architectural separation is:

| Layer | Component | Responsibility |
|---|---|---|
| Client | Browser | Sends HTTP request |
| Web Layer | Nginx | Receives and forwards HTTP requests |
| Application Layer | Express | Runs backend/API logic |
| Database Layer | MongoDB | Stores application data |
| Cache/Data Service | Redis | Provides Redis functionality |

---

## 22. Direct Express vs Nginx Reverse Proxy

| Feature | Direct Express | Through Nginx |
|---|---|---|
| Request Entry | Express | Nginx |
| URL | http://127.0.0.1:5001 | http://blog.local |
| Public HTTP Port | 5001 | 80 |
| Nginx | Not involved | Involved |
| Express | Receives request | Receives proxied request |
| Reverse Proxy | No | Yes |
| Proxy Headers | Not applicable | Configured |
| Access Log | Express-side behavior | Nginx access log available |
| Web Server Layer | Express directly | Nginx |
| Production-style separation | Limited | Better separation |

The backend itself did not change.

The major change was the addition of Nginx as the HTTP entry layer.

---

## 23. Nginx vs Express Responsibilities in the Final Project

| Responsibility | Nginx | Express |
|---|---|---|
| Listen on port 80 | Yes | No |
| Listen on port 5001 | No | Yes |
| Receive public HTTP request | Yes | Indirectly |
| Reverse proxy | Yes | No |
| API route handling | No | Yes |
| Controller execution | No | Yes |
| MongoDB communication | No | Yes |
| Redis communication | No | Yes |
| Static file serving | Yes | Possible |
| Request forwarding | Yes | No |
| Application business logic | No | Yes |

This separation is one of the main concepts learned in Step 21.

---

## 24. Nginx Logs and Verification Evidence

Nginx access logs were inspected using:

    sudo tail -n 10 /var/log/nginx/access.log

The logs contained requests such as:

    "GET /api/blogs/ HTTP/1.1" 404

This proves that Nginx received the request and recorded the HTTP result.

The error log was inspected using:

    sudo tail -n 20 /var/log/nginx/error.log

No corresponding Nginx error was observed for the successful proxy configuration.

Therefore:

| Evidence | Meaning |
|---|---|
| Access log entry | Nginx received the request |
| No Nginx error | No Nginx-level error was generated |
| Express response body | Backend generated the application response |
| Same direct/proxy response | Proxy successfully reached Express |

---

## 25. Final Verification Results

| Verification | Result | Meaning |
|---|---|---|
| Nginx installed | PASS | Nginx 1.28.3 available |
| Nginx service | PASS | Service active |
| Nginx processes | PASS | Master and worker processes running |
| Port 80 | PASS | Nginx listening |
| Port 5001 | PASS | Express listening |
| Static serving | PASS | Nginx served test content |
| Reverse proxy | PASS | Nginx forwarded requests to Express |
| Proxy headers | PASS | Required headers configured |
| Access log | PASS | Requests recorded |
| Error log | PASS | No Nginx error observed during final test |
| Configuration test | PASS | nginx -t successful |
| blog.local resolution | PASS | Resolved to 127.0.0.1 |
| Hostname request | PASS | blog.local reached Nginx |
| Browser request | PASS | Browser reached Express through Nginx |

---

## 26. Final Nginx Configuration Verification

The final configuration was tested using:

    sudo nginx -t

Actual result:

    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful

This confirms that the final Nginx configuration was syntactically valid.

Nginx was then reloaded using:

    sudo systemctl reload nginx

The service remained active.

---

## 27. Final Port Verification

The final listening ports were inspected using:

    sudo ss -lntp | grep -E ':80 |:8080 |:5001 '

Final important listeners:

    0.0.0.0:80
    [::]:80
    *:5001

The final architecture therefore uses:

| Port | Process | Purpose |
|---|---|---|
| 80 | Nginx | HTTP entry point |
| 5001 | Express | Backend application |
| 8080 | Not used in final configuration | Temporary learning port |

Port 8080 was intentionally used only during the intermediate static/reverse-proxy lab.

---

## 28. Browser-Level Verification

After configuring:

    127.0.0.1 blog.local

the browser accessed:

    http://blog.local

The browser displayed:

    Cannot GET /

This response came from the Express application because the application does not define a GET / route.

The important point is that the browser successfully reached the backend through Nginx.

The complete path was:

    Browser
       |
       | http://blog.local
       v
    /etc/hosts
       |
       | 127.0.0.1
       v
    Nginx :80
       |
       | proxy_pass
       v
    Express :5001
       |
       v
    Cannot GET /

Therefore the browser-level integration was successfully verified.

---

## 29. Troubleshooting Lessons

Step 21 provided several practical troubleshooting lessons.

| Situation | Actual Lesson |
|---|---|
| Default Nginx page appeared | Nginx was working, but the project server block was not yet being used |
| Port 8080 returned static content | Static server configuration was working |
| Direct Express returned 404 | Express was reachable but GET / was not defined |
| Nginx proxy returned the same Express 404 | Reverse proxy was working |
| blog.local initially failed in browser | Local hostname resolution was missing |
| getent hosts returned 127.0.0.1 | Hostname resolution was fixed |
| nginx -t succeeded | Configuration syntax was valid |
| Access log contained GET requests | Nginx received the requests |
| Error log had no corresponding error | Nginx was not reporting a proxy failure |

The most important troubleshooting principle was to isolate each layer instead of assuming every HTTP error is an Nginx problem.

---

## 30. Troubleshooting Layer by Layer

The final diagnostic model is:

    Layer 1
    Hostname Resolution
        |
        | getent hosts blog.local
        v
    Layer 2
    Nginx Service
        |
        | systemctl is-active nginx
        v
    Layer 3
    Nginx Port
        |
        | ss -lntp
        v
    Layer 4
    Nginx Configuration
        |
        | nginx -t
        v
    Layer 5
    Upstream Express
        |
        | curl :5001
        v
    Layer 6
    Reverse Proxy
        |
        | curl :80
        v
    Layer 7
    Application Response

This layered troubleshooting approach prevents unrelated components from being changed unnecessarily.

---

## 31. What Changed in the Project

The existing Express application code did not need to be rebuilt for Nginx integration.

The main infrastructure change was:

    Before:

    Browser
       |
       v
    Express :5001


    After:

    Browser
       |
       v
    Nginx :80
       |
       v
    Express :5001

The backend application remained responsible for its existing API, MongoDB and Redis integration.

Nginx was introduced as the web-facing reverse proxy layer.

---

## 32. What Was Not Implemented in Step 21

The following topics were intentionally outside the actual Step 21 practical scope.

| Topic | Status | Reason |
|---|---|---|
| Advanced load balancing | Not implemented | Not required for the current single-backend project |
| Nginx caching | Not implemented | Not required for this step |
| Rate limiting | Not implemented | Not required for the current lab |
| Advanced security hardening | Not implemented | Separate advanced concern |
| Production TLS termination | Not implemented | TLS was already covered in Step 20 |
| Kubernetes Ingress | Not implemented | Kubernetes belongs to Step 27 |
| Dockerized Nginx | Not implemented | Docker belongs to Step 22 |
| Docker Compose Nginx architecture | Not implemented | Docker Compose belongs to Step 23 |
| CI/CD deployment | Not implemented | GitHub Actions belongs to Step 24 |
| Cloud deployment | Not implemented | Cloud belongs to Step 25 |

These topics were not recorded as completed because they were not part of the actual Step 21 lab.

---

## 33. Skills Gained

After completing Step 21, the following practical skills were gained:

| Skill | Practical Evidence |
|---|---|
| Nginx installation/environment inspection | nginx -v and nginx -V |
| Nginx service management | systemctl status, is-active and reload |
| Nginx process inspection | ps |
| Network listener inspection | ss |
| Configuration inspection | nginx.conf and site configurations |
| Server block configuration | blog-app server block |
| Static file serving | /var/www/blog-app |
| Reverse proxy | proxy_pass to Express :5001 |
| Proxy header configuration | proxy_set_header |
| Log inspection | access.log and error.log |
| Configuration validation | nginx -t |
| Local hostname configuration | /etc/hosts |
| Hostname verification | getent hosts |
| Backend connectivity testing | curl :5001 |
| Reverse proxy testing | curl through Nginx |
| Browser-level verification | http://blog.local |
| Layer-by-layer troubleshooting | Hostname -> Nginx -> Express |

---

## 34. Final Architecture Summary

The final Step 21 architecture is:

    +----------------------+
    |      Browser         |
    |  http://blog.local   |
    +----------+-----------+
               |
               v
    +----------------------+
    |       /etc/hosts     |
    | blog.local ->        |
    | 127.0.0.1            |
    +----------+-----------+
               |
               v
    +----------------------+
    |      Nginx :80       |
    |    Web / Proxy Layer |
    +----------+-----------+
               |
               | proxy_pass
               v
    +----------------------+
    |    Express :5001     |
    |   Backend/API Layer  |
    +----------+-----------+
               |
          +----+----+
          |         |
          v         v
      MongoDB     Redis

The important architectural change introduced by Step 21 is the separation between the web-facing layer and the backend application layer.

---

## 35. Step 21 Final Result

| Area | Final State |
|---|---|
| Nginx | Configured |
| Nginx Service | Active |
| Nginx Port | 80 |
| Express Port | 5001 |
| Server Block | blog-app |
| Hostname | blog.local |
| Static Serving | Practiced and verified |
| Reverse Proxy | Configured and verified |
| Proxy Headers | Configured |
| Logs | Inspected |
| Configuration Test | Successful |
| Default Site | Disabled from enabled configuration |
| Local Hostname | Resolved |
| Browser Integration | Verified |
| MERN Integration | Nginx placed in front of existing Express backend |

---

## 36. Step 21 Completion Statement

Step 21 — Nginx is complete.

The existing MERN backend was not rebuilt or unnecessarily modified.

Nginx was introduced as the web-facing layer in front of the existing Express backend.

The final request path is:

    Browser
       |
       v
    http://blog.local
       |
       v
    127.0.0.1
       |
       v
    Nginx :80
       |
       v
    proxy_pass
       |
       v
    Express :5001
       |
       +----> MongoDB
       |
       +----> Redis

The practical work covered Nginx environment inspection, service management, configuration structure, server blocks, static serving, reverse proxying, proxy headers, logs, hostname resolution, troubleshooting and final MERN project integration.

Step 21 therefore establishes the Nginx foundation required for the upcoming containerization and DevOps stages of the roadmap.
