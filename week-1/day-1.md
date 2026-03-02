

# 📘 Day 1, Week 1: Understanding the Internet & Setting Up Your Tools

## **What You'll Learn Today**

By the end of today, you will:
1. Understand what happens behind the scenes when you visit a website
2. Know key terms like **client, server, HTTP, DNS, IP address**
3. Have **VS Code** installed and configured for web development
4. Have created your **very first webpage** (even if it just says "Hello, World!")

---

# 📖 PART 1: HOW THE INTERNET WORKS
## *(Read time: ~45 minutes — take it slow, take notes)*

---

## 1.1 — The Big Picture: What IS the Internet?

Imagine you want to send a letter to a friend in another city.

```
You (write letter) → Post Office → Friend's Post Office → Friend (reads letter)
```

The internet works in a very similar way:

```
You (type URL) → Your Computer sends a request → Internet → Server sends back a webpage
```

The internet is simply **a global network of computers that can communicate with each other**. That's it. It's not magic, it's not "in the cloud" — it's physical cables (most of them under the ocean!), routers, and computers talking to each other using agreed-upon rules.

> 🧠 **Key Insight:** Every website you've ever visited is just a file (or a collection of files) stored on someone else's computer (a server) somewhere in the world.

---

## 1.2 — Clients and Servers

These are the two most fundamental roles on the internet:

### **Client (You)**
A client is **any device that requests information**. Your laptop, your phone, your tablet — when you open a browser and type a URL, **you are the client**.

The client's job is to:
- Send a **request** ("Hey, give me the Google homepage!")
- Receive a **response** (the HTML, CSS, and JavaScript files that make up the page)
- **Render** (display) the webpage so you can see and interact with it

### **Server**
A server is **a computer that stores website files and sends them to clients when asked**.

The server's job is to:
- **Wait** for requests (it runs 24/7, always listening)
- **Process** the request ("Someone wants the homepage? Let me find those files...")
- **Send back** the response (the files needed to display the page)

```
  CLIENT (Your Browser)                    SERVER (Remote Computer)
  ┌──────────────────┐                    ┌──────────────────┐
  │                  │   "Give me the     │                  │
  │   Google Chrome  │ ── homepage!" ───→ │  Google's Server  │
  │   Firefox        │                    │  (stores files)   │
  │   Safari         │ ←── "Here are ──  │                  │
  │   Edge           │    the files!"     │                  │
  └──────────────────┘                    └──────────────────┘
```

### Real-World Analogy

Think of a **restaurant**:
- **You** (the customer) = the **client**
- **The kitchen** = the **server**
- **Your order** = the **request**
- **Your food** = the **response**
- **The waiter** = the **internet** (carries messages back and forth)
- **The menu** = the **URL** (tells the kitchen what you want)

> 🧠 **Key Insight:** The word "server" sounds fancy, but it's literally just a computer. You could turn your own laptop into a server if you wanted to. The only difference is that servers are designed to stay on 24/7 and handle many requests at once.

---

## 1.3 — What is a URL?

You use URLs every day, but let's break one down:

```
https://www.example.com:443/about/team?sort=name#section2
│       │       │        │      │         │        │
│       │       │        │      │         │        └── Fragment
│       │       │        │      │         └── Query
│       │       │        │      └── Path
│       │       │        └── Port
│       │       └── Domain Name
│       └── Subdomain
└── Protocol
```

**Don't panic!** You don't need to memorize all of this. Let's focus on the important parts:

