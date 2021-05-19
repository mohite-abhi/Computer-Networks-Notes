# 3. Data link Layer

**Introduction**
- Responsibility
	- takes data from network layer, provides to physical layer, which then sends it
	- within a network(lan), only data link layer is enough for communication
	- does hop to hop delivery or node to node delivery
	- does flow control (ARQ- automatic repeat request)
		- simplest (just keep sending)
		- stop n wait
		- go back n
		- selective repeat
	- does error control
		- crc
		- checksum
	- access control (to stop collision)
		- csma/cd
		- aloha
		- token ring/bus
	- uses physical address/ mac address/ nic card
	- creating frames out of packets (adds head & tails)






**Frames**
- ![e0aca973d1e8ed0f72b7906f53d389f7.png](../_resources/22c85bbcdf1e4b59bca6d39e8e988943.png)
- ![1dec6f214041a264151c0d7e6e9ee403.png](../_resources/8e3c834f66e24c568ccb3e0d686ab265.png)




**Flow Control**
- Stop and Wait
	- ![e2d9db8d4dc6c6fcbb7660d13fd2207f.png](../_resources/fd88c12dec9e4b1ab71aa55b67decc01.png)
	- keeps copy of sent packet
	- sends one frame and waits for acknowledgement, so takes lot of time
		- hence low efficiency
	- if no ack, after wait, sends again from memory
	- sender window size = 1
	- reciever window size = 1
	- efficiency = 1 / (1 + 2x)  = Tt / (Tt + 2 * Tp)
		- x = Tp/Tt
			- Tt = transmission delay = data / datarate
			- Tp = propogation delay = dis / time
	- retransmission (on error) = 1
	- Avai. Seq. No =  sender window size + reciever window size
	- examples
		- ![9522728baf9dd79b5c005e03d08b93d8.png](../_resources/e171c844dfe545b4b1f15f302ed5ae3c.png)



- Go back N
	- it is sliding window protocol
	- we send multiple frames 
		- and then wait for next ack
	- sender window size = 2<sup>k</sup> - 1
		- k = no. of bits to represent window 
		- window slides one or more slots, after valid ack recieved
	- reciever window size = 1
		- packets recieved in order
	- efficiency = (2<sup>k</sup> - 1) * (1 / (1 + 2 * x))
		- x = Tp/Tt
	- commulative acknoledgement
		- if 1,2,3 frames send so ack sends 4
	- retransmission = 2<sup>k</sup> - 1
		- lets say if first frame lost/corrupt
	- examples
		- ![b13d0184ebea0cee7ed91b69faa68253.png](../_resources/30012eaa41c94531b7d172fa5fddbc8b.png)
		- ![85afefad08df67aea188e24047e14e72.png](../_resources/0553a1eafad54d029dcd2cb84d29dea4.png)
		- ![05b6ebfaff7fb9e2fa9a9cad21a61fa6.png](../_resources/8e5702fbe6b54e7780d7b49348fd68b5.png)
		- Station A needs to send a message consisting of 9 packets to station B using a sliding window (window size 3) and go back n error control strategy. All packets are ready and immediately available for transmission. If every 5th packet that A transmits gets lost (but no ACKs from B ever get lost), then what is the number of packets that A will transmit for sending the message to B?
			- solution : 
				```
				pac1 -> 		//{1}
				<- ack2 			//window slides one slot
				pac2 -> 		//{2}
				<- ack3  		//window slides one slot
				pac3 -> 		//{3}
				<- ack4 			//window slides one slot
				pac4 -> 		//{4}
				<- ack5 			//window slides one slot
				
				pac5 -> X		//{5}
				pac6 ->			//{6} discarded
				pac7 ->			//{7} discarded
				
				pac5 ->			//{8} resent
				<- ack6 			//window slides one slot
				pac6 ->			//{9} resent
				<- ack7 			//window slides one slot
				
				pac7 -> X		//{10} resent
				pac8 ->			//{11} discarded
				pac9 ->			//{12} discarded
				
				pac7 ->			//{13} resent
				<- ack8 			//window slides one slot, creating 1 empty slot
				pac8 ->			//{14} resent5
				<- ack9 			//window slides one slot, creating 2 empty slots
				
				pac9 -> X		//{15} resent
				//wait
				//wait
				pac9 ->			//{16} resent
				<- ack10			//window slides to become empty
				
				
				//total frames sent = 16
				
				```
				
		- A 20 Kbps satellite link has a propagation delay of 400 ms. The transmitter employs the “go back n ARQ” scheme with n set to 10. Assuming that each frame is 100 bytes long, what is the maximum data rate possible?
			- solution : 
				```
				Tp = 400ms
				Tt = 100 * 8 / 20 * 1000 = 40 ms
				x = Tp/Tt = 10
				efficiency = N * (1/(1+2*x)) = 0.0476
				data rate = 20 kbps * efficiency = 952 bps 
				```

- Select Repeat
	- multiple frames like go back n
	- sender window = 2<sup>k - 1</sup>
		- k = no. of bits used to represent
	- reciever window size = 2<sup>k - 1</sup> 
		- k = no. of bits used to represent
	- accepts out of order packet
	- efficiency = (2<sup>k - 1</sup>) * (1 / (1 + 2 * x)) 
		- x = PT/TT
	- retransmission = 1
	- uses commulative, independent and negative ack (for error) 
	- searching and sorting is used, so a bit complex
	- example
		- ![78eaa3ab0ad30ed6767cad4121efba5b.png](../_resources/63c43f0549ad406a9f313e241afd98d6.png)
			- thats why window must be 2<sup>m - 1</sup>
		- ![44623166b376ee6fd7f3941412aaa4e1.png](../_resources/d2d2d48a26af4ba8ab60302eea019753.png)




- HDLC
	- what
		- high-level data link control
		- bit oriented protocol (views framess as sequence of bits)
		- over point to point and multipoint
			- which uses above arq techniques
	- frame
		- ![4cddecd065b23c2774c980916559204a.png](../_resources/d83d911435f049d598ad46819074f5b2.png)
			- i frame for information carrying (0)
			- s frame is supervisory frame, for flow and error control (10)
			- u is unnumbered frame, for miscellenius activities (11)
		- ![e8a7878e7cb3b1e5c636b3e7e265a3de.png](../_resources/6f967e89fa6840febcc7af4fcd8fba2b.png)
		- ![70f6b74ca0324d67a76eb84be7ddd62f.png](../_resources/a95949c5852a43e59877f1833435545c.png)
		- ![3f370a6fa4a9dc6f4678334d91662065.png](../_resources/c5518e10d250432a9d4b7cf95766db59.png)
		- ![e67aef5a761291eaed16dfe89404b903.png](../_resources/98388e1f73e745e7b2177fb7ddbcfa1a.png)
		- ![d19b54162c94fd3cdc4103447baf1810.png](../_resources/689c2fd701344dc8a8abfe2edb4c2fcd.png)


