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

