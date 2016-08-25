Bitfusion Ubuntu 14 Cuda 7.5 AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01DCKFASQ)


EC2 Instance Access
-------------------------------------------------------------------------------

To get started, launch an AWS instances using this AMI from the EC2
Console. If you are not familiar with this process please review the AWS
documentation provided here:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html

Accessing the instance via SSH:

```
ssh -i <path to your pem file> ubuntu@{ EC2 Instance Public IP }
```

Local GPU Instance Cuda Examples: nvida-smi, deviceQuery
-------------------------------------------------------------------------------

Test nvidia driver and CUDA functionality with the following commands:

```
  nvidia-smi

  /usr/local/cuda/samples/bin/x86_64/linux/release/deviceQuery
```

Boost GPU Cluster Cuda Example: deviceQuery
-------------------------------------------------------------------------------

To start this AMI in a Boost Cluster formation select one of the cluster
configuration for this AMI from the AWS Marketplace:

  https://aws.amazon.com/marketplace/pp/B01DCKFASQ

Once the Boost Cluster is up and running you can utilize Boost to execute your
application across all the nodes in the cluster as follows:

```
  bfboost client <command>
```

For example you can execute device query as follows:

```
  bfboost client  /usr/local/cuda/samples/bin/x86_64/linux/release/deviceQuery
```

Detailed Boost and Boost Cluster documentaion is available at:

  http://www.bitfusion.io/documentation-boost-aws/


Boost Local CPU Example
-------------------------------------------------------------------------------

Running Octave with and without Bitfusion Boost locally:

Install Octave (this will take a few minutes):

```
  sudo apt-get install octave
```

Create a simple octave script

```
  vi sgemm.m
```

Enter the following code and save the file:

```
  N = 2048;
  A = single(rand(N,N));
  B = single(rand(N,N));
  start = clock();
  C = A * B;
  elapsedTime = etime(clock(), start);
  disp(elapsedTime);
  gFlops = 2*N*N*N/(elapsedTime * 1e+9);
  disp(gFlops);
```

Run without Bitfusion Boost:

```
  octave ./sgemm.m
```

Run with Bitfusion Boost:

```
  bfboost run "octave ./sgemm.m"
```

Each run will produce two sets of numbers. The top number is the elapsed time
and the bottom number is the GFLOPs. When you run with bfboost you should see
significantly lower run times and higher GFLOPs, which indicate better performance.



Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.medium	t2.large
m3.medium	m3.large	m3.xlarge	m3.2xlarge
m4.large	m4.xlarge	m4.2xlarge	m4.4xlarge	m4.10xlarge
c3.large	c3.xlarge	c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large	c4.xlarge	c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large	r3.xlarge	r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge	i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge	d2.2xlarge	d2.4xlarge	d2.8xlarge
g2.2xlarge	g2.8xlarge
```

Version History
-------------------------------------------------------------------------------


v2016.04

 * Updated to latest version Bitfusion Boost 0.1.0+1562
 * Updated to latest nvidia driver 352.99
 * Updated CuDNN to 5.1
 * Added gpustat


v0.03

 * Upgraded to boost 0.1.0+1518
 * Upgraded to cudnn 5
 * Upgraded to latest cuda 7.5 debian package


v0.02

 * Added CFN Bootstrap.  Required for cloudformation based boost clusters.


v0.01

 * Nvidia Driver Version 352.79
 * Cuda Toolkit Version 7.5
 * Nvidia cuDNN Version 4
 * bfboost 0.1.0+1402




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io

