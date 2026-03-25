## 1. What is the TCP 3‑Way Handshake?

The **TCP 3‑way handshake** is the process used to:

1. Start a **reliable connection** between a client and a server.
2. Let both sides **agree on initial sequence numbers**.
3. Confirm that **both directions** can send and receive.

It’s called “3‑way” because it uses **three messages**:

1. `SYN`  
2. `SYN + ACK`  
3. `ACK`

After these three steps:
- The connection is **established**.
- Both sides know they can send and receive data.
- TCP can now provide its guarantees: reliability, ordering, etc.

We’ll walk through this step by step in a way that’s useful for a developer just starting with networking (Phase 1 level).

---

## 2. Before the handshake: client vs server roles

In TCP, the two endpoints have different roles in establishing a connection:

- **Server**
  - Listens on a specific **port** (e.g., `80` for HTTP, `443` for HTTPS).
  - Waits for incoming connection attempts.
  - Example: A web server listening on `0.0.0.0:443`.

- **Client**
  - Initiates the connection.
  - Uses an **ephemeral (random high) port** for itself (e.g., `52314`).
  - Example: Your browser connecting from `192.0.2.5:52314` to `203.0.113.10:443`.

So the connection is between:

```text
Client: 192.0.2.5:52314
Server: 203.0.113.10:443
```

---

## 3. Basic idea of sequence and acknowledgment numbers

TCP treats data as a **stream of bytes**, and each byte has a **sequence number**.

- When a TCP endpoint sends data, it labels bytes with sequence numbers.
- The receiver sends back **ACK numbers**:
  - “I’ve received everything up to byte N‑1; please send from byte N next.”

During the **handshake**, they don’t send actual data yet, but they **agree on starting sequence numbers**. That’s what the handshake really sets up.

---

## 4. The 3 steps of the handshake

Let’s use this naming:

- Client’s initial sequence number: `ISN_C`
- Server’s initial sequence number: `ISN_S`

### Step 1 – Client → Server: SYN

The client starts by sending a **SYN** segment.

- **SYN** (Synchronize) flag is set.
- Client chooses a random **initial sequence number** `ISN_C`.
- No application data is sent yet—just control info.

So the first packet is conceptually:

```text
Client → Server:
  Flags: SYN
  Seq:   ISN_C
  Ack:   (not valid yet)
```

Purpose:
- “I want to start a TCP connection, and my first sequence number will be `ISN_C`.”

---

### Step 2 – Server → Client: SYN + ACK

If the server accepts the connection:

- It responds with a packet that has **both SYN and ACK flags set**:
  - **SYN**: server also wants to synchronize its own sequence numbers.
  - **ACK**: server acknowledges the client’s SYN.

Server also chooses its own **initial sequence number**, `ISN_S`.

So the second packet is:

```text
Server → Client:
  Flags: SYN, ACK
  Seq:   ISN_S
  Ack:   ISN_C + 1
```

Meaning:
- “I agree to start a TCP connection.”
- “My sequence will start at `ISN_S`.”
- “I received your SYN with sequence `ISN_C`; I’m acknowledging up to `ISN_C + 1`.”

Why `ISN_C + 1`?  
The SYN itself “consumes” one sequence number, even though it doesn’t carry data. So the next expected byte from the client is `ISN_C + 1`.

---

### Step 3 – Client → Server: ACK

The client now sends a third packet with the **ACK** flag set:

- It acknowledges the server’s SYN.
- No SYN flag now (handshake is almost done).
- No application data yet (though in some optimizations, data may be piggybacked).

Third packet:

```text
Client → Server:
  Flags: ACK
  Seq:   ISN_C + 1      (client’s next byte)
  Ack:   ISN_S + 1
```

Meaning:
- “I’ve received your SYN (sequence `ISN_S`), so I’m acknowledging up to `ISN_S + 1`.”
- “My next byte of data will start at `ISN_C + 1`.”

At this point:

- Both sides know each other’s initial sequence numbers.
- Both sides have acknowledged each other.
- The **connection state becomes ESTABLISHED** on both ends.

---

## 5. Visual summary of the 3‑Way Handshake

Putting it all together in a simple diagram:

