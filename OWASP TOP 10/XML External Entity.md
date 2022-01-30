[[OWASP]]
An XML External Entity (XXE) attack is a vulnerability that abuses features of XML parsers/data. It often allows an attacker to interact with any backend or external systems that the application itself can access and can allow the attacker to read the file on that system. They can also cause Denial of Service (DoS) attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web application to make requests to other applications. XXE may even enable port scanning and lead to remote code execution.  
  
There are two types of XXE attacks: in-band and out-of-band (OOB-XXE):

1) **An in-band XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.
**
2) **out-of-band XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.**

Before we move on to learn about XXE exploitation we'll have to understand XML properly.  
  
# What is XML?  
  
XML (eXtensible Markup Language) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is a markup language used for storing and transporting data.   
  
Why we use XML?  
  
1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.  
  
2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.  
  
3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.  
  
4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.  
  
Syntax  
  
Every XML document mostly starts with what is known as XML Prolog.  
  
`<?xml version="1.0" encoding="UTF-8"?>`

  
Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents.  
  
Every XML document must contain a `ROOT` element. For example:  

`<?xml version="1.0" encoding="UTF-8"?>  
<mail>  
   <to>falcon</to>  
   <from>feast</from>  
   <subject>About XXE</subject>  
   <text>Teach about XXE</text>  
</mail>`  

  
In the above example the `<mail>` is the ROOT element of that document and `<to>`, `<from>`, `<subject>`, `<text>` are the children elements. If the XML document doesn't have any root element then it would be considered`wrong` or `invalid` XML doc.  
  
Another thing to remember is that XML is a case sensitive language. If a tag starts like `<to>` then it has to end by `</to>` and not by something like `</To>`(notice the capitalization of `T`)  
  
Like HTML we can use attributes in XML too. The syntax for having attributes is also very similar to HTML. For example:  
`<text category = "message">You need to learn about XXE</text>  
`

In the above example `category` is the attribute name and `message` is the attribute value.

# DTD (Document Type Definition)
Before we move on to start learning about XXE we'll have to understand what is DTD in XML.

DTD stands for Document Type Definition. A DTD defines the structure and the legal elements and attributes of an XML document.

Let us try to understand this with the help of an example. Say we have a file named `note.dtd` with the following content:  

`<!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)> <!ELEMENT to (#PCDATA)> <!ELEMENT from (#PCDATA)> <!ELEMENT heading (#PCDATA)> <!ELEMENT body (#PCDATA)> ]>`  

Now we can use this DTD to validate the information of some XML document and make sure that the XML file conforms to the rules of that DTD.

Ex: Below is given an XML document that uses `note.dtd`

<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE note SYSTEM "note.dtd">  
<note>  
    <to>falcon</to>  
    <from>feast</from>  
    <heading>hacking</heading>  
    <body>XXE attack</body>  
</note> ```

  
  
  So now let's understand how that DTD validates the XML. Here's what all those terms used in note.dtd mean  

-   !DOCTYPE note -  Defines a root element of the document named note
-   !ELEMENT note - Defines that the note element must contain the elements: "to, from, heading, body"
-   !ELEMENT to - Defines the `to` element to be of type "#PCDATA"
-   !ELEMENT from - Defines the `from` element to be of type "#PCDATA"
-   !ELEMENT heading  - Defines the `heading` element to be of type "#PCDATA"
-   !ELEMENT body - Defines the body `element` to be of type "#PCDATA"

    NOTE: #PCDATA means parseable character data.


<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:////home/falcon/.ssh/id_rsa>]>
<root>&read;</root>


The above code would display this: 

![[Pasted image 20211013120635.png]]