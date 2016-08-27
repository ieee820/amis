-------------------------------------------------------------------------------

Bitfusion Boost Ubuntu 14 Cuda 7 AMI Readme

-------------------------------------------------------------------------------

To get started, launch an AWS instances using this AMI from the EC2
Console. If you are not familiar with this process please review the AWS
documentation provided here:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html

Accessing the instance via SSH:

ssh -i <path to your pem file> ubuntu@<Launched Instance Public DNS>

Once you have logged in into the instance you are instantly access the bfboost
client.

-------------------------------------------------------------------------------

Cuda Example - deviceQuery, nvida-smi

-------------------------------------------------------------------------------

Test nvidia driver functionality with the following commands:

  /usr/local/cuda-7.0/samples/bin/x86_64/linux/release/deviceQuery

  nvidia-smi


-------------------------------------------------------------------------------

Local Boost CPU Example

-------------------------------------------------------------------------------

Running Octave with and without Bitfusion Boost locally.

Install Octave (this will take a few minutes):

  sudo apt-get install octave
  
Create a simple octave script

  vi sgemm.m
  
Enter the following code and save the file:

  N = 2048;
  A = single(rand(N,N));
  B = single(rand(N,N));
  start = clock();
  C = A * B;
  elapsedTime = etime(clock(), start);
  disp(elapsedTime);
  gFlops = 2*N*N*N/(elapsedTime * 1e+9);
  disp(gFlops);

Run without Bitfusion Boost: 

  octave ./sgemm.m
	
Run with Bitfusion Boost:

  bfboost run "octave ./sgemm.m"
  
Each run will produce two sets of numbers. The top number is the elapsed time 
and the bottom number is the GFLOPs. When you run with bfboost you should see 
significantly lower run times and higher GFLOPs, which indicate better performance.


-------------------------------------------------------------------------------

Client Configuration for Remote GPU Acceleration

-------------------------------------------------------------------------------

While you can utilize the Boost Client AMI in a standalone configuration on a
CPU based instance, in order to utilize GPUs you need to connect to a
Bitfusion Boost Server Cluster. The easiest way to deploy this configuration
is  via our Cloudformation template for AWS which can be accessed using this
link:

  http://amzn.to/1RpgWvu

Alternatively you can start the GPU server instance manually using the
the Bitfusion Boost Server GPU AMI:

  https://aws.amazon.com/marketplace/pp/B01BHX6X36

For the manual configuration you need to specify the private IP address of the
server in the Bitfusion Boost configuration file:

 /etc/bitfusionio/adaptor.conf

Please specify one server IP address per line. You can specify multiple Bitfusion
Boost (GPU) servers to potentially obtain increased performance.

Please refer to the following documentation for detailed AWS instructions.

 https://bitfusionio.readme.io/docs/boost-on-aws
  

-------------------------------------------------------------------------------

Remote GPU Query with Bitfusion Boost Server

-------------------------------------------------------------------------------

Execute the following command:

  bfboost client /usr/local/cuda-7.0/samples/bin/x86_64/linux/release/deviceQuery

This will display properties of the CUDA devices present on the remote system. You 
can execute any GPU application using the bfboost command

  bfboost client <command>


-------------------------------------------------------------------------------

Supported AWS Instances

-------------------------------------------------------------------------------

t2.medium	t2.large
m3.medium	m3.large	m3.xlarge	m3.2xlarge
m4.large	m4.xlarge	m4.2xlarge	m4.4xlarge	m4.10xlarge
c3.large	c3.xlarge	c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large	c4.xlarge	c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large	r3.xlarge	r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge	i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge	d2.2xlarge	d2.4xlarge	d2.8xlarge
g2.2xlarge	g2.8xlarge


-------------------------------------------------------------------------------

Version History

-------------------------------------------------------------------------------


v0.03

Added CFN Boostrap.  Required for cloudformation based boost clusters.


v0.02

Updated bfboost to version 0.1.0+1402
Boost performance improvements
Simpler upstart process for boost servers
nvcc added to execution path


v0.01

Nvidia Driver Version:  346.47
Cuda Toolkit Version:   7.0.27
bfboost:                0.1.0+1190



-------------------------------------------------------------------------------

Support

-------------------------------------------------------------------------------

Please send all comments and support request to amisupport@bitfusion.io