### **Protocol: `https://`**
- This tells the browser **HOW** to communicate
- `http` = HyperText Transfer Protocol (the rules for transferring web pages)
- `https` = HTTP **Secure** (same thing, but encrypted — your data is scrambled so hackers can't read it)
- Think of it as choosing between sending a **postcard** (HTTP — anyone can read it) vs a **sealed, locked envelope** (HTTPS — only sender and receiver can read it)

### **Domain Name: `example.com`**
- This is the **human-friendly name** for a website
- It's much easier to remember `google.com` than `142.250.80.46`, right?
- Domain names are like **contact names in your phone** — they map to actual numbers (IP addresses)

### **Path: `/about/team`**
- This tells the server **WHICH specific page** you want
- Think of it like a folder structure on your computer:
  - `/about/team` means "go to the 'about' folder, then find the 'team' page"
- If you just type `example.com` with no path, you get the **homepage** (usually `index.html`)

> 🧠 **Key Insight:** When you type just `google.com`, your browser automatically adds `https://` and requests the homepage. You've been using URLs your whole life — now you know what each part means!

---

## 1.4 — IP Addresses: The Real Addresses of the Internet

Every computer connected to the internet has a unique address called an **IP address** (Internet Protocol address).

```
Examples of IP addresses:
  142.250.80.46     ← This is Google
  151.101.1.69      ← This is Reddit
  192.168.1.1       ← This is probably your home router
```

### Why Do We Need IP Addresses?

Remember our postal analogy? When you send a letter, you need a **physical address** (123 Main Street, City, Country). On the internet, the IP address IS that physical address.

Every device on the internet — your phone, your laptop, Google's servers, Netflix's servers — has one.

### Types of IP Addresses

**IPv4** (the older format):
```
192.168.1.1
```
Four groups of numbers (0-255), separated by dots. This gives us about 4.3 billion possible addresses. Sounds like a lot, but we've actually **run out** because there are so many devices connected to the internet now!

**IPv6** (the newer format):
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```
Much longer, which means many more possible addresses (340 trillion trillion trillion — yes, that's a real number).

> 🧠 **Key Insight:** You'll almost never need to type an IP address. That's what **DNS** is for (coming next!). But understanding that every website has a numerical address behind its domain name is important.

---

## 1.5 — DNS: The Internet's Phone Book

**DNS** stands for **Domain Name System**.

### The Problem DNS Solves

Imagine if to call your friend, you had to memorize their phone number: `+1-555-847-2938`. Now imagine memorizing a different number for every person you know. Terrible, right?

That's why we have **contacts** in our phone. You just tap "Mom" and the phone knows to dial `+1-555-123-4567`.

**DNS works the same way for websites:**

```
You type: google.com          (the human-friendly name)
DNS translates to: 142.250.80.46   (the actual server address)
```

### How DNS Works (Step by Step)

When you type `google.com` in your browser:

```
Step 1: Browser checks its own CACHE
        "Have I looked up google.com recently?"
        ├── YES → Use the saved IP address (skip to Step 5)
        └── NO  → Continue to Step 2

Step 2: Browser asks your OPERATING SYSTEM
        "Hey Windows/Mac, do you know google.com?"
        ├── YES → Use that IP address
        └── NO  → Continue to Step 3

Step 3: Your computer asks a DNS RESOLVER (usually your ISP's server)
        "Hey ISP, what's the IP address for google.com?"
        The resolver may ask other DNS servers in a chain:
        
        Root Server → "I don't know google.com, but ask the .com server"
        .com Server → "I don't know google.com, but ask Google's name server"
        Google's Name Server → "google.com is at 142.250.80.46!"

Step 4: The IP address is sent back to your browser
        Each server along the way CACHES (saves) the answer
        so future lookups are faster

Step 5: Your browser now contacts 142.250.80.46 directly
```

### A Complete Analogy

Imagine you're in a new city and want to find "Joe's Pizza":
1. You ask a **friend** nearby → *"No idea"* (browser cache miss)
2. You ask a **local person** → *"No idea"* (OS cache miss)
3. You call **the city information line** → They say *"Let me check..."* (DNS resolver)
4. They call the **restaurant directory** → *"Joe's Pizza is at 742 Elm Street!"* (Name server)
5. Now you have the address and can **walk there directly** (Browser connects to IP)

> 🧠 **Key Insight:** DNS lookups happen in **milliseconds**. You never notice them. But every single time you visit a website for the first time, this process happens behind the scenes.

---

## 1.6 — HTTP: The Language of the Web

**HTTP** stands for **HyperText Transfer Protocol**.

If DNS is about **finding the right address**, HTTP is about **the conversation that happens once you get there**.

### What is a "Protocol"?

A protocol is just a set of **rules for communication**. Think about the rules of a phone call:
1. You dial a number
2. The other person says "Hello?"
3. You identify yourself
4. You have a conversation
5. Someone says "Bye" and you hang up

HTTP is the same idea — it's the agreed-upon rules for how a browser talks to a server.

### HTTP Request (What your browser sends)

When you type a URL and hit Enter, your browser sends an **HTTP request** like this:

```
GET /index.html HTTP/1.1
Host: www.example.com
Accept: text/html
User-Agent: Chrome/120
```

Breaking this down:
- **`GET`** = the HTTP **method** — "I want to GET (retrieve) something"
- **`/index.html`** = the specific file I want
- **`Host: www.example.com`** = the website I'm requesting from
- **`Accept: text/html`** = I'm expecting an HTML file back
- **`User-Agent: Chrome/120`** = I'm using Chrome browser, version 120

### HTTP Response (What the server sends back)

The server receives the request and sends back a **response**:

```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 3420

<!DOCTYPE html>
<html>
  <head>
    <title>Example</title>
  </head>
  <body>
    <h1>Welcome!</h1>
    ...
  </body>
</html>
```

Breaking this down:
- **`200 OK`** = **Status code** — everything went well, here's your page!
- **`Content-Type: text/html`** = I'm sending you an HTML file
- Then the actual **HTML content** of the page follows

### HTTP Status Codes You Should Know

| Code | Meaning | When You See It |
|------|---------|----------------|
| **200** | OK — Success! | Everything worked normally |
| **301** | Moved Permanently | The page has moved to a new URL |
| **404** | Not Found | The page doesn't exist (typo in URL?) |
| **403** | Forbidden | You don't have permission to view this |
| **500** | Internal Server Error | Something broke on the server's side |

> **You've definitely seen 404 before!** Now you know what it actually means — the server is saying "I received your request, but I can't find the file you're asking for."

### HTTP vs HTTPS

```
HTTP:  Your data travels as plain text
       Anyone intercepting it can read everything
       ❌ Like writing your password on a postcard

HTTPS: Your data is ENCRYPTED before traveling
       Even if intercepted, it looks like gibberish
       ✅ Like putting your password in a locked safe
```

**HTTPS uses something called TLS/SSL** to encrypt the data. You don't need to know the details of encryption right now — just know that HTTPS is the **secure version** and almost all modern websites use it. Your browser shows a 🔒 **lock icon** when a site uses HTTPS.

---

## 1.7 — Putting It All Together: What Happens When You Type `google.com`

Let's walk through the **complete journey**, step by step:

```
STEP 1: YOU TYPE "google.com" AND PRESS ENTER
        ↓
STEP 2: BROWSER ADDS "https://" 
        → Full URL: https://www.google.com
        ↓
STEP 3: DNS LOOKUP
        → Browser asks: "What's the IP address for google.com?"
        → DNS responds: "It's 142.250.80.46"
        ↓
STEP 4: TCP CONNECTION
        → Browser establishes a connection with the server at 142.250.80.46
        → They do a "handshake" (agree to communicate)
        ↓
STEP 5: HTTP REQUEST
        → Browser sends: "GET / HTTP/1.1" (give me the homepage)
        ↓
STEP 6: SERVER PROCESSES THE REQUEST
        → Google's server finds the homepage files
        → Prepares the response
        ↓
STEP 7: HTTP RESPONSE
        → Server sends back: "200 OK" + HTML file
        ↓
STEP 8: BROWSER RECEIVES HTML
        → Starts reading the HTML file
        → Discovers it needs CSS files, JavaScript files, images
        ↓
STEP 9: ADDITIONAL REQUESTS
        → Browser sends MORE requests for each CSS, JS, and image file
        → Server sends each one back
        ↓
STEP 10: BROWSER RENDERS THE PAGE
        → Combines HTML (structure) + CSS (styling) + JS (interactivity)
        → Displays the final, beautiful webpage on your screen
        ↓
STEP 11: YOU SEE GOOGLE'S HOMEPAGE ✅
        Total time: Usually under 1 second!
```

### Visualizing All the Pieces Together

```
┌─────────────────────────────────────────────────────────────────┐
│                        YOUR COMPUTER                            │
│  ┌──────────────┐                                               │
│  │   Browser     │──── "What IP is google.com?" ───→ DNS Server │
│  │   (Client)    │←─── "142.250.80.46" ────────────             │
│  │              │                                               │
│  │              │──── HTTP Request ──────────────→ Google Server │
│  │              │←─── HTTP Response (HTML+CSS+JS)  (142.250...)  │
│  │              │                                               │
│  │  Renders the │                                               │
│  │  webpage! 🎉 │                                               │
│  └──────────────┘                                               │
└─────────────────────────────────────────────────────────────────┘
```

> 🧠 **Key Insight:** ALL of this happens in **less than a second**. The internet is mind-blowingly fast. And now you understand what's happening behind the scenes every single time you visit a website!

---

## 1.8 — What are HTML, CSS, and JavaScript?

Before we start building, let's understand the three technologies that make up **every webpage**:

### The House Analogy

```
HTML  = The STRUCTURE of a house
        (walls, rooms, doors, windows, foundation)
        "WHAT is on the page?"

CSS   = The DECORATION of a house
        (paint colors, furniture style, curtains, layout)
        "HOW does it LOOK?"

JS    = The FUNCTIONALITY of a house
        (light switches, garage door opener, doorbell, smart home)
        "HOW does it BEHAVE?"
```

### Concrete Example

Imagine a button on a webpage:

```
HTML says:  "There is a button here, and it says 'Click Me'"
            <button>Click Me</button>

CSS says:   "The button should be blue, rounded, with white text"
            button { background: blue; color: white; border-radius: 10px; }

JS says:    "When someone clicks the button, show an alert message"
            button.addEventListener('click', () => alert('Hello!'));
```

### This Week's Focus

**This week, we ONLY focus on HTML.** Your pages will look plain and unstyled — and that's perfectly fine! You need to build a strong foundation before adding decoration and functionality.

---

## ✅ PART 1 KNOWLEDGE CHECK

Before moving on, test yourself. **Try to answer WITHOUT scrolling up:**

1. What is a client? What is a server?
2. What does DNS do? Why do we need it?
3. What does HTTP stand for? What is its job?
4. What does a 404 status code mean?
5. What's the difference between HTTP and HTTPS?
6. In your own words, what happens when you type a URL into your browser?
7. What are the roles of HTML, CSS, and JavaScript?

> **📝 Write your answers down!** Writing reinforces memory far more than just thinking "yeah, I know that."

**Go write your answers now.** Seriously. Even if they're not perfect, the act of trying to recall is what builds understanding.

---
---

# 🛠️ PART 2: SETTING UP YOUR TOOLS
## *(Hands-on time: ~45 minutes)*

---

## 2.1 — Installing VS Code

**VS Code (Visual Studio Code)** is a **code editor** — a program specifically designed for writing code. You *could* write code in Notepad, but VS Code gives you:

- **Syntax highlighting** (code is color-coded so it's easier to read)
- **Auto-completion** (suggests code as you type)
- **Error detection** (underlines mistakes)
- **Extensions** (add-on features created by the community)
- **Integrated terminal** (a command line built right into the editor)

### Installation Steps

```
1. Open your browser
2. Go to: https://code.visualstudio.com
3. Click the big blue "Download" button
   → It automatically detects your operating system (Windows/Mac/Linux)
4. Run the installer:
   
   WINDOWS:
   → Run the downloaded .exe file
   → Check "Add to PATH" when asked ✅ (IMPORTANT!)
   → Check "Add 'Open with Code' to context menu" ✅ (Helpful!)
   → Click Install → Finish
   
   MAC:
   → Open the downloaded .zip file
   → Drag "Visual Studio Code" into your Applications folder
   → Open it from Applications
   → If asked "Are you sure you want to open it?" → Click Open

5. Launch VS Code
   → You should see a Welcome tab
```

### Quick Tour of VS Code

When you first open VS Code, here's what you see:

```
┌──────────────────────────────────────────────────────────┐
│  File  Edit  View  ...                    (Menu Bar)     │
├────┬─────────────────────────────────────────────────────┤
│    │                                                     │
│ S  │                                                     │
│ I  │            Welcome Tab                              │
│ D  │            (or your files will show here)           │
│ E  │                                                     │
│    │                                                     │
│ B  │                                                     │
│ A  │                                                     │
│ R  │                                                     │
│    │                                                     │
├────┴─────────────────────────────────────────────────────┤
│  Terminal / Output panel (toggle with Ctrl+`)            │
├──────────────────────────────────────────────────────────┤
│  Status Bar (bottom)                                     │
└──────────────────────────────────────────────────────────┘
```

**The Side Bar** (left side) has icons for:
- 📄 **Explorer** — see your files and folders
- 🔍 **Search** — find text across all files
- 🔀 **Source Control** — for Git (you'll learn this later)
- 🐛 **Run/Debug** — for debugging code
- 🧩 **Extensions** — install add-ons

> **Keyboard shortcut to remember:** `Ctrl + `` ` (backtick, the key below Escape) toggles the **terminal panel**. You'll use this often later!

---

## 2.2 — Installing Essential Extensions

Extensions add extra features to VS Code. Let's install three that will help you immediately:

### How to Install Extensions

```
1. Click the Extensions icon on the left sidebar (it looks like 4 squares: 🧩)
   OR press Ctrl+Shift+X (Windows/Linux) / Cmd+Shift+X (Mac)
   
2. A search bar appears at the top
3. Type the name of the extension
4. Click "Install" on the correct one
```

### Extension 1: Live Server

```
Search for: "Live Server"
Author: Ritwick Dey
Install it: Click "Install"
```

**What it does:** Automatically opens your webpage in the browser AND **refreshes it every time you save**. Without this, you'd have to manually refresh the browser every time you change your code. With this, changes appear instantly.

**How to use it:** Right-click on an HTML file → "Open with Live Server"

### Extension 2: Prettier — Code Formatter

```
Search for: "Prettier - Code formatter"
Author: Prettier
Install it: Click "Install"
```

**What it does:** Automatically formats your code to look clean and consistent. Fixes indentation, spacing, and line breaks.

**How to use it:** Press `Shift+Alt+F` (Windows) or `Shift+Option+F` (Mac) to format your file. Or set it to format automatically on save.

**Setting up auto-format on save:**
```
1. Press Ctrl+, (comma) to open Settings
2. Search for "format on save"
3. Check the box: "Editor: Format On Save" ✅
4. Search for "default formatter"
5. Set "Editor: Default Formatter" to "Prettier - Code formatter"
```

### Extension 3: Auto Rename Tag

```
Search for: "Auto Rename Tag"
Author: Jun Han
Install it: Click "Install"
```

**What it does:** When you rename an HTML opening tag, it automatically renames the closing tag too (and vice versa).

**Example:** If you have `<div>...</div>` and you change `<div>` to `<section>`, it automatically changes `</div>` to `</section>`.

> ✅ **After installing all three, you might want to restart VS Code.** Close it and open it again to make sure everything loads properly.

---

## 2.3 — Creating Your Project Folder

Now let's create the folder where your first project will live.

### Step-by-Step:

```
1. CREATE THE FOLDER:
   
   WINDOWS:
   → Open File Explorer
   → Navigate to a location you'll remember (Desktop, Documents, etc.)
   → Right-click → New → Folder
   → Name it: about-me
   
   MAC:
   → Open Finder
   → Navigate to a location you'll remember
   → Right-click → New Folder
   → Name it: about-me

2. OPEN THE FOLDER IN VS CODE:
   → Open VS Code
   → File → Open Folder (Windows/Linux) OR File → Open... (Mac)
   → Navigate to your "about-me" folder
   → Select it and click "Open" or "Select Folder"
   
   You should now see "ABOUT-ME" in the Explorer sidebar on the left
   (it might say "No files yet" or be empty — that's correct!)
```

> ⚠️ **Important:** Always open the **folder** in VS Code, not individual files. This way VS Code knows about your entire project and features like Live Server work correctly.

---

## 2.4 — Creating Your First HTML File

### Step-by-Step:

```
METHOD 1 (Using VS Code):
1. Look at the Explorer panel on the left (it shows "ABOUT-ME")
2. Hover over "ABOUT-ME" — you'll see small icons appear
3. Click the "New File" icon (looks like a page with a + sign)
4. Type: index.html
5. Press Enter

METHOD 2 (Using keyboard):
1. Press Ctrl+N (creates a new untitled file)
2. Press Ctrl+S to save
3. Navigate to your about-me folder
4. Name it: index.html
5. Click Save
```

### Why "index.html"?

The name `index.html` is special in web development:

```
When a server receives a request for a folder (like example.com/about/),
it automatically looks for a file called "index.html" in that folder.

It's like the "front door" of any folder on a website.

example.com/           → serves index.html from the root folder
example.com/about/     → serves index.html from the /about/ folder
example.com/contact/   → serves index.html from the /contact/ folder
```

> 🧠 **Key Insight:** You could name your file `potato.html` and it would work fine locally. But `index.html` is the convention, and servers look for it by default. Always name your main page `index.html`.

---

## 2.5 — The HTML Boilerplate (Your First Code!)

Now let's write your first HTML code. This is called the **boilerplate** — the minimum code every HTML page needs.

### The Magic Shortcut

With your `index.html` file open:

```
1. Make sure the file is empty
2. Type exactly:  !
3. Press Tab (or Enter)
```

VS Code will generate this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

**Now let's understand every single line.**

---

### Line-by-Line Explanation

#### Line 1: `<!DOCTYPE html>`
```html
<!DOCTYPE html>
```
- This is a **declaration** (not a tag — notice it doesn't have a closing tag)
- It tells the browser: **"This document is HTML5"** (the latest version of HTML)
- Without it, browsers might display your page in "quirks mode" — an unpredictable compatibility mode
- **Always include this as the very first line**

#### Lines 2 and 12: `<html lang="en">` ... `</html>`
```html
<html lang="en">
  ...everything goes here...
</html>
```
- The `<html>` tag is the **root element** — it wraps EVERYTHING on the page
- `lang="en"` tells the browser the page content is in **English**
  - This helps **screen readers** pronounce words correctly
  - This helps **Google** know what language your site is in
  - For Spanish, you'd use `lang="es"`, for French `lang="fr"`, etc.

#### Lines 3 and 7: `<head>` ... `</head>`
```html
<head>
    ...metadata goes here...
</head>
```
- The `<head>` contains **metadata** — information ABOUT the page, but NOT visible content
- Things that go in `<head>`:
  - Page title (shown in the browser tab)
  - Character encoding
  - Links to CSS files
  - Links to JavaScript files
  - SEO information
- **Nothing in `<head>` is displayed on the page itself**

#### Line 4: `<meta charset="UTF-8">`
```html
<meta charset="UTF-8">
```
- **Character encoding** — tells the browser how to interpret text characters
- **UTF-8** supports virtually every character from every language in the world
  - Without it: special characters like é, ñ, ü, 中文, العربية might display as ????
  - With it: everything displays correctly ✅
- **Always include this**

#### Line 5: `<meta name="viewport" ...>`
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- This makes your page **responsive** (looks good on mobile devices)
- `width=device-width` = "Make the page width match the device's screen width"
- `initial-scale=1.0` = "Don't zoom in or out by default"
- Without this, your page on a phone would look like a tiny desktop page that you have to pinch-zoom into
- **Always include this**

#### Line 6: `<title>Document</title>`
```html
<title>Document</title>
```
- This sets the text shown in the **browser tab**
- Also used as the title in **search engine results** (Google)
- Also used when someone **bookmarks** your page
- **Change it** to something meaningful:
```html
<title>About Me - Your Name</title>
```

#### Lines 8 and 10: `<body>` ... `</body>`
```html
<body>
    ...visible content goes here...
</body>
```
- **Everything visible on the webpage goes inside `<body>`**
- Text, images, links, buttons, forms — ALL of it goes here
- This is where you'll spend most of your time writing HTML

### Visual Summary

```
<!DOCTYPE html>              ← "I'm an HTML5 document"
<html lang="en">             ← Root element (wraps everything)
  <head>                     ← Invisible metadata
    <meta charset="UTF-8">          ← Character support
    <meta name="viewport" ...>      ← Mobile responsiveness
    <title>Page Title</title>       ← Browser tab title
  </head>
  <body>                     ← VISIBLE content starts here
    (your content here)
  </body>
</html>
```

---

## 2.6 — Your First Visible Content: "Hello, World!"

It's a tradition in programming that your first program always displays "Hello, World!" Let's continue that tradition.

### Step-by-Step:

1. **Place your cursor between the `<body>` and `</body>` tags** (on the empty line 9)

2. **Type the following:**
```html
<body>
    <h1>Hello, World!</h1>
    <p>This is my very first webpage. I built this!</p>
</body>
```

3. **Save the file:** Press `Ctrl+S` (Windows/Linux) or `Cmd+S` (Mac)

4. **Open it in the browser:**
   - Right-click on `index.html` in the VS Code Explorer (left sidebar)
   - Click **"Open with Live Server"**
   - A browser window should open automatically!

### What You Should See

```
┌──────────────────────────────────────┐
│ About Me - Your Name          ×  │  │  ← Browser tab
├──────────────────────────────────────┤
│                                      │
│  Hello, World!                       │  ← Big, bold heading
│                                      │
│  This is my very first webpage.      │  ← Normal paragraph text
│  I built this!                       │
│                                      │
│                                      │
└──────────────────────────────────────┘
```

> 🎉 **Congratulations!** You just built your first webpage! It's simple, but it's REAL. Every professional website started exactly like this.

---

## 2.7 — Experimenting: Break Things on Purpose

The best way to learn is to experiment. Try each of these, one at a time, and **predict what will happen before you save:**

### Experiment 1: Add more text
```html
<body>
    <h1>Hello, World!</h1>
    <p>This is my very first webpage. I built this!</p>
    <p>This is a second paragraph. Each p tag creates a new paragraph.</p>
</body>
```
**Save and check.** Did it appear on a new line? Why? (Because `<p>` is a **block element** — it takes up the full width and starts on a new line)

### Experiment 2: Try different heading levels
```html
<body>
    <h1>This is Heading 1 (Biggest)</h1>
    <h2>This is Heading 2</h2>
    <h3>This is Heading 3</h3>
    <h4>This is Heading 4</h4>
    <h5>This is Heading 5</h5>
    <h6>This is Heading 6 (Smallest)</h6>
</body>
```
**Save and check.** Notice how each heading gets progressively smaller.

### Experiment 3: What happens if you forget a closing tag?
```html
<body>
    <h1>Hello, World!
    <p>What happens to this paragraph?</p>
</body>
```
**Save and check.** What does the paragraph look like? Is it styled as a heading too? This demonstrates why **closing tags matter**.

**Now fix it** by adding back the `</h1>`:
```html
    <h1>Hello, World!</h1>
```

### Experiment 4: What happens with extra spaces and blank lines?
```html
<body>
    <h1>Hello,          World!</h1>
    <p>This    has    lots    of    spaces.</p>
    <p>This

    has

    blank lines.</p>
</body>
```
**Save and check.** Surprised? HTML **collapses whitespace**. Multiple spaces become one space. Line breaks inside a tag are ignored. This is a very important behavior to understand!

> 🧠 **Key Insight:** HTML ignores extra spaces and line breaks in your code. To create a new line, you need the `<br>` tag. To create a new paragraph, you need a new `<p>` tag. The spacing in your CODE doesn't affect the spacing in the BROWSER.

### Experiment 5: Try `<br>` for line breaks
```html
<p>Line one<br>Line two<br>Line three</p>
```
**Save and check.** Now the text breaks into separate lines, but stays within the same paragraph. Compare this to using separate `<p>` tags.

---

## 2.8 — Understanding Tags, Elements, and Attributes

Before we end today, let's formalize the vocabulary:

### HTML Tag
A **tag** is the code inside angle brackets:
```html
<h1>     ← Opening tag
</h1>    ← Closing tag (has a forward slash)
<br>     ← Self-closing tag (no closing tag needed)
```

### HTML Element
An **element** is the complete unit — opening tag + content + closing tag:
```html
<h1>Hello, World!</h1>
│              │      │
│   Opening    │ Closing
│     Tag    Content  Tag
│                     │
└─── Entire Element ──┘
```

### HTML Attribute
An **attribute** provides extra information inside the opening tag:
```html
<html lang="en">
       │     │
       │     └── Attribute VALUE
       └── Attribute NAME
```

More examples:
```html
<img src="photo.jpg" alt="My photo" width="300">
      │                │              │
      │                │              └── width attribute
      │                └── alt attribute (alternative text)
      └── src attribute (source file)
```

### The Pattern
```
<tagname attribute="value">Content</tagname>
```

---

# ✅ PART 2 KNOWLEDGE CHECK

Test yourself:

1. What is the purpose of `<!DOCTYPE html>`?
2. What goes in `<head>` vs `<body>`?
3. Why is `<meta charset="UTF-8">` important?
4. What does `<title>` control?
5. What happens when HTML encounters multiple spaces in your code?
6. What is the difference between a tag and an element?
7. What is an attribute? Give an example.

---

# 📝 PART 3: EVENING REFLECTION
## *(15-20 minutes)*

---

## Write in Your Learning Journal

Create a file called `day1-notes.md` in your `about-me` folder (or use a physical notebook) and answer these:

### What I Learned Today
```
Write 3-5 bullet points of the most important things you learned.
Example:
- Every website is just files stored on a server somewhere
- DNS converts domain names to IP addresses
- HTML uses tags to structure content
```

### What Confused Me
```
Write anything that didn't fully click.
It's OKAY to be confused — acknowledging confusion is the first step to understanding.
Example:
- I'm not sure I fully understand how DNS caching works
- I'm confused about when to use <br> vs <p>
```

### What Surprised Me
```
Write anything unexpected.
Example:
- I was surprised that HTML ignores extra spaces
- I didn't know that the entire internet runs on just a few protocols
```

### Tomorrow's Plan
```
Tomorrow I'll learn about:
- More HTML tags: images, links, bold, italic
- Start building the actual content of my About Me page
```

---

# 📊 Day 1 Completion Checklist

Check off each item as you complete it:

```
UNDERSTANDING:
☐ I can explain what a client and server are
☐ I can explain what DNS does
☐ I can describe what HTTP is and name at least 2 status codes
☐ I can explain the difference between HTTP and HTTPS
☐ I can describe what happens when I type a URL into my browser
☐ I know the difference between HTML, CSS, and JavaScript

SETUP:
☐ VS Code is installed and opens correctly
☐ Live Server extension is installed
☐ Prettier extension is installed
☐ Auto Rename Tag extension is installed
☐ Prettier is set to format on save

PROJECT:
☐ "about-me" folder exists
☐ index.html file is created inside it
☐ HTML boilerplate is in place (from the ! shortcut)
☐ Title has been changed from "Document" to something meaningful
☐ "Hello, World!" heading is visible in the browser
☐ A paragraph of text is visible in the browser
☐ I experimented with at least 3 things (headings, spacing, etc.)

REFLECTION:
☐ I completed the knowledge checks (Parts 1 and 2)
☐ I wrote in my learning journal
```

---

> 🌟 **You completed Day 1!** You now understand more about how the internet works than most people who use it every day. And you built your first webpage! Tomorrow, you'll start adding real content — images, links, and more. Rest well, and come back ready to build!
