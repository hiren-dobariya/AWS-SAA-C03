# AWS Solution Architect Association - SAA-C03

<h2>AWS Networking Concepts</h2>

<h3>IP Addressing</h3>
<ul>
  <li><strong>IPv4:</strong> Supports 3.4 billion unique addresses and is widely used for general networking purposes.</li>
  <li><strong>IPv6:</strong> The newer protocol, primarily utilized in IoT operations, offering a vastly expanded address space.</li>
   <li><strong>Public IP:</strong> Accessible over the internet.</li>
  <li><strong>Private IP:</strong> Accessible only within a private network.</li>
</ul>


<h3>Elastic IP</h3>
<p>In AWS, a new public IP address is assigned each time an EC2 instance is stopped and started. To maintain a fixed public IP, an Elastic IP address is required. However, AWS limits users to a maximum of 5 Elastic IP addresses per account. It is recommended to minimize their use as they are not considered a best practice.</p>

<h3>Placement Groups</h3>
<p>Placement Groups allow control over the placement of EC2 instances, optimizing for performance, resilience, or cost. Three strategies are available:</p>
<ul>
  <li><strong>Cluster:</strong> Groups instances in a single Availability Zone to minimize latency and maximize throughput.</li>
  <li><strong>Spread:</strong> Distributes instances across distinct hardware, with a limit of 7 instances per group per Availability Zone. This is ideal for critical applications requiring isolation from hardware failure.</li>
  <li><strong>Partition:</strong> Spreads instances across multiple partitions, which utilize separate racks within an Availability Zone. This strategy supports up to hundreds of instances and is suited for large-scale distributed systems like Hadoop, Cassandra, and Kafka.</li>
</ul>

<h3>Elastic Network Interfaces (ENI)</h3>
<p>An Elastic Network Interface (ENI) is a logical network component within a VPC that functions as a virtual network card. Key attributes of an ENI include:</p>
<ul>
  <li>One Primary IPv4 addresse, One or more secondary private IPv4 addresses</li>
  <li>One Elastic IPv4 address (for private or public IPs)</li>
  <li>One or more security groups</li>
  <li>A MAC address</li>
</ul>
<p>ENIs can be created independently and dynamically attached to EC2 instances, facilitating seamless failover. For example, an F5 load balancer bound to a private IP address can failover by moving its ENI from one instance to another, ensuring high availability. Note that ENIs are specific to the Availability Zone in which they are created.</p>

<a href="https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/">
  <img src="https://github.com/user-attachments/assets/b1249bd5-0c22-4fc7-bdc2-e49c4c1d2549" alt="Elastic Network Interfaces in the Virtual Private Cloud">
</a>

<h3>Advantage of ENI</h3>

<strong>Management Network / Backnet</strong>
<p>You can set up your web, application, and database servers in a dual-network setup. The first network interface (ENI) of the instance connects to a public subnet and sends all traffic (0.0.0.0/0) to the Internet Gateway of the VPC. The second ENI connects to a private subnet, sending all traffic to your VPN Gateway connected to your company's network. This private network is used for tasks like SSH access, management, and logging. You can apply different security groups to each ENI, allowing port 80 traffic through the public ENI and port 22 traffic through the private ENI.</p>

<strong>Multi-Interface Applications</strong>
<p>You can use an EC2 instance to host load balancers, proxy servers, and NAT servers, forwarding traffic between subnets. In this case, you would disable the Source/Destination Check Flag to let the instance handle traffic that isn’t directly addressed to it. Vendors of networking and security products are expected to build AMIs that support this dual-ENI setup.</p>

<strong>MAC-Based Licensing</strong>
<p>If you're using software that is licensed to a specific MAC address, you can tie it to the MAC address of an ENI. If you need to switch instances or change instance types later, you can attach the same ENI (and its MAC address) to a new instance to maintain the license.</p>

<strong>Low-Cost High Availability</strong>
<p>To achieve high availability on a budget, you can attach an ENI to an instance. If the instance fails, you can launch a new one and attach the same ENI to it. This will restore traffic flow within a few seconds.</p>

<h3>EC2 Hibernate</h3>

