Chapter 2 A Single Segment Network – Data Link Layer
====

## Data Link Layer

#### Main tasks

- Transfer network layer data from one machine to another machine via “a data link”.
- Convert the data between raw bit stream of the physical layer and groups of bits →bytes → frames.
- Perform flow control between sender and receiver.

## Types of Networks

#### Point-to-point network

- Each link connects two end points: hosts or any network devices/elements 
- Usually for long distance connections 
- Examples: DSL (digital Subscriber Loop), POS (Packet over SONET/SDH), GbE (Gigabit Ethernet)

#### Broadcast network

- A number of stations share a common transmission medium 
- Usually for local networks 
- Examples: CSMA/CD Ethernet, WLAN (Wireless Local Area Network), a.k.a. Wi-Fi 

## Point-to-Point Protocol

- The Point-to-Point Protocol (PPP) is a data link protocol 

- The main purpose of PPP is encapsulation and transmission of IP datagrams, or other network layer protocol data, over a serial link. 

- Currently, PPP is used by most dial-up Internet access, Digital Subscriber Loop (DSL), and cable broadband services. 

  ![ppp](sources/ch2-2.png)

### Point-to-Point Protocol (RFC 1661)

PPP consists of two types of control protocols:

- Link Control Protocol (LCP)

  1. Responsible for agreeing on PPP encapsulation options, packet size limits, and  detecting common mis-configuration errors over the data link

  2. Optional features to provide peer authentication, detect link status

- Network Control Protocol (NCP)

  1. **PPP supports a family of NCPs and treat each network protocol like an interface**

  2. **IP Control Protocol (IPCP, RFC 1332), used for configure the link to transmit IP datagrams**

     ```
     discuss: wat???
     ```

## PPP Encapsulation

See [PPP Encapsulation](format.md)

## Local Area Networks

- Local Area Networks (LANs) typically connect computers within a building or a campus. 
- Many LANs are broadcast networks. 
- Bus and Ring are two typical LAN topologies used in early days 
- The protocol that determines who can transmit on a broadcast channel is called Medium Access Control (MAC) protocol. 

![ch2-4](sources/ch2-4.png)

## MAC and LLC

- In any broadcast network, the stations must ensure that only one station transmits at a time on the shared communication channel. **(MAC Layer's responsibility)**
- The protocol that determines who can transmit on a broadcast channel is called Medium Access Control (MAC) protocol. 
- The MAC protocol is implemented in the MAC sublayer which is the lower sublayer of the data link layer. 
- The higher portion of the data link layer is often called Logical Link Control (LLC). 

### MAC

- **Discuss: MAC Layer's responsibility is to control the order of concurrent data to shared medium. **
- MAC algorithm: 
  - ALOHA
  - [CSMA/CD](#csmacd)
  - CSMA/CA

#### CSMA/CD

![ch2-5](sources/ch2-5.png)

```
1.Each station listens before it transmits.
2.If the channel is busy, it waits until the channel goes idle, and then it transmits.
3.If the channel is idle it transmits immediately and continue sensing for 2a seconds.
4.If collision is detected, transmit a brief jamming signal then cease transmission.
5. Wait for a random time, and retransmit. The random time is determined by exponential backoff algorithm.

a is assumed to be the maximum propagation delay on the network
```



**DISCUSS**: pros and cons

```

```

#### Collisions in Ethernet

- The collision resolution process of Ethernet requires that a collision is detected while a station is still transmitting.
- Restrictions: Each frame should be at least twice as long as the time to detect a collision (2a).

![collision](sources/ch2-6.png)

**DISCUSS**: In what way the host sniffing the collision? Is it because host A get data while it is sending data to the shared medium? If it is half duplex, will the data received be messed up?

```

```

#### Exponential Backoff Algorithm

- If a station is involved in a collision, it waits a random amount of time before attempting a retransmission.
- The random time is determined by the following algorithm:
  - Set “slot time” to 2a. 
  - After first collision wait 0 or 1 time unit. 
  - After the ith collision, wait a random number between 0 and 2i-1 time slots. 
  - Do not increase random number range if i>9. 
  - Give up after 16 collisions. 
```python
MAX_TRY = 16
MAX_WAIT = 9
def sendData(data):
    slotTime = 2 * a; # a means the max propagation delay
    waitTime = 0
    while not detectCollision() and waitTime < MAX_TRY:
        waitTime = min(MAX_WAIT, waitTime+1)
        wait(random.randint(2**(waitTime - 1)) * a)
    return False if waitTime == MAX_TRY else True
```

![ch2-7](sources/ch2-7.png)

### LLC

- LLC can provide different services to the network layer:
  - “unacknowledged” connectionless service
  - acknowledged connectionless service
  - connection-oriented service

## Ethernet Switches

In an Ethernet LAN, hosts can be 

- Attached to a common cable, or 
- Connected by Ethernet switches. 

Ethernet switches are MAC layer devices that switch frames between ports connected to different LAN segments.

- Offer guaranteed bandwidth for segments. 
- Separate a LAN into collision domains. 

## Ethernet Encapsulation 

#### RFC 894

See [Ethernet Encapsulation](format.md)

#### IEEE 802.2/802.3

See [Ethernet Encapsulation](format.md)

