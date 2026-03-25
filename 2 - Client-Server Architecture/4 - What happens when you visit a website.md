Here’s a Phase‑1–level, detailed tutorial on:

# What Happens When You Visit a Website?

Topic from Phase 1 → Client–Server Architecture:
- You now know:
  - Client–Server model  
  - Client (browser) vs Server  
  - HTTP request–response cycle  

Now we’ll connect everything into **one story**: from typing a URL to seeing a page.

We’ll walk through what happens when you go to:

`https://example.com`

---

## 1. You type a URL and hit Enter

You enter a URL in the browser’s address bar and press Enter, or click a link/bookmark.

Example URL parts:

`https://example.com:443/about?lang=en#team`

Breakdown:

- `https` → **protocol** (HTTP or HTTPS)
- `example.com` → **domain name**
- `443` → **port** (optional in URL; 443 is default for HTTPS, 80 for HTTP)
- `/about` → **path**
- `?lang=en` → **query string** (extra data)
- `#team` → **fragment** (handled only by browser; not sent to server)

The browser uses this information to figure out:
- Which **server** to talk to (via domain).
- Which **protocol** to use (HTTP/HTTPS).
- Which **resource** to ask for (path + query).

---

## 2. Browser checks cache first (local shortcuts)

Before going out to the internet, the browser asks:

> “Do I already have this resource saved locally?”

It might check:
- **Browser cache** (previously stored HTML/CSS/JS/images)
- **OS-level DNS cache** (previous IP for this domain)
- **Service workers** (for PWAs/offline apps; later in the roadmap)

If it finds a valid, fresh copy and caching rules allow it:
- It might use that without contacting the server at all.

If not, next step: **resolve the domain**.

---

## 3. DNS lookup – finding the server’s IP address

Your browser only understands IP addresses (e.g., `93.184.216.34`), not human-readable names like `example.com`.

So it asks:

> “What is the IP address of example.com?”

This is a **DNS (Domain Name System)** query.

The resolution process (simplified):

1. Browser checks local DNS cache.
2. If not found, OS asks the **DNS resolver** (usually your ISP or a public DNS like 8.8.8.8).
3. The resolver:
   - May have it cached.
   - If not, it queries the DNS hierarchy:
     - Root DNS servers → `.com` name servers → `example.com` name servers.
4. Eventually, it gets back an IP, e.g.:

   `example.com → 93.184.216.34`

Browser now knows which **server machine** to contact.

---

## 4. Browser establishes a TCP connection

The web uses **TCP** (Transmission Control Protocol) under HTTP/1.1 and HTTP/2.

Steps:

1. Browser initiates a **TCP connection** to:
   - IP: `93.184.216.34`
   - Port: `443` (for HTTPS) or `80` (for HTTP)
2. A **3‑way handshake** occurs:
   - Client → SYN  
   - Server → SYN‑ACK  
   - Client → ACK  

After this:
- A **reliable connection** is established.
- Both parties can exchange data without worrying about packet ordering or loss (TCP handles it).

---

## 5. (For HTTPS) TLS/SSL handshake – making it secure

If the URL is `https://...` (which it usually is nowadays):

1. After TCP handshake, there is a **TLS handshake**:
   - Browser and server agree on:
     - Encryption algorithms.
     - Session keys.
   - Server sends its **SSL certificate**.
   - Browser verifies:
     - Is this cert valid?
     - Signed by a trusted Certificate Authority (CA)?
     - Matches the domain name?

2. If all checks pass:
   - A secure, encrypted channel is established.
   - Now HTTP data (requests/responses) will be sent **inside this encrypted tunnel**.

If HTTPS handshake fails:
- Browser warning: “Your connection is not private”, certificate errors, etc.

---

## 6. Browser sends an HTTP request

With the connection ready, browser builds and sends an **HTTP request**.

Example for `https://example.com/`:

```http
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 ...
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Connection: keep-alive
```

Key elements:

- **Method**: `GET` (asking for the homepage)
- **Path**: `/`
- **Headers**: describe browser capabilities, cookies, accepted formats, etc.
- **Body**: none for a simple GET

If you submit a form, it might be a `POST` with a **request body**.

---

## 7. Server receives the request

On the other side, a server is listening (e.g., Nginx, Apache, or a Node.js/Express app).

The server:

1. Accepts the TCP connection.
2. Reads the **HTTP request**.
3. Parses:
   - Method (`GET`)
   - Path (`/`)
   - Headers (Host, User-Agent, Cookies, etc.)
4. Routes the request to the correct handler:
   - Static file (like `index.html`)
   - Backend route (e.g., `/api/...`)

---

## 8. Server processes the request

What happens next depends on the application:

- For a **static website**:
  - The server might simply read `index.html` from disk.
  - No database, no heavy logic.

- For a **dynamic website or web app**:
  - Server might:
    - Check cookies/session to identify user.
    - Query a database (MongoDB, MySQL, etc.).
    - Apply business logic.
    - Render a template (EJS, Handlebars, etc.).
    - Or prepare a JSON response for an API.

The server then prepares an **HTTP response**.

---

## 9. Server sends back an HTTP response

Example HTML response:

```http
HTTP/1.1 200 OK
Date: Tue, 25 Mar 2025 12:34:56 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 2048
Connection: keep-alive

<!DOCTYPE html>
<html>
<head>
  <title>Example Site</title>
  <link rel="stylesheet" href="/styles.css">
</head>
<body>
  <h1>Hello, world!</h1>
  <img src="/logo.png" alt="Logo">
  <script src="/app.js"></script>
</body>
</html>
```

Important parts:

- **Status line**: `HTTP/1.1 200 OK`
  - Indicates success (200).
