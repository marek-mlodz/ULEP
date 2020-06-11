<p style="text-align: center;"><span style="font-weight: 400;"><img src = "image2.png"></span></p>
<p style="text-align: center;"><span style="font-weight: 400;">ULEP </span><span style="font-weight: 400;">Ultra Lightweight Embedded Protocol</span></p>
<h1><span style="font-weight: 400;">1. Protocol description and purpose</span></h1>
<p><span style="font-weight: 400;">Ultra Lightweight Embedded Protocol is a TCP/IP application protocol designed to provide connection for a large number of similar devices. Similarly like in subscribe/publish model data is associated with topics, however it doesn&rsquo;t require&nbsp; client devices to subscribe to it. Topics are described by numbers in range 1-63 and can be mapped to every client on the server side.&nbsp;</span></p>
<p><span style="font-weight: 400;">Protocol was created to provide a transport layer for applications running over limited bandwidth or data transfer cost expensive link, e.g. satellite connection.</span></p>
<h1><span style="font-weight: 400;">2. System architecture</span></h1>
<p><span style="font-weight: 400;">System basis on large, but limited number of active clients, which connect to the base server using TCP/IP protocol. System allows to change the format of some communications between users and server. Because of that it is required to have consistent configuration of all devices.&nbsp;</span></p>
<p><span style="font-weight: 400;">System can work in 2 modes:</span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">Duplex transmission mode - messages transmitted to server in most cases have acknowledge responses</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Non-duplex transmission mode - only client messages are transmitted, server device is not sending responses. Client devices must assume that the server application is running on a particular host, messages were delivered properly, especially the connect message. Transmission works only in direction client -&gt; server.</span></li>
</ul>
<p><span style="font-weight: 400;">Protocol provides distinction of messages sent by different clients. Every client has its own ID in range (by default) 0x01-0xFFFFFFFF. Every incoming connection is assigned to a particular device, what can be stored in an external database. Authorization and authentication can also be based on an external database.</span></p>
<h1><span style="font-weight: 400;">3. Workflow</span></h1>
<p><span style="font-weight: 400;">Client connects to a known server application using CONNECT message:</span></p>
<p><span style="font-weight: 400;"><img src = "image3.png"></span></p>
<p><span style="font-weight: 400;">Chart 3.1 Connect workflow</span></p>
<p><span style="font-weight: 400;">For connect message server will answer with a connack. If connection is successful, data transmission can begin in both directions:</span></p>
<p><img src = "image4.png"></p>
<p><span style="font-weight: 400;">Chart 3.2 Data transmission workflow</span></p>
<p><span style="font-weight: 400;">When a client decides to close the connection, it has to send a disconnect message. After receiving this message, there should be no further data flow:</span></p>
<p><span style="font-weight: 400;"><img src = "image1.png"></span></p>
<p><span style="font-weight: 400;">Chart 3.3 Disconnect workflow</span></p>
<h1><span style="font-weight: 400;">4. Messages description</span></h1>
<p>&nbsp;</p>
<h2><strong>4.1 Messages types and format</strong></h2>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">All messages starts with header byte which has format shown in table 4.1.1:</span></p>
<table border="1">
<tbody>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Type specific usage</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.1.1bHeader byte format</span></p>
<p><span style="font-weight: 400;">All messages have type value present in bits 7 and 6 in byte 1. These types are described in Table 4.1.2.</span></p>
<table border="1">
<tbody>
<tr>
<td>
<p><span style="font-weight: 400;">Type</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Value</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">CONNECT/CONNACK</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0x00</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Message sent while connecting and acknowledging connection</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">TRANSMIT</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0x40</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Message sent from client to server and from server to client</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">TRANSACK</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0x80</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Acknowledge message just after receiving message</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">DISCONNECT</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0xC0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Message sent while client disconnects</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.1.2 Message types</span></p>
<p>&nbsp;</p>
<h2><strong>4.2 Connect/Connack</strong></h2>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Client is the side, which initializes connection. Client sends a fixed length message of connect type and waits for a response from the server with a connack message. Connack has a fixed length of 1 byte. Connect and connack messages are the same type.</span></p>
<p>&nbsp;</p>
<h3><strong>4.2.1 Connect</strong></h3>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Connect message is 22 byte long message, which should be sent by client during first communication. It contains 4 information in proper bit configuration described in table 4.2.1.1. Those values are:</span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">Type - 2 bits defining message type</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Dup - duplex transmission:</span></li>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">0 - duplex transmission is off, server won&rsquo;t be replying to messages, but if all are correct, data will be processed, data transmission possible only from client to server, keep alive parameter is ignored</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">1 - duplex transmission is on, server will be replying</span></li>
</ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">Keep alive - 5 bits value of levels of keep alive time (predefined in whole system)</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Client ID - client individual identifier defined in 4 bytes (unsigned 32 bit integer), 0x00000000 is reserved (can&rsquo;t be assigned to any device)</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Server API key for authorization - value which has to be equal to predefined value stored in server application.</span></li>
</ul>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0x40)</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Dup</span></p>
</td>
<td colspan="5">
<p><span style="font-weight: 400;">Keep alive</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
<tr>
<td colspan="9">
<p><strong>Byte 2-6 (default)</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="8">
<p><span style="font-weight: 400;">Client ID</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
<tr>
<td colspan="9">
<p><strong>Byte 7-22 (default)</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="8">
<p><span style="font-weight: 400;">API Key</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.2.1.1</span></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h3><strong>4.2.2 Connack</strong></h3>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Connack message is 1 byte long and contains 2 fields:</span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">Type - as in connect message (0x00)</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Return code - feedback value from server which determines if connection is successful or there is some issue with connection.</span></li>
</ul>
<p><span style="font-weight: 400;">Format of connack message is described in table 4.2.2.1</span></p>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0x00)</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Return code</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.2.2.1</span></p>
<p><span style="font-weight: 400;">Return codes are described in table 4.2.1.2</span></p>
<table border="1">
<tbody>
<tr>
<td>
<p><strong>Value</strong></p>
</td>
<td>
<p><strong>Name</strong></p>
</td>
<td>
<p><strong>Description</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">0x00</span></p>
</td>
<td>
<p><span style="font-weight: 400;">CONN_OK</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Connection successful&nbsp;</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">0x01</span></p>
</td>
<td>
<p><span style="font-weight: 400;">AUTH_FAIL</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Authorization failed - wrong API key</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">0x02</span></p>
</td>
<td>
<p><span style="font-weight: 400;">ID_PROHIBITED</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Wrong client ID value - server doesn&rsquo;t allow it to authenticate</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">0x03</span></p>
</td>
<td>
<p><span style="font-weight: 400;">CONN_NOK</span></p>
</td>
<td>
<p><span style="font-weight: 400;">Unknown (other) issue</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.2.1.2</span></p>
<p>&nbsp;</p>
<h2><strong>4.3 Transmit</strong></h2>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Transmit message contains 3 bytes header followed by defined data bytes containing message.</span></p>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0x40)</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Topic identifier</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
<tr>
<td colspan="9">
<p><strong>Byte 2</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="8">
<p><span style="font-weight: 400;">Message ID</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
<tr>
<td colspan="9">
<p><strong>Byte 3</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="8">
<p><span style="font-weight: 400;">Message length</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
<tr>
<td colspan="9">
<p><strong>Byte 4+ (optional)</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="8">
<p><span style="font-weight: 400;">User data</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<h2><strong>4.4 Transack</strong></h2>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Transack message has a fixed length of 2 bytes and its content should be equal to the first 2 bytes of the transmit message which it is acknowledging except of the message type. Message should point to the received message by its topic identifier and message ID. Description of message is presented in table 4.4.1.</span></p>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0x80)</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Topic identifier</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
<td>
<p><span style="font-weight: 400;">X</span></p>
</td>
</tr>
<tr>
<td colspan="9">
<p><strong>Byte 2</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="8">
<p><span style="font-weight: 400;">Message ID</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.4.1</span><span style="font-weight: 400;"><br /><br /></span></p>
<p>&nbsp;</p>
<h2><strong>4.5 Ping pong</strong></h2>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Ping and pong messages are introduced to provide control of keeping a connection alive. Those messages derive from Transmit and Transack messages. Topic identifier 0x00 is reserved for ping messages. Ping message is sent from client device to server, Pong is response from server to client on Ping message.</span></p>
<p><span style="font-weight: 400;">Every message received by the server is resetting the keep alive timer. Ping messages are simply null transmissions to reset this timer.</span></p>
<p>&nbsp;</p>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0x40)</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Topic identifier</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.5.1 Ping message</span></p>
<p>&nbsp;</p>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0x80)</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Topic identifier</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.5.2 Ping message</span></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2><strong>4.6 Disconnect</strong></h2>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Disconnect message is fixed length of 1 byte and can be sent from client and server side. It contains only type value, presented in table 4.5.1. When this message is transmitted or received, both sides should close the connection.</span></p>
<p>&nbsp;</p>
<table border="1">
<tbody>
<tr>
<td colspan="9">
<p><strong>Byte 1</strong></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Bit number</span></p>
</td>
<td>
<p><span style="font-weight: 400;">7</span></p>
</td>
<td>
<p><span style="font-weight: 400;">6</span></p>
</td>
<td>
<p><span style="font-weight: 400;">5</span></p>
</td>
<td>
<p><span style="font-weight: 400;">4</span></p>
</td>
<td>
<p><span style="font-weight: 400;">3</span></p>
</td>
<td>
<p><span style="font-weight: 400;">2</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Description</span></p>
</td>
<td colspan="2">
<p><span style="font-weight: 400;">Type (0xC0)</span></p>
</td>
<td colspan="6">
<p><span style="font-weight: 400;">Reserved</span></p>
</td>
</tr>
<tr>
<td>
<p><span style="font-weight: 400;">Values</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">1</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
<td>
<p><span style="font-weight: 400;">0</span></p>
</td>
</tr>
</tbody>
</table>
<p><span style="font-weight: 400;">Table 4.6.1</span></p>
<p>&nbsp;</p>
<h1><span style="font-weight: 400;">5. Example of communication process</span></h1>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Server settings:<br /></span><span style="font-weight: 400;">Api key: 0123456789abcdef</span></p>
<p><strong>Connect packet:</strong></p>
<p><span style="font-weight: 400;">0x3C </span><span style="font-weight: 400; color: #ff0000;">0x00 0x00 0x00 0x01</span> <span style="font-weight: 400; color: #00ff00;">0x30 0x31 0x32 0x33 0x34 0x35 0x36 0x37 0x38 0x39 0x61 0x62 0x63 0x64 0x65 0x66</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0x00 type combined with 0x3C (60) of keep alive</span></p>
<p><span style="font-weight: 400; color: #ff0000;">Cliend ID</span><span style="font-weight: 400;"> - 0x00000001 - 1</span></p>
<p><span style="font-weight: 400; color: #00ff00;">Api key</span><span style="font-weight: 400;"> - 0123456789abcdef utf-8 encoded*</span></p>
<p>&nbsp;</p>
<p><strong>Connack packet:</strong></p>
<p><span style="font-weight: 400;">0x00</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0x00 type combined with 0x00 return code (OK)</span></p>
<p>&nbsp;</p>
<p><strong>Transmit packet (client -&gt; server):</strong></p>
<p><span style="font-weight: 400;">0x41 </span><span style="font-weight: 400; color: #ff0000;">0x00</span> <span style="font-weight: 400; color: #00ff00;">0x04</span> <span style="font-weight: 400; color: #0000ff;">0x74 0x65 0x73 0x74</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0x40 type combined with 0x01 (1) topic ID</span></p>
<p><span style="font-weight: 400; color: #ff0000;">Message ID</span><span style="font-weight: 400;"> - 0x00 (0) as counter starts from 0</span></p>
<p><span style="font-weight: 400; color: #00ff00;">Message length</span><span style="font-weight: 400;"> - 0x04 (4) bytes long</span></p>
<p><span style="font-weight: 400; color: #0000ff;">Message data</span><span style="font-weight: 400;"> - </span><span style="font-weight: 400;">test</span><span style="font-weight: 400;"> utf-8 encoded</span></p>
<p>&nbsp;</p>
<p><strong>Transack packet (server -&gt; client)</strong></p>
<p><span style="font-weight: 400;">0x81 </span><span style="font-weight: 400; color: #ff0000;">0x00</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0x80 combined with 0x01 (1) topic ID</span></p>
<p><span style="font-weight: 400; color: #ff0000;">Message ID</span><span style="font-weight: 400;"> - 0x00 (0) as message ID of received message</span></p>
<p>&nbsp;</p>
<p><strong>Transmit packet (server -&gt; client):</strong></p>
<p><span style="font-weight: 400;">0x41 </span><span style="font-weight: 400; color: #ff0000;">0x00</span> <span style="font-weight: 400; color: #00ff00;">0x04</span> <span style="font-weight: 400; color: #0000ff;">0x74 0x65 0x73 0x74</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0x40 type combined with 0x01 (1) topic ID</span></p>
<p><span style="font-weight: 400; color: #ff0000;">Message ID</span><span style="font-weight: 400;"> - 0x00 (0) as counter starts from 0</span></p>
<p><span style="font-weight: 400; color: #00ff00;">Message length</span><span style="font-weight: 400;"> - 0x04 (4) bytes long</span></p>
<p><span style="font-weight: 400; color: #0000ff;">Message data</span><span style="font-weight: 400;"> - </span><span style="font-weight: 400;">test</span><span style="font-weight: 400;"> utf-8 encoded</span></p>
<p><br /><br /></p>
<p><strong>Transack packet (client -&gt; server)</strong></p>
<p><span style="font-weight: 400;">0x81 </span><span style="font-weight: 400; color: #ff0000;">0x00</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0x80 combined with 0x01 (1) topic ID</span></p>
<p><span style="font-weight: 400; color: #ff0000;">Message ID</span><span style="font-weight: 400;"> - 0x00 (0) as message ID of received message</span></p>
<p>&nbsp;</p>
<p><strong>Disconnect packet (client -&gt; server)</strong></p>
<p><span style="font-weight: 400;">0xC0</span></p>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Header - 0xC0 as it is fixed message for disconnect</span></p>
