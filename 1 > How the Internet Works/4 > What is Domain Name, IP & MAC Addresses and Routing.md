This tutorial fits under your roadmap section:

> **Phase 1 → How the Internet Works → What is Domain Name, IP & MAC Addresses and Routing**

You’ll learn:

- What **domain names**, **IP addresses**, and **MAC addresses** are  
- How they relate to each other  
- What **routing** is and how it moves data across networks  

Along the way, you’ll get **thought questions** and **mini-challenges** so you’re pushed to reason, not just read.

---

## 1. Big Picture: “How does data know where to go?”

Imagine sending a package to a friend:

- You write their **street address** (city, postal code, house number).
- Inside the house, someone sees whose name is on the package and **delivers it to the right person/room**.

On the Internet, the same idea appears in layers:

- **Domain Name** → Human-friendly label (`google.com`)
- **IP Address** → Network address (like city + street + house)
- **MAC Address** → Hardware address on a local network (like the exact apartment/door)
- **Port** (we’ll touch it) → Which program inside the computer (like room number)
- **Routing** → The “postal system” logic that decides how to move data hop by hop

You’ll see how each piece participates in getting data from **your device** to a **server** and back.

---

## 2. Domain Names – Web Addresses for Humans

### 2.1. What is a domain name?

A **domain name** is a human-readable label for a resource on the Internet, usually a website.

Examples:
- `google.com`
- `github.com`
- `myportfolio.dev`
- `docs.example.org`

Humans are bad at remembering IPs like `142.250.72.174`, but good at names like `google.com`.  
Domain names solve that.

### 2.2. Domain name structure

Typical domain: `www.example.com`

- `.com` → Top-Level Domain (TLD)
- `example` → Second-level domain (registered name)
- `www` → Subdomain (often the web version, but not required)

Other examples:
- `api.example.com` (subdomain `api`)
- `blog.mysite.dev`
- `subdomain.department.university.edu`

### 2.3. Domain names and DNS

To actually send data, computers need **IP addresses**, not names.

So we have **DNS (Domain Name System)**, a global “phonebook” that maps:

> `example.com` → `93.184.216.34` (an IP address)

Process:
1. You type `example.com` in the browser.
2. Your computer asks a DNS resolver: “What IP is `example.com`?”
3. DNS responds with an IP.
4. Your computer uses that IP to send packets.

---

### Think & Reflect #1

1. Why don’t we just use IP addresses everywhere and skip domain names entirely?
2. If a website changes its **server IP address**, how can DNS help users still reach it without learning a new address?

Write 3–5 sentences answering both questions in your own words.

---

## 3. IP Addresses – Network Addresses in the Internet

### 3.1. What is an IP address?

An **IP address** is a numeric label for a device (or router, or server) on an IP network.  
It lets routers know **where to send packets**.

Two main versions:
- **IPv4**: 4 numbers separated by dots, each 0–255  
  - e.g. `192.168.1.10`, `8.8.8.8`
- **IPv6**: longer, hexadecimal, separated by `:`  
  - e.g. `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

You can think of IP as the “street + house number” in the worldwide network.

### 3.2. Public vs Private IP addresses

Not all IPs are equal. Some are **public**, some are **private**.

- **Public IP**:
  - Unique on the global Internet
  - Reachable from outside networks
  - Example: your home router’s Internet-facing IP

- **Private IP**:
  - Used inside local networks (home/office)
  - Not directly reachable from the Internet
  - Common private ranges (IPv4):
    - `10.0.0.0 – 10.255.255.255`
    - `172.16.0.0 – 172.31.255.255`
    - `192.168.0.0 – 192.168.255.255`

For example:
- Your laptop might be `192.168.0.5`
- Your phone might be `192.168.0.6`
- Your router might be `192.168.0.1` (gateway)

To the **outside world**, they all share your router’s **public IP**, and the router uses **NAT** (Network Address Translation) internally.

### 3.3. IP is at the “network layer”

In the TCP/IP model:
- IP works at the **network layer**.
- It deals with **routing** across different networks.
- It doesn’t care about:
  - Which app is sending traffic (browser, game, etc.)
  - Ensuring packets arrive in order (that’s TCP/UDP’s job).

---

### Mini-Challenge #1 – Your IPs

1. Find your **public IP**:  
   - Search “what is my IP” in your browser.
2. Find your **local (private) IP**:
   - Windows: `ipconfig` in Command Prompt
   - macOS/Linux: `ifconfig` or `ip addr` in Terminal

Questions:
- Are they the same or different?
- Which one is in a private range?

Write down your two IPs and one sentence explaining where each is used.

---

## 4. MAC Addresses – Hardware IDs on a Local Network

### 4.1. What is a MAC address?

A **MAC address** (Media Access Control address) is a unique identifier assigned to a **network interface card** (NIC):

- Looks like: `A8:3E:6D:44:92:1B`
- 6 pairs of hexadecimal digits (0–9, A–F)
- Usually **burned into hardware** by the manufacturer

This identifies your physical network interface (Wi‑Fi card, Ethernet card) on a **local network**.

Think of MAC as:
> “The card’s permanent ID inside the local network segment.”

### 4.2. Where is MAC used?

MAC addresses operate at the **data link layer** (e.g., Ethernet, Wi‑Fi).

Use cases:
- Your **router** needs to know which device on your local network should receive a frame.
- A **switch** uses MAC addresses to forward frames only to the correct port instead of broadcasting to all.

When you send something to another device within your local network:

1. Data is wrapped in an **Ethernet frame**.
2. Frame includes:
   - Source MAC address (your NIC)
   - Destination MAC address (target NIC)
3. The switch uses MAC tables to deliver it to the right device.

Between networks (across the Internet), **routers strip and regenerate** data link headers (including MAC), so MACs don’t travel end-to-end.

### 4.3. IP vs MAC – how they work together locally

- **IP address**:
  - Logical address (can change, can be reassigned)
  - Used for routing from one network to another

- **MAC address**:
  - Physical-ish address (attached to the NIC)
  - Used **only within the local network**

Example in your home:
- Your computer wants to send to `192.168.0.1` (router’s IP).
- It first uses **ARP (Address Resolution Protocol)** to ask:
  > “Who has IP `192.168.0.1`? Tell me your MAC address.”
- Router replies with its MAC.
- Then your computer sends frames to that MAC on the local network.

---

### Think & Reflect #2

1. Why do we need **MAC addresses** if we already have IP addresses?
2. Why are MAC addresses mostly relevant **inside** a local network and not across the whole Internet?

Try to reason this out before searching. Answer in 3–4 sentences.

---

## 5. Domain → IP → MAC: How They Connect

Let’s connect everything:

When you visit `https://example.com`:

1. **Domain name → IP (DNS)**
   - Your system looks up `example.com` via DNS.
   - It gets, say, `93.184.216.34` as the IP address of the server.

2. **IP routing across the Internet**
   - Your computer wraps data (HTTP request) inside a TCP segment, then an IP packet with:
     - Source IP: your (public or NAT-translated) IP
     - Destination IP: `93.184.216.34`
   - That packet is sent to your router and beyond, across many networks.

3. **MAC on the local link**
   - On your Wi‑Fi/Ethernet, the packet is further wrapped into a **frame** with:
     - Source MAC: your NIC
     - Destination MAC: your router’s NIC (local hop)
   - Each local hop (PC→router, router→ISP edge, etc.) uses MAC addresses appropriate for that local link.

So:
- Domain names are **for humans**.
- DNS maps domains → IPs (network layer).
- IP + routing gets packets across different networks.
- MAC addresses deliver frames on each **local hop**.

---

### Mini-Challenge #2 – Layer Mapping

For the action: “Visit `https://google.com` from your laptop at home”

Identify which layer each item belongs to:

- Domain name
- IP address
- MAC address
- HTTP request
- DNS lookup
- Ethernet/Wi‑Fi