- **Headers**:
  - `Content-Type: text/html` → body is HTML.
  - `Content-Length` → length in bytes.
  - `Set-Cookie`, cache headers, etc., may also be here.
- **Body**:
  - Full HTML of the page.

If an error occurs, status might be:
- `404 Not Found`
- `500 Internal Server Error`
- etc.

---

## 10. Browser receives and parses the response

Once the response arrives:

1. Browser reads **status code**:
   - 200–299 → success.
   - 300–399 → redirect (browser may auto-follow).
   - 400–499 → client errors (bad request, unauthorized, not found).
   - 500–599 → server errors.

2. Reads **headers**:
   - `Content-Type` tells how to interpret body.
   - `Set-Cookie` sets cookies in browser.
   - Cache instructions (`Cache-Control`, `ETag`, etc.).

3. Reads **body**:
   - For HTML → passes it to the **HTML parser**.

---

## 11. Browser builds the DOM and render tree

Using the HTML:

1. **HTML parsing → DOM (Document Object Model)**:
   - Each tag becomes a **node** in a tree structure.
   - Example:

     ```html
     <body>
       <h1>Hello</h1>
       <p>Welcome</p>
     </body>
     ```

     becomes a DOM tree:
     - html
       - head
       - body
         - h1
         - p

2. Simultaneously, when the browser sees:
   - `<link rel="stylesheet" href="/styles.css">`
   - `<script src="/app.js"></script>`
   - `<img src="/logo.png">`

   It schedules **additional HTTP requests** for those resources.

3. CSS files are parsed into **CSSOM** (CSS Object Model).
4. Browser combines **DOM + CSSOM → Render Tree**.
5. Then does **layout** (calculates positions/sizes) and **paint** (draws pixels to screen).

This is the **rendering process** (later expanded in Phase 3: CSS & browser rendering).

---

## 12. Multiple parallel requests for resources

For each referenced asset (CSS, JS, images, fonts, etc.), the browser:

- Creates new **HTTP GET** requests, often reusing existing TCP connection(s) or using HTTP/2 multiplexing.
- Each asset has its own **mini request–response cycle**:

Example for CSS:

```http
GET /styles.css HTTP/1.1
Host: example.com
Accept: text/css,*/*;q=0.1
```

Server responds:

```http
HTTP/1.1 200 OK
Content-Type: text/css

body { font-family: sans-serif; }
h1 { color: blue; }
```

Same idea for JS, images, fonts, etc.

As these arrive:
- CSS updates the layout/styling.
- JS runs and may modify the DOM, make further network calls (AJAX/fetch), etc.

---

## 13. JavaScript execution and further network calls

When `<script src="/app.js"></script>` loads:

1. Browser downloads `app.js` via HTTP.
2. Parses and executes the JavaScript.

Your JS code can:
- Manipulate DOM (add/remove elements).
- Listen for user events (clicks, input, scroll, etc.).
- Make **AJAX/fetch** calls to APIs:

```js
fetch('/api/data')
  .then(res => res.json())
  .then(data => {
    // update page
  });
```

Each `fetch`:

- Triggers another **HTTP request**.
- The server processes it and returns a **response (often JSON)**.
- JS handles the data and updates UI.

This is how **dynamic single-page apps (SPAs)** work.

---

## 14. Caching, cookies, and subsequent visits

On the first visit, browser may store:

- **Files in cache**:
  - To avoid re-downloading unchanged CSS/JS/images.
- **Cookies**:
  - Session IDs (for login state).
  - Preferences (theme, language).

Next time you visit:

- Browser might:
  - Reuse cached resources if still valid.
  - Include cookies in the request headers (`Cookie: session_id=xyz`).
- Server:
  - Recognizes your session.
  - Returns personalized content.
  - Might return `304 Not Modified` to tell browser to use cached copy.

This makes subsequent page loads **faster** and **personalized**.

---

## 15. Errors and special cases

When you visit a website, things may go wrong at different stages:

- **DNS error**:
  - Domain can’t be resolved → “Server not found”.
- **TCP/connection error**:
  - Server unreachable → “Connection timed out” / “Refused to connect”.
- **TLS/certificate error**:
  - Invalid cert → “Your connection is not private”.
- **HTTP error codes**:
  - `404 Not Found` → wrong URL/path.
  - `403 Forbidden` → not allowed.
  - `500 Internal Server Error` → bug on server.
- **CORS issues** (for API calls from JS):
  - Browser blocks requests to a different origin without proper headers.

Understanding the **steps** helps you figure out **where** the problem likely is.

---

## 16. High‑level recap (story form)

When you visit `https://example.com`:

1. **Browser**:
   - Parses the URL.
   - Checks cache.
   - Resolves `example.com` to an IP via **DNS**.
2. **Network/TCP**:
   - Opens a TCP connection to IP:443.
3. **TLS (for HTTPS)**:
   - Performs SSL/TLS handshake.
   - Establishes a secure, encrypted channel.
4. **HTTP (request)**:
   - Browser sends `GET /` with headers.
5. **Server**:
   - Receives request, runs backend logic.
   - May query databases, templates, etc.
   - Builds and sends an `HTTP 200 OK` with HTML.
6. **Browser**:
   - Parses HTML → builds DOM.
   - Requests linked CSS, JS, images via more HTTP GETs.
   - Builds CSSOM, Render Tree → layout → paint.
   - Executes JS, which may trigger more network calls and DOM updates.
7. **You**:
   - See the fully rendered webpage and interact with it.

Every click/link/form/route change is **some variation** of this story.

---

If you’d like to continue Phase‑1 topics, the next natural ones under “Client–Server Architecture” are:

- Difference between Front-end and Back-end  
- What are Static Websites and Dynamic Websites  
- What is web hosting and how it works  

Tell me which one you want next, and I’ll build a tutorial at the same depth.
