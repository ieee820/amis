
Bitfusion Boost Ubuntu 14 Caffe AMI Readme
==============================================================================

AMI
-------------------------------------------------------------------------------

<p align="center">
<a href="http://https://aws.amazon.com/marketplace/pp/B01B52CMSO">Launch the AMI</a>
</p>

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

PyCaffe
-------------------------------------------------------------------------------

We have compiled and installed PyCaffe for both CPUs and GPUs on this AMI.  By
default the PYTHONPATH is set to the GPU version.  If you want to make use of
the CPU compiled module set the PYTHONPATH with the following command:

```
  export PYTHONPATH=/opt/caffe-cpu/python:$PYTHONPATH
```

To set it back to the GPU module:

```
   export PYTHONPATH=/opt/caffe-gpu/python:$PYTHONPATH
```

#### PyCaffe Import Warning:

    During an import you may see the following warning:
```
   /opt/caffe-cpu/python/caffe/pycaffe.py:13: RuntimeWarning: to-Python converter for boost::shared_ptr<caffe::Net<float> > already registered; second conversion method ignored.
     from ._caffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, \
   /opt/caffe-cpu/python/caffe/pycaffe.py:13: RuntimeWarning: to-Python converter for boost::shared_ptr<caffe::Blob<float> > already registered; second conversion method ignored.
     from ._caffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, \
   /opt/caffe-cpu/python/caffe/pycaffe.py:13: RuntimeWarning: to-Python converter for boost::shared_ptr<caffe::Solver<float> > already registered; second conversion method ignored.
     from ._caffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, \
```

This is a harmless warning due to differences in boost versions, more info
here:

 * https://groups.google.com/forum/#!topic/caffe-users/C_air48cISU/discussion


Local Caffe CPU Example
-------------------------------------------------------------------------------

We have pre-installed the Caffe CPU version and examples in the following
folder:

```
  /opt/caffe-cpu
```

##### Example - Training LeNet on MNIST with Caffe:

Readme: /opt/caffe-cpu/examples/mnist/readme.md

You will first need to download and convert the data format from the MNIST
website. To do this, run the following commands:

```
  cd /opt/caffe-cpu
  ./data/mnist/get_mnist.sh
  ./examples/mnist/create_mnist.sh
```

Ensure the solver is set to CPU mode by editing the following file:

```
  vi examples/mnist/lenet_solver.prototxt
```

And make sure the solver line is as follows:

```
   solver_mode: CPU
```
To run the test:

```
    ./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt
```

Local Caffe GPU Example
-------------------------------------------------------------------------------

We have pre-installed the Caffe GPU version and examples in the following
folder:

```
  /opt/caffe-gpu
```

#### Example - Training LeNet on MNIST with Caffe:

Readme: /opt/caffe-gpu/examples/mnist/readme.md

You will first need to download and convert the data format from the MNIST
website. To do this, run the following commands:

```
  cd /opt/caffe-gpu
  ./data/mnist/get_mnist.sh
  ./examples/mnist/create_mnist.sh
```

Ensure the solver is set to GPU mode by editing the following file:

```
  vi examples/mnist/lenet_solver.prototxt
```

And make sure the solver line is as follows:

```
  solver_mode: GPU
```

To run the test:

```
  ./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt -gpu all
```

Bitfusion Boost Caffe GPU Cluster
-------------------------------------------------------------------------------

To start this AMI in a Boost Cluster formation select one of the cluster
configuration for this AMI from the AWS Marketplace:

https://aws.amazon.com/marketplace/pp/B01B52CMSO

Once the Boost Cluster is up and running you can utilize Boost to execute your
application across all the nodes in the cluster as follows:

```
  bfboost client <command>
```

For example you can execute device query as follows:

```
  bfboost client  /usr/local/cuda/samples/bin/x86_64/linux/release/deviceQuery
```

Example - Training LeNet on MNIST with Caffe:

Readme: /opt/caffe-gpu/examples/mnist/readme.md

You will first need to download and convert the data format from the MNIST
website. To do this, run the following commands:

```
  cd /opt/caffe-gpu

  ./data/mnist/get_mnist.sh
  ./examples/mnist/create_mnist.sh
```

To run the test:

```
  bfboost client "./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt -gpu all"
```

Detailed Boost and Boost Cluster documentaion is available at:

  http://www.bitfusion.io/documentation-boost-aws/


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


v0.05

 * Upgrade to cudnn 5
 * Upgrade to nvidia driver 352
 * Upgrade to cuda 7.5 toolkit
 * Upgrade to the latest version of boost 0.1.0+1518
 * Upgrade to the latest version of caffe


v0.04

 * Added CFN Bootstrap.  Required for cloudformation based boost clusters.


v0.03

 * Updated bfboost to version 0.1.0+1402, performance improvements
 * simpler upstart process for servers
 * Jupyter support
 * Python OpenCV support
 * nvcc added to execution path
 * {'Bugfix': 'odd number GPUs - merged https://github.com/BVLC/caffe/pull/3586'}
 * {'Bugfix': 'pycaffe gpu support'}


v0.02

 * Added pycaffe support


v0.01

 * Nvidia Driver Version 346.46
 * Cuda Toolkit Version 7.0.27
 * Nvidia cuDNN Version 3
 * bfboost 0.1.0+1190




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.iouest to support@bitfusion.io
