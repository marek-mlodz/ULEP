![image](image2.png)
#ULEP Ultra Lightweight Embedded Protocol


##1. Protocol purpose
Protocol was created to provide a transport layer for applications running over limited bandwidth or data transfer cost expensive link, e.g. satellite connection.
##2. System architecture
System basis on large, but limited number of active clients, which connect to the base server using TCP/IP protocol. Every message has a header which contains message data type and some type specific data.
## 3.Workflow
Client connects to a known server application using CONNECT message:
