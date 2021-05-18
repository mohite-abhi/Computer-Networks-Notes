# 1. Introduction

**Data Communication**
- why
	- we need to share data, so we communicate
- how
	- sender sends data, reciever recieves (both are at diff. machines) through channel
	- but, reciever need to understand data, for which we need protocols

-  functionality / protocols of two way communication (more than 70)
	-  mandatory functionalities
		-  error control
		-  flow control
		-  multiplexing/demultiplexing
	-  optional
		-  encryption/decription
		-  checkpoint (resumable download)
	-  Organisation of these functionalities is done with models like OSI/TCP


**Network Models**
- TCP/IP by ARPANet(OSI Layer)
	- Application Layer (specific address)
		- Applicatin Layer 
			- process to process delivery of data
		- Presentation Layer 
			- encoding, encryption
		- Session Layer
			- session maintainance
	- Transport Layer  (TCP,UDP,HTTP) (port address)
		- host to host delivery delivery of segments
	- Internet Layer (logical address)
		- Network Layer
			- source to destination delivery of packets
			- ipv4, ipv6, routing
	- Network Access Layer (physical address)
		- Datalink Layer (frames) 
			- hop to hop delivery 
		- Physical Layer (bits)
			- node to node delivery of bits 