Try to place them under these categories: **human-friendly naming**, **network layer**, **data link layer**, **application layer**.

Write your mapping; even if you’re unsure, guessing thoughtfully is part of learning.

---

## 6. What is Routing?

Now that you know what needs to be addressed (IP & MAC), let’s see how data **travels** across the world: **routing**.

### 6.1. Routing in simple terms

**Routing** is the process of deciding **which path** data packets should take from the **source IP** to the **destination IP** across different networks.

Key points:
- Your device doesn’t know the whole path.
- It sends packets to a **default gateway** (your router).
- Each **router** along the way:
  - Looks at the **destination IP**
  - Consults its **routing table**
  - Decides the **next hop** (the next router)

Analogies:
- Postal system: local post office → regional center → other country’s center → local office → recipient.
- Airline travel: local airport → hub airport → destination airport.

### 6.2. Routing tables

Each router stores a **routing table**:

- Entries like:
  - For network `10.0.0.0/8` → use next hop X
  - For network `192.168.0.0/16` → use next hop Y
  - For “everything else” (default route) → use next hop Z

When a packet arrives:
1. Router checks the packet’s **destination IP**.
2. Finds the **best matching rule** in the routing table.
3. Forwards the packet to the **next hop**.

This continues until:
- The packet reaches a router that is directly connected to the destination network.
- That router delivers it via local MAC addressing to the final machine.

### 6.3. Default gateway at home

On your home network:
- Your **router** is your **default gateway**.
- Your computer’s routing table says:
  - “For local network (e.g. `192.168.0.0/24`), send directly.”
  - “For everything else (Internet), send to `192.168.0.1` (gateway).”

So if the destination IP is not local, your computer:
- Wraps the packet
- Sends to the router’s **MAC address**
- Letting the router handle the rest (it knows the broader Internet).

---

### Mini-Challenge #3 – View Your Routing Table

On your machine:

- Windows: open Command Prompt and run  
  `route print`
- macOS/Linux: open Terminal and run  
  `netstat -rn` or `ip route`

Look for:
- The **default route** (often `0.0.0.0` or `default`)
- The **gateway IP** (usually your router, e.g. `192.168.0.1`)

Questions:
1. What is your default gateway IP?
2. Does it match your router’s address in the browser (often `192.168.0.1` or `192.168.1.1`)?

Write down the default gateway and how you think it’s used in routing.

---

## 7. Global Routing: Networks of Networks

Your router knows how to reach your ISP.  
Your ISP’s routers know how to reach **other ISPs** and networks.

The Internet is a **network of networks (autonomous systems)**:

- Each large network (ISP, big company, cloud provider) is an **Autonomous System (AS)**.
- They connect at **Internet Exchange Points (IXPs)** and via private links.
- They use routing protocols (like **BGP**) to exchange information about:
  - Which IP ranges (prefixes) they can reach
  - Which paths are preferred

When your packet goes from you to a faraway server:
1. Your router → ISP → regional routers
2. Then across one or more large provider networks
3. Finally to the destination’s ISP and local routers.

You can observe this using `traceroute` / `tracert` (as in previous tutorials).

---

### Think & Reflect #3

1. Why doesn’t every router need to know the **full path** to every possible IP address on Earth?
2. What would happen if routers tried to keep a separate entry for every individual IP instead of ranges (like `192.168.0.0/16`)?

Explain your reasoning in 3–5 sentences.

---

## 8. Putting It All Together: A Full Journey

Let’s narrate the journey of visiting `https://example.com`:

1. **You type `example.com` in the browser.**
2. **DNS lookup**:
   - Computer asks DNS: “IP for `example.com`?”
   - DNS replies: `93.184.216.34`.

3. **Create request**:
   - Browser creates an HTTP request.
   - OS wraps it in TCP, then IP with:
     - Source IP: your public IP (or local IP → NAT)
     - Destination IP: `93.184.216.34`.

4. **Local delivery (MAC)**:
   - Packet wrapped in an Ethernet/Wi‑Fi frame:
     - Source MAC: your device’s MAC
     - Destination MAC: router’s MAC (default gateway)
   - Frame travels from your NIC → router.

