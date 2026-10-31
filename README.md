# The OSI Model Explained (notes)

A beginner-friendly guide to the **OSI model** and its **7 layers**. No prior networking knowledge needed.

---

## What is the OSI model?

**OSI** stands for **Open Systems Interconnection**.

When you open a website, many things happen: signals travel through cables or Wi-Fi, data is split into pieces, routed across the world, put back together, and shown in your browser. Different companies build different parts of this (cables, routers, operating systems, browsers). For everything to work together, everyone needs to agree on **who does which job**.

The OSI model is that agreement. It splits the job of "sending data from one computer to another" into **7 layers**, and each layer has **one specific responsibility**.

### Analogy: sending a letter

- You write the letter (the content)
- You put it in an envelope and write the address
- The post office sorts it and picks a route
- Trucks and planes physically carry it

Each person only cares about their own job. The truck driver doesn't read your letter, and you don't drive the truck. Network layers work the same way.

### Key ideas

1. Each layer does **one job** and only talks to the layer directly above and below it.
2. When you **send** data, it goes **down** the layers (7 → 1). When you **receive** data, it goes **up** (1 → 7).
3. OSI is a **model for understanding**, not real software. The real internet uses a simpler model called TCP/IP, but OSI is the standard way to learn and talk about networking.

### Why it matters

People constantly say things like "that's a Layer 7 attack" or "this firewall works at Layer 3." Knowing the layers helps you describe exactly **where** a problem or attack happens, which is especially useful in IT and cybersecurity.

---

## The 7 layers at a glance

| # | Layer | Job | Data unit |
|---|-------|-----|-----------|
| 7 | Application | Network services for apps | Data |
| 6 | Presentation | Translate, encrypt, compress | Data |
| 5 | Session | Manage conversations | Data |
| 4 | Transport | Deliver to the right program (ports, TCP/UDP) | Segment |
| 3 | Network | Deliver across networks (IP, routers) | Packet |
| 2 | Data Link | Deliver on the local network (MAC, switches) | Frame |
| 1 | Physical | Send raw bits | Bits |

**Memory trick (top to bottom):** *"All People Seem To Need Data Processing"*
(Application, Presentation, Session, Transport, Network, Data Link, Physical)

We'll go from the bottom (Layer 1) to the top (Layer 7).

---

## Layer 1: Physical

**Job:** Move raw **bits** (0s and 1s) between devices over a physical medium.

This layer doesn't know what the data means. It only turns bits into something that can physically travel, and back into bits at the other end.

**How bits travel:**
- **Electrical signals** over copper cables (Ethernet): high voltage = 1, low voltage = 0
- **Light pulses** through fiber optic cables: light on = 1, light off = 0
- **Radio waves** through the air (Wi-Fi, Bluetooth, mobile data)

**Examples:** Ethernet cables, fiber, Wi-Fi signals, hubs, repeaters, network ports (RJ45)

**Analogy:** The road, truck, and plane. It carries the package but doesn't know what's inside.

**Data unit:** Bits

**Typical problems:** unplugged or damaged cable, weak Wi-Fi signal, broken port, interference

**Security angle:** physical attacks such as cutting or tapping a cable, jamming Wi-Fi, or plugging a rogue device into an open network port.

---

## Layer 2: Data Link

**Job:** Deliver data between devices on the **same local network**, and check for errors.

**What it adds:**
1. **Framing:** groups bits into organized chunks called **frames**
2. **MAC addresses:** every network card has a unique hardware address (like `A4:5E:60:C3:1B:9F`). Layer 2 uses these to say who a frame is from and who it's for.
3. **Error detection:** a checksum at the end of the frame lets the receiver detect corruption and discard bad frames

**A frame, simplified:**

| Destination MAC | Source MAC | The actual data | Error check |
|---|---|---|---|

### What is a local network (LAN)?

A group of devices connected together in one small area, like a home, classroom, or office. Your phone, laptop, and printer on the same Wi-Fi router form one local network. Layer 2 can only deliver within one LAN. It can't navigate across the internet.

### What is a switch?

A **switch** is a box with many ports where devices are plugged in. It reads the **destination MAC address** of each frame and sends it **only to the right port**. Over time it builds a table:

| Port | MAC address |
|---|---|
| 1 | Laptop |
| 2 | Printer |
| 3 | PC |

A **hub** (Layer 1) is dumb and copies every signal to every port. A **switch** is smart and sends frames only where needed, which is faster and more private.

**Analogy:** A mail clerk inside one apartment building who delivers to the right apartment, but can't deliver to another city.

**Examples:** switches, Ethernet, Wi-Fi (802.11), network cards

**Data unit:** Frame

**Security angle:**
- **MAC spoofing:** faking a MAC address to impersonate a device
- **MAC flooding:** overwhelming a switch so it broadcasts everything, letting an attacker sniff traffic
- **ARP spoofing:** tricking devices into sending their traffic to the attacker

---

## Layer 3: Network

