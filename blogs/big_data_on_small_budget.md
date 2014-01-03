## Big Data on Small Budget

Excerpt: Processing big data can be done on a small budget, we will talk about building a Hadoop cluster with 45TB capacity with $15k, and compare it with the costs of Elastic Map/Reduce clusters on AWS.

Processing big data requires a lot of CPU power and storage space, you will need a cluster of powerful servers to process data in a distributed fashion. Setting up a cluster could be very expensive. Here we look at several relatively low cost options. We are not talking about petabytes big data at the major league scale like Fackbook and Twitter, but big enough that can not be handled by traditional RDBMS - at 10s to 100s terabytes. We will be focusing on building a 10 node cluster for using Hadoop, but it can be used for other distributed computing platforms like Cassandra or Storm.

1. AWS Elastic Map/Reduce
2. DIY using AWS EC2 instances
3. DIY using PCs built with off-the-shelf components

### AWS Elastic Map/Reduce

Amazon's Elastic Map/Reduce provides the easiest way to setup a Hadoop cluster, so if your budget allows, this is a great option. Setting up a Elastic M/R cluster is simple - just a few clicks from the AWS console and you will be up and running.  Depending on your requirement, you may keep the cluster running 100% of the time, or you can fire up a cluster and terminate it when you are done with your data processing tasks. The cost of the cluster is determined by the hardware configuration(type of server instances) and the software configuration(which Hadoop distribution: Amazon or MapR). And currently Elastic Map/Reduce provides the those applications: Hive, Pig, and Hbase. 

Here are the costs of some commonly used EC2 instance types, except hs1.8xlarge - the most expensive instance so far, it's listed here for reference. The full EC2 instance pricing list is available [here](http://aws.amazon.com/elasticmapreduce/pricing/)
<table border="0.5">
<thead>
<tr><th>Instance Type</th><th>vCPU</th><th>ECU</th><th>Memory(GB)</th><th>Storage(GB)</th><th>SSD(GB)</th><th>Cost/Hour</th><th>Cost/Month</th></tr>
</thead>
<tbody>
<tr><td>m2.2xlarge</td><td>4</td><td>13</td><td>34</td><td>850</td><td></td><td>$0.82</td><td>$590</td></tr>
<tr><td>m2.4xlarge</td><td>8</td><td>26</td><td>68</td><td>1680</td><td></td><td>$1.64</td><td>$1180</td></tr>
<tr><td>m1.xlarge</td><td>4</td><td>8</td><td>15</td><td>840</td><td></td><td>$0.48</td><td>$345</td></tr>
<tr><td>c1.xlarge</td><td>8</td><td>20</td><td>7</td><td>840</td><td></td><td>$0.58</td><td>$418</td></tr>
<tr><td>hs1.8xlarge</td><td>16</td><td>35</td><td>117</td><td>49152</td><td></td><td>$4.60</td><td>$3312</td></tr>
</tbody>
</table>