5. **Routing through the Internet**:
   - Router strips the local frame, examines the IP header.
   - Forwards the packet to the next router based on routing table.
   - Each router does similar steps, across ISPs and networks, possibly through undersea cables.

6. **Final network**:
   - Last router that’s directly connected to the server’s local network:
     - Sends a frame with:
       - Source MAC: that router’s interface
       - Destination MAC: server’s NIC
   - IP destination = server’s IP; MAC gets it onto the correct local machine.

7. **Response**:
   - Server builds an HTTP response.
   - Wraps it in TCP/IP with reversed IPs.
   - Sends it back along similar but not necessarily identical hops.
   - At each hop, MAC addresses change; IP source/destination stay the same.

8. **Browser displays page**:
   - Your device reassembles packets (TCP).
   - Browser parses HTML, CSS, JS, and renders the page.

Throughout:
- **Domain name** only at the beginning (resolved to IP).
- **IP addresses** persist end-to-end.
- **MAC addresses** change at each local hop.
- **Routing** decides each next hop based on IP.

---

## 9. Concept Check – Quick Questions (Self-Quiz)

Try to answer these without looking back. If you’re unsure, answer with your best guess, then verify.

1. What is the main reason we use **domain names** instead of only IP addresses?
2. Can your computer have **multiple IP addresses**? Under what conditions?
3. Does your MAC address change when you move from Wi‑Fi to Ethernet on the same machine? Why or why not?
4. When your browser connects to a site:
   - Which is used by **routers** to forward packets: IP or MAC?
   - Which is used by a **switch** inside your LAN: IP or MAC?
5. Where in the journey do **routing tables** matter the most?

Write down short answers; checking them yourself will cement the concepts.

---

## 10. Small Practical Tasks

### Task 1 – Find Your MAC Address and Router MAC

1. On your machine:
   - Windows: `ipconfig /all`
   - macOS/Linux: `ifconfig` or `ip link`
2. Find the MAC of:
   - Your Wi‑Fi or Ethernet adapter.
3. Check your router’s MAC:
   - Often shown in the router admin page (`192.168.0.1` or similar), or printed on the router.

Question:
- Why do you and the router both need MAC addresses on the local network?

---

### Task 2 – Domain/IP Swap

Pick a site, e.g., `example.com`.

1. Find its IP:  
   - Use `ping example.com` or an online DNS lookup.
2. Paste the IP directly into your browser address bar:  
   - e.g., `http://93.184.216.34`

Observation:
- Does it show the same site or something different?
- If different, why might that be? (Hint: virtual hosting, multiple domains per IP, HTTPS certificates.)

---

### Task 3 – Draw the Address Layers

On paper, draw three nested layers:

- Outer: **Domain Name**
- Middle: **IP Address**
- Inner: **MAC Address**

Then:
- For a sample journey (your laptop → your router → a server), write one concrete example of each (e.g. `google.com`, `142.250.72.206`, `A8:3E:6D:...`) and how/where it’s used.

This visual will help when you move later to more advanced topics in networking and web dev.

---

## 11. How This Fits Your Overall Learning

In your roadmap, this topic helps unlock:

- **How ISP and DNS work together to deliver data**  
- **Client–Server Architecture** (understanding addresses is critical)
- **HTTP & HTTPS** (they ride on top of IP and routing)
- Later: deployment, scaling, microservices, etc.

Once you’re comfortable with domains, IPs, MACs, and routing, you’ll find it much easier to grasp:

- Why “pointing a domain to a server IP” is needed
- How “load balancers” sit in front of multiple servers
- Why “private networks”, “VPCs”, and “subnets” exist in cloud platforms

---

If you’d like next:

- I can give you a **10-question quiz** (mix of conceptual and practical) on domains, IP, MAC, and routing.
- Or, I can walk you through a **step-by-step trace** (with real commands) of a request from your machine all the way to a remote server, annotating addresses and hops.