- Point to point protocol
	- what
		- most common for point to ponint
	- frame
		- ![aa94c65c80bd8cfd7a9b50b7f42f4d37.png](../_resources/7aa1b86090d643e9856fec062f8aea1a.png)
		- PPP is a byte-oriented protocol using byte stuffing with the escape byte 01111101.
		- ![6b52fa481563c6757d214405d52b4936.png](../_resources/e8a960e196ed4793a1f36b922ddf64c5.png)
		- ![93b9b8086b7aca0d84bea7d9e219883b.png](../_resources/453661bd38044f6db818a4a7f5a03dd1.png)
		- ![2da41188b9b71664dcbddfb001c0ad70.png](../_resources/5402c348913d4dd7ac47f9da46cc92ff.png)
		- ![294601aecd445e965b58cef7215367c5.png](../_resources/1130d00eab974647b3018fb73bf2ac4b.png)
		- ![99c5ab9f2d2b718dc4eaaff8527c5077.png](../_resources/782d3045b69d4e95ad796642973a076a.png)
		- ![513166e12d4f4c30054fb687eeb79a08.png](../_resources/253c5dea8b2c4f2685352d469caeab3a.png)
		- ![7f9a600243248c0ec246cac97edade04.png](../_resources/2d554dbfbd514cc1bb2b6b32a4732414.png)
		- ![c0a982b2ab4e5ec83e2b9b8ead9c41f7.png](../_resources/d1fd5873f6aa4f0eb75381267b65906f.png)
		- ![735bd94af3aed7504fb1f1b531e7c6b7.png](../_resources/ce33f0b783434b3daf48977fe51f150b.png)
		- ![2dcce63702092c59aa6848b81371f638.png](../_resources/147ac08bda1b4b2d95564a7b7a1d3f56.png)
		- ![22192c8050a555c151a6f22b5a6ea061.png](../_resources/719f38e282774782b3876474dc012982.png)
		- ![f554d50fcaf1d7218bc979c3251248b9.png](../_resources/025c661d3472412b891603bb7d60a98c.png)
		- ![feb0d7c51f57168265ceca0fb54a0c39.png](../_resources/02163588477044f3abb79f3cb90a7d79.png)




**Error Detection and Correction**
- Introduction
	- types of error
		- single bit error
		- burst error
	- detection methods
		- simple parity (even, odd)
		- 2d parity check
		- checksum
		- crc (cyclic redundancy check)
	- correction method
		- hamming code


- Hamming distance
	- the no. of 1 in xor of two binaries
	- for s bit error detection
		- min. hamming dis = s + 1
	- for t bit error correction
		- min. hamming dis = 2t + 1



- simple parity
	- least expensive
	- redundant bit = 1
	- even parity
		- keep no. of 1 even
		- if there is bit error
		- it turns out odd
		- so we detect there is error
		- tho, if (even) bits change, can't detect
		- min. hamming dis, d = 2	
			- so, we can detect upto d - 1 no. of error bits
	- similarly odd parity, for odd no. of 1's



- CRC (cyclic redundancy check)
	- can detect
		- burst error of length <= to polynomial degree
	- we have a polynomial divisor say x<sup>4</sup> + x<sup>3</sup> + 0 x<sup>2</sup>+ 0 x + 1
		- we convert it to binary as : 11001
	- now, to send messege of len = m
		- we append 0s equal to max power(4 here) at end of message
		- then we xor divide this with our divisor binary, 
		- we append last 4 bits of remainder to message and send (redundant, r = 4)
	- to recieve and detect error
		- we xor divide the message, with our divisor binary and if we get 0 as remainder no error, else single, odd or burst(len=4) error
	- efficiency = m/(m+r)

	- example
		- code was : 1010101010
		- divisor binary is
			- x<sup>4</sup> + x<sup>3</sup> + 1
			- or 11001
		- ![6016ec41ab4af0d7b1688967fe4deb83.png](../_resources/75ae43a8050a4309ad7cf96e81da16ab.png)
		- code becomes 1010101010 0010


	- ![db8dcc3bb78a83d4e7d10b77bb04a2a2.png](../_resources/b6cd002a5fec493bb3d734c7a132ed8f.png)
	- ![d95f91dd64216dddbd1700d1892787be.png](../_resources/6f58e3eb5554459a9160e6c0555f1991.png)