<p>Support all instance families</p>
<p>Instance RAM size must be less than 150GB</p>
<p>Bare Metal Instance type not supported</p>
<p>Root volume must EBS, encrypted, not instance store and large</p>
<p>Available for On-Demanad, Sport and Reserve instances</p>
<p>Instance can not be hibernated more than 60 days</p>

<h3>EBS(Elastic Block Store) Volume </h3>
<p>EBS volume is network drive, you can attached to your instance while they run</p>
<p>It allows you to persist data even after restart/termination </p>
<p>It can be monunted to once instance at a time. Once instance can have multiple EBS volumes</p>
<p>They are bound to specific AZ </p>
<p><strong>Free tier</strong> : free 30 GB SSD</p>
<p>Communicate between Instance and EBS volume use network which means a bit of letancy</p>
<p>EBS Volume easyly attached and detached once EC2 instance to another EC2 instance. To move a volume across, first need to snapshot it</p>
<p>EBS Volume performance depend on size of GB and IOPS</p>
<p>Root EBS volume is deleted on instance termniate</p>

<h3>EBS Snapshots</h3>
<p>Make a backup = snapshot of your volume at point in time</p>
<p>Can copy snapshot across the AZ or region</p>
<p>EBS snapshot can archive, "archive tier" 75% cheaper </p>
<p>Restoring archive takes 24 to 72 hours</p>
<p>set up rules to retain recycled snapshot so accidental deleted snapshot can recover</p>
<p>retention period is 1 day to 1 year </p>
<p><strong>Fast Snapshot Restore(FSR)</strong>: Forceful initialization of snapshot to have no latency. This comes with cost</p>
<p>Volume can create from Snapshot</p>

<h3>AMI (Amazon Machine Image) Overview</h3>
<p>AMI are customization of an EC2 instance</p>
<ul>
  <li>You add your own software, configuration, operating system, monitoring</li>
  <li>faster boot/configuration time becase all software are pre-package</li>
  <li>AMI build for specific region but copy across the regions</li>
</ul>
<p>EC2 instances launch from : </p>
<ul>
  <li> Public AMI : AWS provide</li>
  <li>Own AMI : create your own and maintain</li>
  <li>AWS Marketplace AMI : Created by someone else and sell </li>
</ul>

<h3>EC2 Instance Store</h3>
<ul>
  <li>EBS volume store are network drives with good but limited performance</li>
  <li>For high performance hardware disk, use EC2 Instance Store</li>
  <li>Better I/O operation</li>
  <li>EC2 Instance Store loss storage if they're stopped </li>
  <li>Good for Cache, buffer, temporary data, scratch data</li>
  <li>Risk of data loss if hardware fails</li>
</ul>

<h3>EBS Volume Types</h3>
<ul>
  <li> <strong>gp2/gp3 (SSD):</strong> General purpose SSD volume that balance price and personal for wide variety of workload  </li>
   <li> <strong>io1/io2 Block Express(SSD):</strong> High performance SSD volume for mission critical low latency or high-throughout workload  </li>
  <li> <strong>st1 (HDD):</strong> Low cost HDD volume design for frequently accessed, throughput intestive workloads </li>
  <li> <strong>sc1 (HDD):</strong> Low cost HDD volune design for less frequently accessed workloads </li>
  <li> Only gp2/gp3 and io1/io2 Block Express can be used as bool volumes</li>
</ul>

<p>EBS Volume type use cases :</p>
<strong>General purpose SSD:</strong>
<ul>
  <li>System boot volume, Virtual desktop, development and test environment. </li>
  <li>Range : 1 GB to 16TB</li>
  <li><strong>gp3 :</strong> Baseline 3000 IOPS and thoroughtput 125 MB. Can be increase to 16000 IOPS and 1000 MB independently</li>
  <li><strong>gp2 :</strong> Small GP2 volume burst to 3000 IOPS. Size of volume and IOPS linked, max IOPS 16000. 3 IOPS per GB so 5334 GB, max IO  reache </li>
