https://tryhackme.com/room/xxe

*This room aims at providing the basic introduction to XML External entity(XXE vulnerability.*

##############################
	Cosmin Tibuleac |  25.11.2021
##############################

export IP=10.10.59.61
# Task 1: Deploy the VM

*An XML External Entity (XXE) attack is a vulnerability that abuses features of XML parsers/data. It often allows an attacker to interact with any backend or external systems that the application itself can access and can allow the attacker to read the file on that system. They can also cause Denial of Service (DoS) attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web application to make requests to other applications. XXE may even enable port scanning and lead to remote code execution.  
  
There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).  
1) An in-band XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.

2) out-of-band XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.*



# Task 2: eXtensible Markup Language


**What is XML?**  
  
XML (eXtensible Markup Language) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is a markup language used for storing and transporting data.  
  
**Why we use XML?**  
  
1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.  
  
2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.  
  
3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.  
  
4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.

**Syntax**  
  
Every XML document mostly starts with what is known as XML Prolog.  
  
`<?xml version="1.0" encoding="UTF-8"?>`

  
Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents.  
  
  
Every XML document must contain a `ROOT` element.