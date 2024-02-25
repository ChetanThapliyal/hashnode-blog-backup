---
title: "Building Blocks of Networks: Devices"
seoTitle: "Cables, Switches, & Routers: The Backbone of Your Internet Speed"
seoDescription: "Uncover the secrets of network devices. Explore cables, switches, routers, and how they impact your internet speed and reliability."
datePublished: Sun Feb 25 2024 19:01:13 GMT+0000 (Coordinated Universal Time)
cuid: clt1vljkc000409l3fkx24aee
slug: building-blocks-of-networks-devices
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708884105386/fe711a6e-262e-4a9c-bdc7-d502a6d82281.png
tags: networking, networking-for-beginners, basics-of-networking, network-devices, networking-for-devops

---

Slow internet? Constant connection drops? No internet during rain? Ever wondered why? Understanding the devices that make up your network is the first step to fixing common problems and optimizing your online experience. Let's dive in and discover the key players in your network's world.

## **Cables: The Pathways of Data**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708883619193/4009928d-e1be-4166-ae43-fbdf27bbb7db.png align="center")

We live in a world where devices outnumber people, and the invisible threads that bind them together are cables. A simple cable might not seem like much, but it plays a crucial role in how we work, play, and communicate.

But not all cables are created equal!

### Copper Cables ⚡

**Copper cables**, the traditional workhorses, has been used for electrical wiring for centuries due to its conductivity. Data travels through copper cables as electrical voltages representing the zeros and ones of digital information. These zeros and ones are represented by different voltage levels within the wire:

* **One:** A higher voltage level is used to signify a digital "one." The actual voltage can vary depending on the specific signaling standard, but a common example is 5 volts.
    
* **Zero:** A lower voltage level, often close to zero volts (ground), represents a digital "zero."
    

Devices and computers connected by copper cables have circuitry designed to detect these voltage fluctuations:

* **Threshold:** They have a built-in voltage threshold. Anything above the threshold is interpreted as a "one," and anything below is interpreted as a "zero."
    
* **Clock signals:** For accurate timing and ensuring data is read correctly, devices often use a separate "clock signal" This clock signal provides a steady pulse that helps the device know precisely when to check the voltage level on the data line.
    

**Example**

Imagine a series of rapid voltage changes sent down the copper cable: 5 volts, 0 volts, 5 volts, 0 volts, 0 volts. The receiving device, synchronized with the clock signal, would interpret this as the binary sequence 10100.

#### **Copper Cables: Cat5e, Cat6, and Cat7**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708884695216/e91d5319-a9a7-4eca-a1db-f8b2c69bd53e.png align="center")

The most common copper cables in networking have names like Cat5, Cat5e, and Cat6. These categories relate to how tightly the wires inside are twisted, which affects data speeds and how well they resist interference. The main differences between these cable categories are their speed, frequency, common uses, and improvements.

| **Feature** | **Cat5e** | **Cat6** | **Cat7** |
| --- | --- | --- | --- |
| Speed | 1 Gbps | 10 Gbps (limited to shorter distances) | 10 Gbps |
| Frequency | 100 MHz | 250 MHz | 600 MHz (sometimes 1000 MHz) |
| Common Uses | Most home networks, streaming, general internet use | Home and business networks requiring higher speeds and better interference resistance | Specialized environments like data centers |
| Improvements | N/A | Thicker conductors, more shielding, tighter twists | Individual shielding for each wire pair, overall cable shielding |
| Connector | RJ45 | RJ45 | Different connector (not RJ45) |

#### Crosstalk in copper cables

While reliable and affordable, copper cables have limitations. "Crosstalk" is a common issue where an electrical pulse on one wire accidentally leaks onto another. This interference gets worse over longer distances or when lots of cables are bundled together, slowing down data and potentially causing errors.

### Fiber

Enter fiber optic cables – the high-speed champions of data transmission. Instead of metal wires, fiber cables contain incredibly thin strands of glass called optical fibers. Data hurtles through these fibers as pulses of light, making them blazing fast compared to copper. What's more, light doesn't suffer from the same interference problems as electricity, allowing fiber cables to carry clear signals over vast distances.

**It's All About Light Pulses**

Unlike copper cables, fiber optic cables don't use voltage levels to transmit data. Instead, they rely on pulses of light:

* **One:** A pulse of light is sent down the fiber to represent a digital "one."
    
* **Zero:** The absence of a light pulse represents a digital "zero."
    

**How Devices Detect Zeros and Ones in Fiber**

Devices connected to fiber optic cables use special components to detect these light pulses:

* **Photodetector:** A photodetector is a light-sensitive device that converts the light pulses back into electrical signals. When a light pulse hits the photodetector, it generates a tiny electrical current.
    
* **Threshold:** Similar to copper cables, there's a threshold for the electrical current. If the current surpasses the threshold, it's registered as a "one." No current (or current below the threshold) means a "zero."
    
* **Clock signals:** As with copper, clock signals are essential in fiber optics to ensure accurate timing and synchronization between the sending and receiving devices.
    

**Example**

Imagine a series of light flashes traveling through the fiber optic cable: flash, no flash, flash, flash, no flash. The receiving device, using the photodetector and in sync with the clock signal, would interpret this as the binary sequence 10110.