- Hamming Code
	- if m is len of message
	- m + r = 2<sup>hamDis</sup> - 1
		- if hamDis = 3
			- we can detect 2 bit, correct 1 bit
	- we put parity at 2<sup>n</sup> for n : 0..log2m
	- lets say, m = 4
	- so our code d0 d1 d2 d3 becomes, p0 p1 d0 p2 d1 d2 d3   ....(1)
	- calculating parity bit 
		- p0 = d0 ^ d1 ^ d3 (at 1st pos, so pick 1 leave 1 ..from eq.(1))
		- p1 = d0 ^ d2 ^ d3 (at 2nd pos, so pick 2, 3 leave 4, 5..)
		- p2 = d1 ^ d2 ^ d3 (at 4th pos, so pick 4, 5, 6, 7 leave 8, 9, 10, 11 ..)
	- correction
		- e0 = p0 ^ d0 ^ d1 ^ d3
		- e1 = p1 ^ d0 ^ d2 ^ d3
		- e2 = p2 ^ d1 ^ d2 ^ d3
		- now, there is error at e2e1e0 th bit, e.g. 
			- 001 means error at 1st bit
			- 000 means no error
	- now, our code can also be d0 d1 d2 d3 p0 p1 p2, as now we know what the parity is.. so we can put it anywhere, but during error correction, we got to put it back to eq. (1)






- checksum
	- steps
		- the messege is divided in m bit units
		- now one more m bit unit is addes at end of message, called checksum
		- now message is sent
		- now when message recieved
		- the reciever sums all the m bit units
		- if sum is 0, then it is okay, else message is discarded

	- more specific steps
		- we add all the m-bit numbers using 1's complement arithmatic
		- meaning, if sum > m-bit, we add overflow bit to the sum
		- now we add the complement of above number at end of message
		- now when reciever adds all numbers, and complements, he should get 0
		- ![78e4d76b113b5590c15c382929de3b60.png](../_resources/12e29729f6b74fedbcc74b1604a9c61e.png)
- in internet we use 16 bit checksum






- data link layer
	- local link control (error and flow control)
	- medium access control (mac)
	



**Multiple / Medium Access control (MAC)**
- if multiple devices in a bus, to control collision
	- random access protocol (device take no permission, any can send data, however much)
		- aloha (pure / slotted)
		- CSMA
		- CSMA/CA, CSMA/CD
	- control access (first decide who is going to send)
		- reservation
		- polling
		- token passing
	- channelization protocols (all send without collision)
		- FDMA
		- TDMA
		- CDMA






- pure aloha
	- ![4069117fa9fa5fdeea62561409c3d5f1.png](../_resources/b82bba0f75184333a8fe2c913aacf699.png)
	- we will transmit data as soon as we need
	- collision can occur
	- ack is send once data recieved, 
		- if data collide sender waits, no ack so retransmission
			- retransmission takes place after Tp * random(0..2<sup>KthRetry</sup>)
	- used in lan based networks
		- so no propogation time(almost 0), only transmission time, fixed for all
	- vulnerable time
		- is 2 * Transmission Time
			- during A is transmitting, if B starts transmitting then A & B collide
			- if C is transmitting, A starts then they collide
			- so no device should have been transmitting for 1 Tt before
			- and no should transmit for 1 Tt after
	- efficiency/throughput
		- G * e <sup>-2G</sup>
			- G = no. of packets trying to transmit at one Tt slot
			- is max when G = 1/2, efficiency is 0.184

			- G = no. of stations * no. of frames per station per second * transmission time of a frame
	- ![377dea1c220d3be6a08ca32938eb8d2f.png](../_resources/b9263c80c39e461b9457ff7f4c11cf55.png)
	- example
		- ![5dde2d17b942f827f80979a0262cc822.png](../_resources/e6ccb8a1a9c34c3ba9d2f69114ad0a61.png)







- slotted aloha
	- we divide timeline into slots = Tt
		- now any device can start transmission, at beginning of a slot
		- here collision take place if multiple device transmit at begin of a slot
	- vulnarable time = Tt
	- efficiency / throughput
		- G * e<sup>-G</sup>
			- max at G = 1
			- is 0.367
	- example
		- ![65766e104d002d23ce65bd2157df1755.png](../_resources/6403858fcf45438e97ff836495210d96.png)







- Carrier Sense Multiple Access (CSMA)
	- we sense the channel through tap, if any transmission is there
	- types
		- 1 - prsistant
			- device keep sensing the tap, if no transmission, and want to send, then send
			- but if multple devices sense free, they all start transmission, then collide
			- ethernet uses this
		- 0- prsistant
			- if want to send, senses tap, if busy, waits random time
			- collision is less, as diff. device have diff. waiting
		- p - prsistant
			- continuously checks, but if free, there is p probability of device will transmit, or wait
			- wifi uses this
	- vulnerable time = propogation time