```text
Client                                      Server
------                                      ------

  1) SYN
  Seq = ISN_C  --------------------------->  (LISTEN)
                                             2) SYN + ACK
                                   <-------  Seq = ISN_S
                                            Ack = ISN_C + 1

  3) ACK
  Seq = ISN_C + 1 ------------------------> 
  Ack = ISN_S + 1

          [TCP CONNECTION ESTABLISHED]
```

After this, **both**:

- Client state = ESTABLISHED
- Server state = ESTABLISHED

Now they can start exchanging data (e.g., HTTP requests and responses).

---

## 6. Why 3 steps? Why not 2?

The 3‑way handshake solves a few important problems:

### 6.1 Agreement in both directions

We need to ensure:
- Client can reach server.
- Server can reach client.

Two messages aren’t enough to confirm both directions and exchange sequence numbers robustly.

### 6.2 Protection against old/duplicate packets

Sometimes old packets from previous connections can still be in the network (delayed). The handshake:

- Uses **fresh sequence numbers**.
- Requires both sides to **acknowledge** each other’s ISNs.
- Helps TCP detect and ignore outdated segments from old connections.

### 6.3 Establishes initial sequence numbers for both sides

Each side chooses an **initial sequence number** to:

- Make connections **unique** (even if IPs/ports repeat over time).
- Improve security slightly against some guessing attacks.
- Help avoid confusion with duplicated segments from older connections.

Without a proper handshake, it would be difficult to coordinate this safely.

---

## 7. What happens after the handshake?

Once the handshake is complete:

- Both sides can start sending **data segments**.
- Data from client to server:
  - Starts at sequence `ISN_C + 1`.
- Data from server to client:
  - Starts at sequence `ISN_S + 1`.

As they send data:

- Each side updates sequence numbers based on bytes sent.
- Each side sends **ACKs** with the next sequence number they expect.
- TCP will retransmit if ACKs are missing (reliability).

Example (client sending “GET / HTTP/1.1\r\n\r\n” over HTTP):
- That string is a sequence of bytes.
- TCP assigns sequence numbers, sends packets, waits for ACKs.

---

## 8. How this looks in real tools (e.g., Wireshark)

If you capture network traffic with a tool like **Wireshark** when opening a website, you’ll see something like:

1. `SYN` from client to server
2. `SYN, ACK` from server to client
3. `ACK` from client to server
4. Then `HTTP GET / ...` over the established TCP connection

In Wireshark, the first few lines might look like:

```text
1  Client → Server  TCP  [SYN] Seq=0 Win=...
2  Server → Client  TCP  [SYN, ACK] Seq=0 Ack=1 Win=...
3  Client → Server  TCP  [ACK] Seq=1 Ack=1 Win=...
4  Client → Server  HTTP GET / ...
```

(Actual sequence numbers are often shown starting at 0 in the UI, but internally they’re large random numbers.)

---

## 9. Handshake from HTTP/HTTPS point of view

When you type a URL in your browser:

1. **DNS lookup**: Find the server’s IP.
2. **TCP 3‑way handshake**: Establish reliable connection (what we’ve just explained).
3. **(For HTTPS)**: Perform **TLS handshake** on top of TCP (for encryption).
4. **HTTP request**: `GET / ...`
5. **HTTP response**: HTML, CSS, JS, images, etc., all over the same TCP connection.

So, the TCP 3‑way handshake is the **foundation step** before HTTP or HTTPS can even start sending real data.

---

## 10. Short recap for your Phase 1 notes

You can summarize “How connection is established using TCP (3‑Way Handshake)” like this:

1. **Client sends SYN**:
   - Chooses an initial sequence number `ISN_C`.
   - Sends a packet: `SYN, Seq = ISN_C`.

2. **Server replies with SYN + ACK**:
   - Chooses its own `ISN_S`.
   - Acknowledges the client’s SYN: `Ack = ISN_C + 1`.
   - Sends `SYN, ACK, Seq = ISN_S, Ack = ISN_C + 1`.

3. **Client sends final ACK**:
   - Acknowledges server’s SYN: `Ack = ISN_S + 1`.
   - Sends `ACK, Seq = ISN_C + 1, Ack = ISN_S + 1`.

After these 3 steps:
- Both sides are in **ESTABLISHED** state.
- They know each other’s initial sequence numbers.
- Data transfer (e.g., HTTP) can begin.

If you want next, I can explain **how a TCP connection is closed (4‑way FIN handshake)** or give you a simple code example (in Node.js or Python) showing client/server sockets around this handshake.