#### **Key Advantages of Fiber Optics**

* **Speed:** Light travels incredibly fast through glass fibers, enabling far higher data transmission speeds compared to copper.
    
* **Interference Immunity:** Light signals aren't affected by electromagnetic interference like electrical signals are. This makes fiber optic cables ideal for noisy environments or when signal integrity is critical.
    
* **Distance:** Light signals can travel much longer distances within fiber optic cables before weakening compared to electrical signals in copper.
    

## **Hubs, Switches and Routers: Managing Data Traffic**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708886230224/a845ef74-2361-466e-adb4-23d6f21b406a.png align="center")

### **Hubs (Layer 1: Physical): The Simplest Connection**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708886012980/bff5e539-5d99-42cd-9b28-f7fe3f611629.png align="center")

A physical layer device that allows for connections from many computers at once. A hub acts like a basic junction box. It simply receives data packets (frames) on one port and broadcasts them out to all other ports. Think of it as shouting in a crowded room – everyone hears the same message, regardless of whether it's intended for them.

#### Collision Domain

It is a network segment where only one device can communicate at a time. If multiple system try sending data at the same time, the electrical pulse sent across the cable can interfere with each other. This causes these systems to have to wait for a quiet period before they try sending their data again.

### **Switches (Layer 2: Data Link): Smarter Traffic Control**

Switches are far more sophisticated than hubs. They operate at Layer 2, the data link layer. This means they can read the MAC addresses (unique hardware identifiers) of devices connected to them. Using this information, switches cleverly build a "map" of the network, allowing them to send data packets *only* to the intended destination. It is like a smart traffic cop. It reads the destination address of data packets and only sends them where they need to go. This dramatically improves network efficiency!

**When to Use What**

* **Hubs:** Rarely used in modern networks. Might be seen in very small, simple home setups or specialized legacy applications.
    
* **Switches:** The standard for building home and business networks. They offer the efficiency, performance, and security needed for today's connected devices.
    

### **Routers (Layer 3: Network): Navigating the Internet**

If hubs and switches are the traffic cops within your local network, routers are the guides that help you navigate the vast expanse of the internet. These intelligent devices operate at Layer 3, the network layer, of the OSI model, making decisions about where to send your data as it hops between different networks.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708886394052/4bd80a4d-3cae-41ad-8f01-f6133325eca4.png align="center")

**Beyond your LAN:** While hubs and switches connect devices within a single network (LAN), routers link *different* networks together, creating the vast internet.

**IP Addresses:** Routers understand IP addresses, the unique numerical identifiers assigned to devices on the internet. Every time you access a website or send an email, your data packets contain both your IP address (the source) and the destination IP address.

**Routing Tables:** Routers maintain routing tables – essentially maps of the internet. These tables tell the router which paths are available to reach different destinations.

**Border Gateway Protocol (BGP):** Routers share data with each other via this protocol, which lets them learn about the most optimal paths to forward traffic. Think of it as the GPS system of the internet.

**Beyond Basic Routing**

Routers also perform other vital functions:

**Connecting Networks:** Routers connect different networks, like your home network to your internet service provider's network.

**Firewalls:** Many routers have built-in firewalls, protecting your network from unauthorized access.

**Network Address Translation (NAT):** Routers enable multiple devices on your home network to share a single public IP address facing the internet.

## **Servers and Clients: The Providers and the Requesters**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708887066781/5869f2b4-fb7e-4534-b868-92faedfe63ac.png align="center")

In most networks, devices are mainly either servers (data providers) or clients (data consumers). Realistically, almost any device can be both client and server depending on the situation. It's more about the function at that moment.

**Example:** Your email server is called that because it primarily serves emails, but it also needs to ask a DNS server for addresses, making it a client at that time.

### **Servers: The Powerhouses of Resources**

Think of a server as a powerful computer designed to store and share resources. These resources can be:

* **Files:** Websites, documents, images, videos
    
* **Databases:** Stores vast collections of organized information
    
* **Applications:** Software you can access over the network
    
* **Services:** Email, printing, specialized processing tasks
    

Servers are optimized for continuous availability, often having redundant power supplies, specialized hardware, and robust software.

### **Clients: The Consumers of Services**

Clients are the devices or software programs that access the resources provided by servers. Examples of clients include:

* **Web browsers:** Chrome, Firefox, Safari (requesting websites)
    
* **Email Clients:** Outlook, Thunderbird (connecting to email servers)
    
* **Smartphones:** Apps on your phone act as clients connecting to various backend services.
    
* **Other Devices:** Smart TVs, gaming consoles, etc.
    

### **The Client-Server Dance**

The relationship between clients and servers is a request-response dialogue:

1. **Request:** A client sends a request to a server for a specific resource or service.
    
2. **Process:** The server processes the request, often fetching data, performing calculations, or preparing a response.
    
3. **Response:** The server sends the requested data or result back to the client.
    
4. **Receipt:** The client receives and displays the information or uses the provided service.
    

## **Conclusion**

The digital world we rely on is built upon a complex network of devices, each playing a crucial role in connecting us to information, entertainment, and each other. From the humble copper cable to the powerful fiber optic lines, the lightning-fast switches, and the intelligent routers – these components work in concert to bring the internet into your home.