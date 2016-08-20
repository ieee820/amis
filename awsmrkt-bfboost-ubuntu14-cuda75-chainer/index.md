Bitfusion Ubuntu 14 Chainer AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01KKDQD84)


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

Chainer - http://chainer.org/
------------------------------------------------------------------------

For more information and a great tutorial please visit http://docs.chainer.org/


### Chainer Core Concept

Sited from the website:

```
Chainer is a flexible framework for neural networks. One major goal is
flexibility, so it must enable us to write complex architectures simply
and intuitively.

Most existing deep learning frameworks are based on the “Define-and-Run”
scheme. That is, first a network is defined and fixed, and then the user
periodically feeds it with mini-batches. Since the network is statically
defined before any forward/backward computation, all the logic must be
embedded into the network architecture as data. Consequently, defining a
network architecture in such systems (e.g. Caffe) follows a declarative
approach. Note that one can still produce such a static network
definition using imperative languages (e.g. torch.nn, Theano-based
frameworks, and TensorFlow).

In contrast, Chainer adopts a “Define-by-Run” scheme, i.e., the network
is defined on-the-fly via the actual forward computation. More
precisely, Chainer stores the history of computation instead of
programming logic. This strategy enables to fully leverage the power of
programming logic in Python. For example, Chainer does not need any
magic to introduce conditionals and loops into the network definitions.
The Define-by-Run scheme is the core concept of Chainer. We will show in
this tutorial how to define networks dynamically.
```

### Verify Chainer is installed
```
$ python
>>> import chainer
>>> chainer.__version__
'1.13.0'
>>>
```
 
 
### Verify Cuda is installed properly 

If correctly set up, nothing happens. Otherwise the following function raises RuntimeError
```
>>> import chainer.cuda
>>> chainer.cuda.check_cuda_available() 
>>>
```

### Chainer Examples

We have included the chainer examples in:
``` 
/home/ubuntu/chainer_examples
```


#### Example 1 - Training with a CPU:

```
  
  $ cd /home/ubuntu/chainer_examples/mnist
  $ ./train_mnist.py
  
```


#### Example 2 - Training with a GPU:

To highlight the GPU performance gains in our test runs, training with the GPU (g2.2xlarge)
took only 1 minute and 33 seconds compared to the CPU example above which took
10 minutes and 18 seconds.


```
  
  $ cd /home/ubuntu/chainer_examples/mnist  
  $ ./train_mnist.py --gpu=0
  
```

Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.nano     t2.micro    t2.medium   t2.large
m3.medium	m3.large	m3.xlarge	m3.2xlarge
m4.large	m4.xlarge	m4.2xlarge	m4.4xlarge	m4.10xlarge
c3.large	c3.xlarge	c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large	c4.xlarge	c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large	r3.xlarge	r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge	i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge	d2.2xlarge	d2.4xlarge	d2.8xlarge
g2.2xlarge	g2.8xlarge
x1.32xlarge
```

Version History
-------------------------------------------------------------------------------


v2016.01

 * NVIDIA cudnn 5
 * NVIDIA driver 352
 * NVIDIA cuda 7.5 toolkit
 * Chainer v1.13.0




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io

