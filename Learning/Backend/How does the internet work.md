The internet relies on **fiber optic cables** laid throughout the world to transmit data. These cables carry information using **light pulses** through a process known as **total internal reflection**, allowing the light to travel long distances with minimal loss.

Data is structured into **packets**, which contain essential information such as the destination address, sender information, and the actual content (payload). Each light pulse represents binary data (1s and 0s), which is encoded and transmitted in these packets.

A single fiber optic cable can transmit many light pulses simultaneously using advanced techniques:

- **Time-Division Multiplexing (TDM)**: Multiple data streams are transmitted in small time slots, interleaved so that they can be sent concurrently.
- **Wavelength-Division Multiplexing (WDM)**: Different wavelengths (colors) of light are used to transmit multiple data streams in parallel, dramatically increasing the cable's capacity.
#### How Data Reaches the End User
1. **Data Transmission Through the Network**:  
    After leaving the sender, the data travels through a series of **routers** and **switches** in the network. These devices direct the data packets along the optimal path, based on network conditions, until they get closer to the end user.
    - **Routers**: These are devices that direct packets between different networks. They inspect the destination IP address in each packet and determine the best route.
    - **Switches**: These operate within local networks, forwarding packets to the correct device in smaller, more localized environments (e.g., within a data center or home network).
2. **Conversion of Light to Electrical Signals**:  
    When the fiber optic cable reaches a **local exchange** (a nearby data hub), the light pulses are converted into **electrical signals** by **optical transceivers**. These signals are then sent over traditional copper cables or wireless networks, depending on the type of connection the user has.
3. **Last Mile Delivery**:  
    The final leg of the journey is known as the **last mile**, where data is delivered to the end user’s home or business. Depending on the infrastructure, the last mile can use:
    - **Fiber-to-the-Home (FTTH)**: Where fiber optics directly connect to the user’s residence, offering the fastest possible speeds.
    - **Copper or Coaxial Cables**: If fiber optics only reach a local hub or cabinet, the data is transmitted over older copper telephone lines or coaxial cables.
    - **Wireless Networks**: In some cases, wireless technologies like **Wi-Fi** or **cellular networks (4G/5G)** are used to transmit the data to the user’s device.
4. **Modem and Router**:  
    Once the data reaches the end user, it goes through a **modem** (if a wired connection like DSL or cable is used), which converts the signal back into digital data that computers and other devices can understand. A **router** then distributes the data to individual devices in the home or office, either over **Wi-Fi** or through Ethernet cables.
5. **Reassembling Packets**:  
    As data packets arrive at the end user’s device (e.g., computer, smartphone), the **Transmission Control Protocol (TCP)** or another networking protocol reassembles the packets in the correct order. If any packets were lost or corrupted, the system requests them to be resent.
6. **End User Interaction**:  
    Finally, the user sees the result of the transmitted data, such as a web page, video stream, or email, through their browser or application.
#### Summary
- The internet uses **fiber optic cables** to transmit data as light pulses over long distances.
- Data is broken into **packets** and transmitted using **multiplexing techniques** like **TDM** and **WDM** to maximize efficiency.
- As data approaches the end user, it is converted from light pulses to electrical signals and delivered via various technologies (fiber, copper, wireless).
- Once at the user’s location, modems, routers, and protocols reassemble and deliver the data to the user’s device.

Sources: *ChatGPT, https://www.youtube.com/watch?v=x3c1ih2NJEg

---
#### What is a switch and why do we need it?
- A switch is a networking device that operates via wired connections (no wireless capability). If wireless technology is required, an access point is used instead of a switch.
- Devices connected to a switch form a Local Area Network (LAN) and can communicate with each other.
- When a computer sends data, it is called a "packet" (or less commonly, a "frame"). The switch receives the packet and forwards it to the correct destination.
- Switch ports are referred to as LAN ports.
- A switch is unnecessary if there’s only one device that needs to connect to the internet.
- Another term for a LAN, often used in large institutions like universities, is Campus Area Network (CAN).
- LANs are typically more secure than Wide Area Networks (WANs) since they don't pass through the internet.
#### What is a router?
- A router connects different networks, enabling devices to communicate across networks, including connecting to the internet.
- A router can connect to a switch and the cable provided by an Internet Service Provider (ISP) to enable internet access.
- While a switch or access point is enough for a LAN, a router is needed for internet connectivity.
- When a computer sends a packet to the internet, it first goes through the switch to the router, which then forwards it to the internet.
- Home routers are often multi-function devices that combine a router, switch, and wireless access point. In environments with many devices, a separate switch may be necessary for optimal performance.
#### What does the internet represent?
- The internet is a global network that connects multiple LANs.
- It doesn’t rely on a single router; instead, it is composed of many distributed routers to avoid a single point of failure and manage limitations like cable length and load.
- If some routers fail, the internet remains functional due to redundancy and load balancing.
- Most global internet communication is handled via underwater fiber optic cables, which are faster and less error-prone than copper cables.
- Once a packet reaches the internet, routers use a "routing table" to determine the best path for it to take. This process is called "forwarding."
- Routers aim to deliver packets efficiently, factoring in congestion and traffic density, not just the shortest path. This is known as congestion control.
#### Meaning of connecting to the internet
- When a computer connects to the internet, it generates a packet and sends it to a router, which forwards it through the internet to its destination.
- For larger content, data is often delivered in chunks via streaming.
#### Wide Area Network (WAN)
- A WAN connects multiple LANs. Unlike the internet, which is a public network, WANs can offer private, more secure communication.
- A common WAN method is via a Virtual Private Network (VPN), which provides secure communication between locations using encrypted "tunnels."
- VPNs improve privacy and security but are not 100% foolproof. Sensitive information often uses end-to-end encryption in addition to VPNs.
- The internet itself is a WAN, but ISPs can also create private WANs, which are typically more expensive than public WANs.
- **Summary:** Switches create LANs, and routers connect LANs to create WANs.
#### Internet Service Provider (ISP)
- ISPs manage the transmission of packets and control routers that facilitate internet communication.
- ISPs are hierarchical: local ISPs connect neighborhoods, regional ISPs connect cities, and global ISPs connect countries.
- Points of Presence (POPs) are physical locations where ISPs house routers, switches, and servers.
- Different ISPs communicate through regional or global ISPs. This hierarchy reduces complexity.
- Peering allows ISPs to establish direct connections between users, improving communication speed and security.
- The internet backbone consists of global ISPs, and Internet Exchange Points (IXPs) enable global synchronization and coordination.

Source: *ChatGPT, https://www.youtube.com/watch?v=zN8YNNHcaZc