**Job:** Deliver data across **different networks**, even across the world.

**What it adds:**
1. **IP addresses:** logical addresses such as `192.168.1.5` (IPv4) or `2001:db8::1` (IPv6). Unlike MAC addresses, they are assigned and can change depending on the network.
2. **Packets:** data wrapped with a header containing the **source IP** and **destination IP**
3. **Routing:** choosing the path a packet takes

**A packet, simplified:**

| Source IP | Destination IP | The actual data |
|---|---|---|

### What is a router?

A **router** is any device that reads the destination IP address and forwards the packet toward the right network. It checks its **routing table** ("to reach this network, send it that way") and passes the packet to the next router. Each router only decides the **next hop**, not the whole path.

A router does not have to be a Wi-Fi device. A home Wi-Fi box is actually three things in one: a router (Layer 3), a switch (Layer 2), and a Wi-Fi access point. Between your home and a website there are many other routers, large machines owned by internet providers and companies, connected by fiber.

**Analogy:** The postal system between cities. It reads the address and decides which sorting center to send the letter to next, until it reaches the right building. Then Layer 2 handles the last step.

### How Layers 2 and 3 work together

- The **IP addresses stay the same** for the whole trip (source device → destination device)
- The **MAC addresses change at every hop** (they only describe the next stop)

### See it yourself: traceroute

Run this in a terminal to see each router your data passes through:

```
tracert google.com          (Windows)
traceroute google.com       (Linux / macOS)
```

- Each numbered line is one **hop** (one router)
- The three times are the response times of 3 test packets, in milliseconds
- A `*` means one test got no reply. This is normal, since some routers ignore these requests.
- Times usually grow with each hop because the routers get farther away

**Private vs public IPs:** addresses starting with `192.168.x.x`, `10.x.x.x`, or `172.16-31.x.x` are **private** and used inside networks like homes or internet providers. They don't travel on the open internet.

**Examples:** routers, IP, ICMP (used by `ping`)

**Data unit:** Packet

**Security angle:**
- **IP spoofing:** faking the source IP
- **DDoS attacks:** flooding a target with packets
- **Network scanning:** discovering which IPs are alive (tools like Nmap)
- **Firewalls** commonly filter by IP address

---

## Layer 4: Transport

**Job:** Deliver data to the right **program** on a device, and decide how reliably.

Layer 3 gets data to the right device. But a device runs many programs at once, so Layer 4 uses **ports** to pick the right one.

### Ports

A **port** is a number (0 to 65535) that identifies a program or service on a device. If the IP address is the building, the port is the apartment number.

| Port | Service |
|---|---|
| 80 | HTTP (websites) |
| 443 | HTTPS (secure websites) |
| 22 | SSH (remote login) |
| 53 | DNS |
| 21 | FTP |

`142.250.122.102:443` means: IP address + port = one specific program on one specific device.

### TCP vs UDP

**TCP (Transmission Control Protocol): reliable but slower**
- Sets up a connection first
- Numbers data so it can be put back in order
- Confirms what arrived and **resends anything lost**
- Used for: websites, email, downloads, logins

**UDP (User Datagram Protocol): fast but no guarantees**
- No connection setup, just sends
- No confirmation, no resending
- Used for: video calls, gaming, live streaming, DNS lookups

| | TCP | UDP |
|---|---|---|
| Reliable? | Yes | No |
| Speed | Slower | Faster |
| Connection setup | Yes | No |
| Example | Web browsing | Video calls |

**Why use UDP?** In a video call, resending a lost half-second of audio would arrive too late to matter. Speed beats perfection.

### The TCP three-way handshake

1. Client → Server: "I want to connect" (**SYN**)
2. Server → Client: "Okay, I'm ready, are you?" (**SYN-ACK**)
3. Client → Server: "Yes, let's go" (**ACK**)

**Data unit:** Segment (TCP) / Datagram (UDP)

**Security angle:**
- **Port scanning:** finding which ports are open (Nmap)
- **SYN flood:** starting loads of connections and never finishing them, to overwhelm a server
- **Firewalls** often allow or block specific ports

---

## Layer 5: Session

**Job:** Open, manage, and close a **conversation (session)** between two programs.

A session is an ongoing exchange with a start, a middle, and an end.

**Analogy:** A phone call. Someone dials (start), you talk (maintain), you hang up (end). Layer 5 manages the call, not the words.

**What it does:**
1. **Starts** sessions
2. **Maintains** them while data flows
3. **Ends** them properly
4. **Checkpoints:** adds save points to big transfers so they can resume after a drop instead of starting over
5. **Dialogue control:** decides whose turn it is to talk

**Examples:** staying logged in to a website, a video call staying open, resuming an interrupted download, a remote SSH session. Protocols often linked to this layer: NetBIOS, RPC, PPTP.

**Note:** This is the fuzziest layer. In real networks, session handling is usually done by the applications themselves. It is still a separate layer in the model because the job exists.

**Data unit:** Data