- CSMA / CD (collision detection in case of wired)
	- can't send ack, as collision will further increase
	- detection method
		- packet will travel Tp time and just before reaching destination, it collides
		- so now the collision signals goes all the way back in Tp seconds 
		- now as the energy in the line is more than sent energy, so collision is detected
	- Tt > 2Tp
		- it takes upto 2 Tp to detect collision, so upto that point we need to be still sending to detect incoming signal
	- throughput	
		- 1 / (1 + 6.44x)
			- x = Tp / Tt
	- ![af75ecc74921b3fbd8aa592d8db943b9.png](../_resources/43e809157ed544199bfd24fabcf5590c.png)
	- example
		- ![67576e510489becaf61b137fd75302ae.png](../_resources/155760d046414d7db19d8b2dd208f2dd.png)
		- ![37c45445cbeee4d4ce37171069fee835.png](../_resources/104eeca7d58a42b1883710c04ad18d7c.png)






- CSMA / CA
	- collision avaoidance
	- used in wlan(wifi)
	- can't detect double energy as energy is lost in air, so can't detect with CSMA / CD
	- ![bd276e1ed4e0f670f57054988b059824.png](../_resources/97eafcfba23c41458304c53bc6b76db0.png)










- control access (station consults one another to decide which station should send)
	- reservation
	- polling
	- token passing



- Reservation
	- there are two time slots
		- reservation slots
			- divided into n mini slots
				- n is no. of stations
			- i<sup>th</sup> station can send a bit at i<sup>th</sup> slot to reserve.
		- variable data transfer slot
			- after reservation, every station knows that who has made reservation
			- so now in order of reservation, data is sent in data transfer slots
			- as everyone knows when who will send, there is no collision at all
	- ![1a78d22622953ea5482ba3f6195c406f.png](../_resources/9070090b7ed04df4bef0035d19c6d08b.png)




- polling
	- a controller system grants access to devices, one by one
	- poll signal
		- controller sends poll to diff dev.
		- if device want to send, it sends & recieves ack
		- if dont want to send, device returns poll reject(NAK)
	- sel signal
		- if controller want to send data, sends sel
		- if recieve ack send data, recieve ack

	- ![dcc4aac02ef6dd103a07a6ede578a573.png](../_resources/0a880b7542cb46449d44f8357aa49f94.png)


- token - passing
	- whoever has token can send
	- token is circulated within devices
	- token management ensures non dissappearance of token, and time limit of token






- channelization protocols
	- FDMA (frequency division multiple access)
		- available bandwidth of common channel divided into bands, seperated by guard bands
	- TDMA (time ..)
		- one bandwidth is time shared
	- CDMA	(code ..)
		
- CDMA	
	- one channel carries all transmission simultaneously
	- working
		- each station is assigned a code, sequence no. called chips
			- each sequence no. has N elements, N = no. of stations
			- if we dot product diff. sequences and add, we get 0
		- if station 
			- wants to send 0, it is encoded as -1
			- wants to send 1, it is encoded as +1
			- doesn't want to send, encoded as 0
		- now, we multiply each encoded message with their sequence, and add all and send
			- ![4a346405883fa880b48f8d79cb4097c1.png](../_resources/31625a8dc0d14b7b827d6faf6be62977.png)
		- now, on recieving, to get data of ith station we dot product with its sequence, and add all sequence bits and divide by N.
			- ![c2d557e14323a80eb16aa05d3ecb4793.png](../_resources/5326ed13b7a84f80914c9157888b2866.png)


	- generating sequence code
		- we use walsh table for this
		- ![1a574e9c5ba9aa0d0059e1895fee1cdb.png](../_resources/06a1b7770f264ab98f1405b4198d421a.png)










