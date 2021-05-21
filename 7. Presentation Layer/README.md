# 7. Presentation Layer

**Introduction**
- responsibilities
	- translation
		- if two machines use different codes. 
		- example
			- A uses ASCII 
			- B uses EBCDIC
		- so it is responsibility to translate one format to another
	- encryption/decryption
		- as security is important, and packets can be stolen by someone
		- before sending, presentation layer can encrypt our data
			- i.e. convert it into an incomprehensible format while retaining the knowledge in it
			- it can be decrypted with the help of a key that is shared by only the end machines
		- and decrypt it at the other end	
	- compression
		-  presentation layer can use compression algorithms to reduce size of the data
			- the compression can be lossless or lossy(with loss) depending on the algorithm used























