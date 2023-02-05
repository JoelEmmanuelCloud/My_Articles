# Understanding AWS 3-Tier Cloud Architecture and Design.

In this article, we're going to learn the 3 tier system design of a cloud architecture. I'm also going to go over some of the questions you can expect during your interview.

So first things first, **why is it called three tiers**?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675609431807/881f5b32-f86e-49e1-94f9-5353ee791150.png align="center")

Because this architecture has three distinct layers or tiers. So the users can access their application using a **front end** or presentation layer, and this presentation layer will interact with the **application layer**, and the application layer on the **backend** saves and retrieves information from the database as needed. These three distinct tiers **presentation layer** (front-end), **application** **layer** (backend), and **database** are the reason this architecture is called three tiers.

## Presentation Layer

Let's focus on the presentation layer. One example of the presentation layer could be the [amazon.com](http://amazon.com) website where you can browse different items, and this website could be running Nginx or Apache web server, or multiple other options as well.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675612965839/411f06be-0143-4723-b72e-3900e6e388bb.png align="center")

If you like an item on amazon and want to add it to your cart, as soon as you click **Add-to-Cart** your order and other information are inserted into a database. Like your home address, as well as your eligibility to shop with a credit or debit card.

So as soon as you click **Add-to-cart**, the **presentation layer** calls the **application layer** on the back-end (where a bunch of business logic is running) which calls the **database** to store your order.

One example of this layer is your Java code. Running in a jar file. And this jar file is running Apache Tomcat Application Server.

### **Database**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675612664899/dc54318d-7530-42b8-9c37-dca3cbae5162.png align="center")

Moving on to the database, you have many choices some of the popular ones include Oracle, MySQL, and PostgreSQL, but then beyond that, you have your NoSQL database such as DynamoDB, etc.

So going back to our three tiers, you can run your web server on virtual machines on Amazon EC2, and we can run the app server as well on Amazon EC2 so the app server can communicate with the database.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675613537689/78c90559-e58d-473e-8ceb-d0c61525f7a9.png align="center")

And both of these tools will have some IP address.

**Elastic Load Balancing (ELB)**

So your interviewer might ask you:

* **how will the web server access the app server**?
    
* **And how will users access this web server from the internet?**
    

Because you must not hardcode these IP addresses into your application. Because this application can go down.

You might need to scale. So it's unreliable to go by one IP address because there might be an IP address change.

The way to deal with all this is 'you can communicate between the web server and the app server using a **Load Balancer'** so the user can access this web server by using the DNS attached to this **Elastic Load Balancer** which is pointing to the web server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675614283343/680c2650-bc99-4512-9c36-72bacf4942e6.png align="center")

**What is an Elastic Load Balancing?**

Elastic Load Balancing **automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones**. It monitors the health of its registered targets and routes traffic only to the healthy targets.

And in the case of any website, in our case [amazon.com](mailto:looks@amazon.com) You can map [amazon.com](http://amazon.com) to the DNS of this **front-end elastic load balancer**, which will point to the web server and the web server will point to the **application load balancer**. The application load balancer will point to the App server.

### Single point of Failure

What are some of the single points of failure in this architecture?

**Elastic load balancers** are managed by the cloud provider and are inherently highly available.

So even though you'll see one icon of elastic load balancing, under the hood this elastic load balancer is running in three different availability zones (AZ). So it is not a single point of failure.

However, the web server and app server, which are each running on single EC2's are single points of failure. And the same thing is true for this database.

So how will you make this architecture scalable and highly available?

### **Auto Scaling Group**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675617382689/0e1e82f0-5745-4bf7-bce5-17593aab0298.png align="center")

You make this architecture scalable and highly available by putting the institute's web server under an Auto Scaling group. And you should have at least two web servers and two app servers EC2's in two different AZ.

In the Auto Scaling group, you can mention the minimum desired number of instances running and that's where you can set how many minimum web servers you want.

### **Network Security**

Your interviewer might ask you about network security:

* **how are you going to secure this kind of design from a network perspective?**
    

You can secure your design by putting as many components in the private subnet or green zone as possible and the thing that should be on the yellow or red site, or public subnet is the **elastic load balancer** fronting that web server. So that internet traffic come to the load balancer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675618668525/c9717e5a-d6d3-49bb-8dc7-b22fd9a8074c.png align="center")

The top load balancer should be put in a public subnet, and the rest of the components are put on a private subnet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675620279564/c0f4c3c4-d008-47cd-95b5-8bda20955735.png align="center")

You can also implement network security constructs for AWS, such as a network access control list security group under EC2 to control traffic.

You can also integrate AWS Web Application Firewall (WAF) with a load balancer that lets you monitor web requests and protect your web applications from malicious requests.

You can use AWS WAF to block or allow requests based on conditions that you specify such as IP addresses.

You can also use AWS WAF pre-configured protections to block common attacks like SQL injection or cross-site scripting.

Now let's turn our attention toward the database tier. You can be asked a lot of questions in this area. One question you can expect is:

* **would you rather use SQL or NoSQL database for your architecture design**?
    

A nice answer would be a NoSQL database like an AWS database. Because AWS native databases offer out-of-the-box high availability, font protection, and replication which you have to manage if you run an SQL database on EC2 and that's a rabbit hole you want to avoid.

### **AWS Database**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675621217373/47a017c6-14be-4260-a8af-b6cd3db90acb.png align="center")

So let's say we are using Amazon Aurora for our three-tier architecture, so might get this question **How are you going to make this database highly available**?

That's the beauty of the AWS native database out of the box, Amazon Aurora is multi-AZ. So one copy of data is saved into multiple availability zones and even if one availability zone goes down, Aurora will automatically promote another copy of the data as the primary instance.

This multi-AZ feature is within a single region. But if you want resiliency beyond a single region, you can use a global database feature that replicates your database into another region.

Keep in mind these two feature is true for DynamoDB as well, which is AWS's flagship NoSQL database.

### **Database Optimization**

Another area you might get probed about is database optimization.

How do you handle high traffic? How can you optimize the database?

Amazon Aurora provides a read replica. So you can use these read replicas to handle traffic while the right profit goes to the primary instance.

You can also introduce caching layer using Amazon elastic cache. And you can also utilize caching in different layers of the architecture. And beyond that, there's always traditional quality for your application.

---

This is just the basics of how you can implement and use a tier 3 cloud architecture design. Hope you found this article useful, please leave a comment if you have something to add and I will really appreciate it if you like and share this post.