**Ethernet**
- IEEE Standerd
	- project 802 was started to enable intercommunication among equipment from variety of manufacturers
		- a way of specifying functions of physical layer and data link layer of major LAN protocols
- what
	- ethernet is a data link layer protocol
		- based on LAN
		- uses csma/cd
		- uses bus topology  
		- bitrate 1 mbps to 400 gbps
- ethernet evolution
	- standard (10 mbps)
	- fast (100 mbps)
	- gigabit (1 gbps)
	- ten-gigabit (10 gbps)
- ethernet frame format
	- 802.3 mac frame
		- ![f95d1ffd9099a90e10ced15d5cd3979d.png](../_resources/21e420ec94864926824a1b3d556df5ae.png)

		- preemble & sfd (7B + 1B)
			- added in physical layer
			- preembel
				- 56 bit 1010101....01 (for sync & alert)
			- sdf
				- 10101011 (synchronization & alert)
		- DA (6B)
			- mac address of next hop, found by ip
			- example 
				- 06:01:02:01:2C:4B
		- SA (6B)
			- mac address of this hop
		- length (of frame) (2B)
			- can be 0 to 2<sup>16</sup>-1 
		- data
			- atleast 46 byte, so that csma/cd can detect collision
				- min. size of frame needs to be (46 + 18) 64 B
				- 18 B is rest of the data
			- max can be 1500 B 
				- max size of frame (1500 + 18) 1518 B
		- CRC (4B)
			- cyclic redundancy check


- unicast and multicast address
	- defined by least significant bit of first byte 
	- if 0
		- unicast
	- else if 1
		- multicast
	- else if all bits 1
		- broadcast

	- example
		- ![bd7f4c53dcd417977024174abbac26c5.png](../_resources/2eb07c0b0770431ab8fa85ad3d16fae3.png)
		- a frame is there and its source address has first byte as FF, what is wrong with it?
			- a source address can never be multicast, because it is always unicast (originates from a single source)









**WiFi (Wireless Fidelity)**
- Introduciton
	- IEEE 802.11
	- like ethernet for small area
	- challenge:mediate access
	- supports
		- power management
		- sequrity mechanism
	- device connect with wifi access point, which is connected to powerOverEthernet, which is connected to router thru wire
	- uses 5 GHz radio band, has 23 overlapping channels
		- can use 2.4 GHz which has three non overlapping channel
	- uses CSMA/CA
	- modes of wifi
		- infrastructure mode
			- devices connect to access point (centralised) to connect to another device, infrasture
		- ad hoc & wifi direct
			- no infrastructure

	- wifi protocols
		- ![2f6db19b972a14b76326158d2139a494.png](../_resources/d99a29533fcc41b2980ba5bffd76019f.png)



- Distribution system
	- 802.11 suitable for ad-hoc configuration, that may or may not be able to connect to all other nodes
		- ![4943cdfca334e6bacb672fb5ae59f178.png](../_resources/c0343a0597204034bc94830bc3e27369.png)
	- scanning
		- nodes (device) select access point with this
		- a mobile node sends a probe frame
		- all APs within reach reply with probe response frame
		- node selects one of access point and sends that AP an association request frame
		- the AP replies with association response frame
	- active scanning
		- mobile nodes continuous sending probe frames, to find what all APs are there, and connect to better one
	- passive scanning
		- APs send Beacon frame periodically to tell its capability