And for a 10 node cluster, the cost can be calculated using the [AWS cost calculator](http://calculator.s3.amazonaws.com/calc5.html#s=EMR)

<table border="0.5">
<thead>
<tr><th>Cluster Size</th><th>Instance Type</th><th>Monthly Cost</th></tr>
</thead>
<tbody>
<tr><td>10 node</td><td>m2.4xlarge</td><td>$16435</td></tr>
<tr><td>10 node</td><td>m2.2xlarge</td><td>$8294</td></tr>
<tr><td>10 node</td><td>c1.xlarge</td><td>$5636</td></tr>
<tr><td>10 node</td><td>m1.xlarge</td><td>$4831</td></tr>
<tr><td>10 node</td><td>hs1.8xlarge</td><td>$41733</td></tr>
</tbody>
</table>

<table border="0.5">
<tbody>
<tr><th>Pros</th><td>Easy to setup, can be lauched and terminated on demand</td><tr>
<tr><th>Cons</th><td>Cost expensive, limited choices of OS, Hadoop distribution and applications</td><tr>
</tbody>
</table>

For 10 node m2.4xlarge cluster the monthly cost is $16435 (also you will need to budget around 1/5 of that to cover network i/o and storage costs).

### DIY using AWS EC2 instances 

AWS Elastic Map/Reduce adds management and software cost on top of the instance costs. If you want to avoid those costs, you can build your cluster using individual EC2 instances, and install and configure the cluster software yourself.

<table border="0.5">
<thead>
<tr><th>Cluster Size</th><th>Instance Type</th><th>Monthly Cost</th></tr>
</thead>
<tbody>
<tr><td>10 node</td><td>m2.4xlarge</td><td>$11800</td></tr>
<tr><td>10 node</td><td>m2.2xlarge</td><td>$5900</td></tr>
<tr><td>10 node</td><td>c1.xlarge</td><td>$4180</td></tr>
<tr><td>10 node</td><td>m1.xlarge</td><td>$3450</td></tr>
<tr><td>10 node</td><td>hs1.8xlarge</td><td>$33120</td></tr>
</tbody>
</table>

<table border="0.5">
<tbody>
<tr><th>Pros</th><td>Relatively cheaper, install any OS, Hadoop distro, and applications</td><tr>
<tr><th>Cons</th><td>Need to manually install and manage the cluster</td><tr>
</tbody>
</table>
 
For a 10 node m2.4xlarge cluster the monthly cost is $11800 (also you will need to budget around 1/5 of that to cover network i/o and storage costs, plus labor cost, and software licensing costs).

If you don't plan to set up a permenant cluster that's available 24/7, you can use spot instances to reduce cost.  For permenant clusters, using reserved instances can lower your costs, [you can find more details here](http://aws.amazon.com/elasticmapreduce/pricing/).


### DIY using servers built with off-the-shelf components

If you are up to building everything yourself, this will be the lowest cost option. Here's the current pricing for components and total cost for a mid-to-high end single server (more or less equivalent to EC2 m2.4xlarge, with faster CPU, much more disk space including fast SSD, but less memory):

<table border="0.5">
<thead>
<tr><th>Server Component</th><th>Configuration</th><th>Cost</th><th>Source</th></tr>
</thead>
<tbody>
<tr><td>CPU</td><td>8 core 4Ghz</td><td>$200</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16819113284">AMD FX-8350</a></td></tr>
<tr><td>Memory</td><td>32GB DDR3 </td><td>$350</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16820231590">G.SKILL</a></td></tr>
<tr><td>Motherboard</td><td>AMD 990FX</td><td>$175</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16813131877">ASUS</a></td></tr>
<tr><td>SSD Drive</td><td>512GB</td><td>$395</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16820249034">Plextor</a></td></tr>
<tr><td>Hard Drive</td><td>4TB</td><td>$159</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16822178338">Seagate</a></td></tr>
<tr><td>Display Card</td><td>Geforce 1GB</td><td>$32</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16814130585">EVGA</a></td></tr>
<tr><td>Case</td><td>ATX Mid Tower</td><td>$89</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16811112331">LIAN LI PC</a></td></tr>
<tr><td>Power Supply</td><td>650W</td><td>$89</td><td><a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16817139005">CORSAIR TX Series</a></td></tr>
<tr><td>total</td><td></td><td>$1500</td><td></td></tr>
</tbody>
</table>

Cost of a 10 node off-the-shelf servers
<table border="0.5">
<thead>
<tr><th>Off-The-Shelf</th><th>vCPU</th><th>ECU</th><th>Memory(GB)</th><th>Storage(GB)</th><th>SSD(GB)</th><th>CPU Speed</th><th>Cost</th></tr>
</thead>
<tbody>
<tr><td>Low End</td><td>4</td><td>12</td><td>16</td><td>2000</td><td>128</td><td>3Ghz</td><td>$800</td></tr>
<tr><td>Mid End</td><td>8</td><td>32</td><td>32</td><td>4000</td><td>512</td><td>4Ghz</td><td>$1500</td></tr>
<tr><td>High End</td><td>8</td><td>32</td><td>64</td><td>16000</td><td>1024</td><td>4Ghz</td><td>$3000</td></tr>
</tbody>
</table>

So a 10 node cluster similar to m2.4xlarge costs around $15000, and a lower end setup equivalent to a 10 m2.2xlarge node cluster costs around $10000 

<table border="0.5">
<tbody>
<tr><th>Pros</th><td>Cheapiest option, install any OS, Hadoop distro, and applications, able to take advantage of physical hardware</td><tr>
<tr><th>Cons</th><td>Need to manually install and manage the cluster, and physical hardware</td><tr>
</tbody>
</table>


## Choosing a Hadoop Distribution

You can choose a Hadoop distribution from one of the following competing offerings, they are all pretty good, so anyone of those should more or less sastify your big data needs:

[Cloudedea CDH4](http://www.cloudera.com/content/cloudera/en/products-and-services/cdh.html)
 
[Hortonworks HDP](http://hortonworks.com/products/hdp/)

[MapR](http://www.mapr.com/products/mapr-editions)




	

	
