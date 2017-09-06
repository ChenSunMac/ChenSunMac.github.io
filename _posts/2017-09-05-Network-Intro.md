---
layout: post
title: Data Communication and Computer Network
date: 2017-09-05
tags: Communication Network
categories: Basics
---


### LAN(Local Area Network) and WAN(Wide Area Network)

- **Ethernet**  is most widely employed LAN technology and uses Star topology
<br>
**Ethernet** uses Carrier Sense Multi Access/Collision Detection (CSMA/CD) technology to detect collisions. On the occurrence of *collision* in Ethernet, all its hosts **roll back**, wait for some random amount of time, and then *re-transmit* the data.
<br>
**Ethernet connector** is, network interface card equipped with 48-bits MAC address.


### OSI and ISO

Open System Interconnect (OSI) is an open standard for all communication systems. OSI model is established by International Standard Organization (ISO).

It has 7 layers:

- **Application Layer**: This layer is responsible for providing interface to the application user. This layer encompasses protocols which directly interact with the user.

- **Presentation Layer**: This layer defines how data in the native format of remote host should be presented in the native format of host.

- **Session Layer**: This layer maintains sessions between remote hosts. For example, once user/password authentication is done, the remote host maintains this session for a while and does not ask for authentication again in that time span.

- **Transport Layer**: This layer is responsible for end-to-end delivery between hosts.

- **Network Layer**: This layer is responsible for address assignment and uniquely addressing hosts in a network.
    + Routers: route packages based on IP address

- **Data Link Layer**: This layer is responsible for reading and writing data from and onto the line. Link errors are detected at this layer.
    + Network Switches (Layer 2): route packages based on MAC address
    + DSLAM

- **Physical Layer**: This layer defines the hardware, cabling wiring, power output, pulse rate etc.


### Internet Model (TCP/IP protocol suite)

Internet uses TCP/IP protocol suite, also known as Internet suite. This defines Internet Model which contains four layered architecture.

- **Application Layer**: This layer defines the protocol which enables user to interact with the network.For example, FTP, HTTP etc.

- **Transport Layer**: This layer defines how data should flow between hosts. Major protocol at this layer is Transmission Control Protocol (TCP). This layer ensures data delivered between hosts is in-order and is responsible for end-to-end delivery.

- **Internet Layer**: Internet Protocol (IP) works on this layer. This layer facilitates host addressing and recognition. This layer defines routing.

- **Link Layer**: This layer provides mechanism of sending and receiving actual data.Unlike its OSI Model counterpart, this layer is independent of underlying network architecture and hardware.

### DSLAM (Stinger Chip)

**DSLAM** stands for Digital subscriber line access multiplexer

<img src="https://upload.wikimedia.org/wikipedia/commons/d/d5/XDSL_Connectivity_Diagram_en.svg"> </img>

The DSLAM equipment collects the data from its many modem ports and aggregates their voice and data traffic into one complex composite "signal" via **multiplexing**.
<br>
The **DSLAM** acts like a **network switch** since its functionality is at *Layer 2* of the ***OSI model***. 
