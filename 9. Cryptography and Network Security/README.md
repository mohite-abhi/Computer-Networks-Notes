9. Cryptography and Network Security

**Security Goals (CIA)**
- confidentiality
	- we need to keep our confidential data safe from anyone accessing them
	- we need to keep our store data safe as well as the confidential data we are transmitting
- integrity
	- our data needs to be changed, updated etc.
	- we have to make sure that the data is changed only through authorized mechanisms and by authorized entities
- availability
	- an authorized entity should be able to access the information, else what use is it




**Attacks**
- security attacks
	- Confidentiality threats
		- snooping 
			- unautharized access/interception of data
			- can be prevented by encryption
		- traffic analysis
			- knowing data that can not be encrypted, like addresses(eg. email address)
	- Integrity threat
		- modification
			- modifying the intercepted data for benifit or just for causing harm
		- masquerading(spoofing)
			- impersonating as someoen else (client or server) for data theft/other benifit
		- replaying
			- obtaining a copy of message and resending it again
			- e.g. replaying a transaction
		- repudiation
			- when someone denies sending message
			- suppose, an e payment site tells to pay, but after payment, it denies payment and asks to pay again
	- Availability threat
		- denial of service
			- sending large no. of bogus requests to server that server crashes or slows down due to load
			- or attacker might intersept server's response to client and delete, or delete request from client






**Services and Techniques**
- cryptography
	- making the data secure and immune to attack using one of following mechanisms
		- symmetric key encipherment
		- asymmetric key encipherment
		- hashing
- steganography
	- concealing message itself by covering it with something else











