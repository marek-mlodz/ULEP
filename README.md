![image](image2.png)

# ULEP Ultra Lightweight Embedded Protocol

## 1. Protocol purpose
Protocol was created to provide a transport layer for applications running over limited bandwidth or data transfer cost expensive link, e.g. satellite connection.

## 2. System architecture
System basis on large, but limited number of active clients, which connect to the base server using TCP/IP protocol. Every message has a header which contains message data type and some type specific data.

## 3.Workflow
Client connects to a known server application using CONNECT message:  
![image](image3.png)  
Chart 3.1 Connect workflow

After successful connection data transmission can begin in both directions:  
![image](image4.png)  
Chart 3.2 Data transmission workflow

When a client decides to close the connection, it has to send a disconnect message. After receiving this message, there should be no further data flow:  
![image](image1.png)

## 4. Messages description

### 4.1 Messages types and format
All messages starts with header byte which has format shown in table 4.1.1:  
Bit number | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 
-|-|-|-|-|-|-|-|-
Description <td colspan=2>Type<td colspan=6>Type specific usage

Table 4.1.1Header byte format

All messages have type value present in bits 7 and 6 in byte 1. These types are described in Table 4.1.2.  
| Type            | Value | Description                                                  |
|-----------------|-------|--------------------------------------------------------------|
| CONNECT/CONNACK | 0x00  | Message sent while connecting and acknowledging connection   |
| TRANSMIT        | 0x40  | Message sent from client to server and from server to client |
| TRANSACK        | 0x80  | Acknowledge message just after receiving message             |
| DISCONNECT      | 0xC0  | Message sent while client disconnects                        |

Table 4.1.2 Message types

### 4.2 Connect/Connack
Client is the side, which initializes connection. Client sends a fixed length message of connect type and waits for a response from the server with a connack message. Connack has a fixed length of 1 byte. Connect and connack messages are the same type.
####  4.2.1 Connect
Connect message is 22 byte long message, which should be sent by client during first communication. It contains 4 information in proper bit configuration described in table 4.2.1.1. Those values are:
* Type - 2 bits defining message type
* Keep alive - 6 bits value of levels of keep alive time (predefined in whole system)
* Client ID - client individual identifier defined in 4 byte space (unsigned 32 bit integer)
* Server API key for authorization - value which has to be equal to predefined value stored in server application.  

|     **Byte 1**    |             |   |            |   |   |   |   |   |
|---------------------|-------------|---|------------|---|---|---|---|---|
| Bit number          | 7           | 6 | 5          | 4 | 3 | 2 | 1 | 0 |
| Description          <td colspan=2>Type (0x40) <td colspan=6> Keep alive |
|        Values       | 0           | 0 | X          | X | X | X | X | X |
|**Byte 2-6 (default)**|             |   |            |   |   |   |   |   |
| Bit number          | 7           | 6 | 5          | 4 | 3 | 2 | 1 | 0 |
| Description         <td colspan=8>Client ID   
| Values              | X           | X | X          | X | X | X | X | X |
|**Byte 7-22 (default)**|             |   |            |   |   |   |   |   |
| Bit number          | 7           | 6 | 5          | 4 | 3 | 2 | 1 | 0 |
| Description          <td colspan=8>API Key
| Values              | X           | X | X          | X | X | X | X | X |

Table 4.2.1.1
