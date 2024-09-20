# Getting started with Wireshark

Author: James Commons

Date: 9/20/2024

## DAYTIME

### 1. TCP 3-way handshake

- 1 192.168.64.8 132.163.97.1 TCP 74 59392 → 13 [SYN] Seq=0
Win=32120 Len=0 MSS=1460 SACK_PERM TSval=2039058013 TSecr=0 WS=128

- 3 132.163.97.1 192.168.64.8 TCP 66 13 → 59392 [SYN, ACK] 
Seq=0 Ack=1 Win=65535 Len=0 MSS=1382 WS=64 SACK_PERM

- 5 192.168.64.8 132.163.97.1 TCP 54 59392 → 13 TACK] Seq=1 Ack=1 
Win=32128 Len=0

### 2. Port number

The client uses port 59392 for this interaction.

### 3. Why port number?

The client also needs a port because the server needs to know which
application it should send data to, and the port number is how the 
application is identified.

### 4. Daytime response

6 132.163.97.1 192.168.64.8 DAYTIME 105 DAYTIME Response

### 5. SYN, ACK

SYN and ACK mean synchronize and acknowledge respectively. SYN is used
to initialize a TCP connection, and ACK is used to let acknowledge a
transmission.

### 6. Who closed the connection?

The daytime server initiated the closing of the TCP connection. We know
this because this server was the first to send a FIN packet, which 
lets the other computer know it wants to finalize the connection.

## HTTP

### 1. How many connections?

It looks like there were two TCP connections openned when I loaded the 
webpage. Looking through the packets, I found SYN packets from two 
different ports (35942, 35950). These two packets initiated two separated 
TCP connections, and the server acknowledged both of them in the 3-way
handshake.

### 2. Homepage request

Yes, here is the summary of the GET request from Wireshark:
5 192.168.64.8 172.233.221.124 НТТР 452 GET /index.html HTTP/1.1

### 3. Photo request

Summary of photo request: 
11 192.168.64.8 172.233.221.124 НТТР 425 GET /jeff-square-colorado.jpg HTTP/1.1

## QUESTIONS

- What does the PSH flag mean in a TCP packet?
- What is the maximum size of a TCP packet?

