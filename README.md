# NAMM B2B JSON Implementation Guide

#### Revision 2020.2 &mdash; Last edit: 27 Jul 2021

>**Note to readers: This document contains links to external sites. To avoid losing your place, you can open links in a new tab; just ctrl-click a link or right-click it and choose "Open link in new tab".**

# Executive Overview

In any economy, robust or challenging, finding efficiencies and cost savings in your business processes is imperative, and the shortest path to savings is the adoption of standard transaction formats. Whether you are a manufacturer, a distributor, or a retailer, you can streamline and automate the production of orders, product catalogs, shipping documents, and forecasts using industry standards for data transfer.

## Reasons to Adopt Industry Technology Standards

1. **Your business can benefit from increased efficiency.** Trading partners who share information through standardized data exchange enjoy reduced operational costs as a result of less human interaction in business transactions.

1. **Your business can profit from higher accuracy.** Using standardized transaction formats allows business trading partners to easily share information, thereby preventing costly mistakes.

1. **You can provide better customer service.** Access to timely information in known, predictable formats allows for faster response time and better service to your customers.

1. **You can make better decisions.** Timely, accurate, and complete information gives you the big picture you need to make informed business decisions.

1. **Your business communications can be simpler.** A standardized, agreed-upon data transfer format simplifies transactions between business partners and removes obstacles imposed by the businesses' individual systems.

Product vendors, suppliers, and purchasers use different computer hardware and software systems to process their business transactions. These systems all have their own proprietary data formats, and yet they all need to process substantially similar kinds of data. Thus, the need for a common language for sharing data among business trading partners becomes critical. More efficient and reliable data transfer means fewer errors in your business transactions, and that means a better bottom line.

## JSON

The common format that makes this possible is called *JavaScript Object Notation*, or *JSON*. JSON is an open-standard file format that uses plain text to represent data objects. It is a very common data format used for asynchronous computer–to-computer communication, including as a replacement for XML data structures, as is the case here with NAMM's new Business-to-Business (B2B) document set.

JSON is a method of precisely and unambiguously describing and storing data so that it can be shared among dissimilar systems, but interpreted and processed identically by each. JSON data conforms to formatting rules that are agreed to by all parties to a transaction, and thus can be freely exchanged among disparate hardware and software systems without fear of misinterpretation, data loss, or exposure of proprietary information.

Although JSON is derived from JavaScript's object description syntax, it is a genuinely language-independent data format and is compatible with, and widely used by, many current programming languages.

Using JSON-based data, business trading partners can exchange information consistently and precisely, improving efficiency and accuracy while reducing operational costs and enhancing all levels of customer service. JSON's standardized data format facilitates sharing information among business partners at all points in the supply chain.

For example, data that is formatted in JSON:

- can be used across all computer platforms  
- integrates with a wide range of software applications  
- is robust enough to convey complex information, yet simple enough to read  
- is in a format used by companies large and small all over the world  
- does not require specialized resources to implement

