5. Transport Layer

**Introduction**
- takes data from application layer and passes to network layer
- Responsibility
	- end to end delivery (port to port delivery)
		- there are multiple applications running, which have port, so Tl, helps in communication bw them
		- uses
			- TCP
				- ip has unreliable, so we use this
				- ensures right number and order of packets
				- ensures no loss of data
				- is connection oriented
	- does error control with checksum
	- does conjesion control and flow control
	- segmentation, segments continuous data from above
	- multiplexing, demultiplexing (merges data by multiple applications)




**TCP (Transmission Control Protocol)**
- what
	- byte streaming protocol, continuous data from application layer, tcp makes segment that have bytes, from the continuous bits
	- for making ip connection oriented, we use tcp (reliability, 3-way-handshaking)
	- full duplex
	- piggybacking (sending data along with ack) (uses go back N, and selective repeat)
	- error control, flow control and congestion control
- segment header
	- size (20-60B)
	- fields
		- source port (16 bit)
		- destination port (16 bit)
			- 2<sup>16</sup> port numbers
			- 0-1023, well known ports
			- port no. given by device to application
			- the combination of ip address and port address is called socket address
		- sequence number (32 bit) (the no. assigned to first byte, if first segment, give a random)
		- acknoledgement number (reciever sends next expected byte no.)
		- HLEN (4 bit) (similar to ip HLEN)
		- 6, 6 bit reserved for future
		- control (6 bit)
			- URG (urgent) (if URG==1 in first segment, then following segments are urgent upto to urgent pointer is reached, also given by first segment)
			- ACK (1 for telling header is acknoledgement)
			- PSH (we don't want the data to stay in header)
			- RST (reset, to reset connection)
			- SYN (for synchronization, for connection making)
			- FIN (for ending connection)
			- window size (16 bit) (sender/reciever tells window size)
		- checksum (16 bit)
		- urgent pointer (urgent bytes no.)
		- option and padding (40 Bytes)
			- max. segment size (MSS)


- TCP Connection
	- Connection establishment (3-way handshaking)
		- ![07a8b7eef5fa207d23e351c62832e4cf.png](../_resources/9ff2c86f02c14090b96ba456151df356.png)
		- connection needs to be established b/w client and server
		- first way handshake 
			- client is in active open state
			- client sends syn segment wit SYN=1, rand sequence no(ISN - init. seq. no, but 0 data bytes)
		- second way handshake
			- server is in passive open state
			- server sends syn+ack segment with SYN=1, ACK=1, ack. no. = (client seq.no + 1), seq. no = ISN(no data)
			- sends its reciever window size, option(MSS - max segment size)
		- third way handshake
			- now, client sends ack segment with ACK=1, ack no.= (server seq no. + 1)
			- sends seq. no = (server ack. no +1 ) only if client want to start sending data, elso no seq.no is consumed
			- sends window size, option(MSS - max. segment size)
			- now, connection has been established, and data can be tansfered




	- Data transfer
		- ![a9d13cfdfc5cd81441ad698718f8284e.png](../_resources/27febc1f2e11414ebcdbd04a5f3c38fd.png)
		- first, connection is established by 3-way handshaking, buffer is established(resources are reserved)
		- now data can be sent both way simultaneously using piggybacking
		- piggybacking : sending data and ack together
		- next packet from client
			- now data is sent with ACK=1, ack no.=(serv. seq. no + bytes recieved + 1), seq no = (serv.ack.no. + 1)
		-  next packet from server
			-  data is sent with ACK=1, ack.no=(cli.seq.no + bytes recieved + 1), seq.no=(cli.ack.no + 1)
		-  pure acknoledgement packet (no data/piggybacking)
			-  header is sent, with unused seq.no= (rec.ack.no + 1), ack.no = (rec.seq.no + bytes recieved + 1)



	- Connection Termination
		- Three way
			- ![1e2d2dc848ea5a8d056cfcb962e23f77.png](../_resources/ba10b4cf1c834b8fba84fe23e2628eb7.png)
			- client sends fin segment, F=1(finish), A=1, with data or no data, seq.no=(rec.ack.no.+1), ack=(rec.seq.no+(bytes.rec)+1)
			- server sends fin+ack segment, F=1, A=1, seq.no=(rec.ack.no+1) with data or not, ack.no=(rec.seq.no.+(if data recieved)+1)
			- client finally sends ack segment, A=1, unused seq.no=(rec.ack.no+1), ack.no=(rec.seq.no+(if data recieved)+1)
			- now connection is fully terminated
		- Four way (half close)
			- ![6d9780cf1e1d39f01938bc1d026bcc87.png](../_resources/99f9480daba4470c972f423477e555d4.png)
			- client sends fin like three way
			- but server will need to send some data later, so it sends only ack segment A=1, seq.no=(rec.ack.no+1) with data or not, ack.no=(rec.seq.no.+(if data recieved)+1)
			- now there is a half close state where no data is exchanged
			- then when server wants to send final data, it sends fin segment, F=1, A=1, seq.no=(rec.ack.no+1) with data, ack.no=same as last.ack.no

