</ul>
<strong>Provisioned IOPS (PIOPS) SSD:</strong>
<ul>
  <li>Critical business application with sustained IOPS performance</li>
  <li>OR application need more than 16000 IOPS</li>
  <li>Great for Database workload</li>
  <li><strong>io1 (4GB-16TB): </strong>Max IOPS 64000 for Nitro EC2 instance and 32000 for others</li>
  <li>Can increase PIOPS independently from storage size</li>
  <li><strong>io2 Block Express (4GB-64TB):</strong> Sub millisecond latency </li>
  <li>Max IOPS 256000 with IOPS:GB ratio of 1000:1</li>
  <li>EBS support multi attached</li>
</ul>

<strong>HDD:</strong>
<ul>
  <li>Can not be use as root volume</li>
  <li>125 GB to 16TB</li>
  <li><strong>Throughput optimize HDD (st1):</strong> Use for Bigdata, Data warehouses, log processing</li>
  <li> Max IOPS 500, max throughout 500MB</li>
  <li><strong>Cold HDD (sc1):</strong> Use for data infrequently accessed</li>
  <li>Max IOPS 250 and max throughout 250 MB</li>
</ul>

<h3>EBS Multi-Attached - io1/io2 family </h3>
<ul>
  <li>Attached the same EBS volume to multiple EC2 instance within same AZ</li>
  <li>Use case : Achieve higher application availability in clustered Linux application</li>
  <li>Application must manage concurrent write operation</li>
  <li>Volume can attached max 16 instance at a time</li>
  <li>Must use a file system that clusterd aware </li>
</ul>

<h3>Amazon EFS - Elastic File System</h3>
<ul>
  <li>Managed Network File System(NFS) that can be mount on EC2</li>
  <li>EFS work with EC2 instance in <strong>multi AZ</strong></li>
  <li>highly available, scalalble , expensive (3x to gp2), pay per use </li>
  <li>Use Case : content management, web service, data sharing, worldpress</li>
  <li>protocol : NFSv4</li>
  <li> uses security group to access to EFS</li>
  <li> Compatible with Linux base AMI (Not Window)</li>
  <li>POSIX standard Linux file system</li>
</ul>

<h3>Load Balancer</h3>
<ul>
  <li>Load Balancer are servers that forward traffic to multiple server</li>
  <li>expose single point of access (DNS) to your application</li>
  <li>simelessly handle failure of downstream instances</li>
  <li>Do regular health check of your instances</li>
  <li>Provide SSL termination(HTTPS) to your website </li>
  <li>Enforce stickeness with cookies </li>
  <li>High available across zones</li>
  <li>Seperate public traffic with private traffic</li>
  
</ul>
<strong>Why Elastic Load Balancer (ELB)</strong>
<ul>
  <li>Elatic Load Balancer is <strong>managed load balancer</strong></li>
  <li>AWS guarantee that it will be working</li>
  <li>AWS takes care of upgrade, maintenance, high availability </li>
  <li>AWS provide few confirmations knobs</li>
  <li>It costs less to set up your own load balancer but it will be lot more effort on your end</li>
  <li>It is integrated with many AWS services EC2, EC2 auto scaling group, Route 53, AWS WAF etc</li>
</ul>

<h3>Types of Load Balancer</h3>
<ul>
  <li><strong>Classic Load Balancer</strong>: HTTP, HTTPS, TCP and SSL (secure TCP), deprecated, 2009</li>
  <li><strong>Application Load Balancer:</strong> HTTP, HTTPS, WebSocket, 2016</li>
  <li><strong>Network Load Balancer:</strong>TCP, TLS(Secure TCP), UDP, 2017 </li>
  <li><strong>Gateway Load Balancer:</strong>Operated at layer 3(Network Layer), 2020</li>
</ul>

