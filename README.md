<h1>Design and configure a high available 3-tier Client Architecture on AWS</h1>

Designing a 3-Tier Client Architecture on AWS teach me a variety of valuable skills, both in terms of technical knowledge and cloud architecture principles. Here’s an outline of what I learned during the design process:

✅ Cloud Architecture Design — Structuring systems for scalability, security, and cost optimization. AWS Service Familiarity — Hands-on experience with core AWS service
(EC2, RDS, VPC, etc.).

✅ Database Tier Design: Gain insight into designing the database layer of the architecture, including choosing between SQL (RDS/Aurora) and NoSQL (DynamoDB) databases.

✅ Networking & Security — Best practices for VPC, subnets, security groups, and IAM roles.

While an infrastructure can, in theory, have any number of tiers, by far the most common pattern is the 3-tier architecture. In 3-Tier architecture, An application server is a component-based product that resides in the middle tier of a server-centric architecture. It provides middleware services for security and state maintenance and also provides data access and persistence. The application servers also contain the business logic. The web servers are located at the top of the diagram. The web servers are responsible for the presentation logic. They are often accompanied by load balancers. Load balancers are responsible for efficiently distributing incoming network traffic across a group of back-end servers. The bottom of this diagram includes the database servers. This tier is responsible for the database logic.

<h2>How can business benefit?</h2>
Designing an AWS 3-Tier Client Architecture provides several business benefits, especially in terms of scalability, security, cost optimization, and high availability. Here’s why businesses choose this model: 
<h2>Scalability & Performance</h2>
<ol>
<li>Auto Scaling: Each tier can scale independently based on demand (e.g., AWS Auto Scaling, ALB).</li>
<li>Elastic Load Balancing (ELB): Distributes traffic across multiple instances, improving responsiveness.</li>
<li>Database Scaling: Amazon RDS and Aurora offer read replicas and caching with ElastiCache.</li>
</ol>
✅ Business Impact: Handles increasing workloads without affecting user experience.

<h2>Security & Compliance</h2>
<ol>
<li>Isolation of Tiers: Using AWS VPC, security groups, and subnets, each layer is protected separately.</li>
<li>Least Privilege Access: The database tier is not directly exposed to the internet, reducing attack risks.</li>
<li>AWS IAM & Security Policies: Fine-grained access control ensures that only authorized services/users access data.</li>
</ol>
✅ Business Impact: Protects sensitive data, reduces security risks, and ensures regulatory compliance (e.g., GDPR, HIPAA).

<h2>Cost Optimization</h2>
<ol>
<li>Efficient Resource Utilization: Separate tiers allow businesses to optimize computing power based on workloads.</li>
<li>Managed Services: Using AWS Lambda (serverless compute) and RDS (managed database) reduces operational overhead.</li>
</ol>
✅ Business Impact: Lowers infrastructure costs while maintaining performance.

This project describes:
<ol>
<li>Public and private subnets</li>
<li>Components in each layer of the 3-tier infrastructure</li>
<li>Simplified diagrams of a common 3-tier infrastructure</li>
<o/l>

<h2>Public and Private Layers</h2>

The 3-tier design separates an infrastructure into three layers: one public and two private layers. 

Public layer: This presentation tier is where the application’s endpoint lives, and where a user is presented with information. 
Private layer: logic tier, which contains code to translate user actions to a functionality.
Private later: data tier, which hosts the databases.
Anything in the public layer is publicly accessible, but all other content and code in the private layers is only accessible from inside the network.

For example, a user’s browser sends a request to the public tier by clicking a link on a webpage, and the request gets forwarded to the other tiers as needed to present the requested webpage or file to the user. In this tiered design, the goal is for the public layer to act as a shield to the private layers. 

![image](https://github.com/user-attachments/assets/42789565-e021-4357-9582-1262f4050601)

<h2>Detailed Breakdown of Public and Private Subnets<h2>
Understanding the roles and configurations of public and private subnets is crucial for effectively implementing the 3-tier network architecture. The main feature that makes a subnet "public" or "private" is how instances in that subnet access the internet. 

<h3>Public Subnets</h3>
Gateways to the Internet Instances/resources in a public subnet have access to and are accessible from the internet.
<h3>Internet Gateway (IGW)</h3>
Public subnets allow its instances to access the internet via an Internet Gateway. This allows resources like Application Load Balancers (ALBs) to be directly accessible from the internet, enabling users to interact with your application.
<h3>NAT Gateway</h3>
Although instances within a public subnet can access the internet, they can also route traffic to private subnets via a NAT Gateway. This configuration is particularly useful for hybrid setups where some resources need to access the internet for updates or external API calls without exposing your entire infrastructure. 
<h3>Private Subnets </h3>
Securing the Core Infrastructure
A private subnet allows its instances to access the internet via Amazon's managed NAT service (NAT Gateway), which is managed by AWS and scales out as needed. Access to the internet (when permitted) is only one-way, originating from within the subnet.

<h3>Managed NAT Service</h3> 
Instances within private subnets are protected from direct internet exposure. When these instances need to access the internet (e.g., to download updates or communicate with external APIs), they do so through a NAT Gateway located in the public subnet. This ensures that outbound traffic is secure and controlled.
<h3>Security Groups</h3>
To further enhance security, AWS Security Groups are used to tightly control the inbound and outbound traffic. Security Groups act as virtual firewalls for your instances.