For the past several years, experts from across all channels of the music products industry have partnered together to design a tool to enhance your business practices. That tool is the NAMM B2B Standard, the most widely used standard in the industry. This document provides the information necessary for trading partners like you to implement and automate transactions using this standard. NAMM encourages you and your business partners to adopt and use these standardized data transfer formats. Visit [our B2B page](http://www.namm.org/initiatives/b2b) to learn more.

# Overview

## Abstract

This document is intended as a conceptual and practical guide to NAMM’s Business-to-Business (B2B) JSON data exchange standard, and specifically a reference to the JSON data and schema files defined by the standard.

## Audience

The target audience for this document is the technical team in a musical products merchandising company whose responsibility includes internal software management, configuration, and integration, especially as relates to interfacing with external data sources. These companies may supply musical products, consume them, or both.

## Use Case

In the course of providing musical products from one company to another, certain data must be exchanged between suppliers and buyers. While the specific software systems of each company will necessarily differ in platform, connectivity, and functionality, it is highly advantageous to both parties that the structure of the data exchanged be consistent from transaction to transaction. It is this need for structural consistency that is addressed and provided by the NAMM B2B standard.

# Implementation

## Transaction Definition

The NAMM B2B standard comprises a set of JSON files containing data in a predefined, consistent format. Each file is designed to precisely define the data required to complete one common transaction between parties, e.g., Purchase Order, Invoice, Shipment Notification, and so on. These documents are described in "JSON Files", below.

## Transaction Sequence

A typical sequence of events between a product supplier (a vendor) and product buyer (a retailer) and the associated documents, observed in chronological order, might be as follows.  

<!-- 4. The supplier sends details about the items ordered to the buyer in an **Electronic Delivery Response** document. -->
1. Both supplier and buyer complete and release mutually-available identification information in a **Party** document.  
1. The supplier sends a catalog of available products to the buyer in an **Item** document.  
1. The buyer decides to purchase items from the catalog and sends a request for them to the supplier in a **Purchase Order** document.  
1. The buyer requests the status of their order with a **PO Status Request** document.  
1. The supplier provides the status of the order in a **PO Status** document.  
1. The supplier makes one or more product shipments to the buyer and sends details for each shipment in an **Advanced Shipment Notification** document.  
1. The supplier sends a request for payment to the buyer in an **Invoice** document.  

Below is a diagram of this process, referencing the step/document numbers above.

![B2B Transaction Diagram](b2b2.png)

## Transport Options

In the B2B scenario described above, the JSON documents must be electronically transmitted between partners in order for data sharing to take place. Therefore, the transmission method used, called a *transport protocol*, must be understood and implemented by both parties to a transaction.

The set of protocols used for Internet communications is called the *Internet Protocol Suite*, commonly known as TCP/IP (Transmission Control Protocol/Internet Protocol). The two most popular transport protocols in use today are HTTP and FTP, discussed below.

### HTTP

HTTP (HyperText Transfer Protocol) can be used to upload (send) files from a local computer to a remote web server. This is the method used to download (receive) web pages from a server into a web browser for viewing, and to download auxiliary files from web pages to your local computer. In this scenario, a programmer must implement a system that runs a script or application that sender can use to upload a file. When the transfer is complete, the receiving system might then run another program to process the data (e.g., to parse it and put its data into the company's accounting system). HTTP supports immediate in-session response, enabling the receiving party to validate the transmitted transaction document (JSON file) and respond by transmitting a document of its own (such as a PO Status Acknowledgement) back to the sender. This type of transfer is typically used to send one file at a time.

### FTP

FTP (File Transfer Protocol) can also be used to upload files from a local computer to a remote server. In this scenario, the sender runs a specific kind of third-party application called an _FTP client_ on their local computer to contact the recipient's server; the FTP client then lets the user select one or more files to transfer and sends them. FTP does not support immediate response; instead, transmitted files may be detected later by other software and managed in groups (*batch processing*). This type of transfer is typically used to send several files at a time.

### HTTP/FTP Comparison

**Transfer speed:** Both protocols have features that increase and decrease transmission speed. For example, while FTP does not take the time to "chunk" files as HTTP does, it uses two connections, one for commands and another for transfers, to accomplish its task. Conversely, while HTTP automatically compresses transmitted data, it adds metadata elements to the transmission stream. Ultimately, when transferring a small number of files that are not individually large (such as those in NAMM's B2B process), the speed difference is negligible.

**Security:** Neither standard HTTP nor FTP supports secure transmission, so each protocol has a secure version &mdash; HTTPS and SFTP &mdash; that can be used to provide secure file transfer between systems. Because FTP/SFTP, once connected, gives an authorized sender relatively broad access to the recipient's file system, where HTTP/HTTPS does not, additional software safeguards or strictly limited navigation permissions may be required. NAMM members are strongly encouraged to use the secure protocols (HTTPS/SFTP) in their B2B transactions to ensure the security and privacy of their communications.

**Popularity:** FTP is a much older system than HTTP, by a decade or more. Thus, historically speaking, FTP is more popular. However, because today virtually every page &mdash; and virtually every upload and download link &mdash; on the entire web is now served via HTTP, it is accurate to say that HTTP is by far the more popular protocol in terms of raw usage. Further, while there are many excellent and free FTP clients available (FireFTP, FileZilla, CoffeeCup Free FTP, WinSCP, etc.), HTTP/HTTPS is significantly easier to control and customize programmatically via browser scripting or programming languages, making it the most popular choice for developers of modern file transfer applications.

**Conclusion:** JSON files contain only "plain text" data, and thus are compatible with any transfer protocol. Which protocol you use is ultimately up to you and your business partners. You may in fact use both protocols to share data among partners, FTP for some and HTTP for others; or you might use FTP to send files and HTTP to receive them. The choice depends on what software you and your partner companies have available or are willing to develop.

## Resource Requirements

Implementing the NAMM B2B standard is not difficult and does not require specialized resources. Most likely, your company already has the basic facilities and resources required for implementation, such as:

- a web server that supports FTP or HTTP transport services  
- an internet connection/hosting facility  
- a domain name (or IP address)  
- commercial or proprietary accounting software  
- a software and/or web developer

You can get additional help with B2B planning and implementation at the [NAMM web site](http://www.namm.org/), through independent consultants, and from certain point-of-sale (POS) solution providers. POS solutions are widely used in the music products industry.

There are also many completely free JSON tutorials, services, and utilities available online, such as the [W3Schools JSON section](https://www.w3schools.com/js/js_json_intro.asp), code editors like the excellent [Notepad++](https://notepad-plus-plus.org/), and web-based tools such as the [JSONLint validator](https://jsonlint.com/) and the [JSON Formatter and Validator](https://jsonformatter.org/).

For more information or to ask specific questions, please email us at  [standards@namm.org](mailto:standards@namm.org).

# JSON Documents and Schemas

## Content Documents
Below are descriptions of the JSON content documents currently defined by the NAMM B2B standard. Note that, as with any content-only implementation, these files merely define the structure of the data and do not require nor support any specific processing agents (i.e., application software).  
  
The responsibility for creating, validating, transmitting, receiving, and parsing the files and their content therefore rests solely with the business entities (suppliers and buyers) using them. That, of course, is where the beauty of predefined and globally accepted standards shines. 

## Schemas
To facilitate data accuracy, each JSON content (data) document has an accompanying control document, also coded in JSON. This document, called a *schema*, contains specific, clearly defined rules against which the content document's structure and data can be validated. A JSON invoice (data) document, for example, would have **invoice.json** as its schema (control) document. 

Schemas ensure that both the structure of the data and the data itself conform to the system's expectations. Using schemas, errors in either data layout or data values can be identified quickly, before they become processing problems.

The various data and schema documents in the NAMM B2B specification are described below, with links to samples and more details. 

>**The B2B "Core" documents &mdash; Purchase Order, Invoice, Advanced Shipping Notice, and Item &mdash; are listed first, followed by the "Auxiliary" documents &mdash; Purchase Order Status Request, Purchase Order Status, <!--Electronic Delivery,--> and Party, and finally by the Common Elements document.**

## Purchase Order

The Purchase Order document, **po.json**, contains a list of items the buyer has selected for purchase from the supplier.

The Purchase Order document first provides basic information about the proposed sale, such as buyer, billing, and supplier data. It then provides overall sale specifics, such as terms and shipping instructions. Finally, it provides a shipping location and a list of ordered items.

- [Sample Purchase Order document](https://github.com/namm-standards/namm-json/blob/master/examples/po_status_example.json)
- [Raw Purchase Order schema](https://github.com/namm-standards/namm-json/blob/master/schema/purchase_order.json)
- [Purchase Order schema technical reference](https://standards.namm.org/docs/purchase_order/index.html)

## Invoice

The Invoice document, **invoice.json**, is a supplier’s request for payment from the buyer. It contains information about the supplier, payee, and buyer, as well as invoice terms and detailed information about the referenced purchase, down to the individual items.

- [Sample Invoice document](https://github.com/namm-standards/namm-json/blob/master/examples/invoice_example.json)
- [Raw Invoice schema](https://github.com/namm-standards/namm-json/blob/master/schema/invoice.json)
- [Invoice schema technical reference](https://standards.namm.org/docs/invoice/index.html)

## Advanced Shipping Notice

The Advanced Shipping Notice (ASN) document, **asn.json**, contains detailed information about one or more orders in progress from supplier to buyer. It may be requested via the Purchase Order document, and provides specific partial or full order shipment details, enabling the buyer to reconcile items ordered with those actually received.

- [Sample ASN document](https://github.com/namm-standards/namm-json/blob/master/examples/asn_example.json)
- [Raw ASN schema](https://github.com/namm-standards/namm-json/blob/master/schema/asn.json)
- [ASN schema technical reference](https://standards.namm.org/docs/asn/index.html)

## Item

The Item document, **item.json**, essentially contains a catalog of items available for sale. The supplier creates this document and provides it to prospective buyers.

The Item document structure is reasonably “flat”, presenting salable items in a simple hierarchy with few levels. Each item has many possible descriptive characteristics, most of which are optional, but provide ease of description for virtually any product, including packaging, origin, and warranty information.

- [Sample Item document](https://github.com/namm-standards/namm-json/blob/master/examples/item_example.json)
- [Raw Item schema](https://github.com/namm-standards/namm-json/blob/master/schema/item.json)
- [Item schema technical reference](https://standards.namm.org/docs/item/index.html)

## Purchase Order Status Request

The Purchase Order Status Request document, **postatusrequest.json**, requests from the supplier the status of an outstanding Purchase Order. In fact, the status of multiple Purchase Orders may be requested in one transaction, obviating the need for multiple requests.

- [Sample Purchase Order Status Request document](https://github.com/namm-standards/namm-json/blob/master/examples/po_status_request_example.json)
- [Raw Purchase Order Status Request schema](https://github.com/namm-standards/namm-json/blob/master/schema/po_status_request.json)
- [Purchase Order Status Request schema technical reference](https://standards.namm.org/docs/po_status_request/index.html)

## Purchase Order Status

The Purchase Order Status document, **postatus.json**, is a dual-purpose transactional document. It may be transmitted from supplier to buyer as an acknowledgement of a submitted Purchase Order, or as a status report for a buyer's open Purchase Orders, generated either periodically or in response to a specific Purchase Order Status Request.

- [Sample Purchase Order Status / Detail Acknowledgement document](https://github.com/namm-standards/namm-json/blob/master/examples/po_tatus.json)
- [Raw Purchase Order Status / Detail Acknowledgement schema](https://github.com/namm-standards/namm-json/blob/master/schema/po_status.json)
- [Purchase Order Status / Detail Acknowledgement schema technical reference](https://standards.namm.org/docs/po_status/index.html)

<!--
## Electronic Delivery Response
The Electronic Delivery Response document, **ed_response.json**, sent by the supplier to the buyer, contains specific information about items ordered, including barcodes, serial numbers, prices, etc. It may be used by the buyer to confirm that the order was received and processed correctly by the supplier.

- [Sample Electronic Delivery document](https://github.com/namm-standards/namm-json/blob/master/examples/ElectronicDelivery.json)
- [Raw Electronic Delivery schema](https://github.com/namm-standards/namm-json/blob/master/schema/ed_response.json)
- [Electronic Delivery schema technical reference](https://standards.namm.org/docs/elecdeliv/index.html)


## Payment Advice

- [Sample Payment Advice document](https://github.com/namm-standards/namm-json/blob/master/examples/PaymentAdvice.json)
- [Raw Payment Advice schema](https://github.com/namm-standards/namm-json/blob/master/schema/PaymentAdvice-v2015.1.schema.json)
- [Payment Advice schema technical reference](./payadvice/index.html)
-->

## Party  

The Party document, **party.json**, contains general information about the parties involved in the B2B transactions that the parties wish to share with each other. Each party, also called a trading partner, creates a Party document.

The Party document contains public identifying information about one trading partner. However, for added flexibility, multiple trading locations for the partner may be specified in the same document. These locations might be such things as different branch offices, product warehouses, or shipping destinations. Each party and each location includes a unique ID, which may be in GLN (Global Location Number) or NAMM format, plus the partner entity’s name, address, phone, email, and other contact information.

- [Sample Party document](https://github.com/namm-standards/namm-json/blob/master/examples/party_example.json)
- [Raw Party schema](https://github.com/namm-standards/namm-json/blob/master/schema/party.json)
- [Party schema technical reference](https://standards.namm.org/docs/party/index.html)

 
## Functional Acknowledgement
>**This document has been added to 2020 version in July 2021".**

The Functional Acknowledgement document, **funtional_ack.json**, is a simple notification from party to party acknowledging the receipt of another transaction such as an item document, purchase order, advanced shipment notice, or invoice. It may also provide information indicating the status of the transactional document. This document is used repeatedly throughout the supply chain.

- [Sample Functional Acknowledgement document](https://github.com/namm-standards/namm-json/blob/master/examples/functional_ack_example.json)
- [Raw Functional Acknowledgement schema](https://github.com/namm-standards/namm-json/blob/master/schema/functional_ack.json)
- [Functional Acknowledgement schema technical reference](https://standards.namm.org/docs/functional_ack/index.html)

<!--
## Price Book

The Price Book document, **pricebook.json**, contains pricing information for various items offered by the supplier. It can specify price changes (breaks) at different purchase tiers (e.g., 1-100 units, 101-500 units, etc.) as well as indicate units of measure (UOM) for the tiers. This document is optional.
-->

<!--
## Sell Through

The Sell Through document, **sellthrough.json**, is an optional post-transaction report sent by the buyer to the supplier, indicating what products have been sold, to which locations, for what price, etc. This document is discretionary and informational only, and thus does not trigger any further action, acknowledgement, or event.
***************************************************************** 
-->

## Common Elements  
Among the schema files, the Common Elements schema, **common.json**, is of particular importance to the NAMM B2B system.

As in any data processing system, certain identical elements can appear in multiple places, both within and across files. For example, the *Address* element might appear in various places in a single document or in different documents, and its sub-components &mdash; *City*, *State*, *Postal Code*, etc. &mdash; might vary in format from document to document. When coded individually, recurring "almost identical" elements such as these present problems in development, maintenance, and usage. A change to or variation in one instance of an element might "break" other instances, especially with regard to processing software.

A significant feature of JSON schemas is the ability to define in one place the common elements that are used in multiple documents, and then simply refer to them later. The NAMM B2B system takes advantage of this feature by collecting common elements into a single schema file. In this file are defined many commonly used B2B elements, from general elements like *Address* and *Currency Code* to more specific elements like *Weight Unit of Measure* and *Purchase Order Line Number*. In other schemas that use these elements, the elements are then included by reference rather than by repitition, thereby eliminating redundant code and potential inconsistencies. Changing an element in one place changes it instantly and identically everywhere it occurs.

This common element localization is an important factor in the success of the overall system. Defining common elements in a single place simplifies development, reduces maintenance, and standardizes processing.

Because this file comprises only schema elements that are referenced by other schemas, it does not have an accompanying content document.

- [Raw Common Elements schema](https://github.com/namm-standards/namm-json/blob/master/schema/common.json)
- [Common Elements schema technical reference](https://standards.namm.org/docs/common/index.html)

>End of document
