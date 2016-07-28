
Bitfusion Ubuntu 14 Torch AMI Readme
==============================================================================

Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01B4ZSX5S)


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

Jupyter Notebook - http://{ EC2 Instance Public IP }:8888
-------------------------------------------------------------------------------

#### Logging In

You can login to the notebook at:

  * http://{EC2 Instance Public IP}:8888
  * The login PASSWORD is set to the Instance ID.

You can get the Instance ID (Jupyter Notebook Password) from the EC2 console by
clicking on the running instance, or if you are logged in via ssh you can obtain
it by executing the following command:

```
  ec2metadata --instance-id
```


#### Updating the HASHED Jupyter Login Password:

**It is highly recommended that you change the Jupyter login password.**

When logged in via ssh you can update the hashed password using the function
notebook.auth.security.passwd():

```
  ipython
  In [1]: from notebook.auth import passwd
  In [2]: passwd()
  Enter password:
  Verify password:
  Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
  exit()
```

You can then add the hashed password to your Jupyter config file, the default
location for this file is ~/.jupyter/jupyter_notebook_config.py

Example:

```
  c.NotebookApp.password = u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
```

To have the new password take effect restart the Jupyter:

```
  sudo service ipython-notebook restart
```


#### Notebook Location

The default notebook directory is /home/ubuntu/pynb.  This directory is
required for Jupyter to function.  If it is deleted you will need
recreate it and ensure it is owned by the ubuntu user.


Local Torch CPU & GPU Examples
-------------------------------------------------------------------------------

#### Example Data & Tests

We have pre-installed Torch examples & demos in the following folders:

 * /opt/torch/demos (https://github.com/torch/demos.git)
 * /home/ubuntu/cudnn.torch/test/ (https://github.com/soumith/cudnn.torch.git)

#### Example 1: CPU
```
  th /opt/torch/demos/profiling/conv-cpu.lua
```

#### Example 2: GPU example with cutorch/cunn
```
  th /opt/torch/demos/profiling/linear-gpu.lua
```

#### Example 3: GPU example using cuDNN
```
  th ./cudnn.torch/test/test.lua
```

Bitfusion Boost Torch GPU Cluster
-------------------------------------------------------------------------------

To start this AMI in a Boost Cluster formation select one of the cluster
configuration for this AMI from the AWS Marketplace:

https://aws.amazon.com/marketplace/pp/B01AT7S6CC

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

 * http://www.bitfusion.io/documentation-boost-aws/

#### Example Data & Tests

We have pre-installed Torch examples & demos in the following folders:

 * /opt/torch/demos (https://github.com/torch/demos.git)
 * /home/ubuntu/cudnn.torch/test/ (https://github.com/soumith/cudnn.torch.git)

#### Example 1: GPU example with cutorch/cunn

```
  bfboost client th /opt/torch/demos/profiling/linear-gpu.lua
```

#### Example 2: GPU example using cuDNN

```
  bfboost client th ./cudnn.torch/test/test.lua
```

Supported AWS Instances
-------------------------------------------------------------------------------

```
t2.nano      t2.micro      t2.medium     t2.large
m3.medium    m3.large      m3.xlarge     m3.2xlarge
m4.large     m4.xlarge     m4.2xlarge    m4.4xlarge    m4.10xlarge
c3.large     c3.xlarge     c3.2xlarge    c3.4xlarge    c3.8xlarge
c4.large     c4.xlarge     c4.2xlarge    c4.4xlarge    c4.8xlarge
r3.large     r3.xlarge     r3.2xlarge    r3.4xlarge    r3.8xlarge
i2.xlarge    i2.2xlarge    i2.4xlarge    i2.8xlarge
d2.xlarge    d2.2xlarge    d2.4xlarge    d2.8xlarge
g2.2xlarge   g2.8xlarge
x1.32xlarge
```

Version History
-------------------------------------------------------------------------------


v2016.6

 * Upgrade to cudnn 5
 * Upgrade to nvidia driver 352
 * Upgrade to cuda 7.5 toolkit
 * Upgrade to the latest version of boost 0.1.0+1532
 * Upgrade to the latest version of Torch and cudnn(R5) bindings


v0.05

 * Added CFN Bootstrap.  Required for cloudformation based boost clusters.


v0.04

 * Updated bfboost deb package to version 0.1.0+1402
 * Simplified upstart process for boost servers
 * Jupyter support (Python2 & iTorch)
 * nvcc added to execution path


v0.03

 * Simpler EULA acceptance


v0.02

 * Updated README document


v0.01

 * Nvidia Driver Version  346.46
 * Cuda Toolkit Version   7.0.27
 * Nvidia cuDNN Version   3
 * Torch Version          7
 * bfboost                0.1.0+1190




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
