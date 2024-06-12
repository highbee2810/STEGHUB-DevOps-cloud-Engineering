# Project Documentation: Web solution with wordpress
In this project storage infrastructure will be prepared on two Linux servers and a basic web solution will be implemented using WordPress.
 **WordPress:** WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS)
 ## Project Tasks:
 1.Configuration of storage subsystem for Web and Database servers based on Linux OS.
 2.Installation of  WordPress and connection to a remote MySQL database server.
## Three-Tier Architecture
![images](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/743b4345-50ee-4d25-a9eb-8a8a5a5b2529)

Three-tier architecture is a well-established software application architecture that organizes applications into three logical and physical computing tiers: the presentation tier, or user interface; 
the application tier, where data is processed; and the data tier, where the data associated with the application is stored and managed.The chief benefit of three-tier architecture is that because each
tier runs on its own infrastructure, each tier can be developed simultaneously by a separate development team, and can be updated or scaled as needed without impacting the other tiers.
**Presentation tier:**
The presentation tier is the user interface and communication layer of the application, where the end user interacts with the application. Its main purpose is to display information to and collect
information from the user. This top-level tier can run on a web browser, as desktop application, or a graphical user interface (GUI), for example. Web presentation tiers are usually developed using
HTML, CSS and JavaScript. Desktop applications can be written in a variety of languages depending on the platform.
**Business or Application tier**
The application tier, also known as the logic tier or middle tier, is the heart of the application. In this tier, information collected in the presentation tier is processed - sometimes against other
information in the data tier - using business logic, a specific set of business rules. The application tier can also add, delete or modify data in the data tier.
The application tier is typically developed using Python, Java, Perl, PHP or Ruby, and communicates with the data tier using API calls.

**Data tier**
The data tier, sometimes called database tier,or data access tier is where the information processed by the application is stored and managed. This can be a relational database management
system such as PostgreSQL, MySQL, MariaDB, Oracle, DB2, Informix or Microsoft SQL Server, or in a NoSQL Database server such as Cassandra, CouchDB or MongoDB.

## The three-tier setup
1. A Laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install
WordPress)
3. An EC2 Linux server as a database (DB) server
## Web server setup
