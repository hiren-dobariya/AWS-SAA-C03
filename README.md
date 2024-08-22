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