<h3>Application Load Balancer</h3>
<ul>
  <li>Application Load Balancer is Layer 7 (HTTP)</li>
  <li>Using Target group, Load Balancing to multiple HTTP application across machines, Routing based on path in URL  OR Hostname/SubDomain or Query Param</li>
  <li>Using Container : Load Balancing to multiple HTTP application on the same machine</li>
  <li>Support for HTTP/2 and Web Socket</li>
  <li>Support redirects, HTTP to HTTPS</li>
  <li>ABS great for micro service and container-based application e.g. Docker & Amazone ECS</li>
  <li>Application server doesn't see the IP of client directly so true IP of client is inserted into the header X-forwarded-For</li>
  <li>We also get Port (X-Forwared-Port) and prototype (X-Forwared-Proto)`</li>
</ul>

<h3>Network Load Balancer</h3>
<ul>
  <li>Network Load Balancer is Layer 4 allow to : </li>
  <li>Forward TCP and UDP traffic to your instances</li>
  <li>Handle millions of request per second</li>
  <li>Less Letency 100ms compare to 400 ms in ALB</li>
  <li>NLB has once static IP per AZ and supports Elastic IP Address</li>
  <li></li>
</ul>

<h3>Gateway Load Balancer</h3>
<u>
  <li>Gatewau Load Balancer is Layer 3 (IP Packets)</li>
  <li>All the traffic route through Firewall, Intrusion Detection, Prevention System etc (3rd party Security Virtual Appliances).
![image](https://github.com/user-attachments/assets/8d2293b0-e896-4b79-8d75-d8c9a521827d)
   
</li>
<li> https://piyushj02.medium.com/aws-gateway-load-balancer-1o1-26d7cf61b798
    https://aws.amazon.com/elasticloadbalancing/gateway-load-balancer/</li>
    <li>Use GENEVE protocal on port 6081</li>
</u>

<h3>Sticky Session</h3>
<ul>
  <li>Sticky session means same client request always server by same instance behind load balancer</li>
  <li>The cookies used for stickiness has an expiration date you control</li>
  <li>use case : make sure user doesn't lose his session data </li>
  <li>disadvantage : May bring imbalance to the load over the backend EC2 instances</li>
</ul>

<h3>Stickey Session : Cookie Name and Types</h3>
<ul>
  <li><strong>Application-based Cookies </strong> </li>
  <li>Custom Cookie: </li>
  <li>Generated by target</li>
  <li>Can include any custom attribute required by the application</li>
  <li>Cookies name must be specified individually for each target group</li>
  <li>Dont use AWSALB, AWSALBAPP, AWSALBTG. It's reserved for used by ELB  </li>
  <li>Application Cookies: </li>
  <li>Generated by Load Balancer</li>
  <li>Cookies name <strong>AWSALBAPP</strong></li>
  <li><strong>Duration Based Cookies</strong></li>
  <li>Cookie generated Load Balancer</li>
  <li>Cookie name is AWSALB for ALB, AWSELB for CLB</li>
</ul>

<h3>SNI - Server Name Indication</h3>
<ul>
  <li>Server Name Indication is work with Transport Layer Security </li>
  <li>Webserver host many website. SNI indicates which hostname is attempting to connect to server at the start of the handshaking process.</li>
  <li>Why ? Shortage of IP4V. One IP associate with multiple host name so server doesn't know which host is requesting to start process.</li>
  <li>Only works for ALB, NLB and CloudFront, Not works with CLB</li>
</ul>

<h3>Auto Scaling Group - Scaling Policies</h3>
<ul>
  <li><strong>Dynamic Scaling</strong></li>
   <li>Target Tracking Scaling:</li>
  <li>Simple to set up</li>
  <li>If you want to average ASG CPU to stay at around 50%</li>
  <li>Simple / Step Scaling:</li>
  <li>When a CloudWatch alarm is triggered e.g CPU > 70% then add 2 units</li>
  <li>When a CloudWatch alarm is triggered e.g. CPU < 30% then remove 1 unit</li>
  <li><strong>Schedule Scaling : </strong></li>
    <li>Anticipate scaling based on known usage patterns</li>
    <li>E.g. Increase a min capacity to 10 at 5 PM on Friday</li>
  <li><strong>Predictive Scaling:</strong></li>
    <li>Continues forcast load and schedule scaling ahead</li>
    <li>After scaling activity happens, you are in cooldown period. Default cooldown period is 300 seconds</li>
    <li>During cool down period, ASG will not launch or terminate additional instances</li>
</ul>

