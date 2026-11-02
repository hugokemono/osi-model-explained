# Networking Devices Explained Simply

A beginner-friendly guide to **networking devices**: what they are, how each one works, and which **OSI layer** each one belongs to. No prior networking knowledge needed, but it helps to know the basics of the [OSI model](#).

---

## What is a networking device?

A **network** is two or more devices connected so they can share information. Your phone, laptop, and printer connected at home make a network.

Devices on a network come in two kinds:

**1. End devices** are where data starts or ends. They are the devices *you* use.
- Phone, laptop, desktop PC, printer, smart TV
- Fancy words: **host** (any device that sends or receives data) and **node** (any device connected to the network)

**2. Networking devices** (also called **intermediary devices**, meaning "in the middle") move data *between* end devices. You usually don't use them directly, but nothing works without them.
- Hub, switch, router, modem, access point, firewall, and others

### Analogy: the postal system

- You and your friend are the **end devices**.
- The post office, sorting centers, and delivery trucks are the **networking devices**.

They don't write the letter. They make sure it reaches the right place.

### Why do we need them?

Imagine 3 computers. You could connect them with cables directly. But what about 100 computers? Or computers in different cities? Without networking devices, every computer would need a cable to every other computer, which is impossible.

Networking devices solve three problems:
1. **Connecting** many devices together
2. **Directing** data to the right destination
3. **Protecting** the network from bad traffic

### What does data look like while travelling?

Data doesn't move as one big lump. It is chopped into small pieces called **packets** (like splitting a big parcel into many small envelopes). Networking devices read the "address label" on each piece and decide where to send it.

### Key words so far

| Word | Meaning |
|------|---------|
| Network | Devices connected to share information |
| End device / host | Where data starts or ends (phone, laptop) |
| Networking device | Sits in the middle and moves data |
| Packet | A small piece of data with an address label |

---

## The devices at a glance

| Device | Job | Reads | OSI layer |
|--------|-----|-------|-----------|
| Repeater | Boosts weak signals | Nothing (just signals) | 1 Physical |
| Hub | Copies data to every port | Nothing | 1 Physical |
| Modem | Translates between the ISP's signal and digital data | Signals | 1 Physical |
| Bridge | Connects two network segments | MAC address | 2 Data Link |
| Switch | Sends data to the right port inside one network | MAC address | 2 Data Link |
| Access point | Gives wireless access to a network | MAC address | 2 Data Link |
| Router | Connects different networks | IP address | 3 Network |
| Firewall | Allows or blocks traffic using rules | IP address, port | 3-4 (up to 7) |
| Gateway | Translates between different protocols | Depends | Any |

**Pattern:** the higher the layer, the smarter the device. Layer 1 devices are "dumb" (just signals), Layer 2 devices understand MAC addresses, and Layer 3 devices understand IP addresses.

We'll go from the simplest device (the hub) to the more advanced ones.

---

## Hub

**Job:** Connect several devices and copy every signal it receives to **all** its ports.

A **hub** is a small box with several **ports**. A *port* here is a physical socket where you plug in a network cable. Plug each computer into one port and they are all connected.

### How does it work?

A hub is "dumb". It doesn't read anything. When one computer sends data, the hub **copies it out of every other port**, whether or not they need it.

```
PC1 --\
PC2 ---\
PC3 ----[ HUB ]   PC1 sends data to PC3
PC4 ---/          -> Hub copies it to PC2, PC3 AND PC4
```

PC2 and PC4 receive the data too, but ignore it because it isn't addressed to them.

**Analogy:** Shouting a message in a room of 5 people when it is meant only for Ali. Everyone hears it.

### Key words

- **Broadcast:** sending data to *everyone* on the network instead of one specific device. A hub does this with everything.
- **Collision:** when two devices send at the same moment, the signals crash into each other and both messages are corrupted. Both must wait and try again.
- **Collision domain:** the group of devices that can collide with each other. On a hub, *all* devices share one big collision domain, so more devices means more collisions and a slower network.
- **Bandwidth:** how much data can travel per second (like the width of a road). On a hub, all devices **share** the same bandwidth.
- **Half-duplex:** a device can send *or* receive, but not both at the same time (like a walkie-talkie where you say "over").

### Problems with hubs

1. **Slow:** shared bandwidth and many collisions
2. **Not secure:** everyone receives everyone's data
3. **Doesn't scale:** it gets worse as you add devices

Hubs are **obsolete** (no longer used) and were replaced by switches. They are still worth learning because they explain *why* switches were invented.

**OSI layer:** Layer 1 (Physical). A hub only repeats electrical signals (0s and 1s) and doesn't understand addresses.

**Data unit:** Bits

**Security angle:**
- **Sniffing:** secretly capturing data as it passes by. On a hub it is easy, because the data is delivered to every device automatically. Tools like Wireshark do this.

---

## Switch

**Job:** Connect devices inside **one network** and send data **only to the correct port**.

A **switch** looks like a hub: a box with many ports. The big difference is that it is **smart**. It learns which device is plugged into which port.

```
PC1 --\
PC2 ---\
PC3 ----[ SWITCH ]   PC1 sends data to PC3
PC4 ---/             -> Switch sends it ONLY to PC3
```

PC2 and PC4 receive nothing.

**Analogy:** A hub shouts to the whole room. A switch walks over and whispers the message directly to Ali.

### What is a MAC address?

Every network device has a unique ID burned into its network card, called a **MAC address**, like `A4:5E:60:C2:1B:9F`. Think of it as the device's **fingerprint** or serial number. No two devices should share one.

### How does a switch know where to send data?

1. **It learns.** When PC1 sends something, the switch reads the *sender's* MAC address and notes: "PC1 is on port 1."
2. **It remembers.** It stores this in the **MAC address table** (also called the CAM table), like a phonebook of MAC address → port number.
3. **It forwards.** When data arrives for PC3, it checks the table and sends it out of PC3's port only.

| MAC address | Port |
|---|---|
| AA:AA:AA:AA:AA:01 | 1 |
| AA:AA:AA:AA:AA:02 | 2 |
| AA:AA:AA:AA:AA:03 | 3 |

### What if the switch doesn't know the destination yet?

At the start the table is empty. If the switch doesn't know where the destination is, it does **flooding**: it sends the data out of all ports except the one it came from. When the right device replies, the switch learns its port and remembers it. The table also forgets entries after a few minutes of silence (called **aging**), so flooding can happen again later.

### Key words

- **Frame:** at Layer 2, data wrapped with MAC addresses is called a *frame*. Think of the envelope with sender and receiver MAC addresses written on it.
- **Unicast:** sending data to *one specific* device.
- **Full-duplex:** a device can send and receive *at the same time* (like a phone call). Switches support this.
- **Each port is its own collision domain:** unlike a hub, collisions basically disappear.

### Hub vs Switch

| | Hub | Switch |
|---|---|---|
| Sends data to | Everyone | Only the right device |
| Intelligence | None | Has a MAC address table |
| Collisions | Many | Almost none |
| Bandwidth | Shared | Each port gets full speed |
| Duplex | Half | Full |
| Security | Very weak | Much better |
| OSI layer | 1 | 2 |

**OSI layer:** Layer 2 (Data Link), because it reads MAC addresses.

**Data unit:** Frame

**Security angle:** a switch is much safer than a hub, but not perfectly safe.
- **Flooding leak:** while the switch floods an unknown destination, every device gets a copy of that frame for a short moment
- **MAC spoofing:** an attacker sends data using the victim's MAC address, so the switch updates its table and starts sending the victim's data to the attacker
- **MAC flooding:** an attacker sends thousands of fake MAC addresses until the table is full, so the switch starts flooding everything like a hub and the attacker can sniff traffic
- **ARP spoofing:** tricking devices into sending their traffic to the attacker
- **Defense:** **port security** limits how many MAC addresses a port may have, or locks a port to one known device

---

## Router

**Job:** Connect **different networks** to each other and choose the best path for data.

A switch connects devices **inside one network**. A **router** connects **different networks**.

**Analogy:** A switch is the roads inside one town. A router is the **highway junction** that links your town to other towns and cities.

```
[Your home network]  ---  ROUTER  ---  [Internet]  ---  [Other country's network]
```

### Key words

- **LAN (Local Area Network):** a small, local network, like your home or college lab.
- **WAN (Wide Area Network):** a big network spread over long distances. The **internet** is the biggest WAN.
- **IP address:** a logical address for a device on a network, like `192.168.1.5`. A MAC address is the device's permanent fingerprint. An IP address is more like a **home address**: it tells where you are, and it can change when you move to another network.
- **Hop:** each router the data passes through is one hop. Data to another country may take 10-20 hops.
- **Routing:** choosing the best path for a packet to reach its destination.

### How does a router work?

1. A packet arrives with a **destination IP address** on it.
2. The router reads that address.
3. It checks its **routing table**, a list of "to reach this network, send the packet this way (next router or port)".
4. It forwards the packet one hop closer.
5. The next router does the same, until the packet arrives.

Each router only decides the **next step**, not the whole route.

### Other jobs a home router does

- **Default gateway:** the "exit door" of your network. When your PC wants to reach anything outside your LAN, it sends the data to the default gateway, which is your router (often `192.168.1.1`).
- **NAT (Network Address Translation):** devices at home have **private IP addresses** that only work inside your network. The internet sees **one public IP address**, the one your ISP gave you. NAT swaps private addresses for the public one when data leaves, and swaps them back when replies come in. Like a **receptionist**: the outside world calls one number, and the receptionist connects the call to the right person inside.
- **DHCP:** automatically gives an IP address to each device that joins, so you don't set them by hand.
- A home router usually also contains a built-in switch and a Wi-Fi access point.

**Private vs public IPs:** addresses starting with `192.168.x.x`, `10.x.x.x`, or `172.16-31.x.x` are **private** and used inside networks. They don't travel on the open internet.

### Switch vs Router

| | Switch | Router |
|---|---|---|
| Connects | Devices inside **one** network | **Different** networks |
| Reads | MAC address | IP address |
| Uses | MAC address table | Routing table |
| Data unit | Frame | Packet |
| OSI layer | 2 (Data Link) | **3 (Network)** |

**OSI layer:** Layer 3 (Network)

**Data unit:** Packet

**Security angle:**
- **Default passwords:** many routers ship with a well-known admin login that people never change
- **Outdated firmware:** old software with known vulnerabilities
- **Weak remote management:** an admin page exposed to the internet
- **IP spoofing and DDoS:** attacks that fake source IPs or flood a router with packets
- **NAT side effect:** NAT hides your internal devices from the internet, but it is **not** a real firewall

---

## Modem

**Job:** Translate between the signal on your ISP's cable and the digital data your network uses.

Your router speaks **digital data** (0s and 1s). The cable coming from your internet provider (phone line, TV cable, or fiber) carries signals in a *different form*: electrical waves, or light on fiber. A **modem** translates between the two machine forms.

**Analogy:** A translator standing between you and someone who speaks another language. Without it, neither side understands.

```
ISP cable --> [ MODEM ] --> [ ROUTER ] --> your devices
```

### Key words

- **ISP (Internet Service Provider):** the company that sells you internet.
- **Modulation:** turning your digital data into the signal the ISP cable carries (going out).
- **Demodulation:** turning the incoming signal back into digital data (coming in).
- **Modem:** the name comes from **Mo**dulator + **Dem**odulator.

### Modem vs Router

- **Modem:** connects you to your ISP (gets you *onto* the internet).
- **Router:** shares that connection with your devices and directs traffic.

You need both. Many ISPs give you one box that does both jobs.

**OSI layer:** Layer 1 (Physical), because it deals with signals, not addresses.

**Security angle:**
- **Default login** on ISP-provided modems
- **Remote management** left on by the provider
- Outdated firmware that you can't easily update yourself

---

## Access Point (AP)

**Job:** Let devices connect to a network **wirelessly** using radio waves.

A switch connects devices using **cables**. An **access point** creates **Wi-Fi**.

**Analogy:** A radio tower that everyone nearby can tune into.

```
Wired network --> [ ACCESS POINT ] ~~~ Wi-Fi ~~~> phones, laptops
```

### Key words

- **Wi-Fi:** wireless networking using radio waves instead of cables.
- **SSID:** the **name** of a Wi-Fi network, the one you see in the Wi-Fi list.
- **Wireless client:** any device connecting by Wi-Fi.
- **Frequency band:** the radio range Wi-Fi uses. **2.4 GHz** has longer range but is slower. **5 GHz** has shorter range but is faster.

### Router vs Access Point

- **Access point:** only gives wireless access to a network that already exists. An office may have many access points to spread Wi-Fi.
- **Wi-Fi router:** the box at home. It is a **modem-connected router + switch + access point** in one.

You can have an access point with no router, and a router with no Wi-Fi at all.

**OSI layer:** Layer 2 (Data Link). It is basically a "wireless switch" that passes frames using MAC addresses.

**Data unit:** Frame

**Security angle:**
- **Weak Wi-Fi password** or old encryption (use WPA2 or WPA3, avoid WEP)
- **Rogue access point:** an unauthorized AP plugged into the network
- **Evil twin:** a fake Wi-Fi with the same name as a real one, to trick people into connecting
- **Wi-Fi sniffing:** radio waves can be received by anyone nearby, so encryption matters

---

## Firewall

**Job:** Look at traffic and decide to **allow** or **block** it, using rules.

A **firewall** is a security guard for your network.

**Analogy:** A guard at a building gate checking every visitor against a list of rules. Some walk in, some are turned away.

```
Internet --> [ FIREWALL ] --> your network
              allow / block
```

### Key words

- **Rule:** an instruction like "block all data from this address" or "allow web browsing".
- **Traffic:** data moving through the network. **Inbound** comes in, **outbound** goes out.
- **Port number:** a number that identifies a service, like a door number on a building (web browsing uses 80 and 443). Don't confuse it with a physical port on a switch.
- **Packet filtering:** the simplest method. It checks each packet's IP address and port against the rules.

A firewall can be **hardware** (a separate box) or **software** (like Windows Firewall on your PC).

**OSI layer:** Mostly Layers 3 and 4 (IP addresses and ports). Advanced firewalls go up to Layer 7.

**Security angle:**
- **Misconfigured rules:** a rule that allows too much defeats the point
- **Allowed traffic can still be malicious:** a firewall that allows port 443 will let attacks inside HTTPS through
- **Firewall evasion:** attackers use techniques to slip past rules, a common topic in pentesting
- **Web application firewall (WAF):** a Layer 7 firewall that protects websites

---

## Gateway

**Job:** Connect networks that **speak different languages** (different protocols) and translate between them.

A **protocol** is a set of rules devices follow to talk to each other, like a language.

**Analogy:** A router passes messages between people who already share a language. A **gateway** is a translator who converts between languages.

In everyday use, people also say "gateway" for the **default gateway**, which is the router that acts as the exit door of your network.

**OSI layer:** Any layer, depending on what it translates.

---

## Repeater

**Job:** Boost weak signals so they can travel farther.

Signals get weaker over distance in a cable or through the air. This fading is called **attenuation**. A **repeater** takes the weak signal, **cleans it up, boosts it, and sends it on**.

**Analogy:** A person standing halfway between you and a faraway friend, who hears your faint shout and repeats it loudly.

A **Wi-Fi extender** is a common repeater: it catches weak Wi-Fi and rebroadcasts it to cover a bigger area.

A hub is basically a repeater with many ports.

**OSI layer:** Layer 1 (Physical). It doesn't understand addresses, it just boosts the signal.

**Security angle:** a repeater extends your network's reach, so it also extends how far an attacker can sit and connect. An unprotected extender is a weak point.

---

## Bridge

**Job:** Connect **two network segments** and pass data between them using MAC addresses.

A **segment** is one small part of a network. A **bridge** learns which devices are on which side and only forwards data that needs to cross.

**Analogy:** A bridge between two islands. Local traffic stays on its own island, and only people who need to cross use the bridge.

A bridge is the older, simpler version of a **switch**. A switch is like a bridge with **many ports**, which is why switches replaced bridges.

**OSI layer:** Layer 2 (Data Link)

**Data unit:** Frame

---

## Putting it all together

### A real journey: your PC to a PC in another country

```
Your PC
  |  (cable or Wi-Fi)
Access point / Switch      <- Layer 2, uses MAC addresses, inside your home
  |
Router                     <- Layer 3, uses IP address, leaves your home network
  | (firewall checks traffic here)
Modem                      <- Layer 1, converts to the signal your ISP uses
  |
ISP and many more routers  <- hop after hop across the internet
  |
Other country's modem, router, switch
  |
The other PC
```

Step by step:
1. Your PC sends data to the **switch** or **access point**, which passes it along inside your home using MAC addresses.
2. The **router** sees the destination IP is outside your network and forwards it to the ISP. **NAT** swaps your private IP for the public one.
3. The **firewall** checks the traffic against its rules.
4. The **modem** converts it into the signal your ISP's cable carries.
5. Many **routers** on the internet each pick the next hop until it reaches the other country.
6. The other side does the same steps in reverse, and the data reaches the other PC.

### Memory tricks

- **Hub = shouts** to everyone. **Switch = whispers** to the right person.
- **Switch = inside** a network (MAC). **Router = between** networks (IP).
- **Modem = translator** to the ISP. **Access point = radio tower** for Wi-Fi.
- **Firewall = guard.** **Repeater = booster.** **Gateway = language translator.** **Bridge = old, simple switch.**

---

## Summary

| Device | Job | Address / key idea | OSI layer | Analogy |
|--------|-----|--------------------|-----------|---------|
| Repeater | Boost weak signals | Signals | 1 | Person repeating a shout |
| Hub | Copy data to all ports | Broadcast | 1 | Shouting in a room |
| Modem | Translate ISP signal to digital data | Modulation | 1 | Translator |
| Bridge | Connect two segments | MAC address | 2 | Bridge between islands |
| Switch | Send data to the right port | MAC address | 2 | Whispering to one person |
| Access point | Wireless access | MAC address | 2 | Radio tower |
| Router | Connect different networks | IP address | 3 | Highway junction |
| Firewall | Allow or block traffic | IP, port, rules | 3-4 (up to 7) | Security guard |
| Gateway | Translate between protocols | Protocol conversion | Any | Language translator |

---

## Quick quiz

1. Which device sends data to only one port, and which address does it read? *(Switch, MAC address)*
2. Which device connects your home network to the internet? *(Router, together with a modem)*
3. Your Wi-Fi reaches only half your house. Which device helps? *(Repeater / extender, or another access point)*
4. Which device would you use to block suspicious traffic? *(Firewall)*
5. Which device works at Layer 1 and copies every signal to every port? *(Hub)*
6. What is the difference between a modem and a router? *(The modem connects you to your ISP. The router shares that connection and directs traffic between networks.)*

---

*Feel free to open an issue or suggest improvements.*