- 802.11 wifi frame format
	- ![66d2e83d110b27b3aeee27a68b6a69f4.png](../_resources/6db51eac19064fb29d70a11201adbfe9.png)
	- ![ba64736e2ae1c4c6fde2fdb7a852fe96.png](../_resources/5e045778722b4da9bc35d3ab6b14d627.png)
		- ![cac34c0b8ae5cfd1bb1e511766c1df5c.png](../_resources/9a3d83ccd63642e5bc40a29d3b0ddfcd.png)
		- ![88af8433a4cf69ead4f7cd08b7ecd771.png](../_resources/9a904562e4bb4cae8c3a2bc645a934fd.png)
		- ![f226123058a90413570932fe0c29667f.png](../_resources/4b837ed642314fec905c6e8d4fea229e.png)
		- ![68732b64d7017b5bde267cea5bec6e76.png](../_resources/3cf8925f99024e42b28fa9a1a38354b4.png)
		- ![4e5342f699389f9c2e2c6f00f61b2b9c.png](../_resources/565e30b1acd345bbba41841907ae4f67.png)
		- ![4da3e27f3cb8b749a10567b2783fad41.png](../_resources/a8689d3379ee48738744c03c2a86cfd7.png)
		- ![3283699144823368a03a2ae5c4d54f80.png](../_resources/98dbfe58f01f4ef69430b3f1abc9c7a3.png)
		- ![97a1dc98fa12f58d28bae18b96b685fb.png](../_resources/a91ed241ecf840fba8506f2811f52608.png)
		- ![832b5ccb2d6e0138b0a1ec1964b79255.png](../_resources/030730d3931f49f3895d49778c38cf30.png)
		- ![8b3a467949a652e49e57ce7668e5a6d3.png](../_resources/920242065c0b4d45bf3f3936fb35627d.png)
		- ![7a937e92496bf4868a6d6a781751623c.png](../_resources/e9a5b28ada4542b2b51bf21bbefd1617.png)
	- ![07b789cdd44c4e425fca1d60ecd6d90a.png](../_resources/3f1c59a4665b4e039e70f9b3f8e6f7a3.png)
	- ![7c0a144ea39838be0f86825a57c328b3.png](../_resources/c2db748f205b4334993458e4f996ea89.png)
	- ![c4a6b0b1807aa15a7259990438fa58ae.png](../_resources/624ffe7b23c7460ca26426944c5144ea.png)
	- ![f03a4777b98d83a884499cad74ab821c.png](../_resources/98d5f0a3bfb142d3837ea700bdc24bb9.png)
- hidden terminal problem
	- when A & B share AP1, B & C share AP2, now they both(A & C) send frame to B not seeing each other, and collision occurs
	- solution 
		- use cts and rts (mac algo)
			- sender sensd rts (request to send) & len of data, it goes to all nodes, to hold medium
			- reciever replies with cts(clear to send)
			- if any node sees cts, won't transmit
			- if node sees rts only, will know it wont effect so can transmit




**Bluetooth**

- IEEE 802.15.1
	- ![4ca53dcf83be4540a78f5673921add0a.png](../_resources/ffc6110e85b1452f85b054393f73721f.png)
	- ![62c2db1f8ecf01a41480b50ce1ee9f39.png](../_resources/9df9271ec0f3408991d3ca65cdc4fa92.png)
	- ![6988d79e79bf683daefd8311040561cf.png](../_resources/1ddd4d1d5dd14f8e88c8c1e30a3f016a.png)
	- ![b45100309c36bc7c8da17d31d8e17ff5.png](../_resources/ea66bdd6585947ae8f226a877d35cd99.png)
	- ![e21df0225b252a37fcb4411f7e7c472f.png](../_resources/bf7b54fe650041b189c50704adcf8e9a.png)




**Virtual LAN (VLAN)**
- 
- ![47e113e7e48e99532e2486dc579f2208.png](../_resources/7782d594cc9e4f1aaa2b8bf4513ec863.png)
- ![05b8d3d05a71771d7554128b0d1bf6bc.png](../_resources/cf8c1a02454e42c0aef158f1c6243eda.png)
- ![624c39e8f10186d6bf8f94313aed6ea7.png](../_resources/5dfaedcaea4b47da8d2853cf1e531d89.png)
- ![b40354b2fef4adf5b0149fd2eaf09119.png](../_resources/2c8fa292b7ca4de4b6b5762e061e0a27.png)
- ![e419cf5e7aa888ffc509b8af156d4b86.png](../_resources/8d4358ad397b4a3783bf5bb1275013ea.png)




































