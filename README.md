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

Lets get started;
<h2>Task 1: Create Your VPC</h2>
Create a VPC and Subnets as well as routing and security groups

• Go to “Your VPCs” from the VPC service on the AWS management console and click on
the orange “Create VPC” button.
![image](https://github.com/user-attachments/assets/6699dcb8-68e3-46ab-af68-daba236f0741)

Only create a VPC here and give it a name. You are free to make your own name or follow along with the one put here.

Give it a 10.0.0.0/16 CIDR block and leave everything else as default. 

Click create.
![image](https://github.com/user-attachments/assets/a89edf77-7aae-4ea2-be68-4564983aa09e)

![image](https://github.com/user-attachments/assets/44a29347-b842-4f6f-ba0d-76cbecfaf972)

 To create your subnets go to Subnets on the left hand side of the VPC service and click on it.
 
 ![image](https://github.com/user-attachments/assets/2bd375e9-2c1d-4068-9d4b-af31e72a3d57)

 ● Add your VPC ID to where it asks.

Assign it a name letting you know that it is your first public subnet

● Put it in any availability zone and give it a CIDR of 10.0.0.1.0/24

![image](https://github.com/user-attachments/assets/8d678b3b-131d-43cd-8ff2-815012671952)

![image](https://github.com/user-attachments/assets/11e2268f-268e-4745-992a-4070885c5dea)

Add a second subnet and name it Private Subnet 1 or something to let you know it is your first private subnet

● Put it in the same availability zone as the first subnet you made and give it a CIDR of 10.0.2.0/24

![image](https://github.com/user-attachments/assets/6e375fe1-215d-43c0-9c4c-02fe4a7e60ee)

Add a third subnet and assign a name letting you know it is the second private subnet you will be making

● Put it in the same availability zone as your first public subnet and give it a CIDR of 10.0.3.0/24

![image](https://github.com/user-attachments/assets/8c8fe5a6-0573-4ae4-94cf-ccd696edf170)

Add a fourth and final subnet and give it a name letting you know it is the third private subnet

● Put it in a different availability zone from the rest of your subnets and give it a CIDR of 10.0.4.0/24

![image](https://github.com/user-attachments/assets/39abf25b-ca79-41af-9616-6284ff6e7926)

![image](https://github.com/user-attachments/assets/01b3e963-1f99-43a5-bf1b-18fb990c246c)

Now create an internet gateway and attach it to the VPC by going to Internet Gateways on the left hand side and clicking “Create Internet Gateway”

● Name it something similar to what is below and then click “Create Internet Gateway”

![image](https://github.com/user-attachments/assets/45b9dfef-6586-43d6-a2e7-8eb8c2d4ad2e)

![image](https://github.com/user-attachments/assets/f34ed00e-9647-4269-8b93-9efb5fbe128b)

Once it is created, attach it to your VPC by clicking “Attach to a VPC” on the top of the screen

● Click the drop down and select your VPC that you mad.

![image](https://github.com/user-attachments/assets/44122e96-c32c-4511-bc5c-10718b2de3ab)

![image](https://github.com/user-attachments/assets/7f10a4b6-09a5-406f-95a9-fc43570a1ef4)

![image](https://github.com/user-attachments/assets/48544291-171c-4ba4-8c26-72f525f6b5ff)

Allocate an Elastic IP address by going to Elastic IPs on the left hand side and click “Allocate Elastic IP address”

![image](https://github.com/user-attachments/assets/373f668f-cc7f-4242-8dbe-e0c919417023)

Everything should be good as default but make sure that you are in the same region you have been creating everything in and then press “Allocate”. You can also add a name tag if you wish but it isn’t necessary.

![image](https://github.com/user-attachments/assets/fe6585b5-31f5-4952-960c-38ab0ca3591b)

Create a NAT Gateway by clicking on Nat Gateways on the left hand side and then clicking “Create NAT Gateway”

![image](https://github.com/user-attachments/assets/8eba56b9-c3de-4233-baaa-5608c6795800)

Give it a name similar to the one below and assign it to a public subnet

● Click the drop down for Elastic IPs and click the one you created previously.

● Click “Create NAT gateway”

![image](https://github.com/user-attachments/assets/af003d69-0451-403e-938a-6844db14b62a)

![image](https://github.com/user-attachments/assets/4a7f7eec-6d44-4da5-b412-2adb8f4f5b5a)

Create Route Tables by first heading to “Route Tables” on the left hand side.

![image](https://github.com/user-attachments/assets/96e5a85c-724d-4d90-a529-d9bf53caf9f8)

Click “Create route table”

● Give it a name letting you know this is the public route table for this project.

![image](https://github.com/user-attachments/assets/7621d61f-1a77-460d-96ac-fad5d7049339)

● Assign your VPC to it and click “Create route table”

![image](https://github.com/user-attachments/assets/4105dbf3-7e40-4eb9-8269-1940821aced2)

Create a second route table naming it something to let you know that this is the private route table for your project and assign your VPC to it.

![image](https://github.com/user-attachments/assets/c4b14b00-0948-4d8b-a9bf-056a220ea240)

![image](https://github.com/user-attachments/assets/f7f20aae-f517-4ff4-94d4-ccd9317e3911)

Now associate your subnets with their respective route table

● Click on the public route table and click on “Subnet association” next to “Details”

![image](https://github.com/user-attachments/assets/11dbf9f5-9008-4b87-8030-37d9a67fa974)

![image](https://github.com/user-attachments/assets/9034b3ce-2859-42d3-9010-e25b05409d62)

Click on your public subnet and then click “Save associations”. Now add a route to our public route table to get access to the internet gateway. Click on “Routes” next to “Details” and click “Edit routes” Add a new route having a destination of anywhere and a target of your internet gateway and click “Save changes”.

![image](https://github.com/user-attachments/assets/50a14389-e7ad-4a48-a94e-66d062bb6c44)

![image](https://github.com/user-attachments/assets/d834860e-e934-407d-95fe-342fe1762030)

Do the same thing for your private route table by clicking on it and going to its subnet associations and editing them. Click on all three of your private subnets and save the associations.

![image](https://github.com/user-attachments/assets/578d3496-371f-476a-aaac-cc9e9ea4556d)

Go to edit the routes of the private table Add a route to the private table that has a destination of anywhere and a target of your Nat gateway that you created earlier.

![image](https://github.com/user-attachments/assets/61c60c75-f714-4890-b196-70d03618d380)

![image](https://github.com/user-attachments/assets/ce66a0a9-fdc2-49aa-b465-f5bc543fad1f)

<h2>Task 2. Create Your Security Groups</h2>

Now to create our security groups (One for our bastion host, web server, app server, and our database). we will head to Security Groups on the left and click “Create security group”.

![image](https://github.com/user-attachments/assets/72d3dcc4-b184-42d6-8a85-e7dda3b64877)

Give it a name and description letting you know it is for a bastion host Assign your VPC to it.

![image](https://github.com/user-attachments/assets/9b895c77-25c2-4ce7-a296-fa62b59a3871)

Give it three inbound rules, one for SSH using your IP and one for HTTP using 0.0.0.0/0 as well as https using 0.0.0.0/0

![image](https://github.com/user-attachments/assets/8feaf179-739a-40c1-9d40-ce58165f4d0d)

![image](https://github.com/user-attachments/assets/db6f817a-aa95-4a15-baf9-39f29501ae3d)

Create another security group Give it a name and description letting you know it is for a Web server Assign your VPC to it.































