**Security angle:**
- **Session hijacking:** stealing someone's active session (for example, a login cookie) to impersonate them without needing their password
- **Session timeouts** automatically end inactive sessions to reduce this risk

---

## Layer 6: Presentation

**Job:** Make sure data is in a form the receiver can understand by **translating, encrypting, and compressing** it.

**1. Translation (formatting)**
Different systems can represent data differently, so this layer converts between formats.
- Text: ASCII, UTF-8
- Images: JPEG, PNG, GIF
- Audio/Video: MP3, MP4
- Data: JSON, XML

**2. Encryption**
Data is scrambled before sending so interceptors only see gibberish, then unscrambled at the other end. **SSL/TLS** (which makes HTTPS secure) is traditionally placed here.

**3. Compression**
Data is made smaller so it travels faster (ZIP files, compressed images and video).

**Analogy:** An interpreter plus a vacuum-packing service. It translates the message and packs it small for the trip.

**Example:** sending a photo on a messaging app. It is compressed, encrypted, and kept in a standard format (JPEG) so the other phone can display it.

**Note:** Like Layer 5, this layer is fuzzy in real life. TLS doesn't fit neatly into one OSI layer, and different books place it differently. The standard exam answer is: **translation, encryption, compression.**

### What is a digital certificate?

A **certificate** is an ID card for a website. When you visit an HTTPS site, it shows its certificate to prove it really is who it claims to be. Certificates are issued by trusted organizations called **Certificate Authorities (CAs)**, such as DigiCert or Let's Encrypt. Browsers come with a built-in list of CAs they trust. If a certificate is not trusted, the browser shows a warning like "Your connection is not private."

**Data unit:** Data

**Security angle:**
- **Weak or outdated encryption** (old SSL versions, weak ciphers) can be broken
- **Man-in-the-middle attacks:** if encryption is missing or a certificate is faked, an attacker can read or change traffic
- **Malicious files** hidden inside images or documents

---

## Layer 7: Application

**Job:** Give programs a way to **use the network**. This is the layer closest to the user.

**Common confusion:** Layer 7 is **not** the app itself. Chrome and WhatsApp are not Layer 7. Layer 7 is the **set of protocols** those apps use to talk over the network.

**Analogy:** Ordering at a restaurant. You don't need to know how the kitchen works, you just use the menu and the ordering process. That agreed process is Layer 7.

| Protocol | What it does | Port |
|---|---|---|
| HTTP / HTTPS | Loads websites | 80 / 443 |
| DNS | Turns names (google.com) into IP addresses | 53 |
| FTP | Transfers files | 21 |
| SSH | Secure remote login | 22 |
| SMTP | Sends email | 25 |
| IMAP / POP3 | Receives email | 143 / 110 |
| DHCP | Automatically gives devices an IP address | 67/68 |

### Example: opening a website

1. **DNS** finds the website's IP address
2. Your browser sends an **HTTP request**: "give me the home page"
3. The server sends back an **HTTP response** with the page
4. Your browser displays it

The six layers below exist to carry these requests and responses.

**Data unit:** Data

**Security angle:** most real-world attacks happen here because this layer faces users directly.
- **SQL injection:** tricking a website's database with malicious input
- **XSS (cross-site scripting):** injecting malicious scripts into pages
- **Phishing:** fake sites and emails that steal passwords
- **Brute-force logins** on SSH, FTP, or web forms
- **DNS attacks** that send you to fake sites
- **Web application firewalls (WAFs)** defend at this layer

---

## Summary

| # | Layer | Job | Address / key idea | Data unit | Examples |
|---|-------|-----|--------------------|-----------|----------|
| 7 | Application | Network services for apps | Protocols | Data | HTTP, DNS, FTP, SSH, SMTP |
| 6 | Presentation | Translate, encrypt, compress | Formats, encryption | Data | TLS/SSL, JPEG, MP3, ZIP |
| 5 | Session | Manage conversations | Sessions, checkpoints | Data | Logins, resuming transfers |
| 4 | Transport | Deliver to the right program | Port numbers | Segment / Datagram | TCP, UDP |
| 3 | Network | Deliver across networks | IP address | Packet | Routers, IP, ICMP |
| 2 | Data Link | Deliver on the local network | MAC address | Frame | Switches, Ethernet, Wi-Fi |
| 1 | Physical | Send raw bits | Signals | Bits | Cables, fiber, hubs, Wi-Fi |

**Remember:** *All People Seem To Need Data Processing* (7 → 1)

---

## Quick quiz

1. Which layer uses MAC addresses? *(Layer 2)*
2. Which layer do routers work at? *(Layer 3)*
3. Which layer uses port numbers and TCP/UDP? *(Layer 4)*
4. Which layer handles encryption? *(Layer 6)*
5. Is Chrome a Layer 7 protocol? *(No. Layer 7 is the protocols, like HTTP, not the app.)*

---

*Feel free to open an issue or suggest improvements.*
