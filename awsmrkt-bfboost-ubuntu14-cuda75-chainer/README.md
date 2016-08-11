Bitfusion Ubuntu 14 Chainer AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](http://www.bitfusion.io/boost-machine-images/)


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
 
 ```/home/ubuntu/chainer_examples```


#### Example 1 - Training with a CPU:

```
  # The command has been prefixed with "time" to
  # time how long it takes to train
  
  $ cd /home/ubuntu/chainer_examples/mnist
  
  # Training with CPU
  $ time ./train_mnist.py
  
    GPU: -1
    # unit: 1000
    # Minibatch-size: 100
    # epoch: 20
    
    epoch       main/loss   validation/main/loss  main/accuracy  validation/main/accuracy
    1           0.190537    0.0917396             0.943083       0.9717                    
    2           0.0756759   0.0681339             0.976483       0.979                     
    3           0.0480199   0.0716059             0.984517       0.977                     
    4           0.0342781   0.0763192             0.988633       0.9784                    
    5           0.0287253   0.0838866             0.99055        0.9761                    
    6           0.0237387   0.073706              0.9925         0.981                     
    7           0.0191159   0.106185              0.993583       0.9748                    
    8           0.0221227   0.0846464             0.992683       0.9801                    
    9           0.0158378   0.0945134             0.9947         0.9789                    
    10          0.0139317   0.0840632             0.995267       0.9818                    
    11          0.0157316   0.0852795             0.9952         0.9815                    
    12          0.00912568  0.113052              0.9971         0.9769                    
    13          0.0143123   0.0996677             0.99575        0.9805                    
    14          0.013235    0.0906042             0.996067       0.9836                    
    15          0.00871427  0.0945784             0.997017       0.9821                    
    16          0.0102738   0.114404              0.996767       0.9806                    
    17          0.0109605   0.111701              0.997          0.9812                    
    18          0.0102326   0.0942914             0.997017       0.9839                    
    19          0.00697239  0.107474              0.9979         0.982                     
    20          0.0108159   0.114549              0.997183       0.9819                    
    
    real	10m18.761s
    user	29m39.903s
    sys	50m26.241s

```


#### Example 2 - Training with a GPU:

This example shows how to train using GPUs and the performance benefits 
from using GPUs. The example using a GPU took 1 minute and 33 seconds as
where the CPU example took 10 minutes and 18 seconds

```
  
  $ cd /home/ubuntu/chainer_examples/mnist 
   
  # Training with GPU (G2.2XLARGE)
  $ time ./train_mnist.py --gpu=0
  
    GPU: 0
    # unit: 1000
    # Minibatch-size: 100
    # epoch: 20
    
    epoch       main/loss   validation/main/loss  main/accuracy  validation/main/accuracy
    1           0.195601    0.11153               0.940201       0.9631                    
    2           0.0733815   0.0896724             0.977049       0.973                     
    3           0.0479178   0.0927158             0.984466       0.9729                    
    4           0.0359405   0.0675299             0.988648       0.9803                    
    5           0.0252756   0.0804972             0.991898       0.979                     
    6           0.0255609   0.078852              0.991582       0.9798                    
    7           0.0179999   0.0736867             0.994049       0.9824                    
    8           0.02208     0.0670069             0.993182       0.9829                    
    9           0.016171    0.0783937             0.994965       0.9832                    
    10          0.01442     0.0890438             0.995332       0.9816                    
    11          0.0140226   0.0847233             0.995415       0.9811                    
    12          0.00881473  0.106724              0.997132       0.979                     
    13          0.0152661   0.104856              0.995232       0.9791                    
    14          0.0122119   0.0983878             0.996099       0.9829                    
    15          0.0144427   0.105973              0.995815       0.9801                    
    16          0.00671748  0.0985684             0.997566       0.9824                    
    17          0.00646931  0.132086              0.997832       0.9773                    
    18          0.0112794   0.113564              0.996849       0.9809                    
    19          0.00941462  0.104766              0.997215       0.9834                    
    20          0.0107395   0.103067              0.996782       0.9835                    
    
    real	1m33.237s
    user	1m31.589s
    sys	0m2.197s  
  
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

