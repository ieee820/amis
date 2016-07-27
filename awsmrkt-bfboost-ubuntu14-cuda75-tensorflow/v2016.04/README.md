
Bitfusion Ubuntu 14 Tensorflow AMI Readme
==============================================================================

Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01EYKBEQ0)


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

TensorFlow
-------------------------------------------------------------------------------

Tensor flow and it's examples are installed within the ubuntu users home
directory (i.e /home/ubuntu).

This AMI has tensorflow installed with AWS GPU support. If a TensorFlow
operation has both CPU and GPU implementations, the GPU devices will be given
priority when the operation is assigned to a device. For example, matmul has
both CPU and GPU kernels. On a system with devices cpu:0 and gpu:0, gpu:0 will
be selected to run matmul.

When you run this AMI on a non-GPU instance you may see the following erros and
warning when running TensorFlow. These can be safely ignored, they simply indicate
that no GPU device was found on the system:
```
  E tensorflow/stream_executor/cuda/cuda_driver.cc:491] failed call to cuInit: CUDA_ERROR_NO_DEVICE
  I tensorflow/stream_executor/cuda/cuda_diagnostics.cc:140] kernel driver does not appear to be running on this host (ip-###-##-##-###): /proc/driver/nvidia/version does not exist
  I tensorflow/core/common_runtime/gpu/gpu_init.cc:81] No GPU devices available on machine.
```

#### TensorFlow Notebook Tutorials

The assignments for the Udacity Deep Learning class with TensorFlow have been
added the Jupyter notebook under the directory udacity.

  http://{ EC2 Instance Public IP }:8888/tree/udacity#

Course information can be found at:

  https://www.udacity.com/course/deep-learning--ud730

Github README can be found at:

  https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/udacity



#### Tensorflow CLI Examples

The TensorFlow examples can be executed either using python to use python 2 or
using the python3 command to utilize python 3.

##### Example 1: Import Python Module

```
  $ python
    >>> import tensorflow as tf
    I tensorflow/stream_executor/dso_loader.cc:105] successfully opened CUDA library libcublas.so.7.5 locally
    I tensorflow/stream_executor/dso_loader.cc:105] successfully opened CUDA library libcudnn.so.4 locally
    I tensorflow/stream_executor/dso_loader.cc:105] successfully opened CUDA library libcufft.so.7.5 locally
    I tensorflow/stream_executor/dso_loader.cc:105] successfully opened CUDA library libcuda.so.1 locally
    I tensorflow/stream_executor/dso_loader.cc:105] successfully opened CUDA library libcurand.so.7.5 locally
    >>>
    >>> hello = tf.constant('Hello, TensorFlow!')
    >>> sess = tf.Session()
    >>> sess.run(hello)
    Hello, TensorFlow!
    >>> a = tf.constant(10)
    >>> b = tf.constant(32)
    >>> sess.run(a+b)
    42
    >>>
```

##### Example 2: Trainning a CNN using MNIST dataset

```
    python ~/tensorflow/tensorflow/models/image/mnist/convolutional.py
```

##### Example 3:  Training a CNN the CIFAR-10 dataset

The tensorflow documentation has a great write up using this dataset:
https://www.tensorflow.org/versions/master/tutorials/deep_cnn/index.html

On a g2.2xlarge:

```
  python ~/tensorflow/tensorflow/models/image/cifar10/cifar10_multi_gpu_train.py
```

To view the GPU usage:

```
  watch -n0.1 nvidia-smi
```

On a g2.8xlarge:

By default the python script will use one GPU. To have the script use all
four GPUs you need to specify "--num_gpus=4" at the end of the command.

```
    python ~/tensorflow/tensorflow/models/image/cifar10/cifar10_multi_gpu_train.py --num_gpus=4
```

To view all the GPUs being used run nvidia-smi:

```
    watch -n0.1 nvidia-smi
```


TensorFlow Magenta
-------------------------------------------------------------------------------

This AMI also has Tensorflow Magenta bundled with it.  A library that uses
machine learning to create compelling art and music.

 * Github: https://github.com/tensorflow/magenta
 * Blog: http://magenta.tensorflow.org/

Keras
-------------------------------------------------------------------------------

This AMI has Keras set to use Tensorflow as it's backend.   Keras is compatible
with Python 2.7 to 3.5.

Keras is a minimalist, highly modular neural networks library, written in
Python and capable of running on top of either TensorFlow or Theano. It was
developed with a focus on enabling fast experimentation. Being able to go from
idea to result with the least possible delay is key to doing good research.

Use Keras if you need a deep learning library that:

 * allows for easy and fast prototyping (through total modularity, minimalism, and extensibility).
 * supports both convolutional networks and recurrent networks, as well as combinations of the two.
 * supports arbitrary connectivity schemes (including multi-input and multi-output training).
 * runs seamlessly on CPU and GPU.

You can read the documentation at http://keras.io/



#### Keras Tensorflow Tutorial:

Keras as a simplified interface to TensorFlow: tutorial, by Francois Chollet

  A complete guide to using Keras as part of a TensorFlow workflow:

    http://blog.keras.io/keras-as-a-simplified-interface-to-tensorflow-tutorial.htmlnds-to-keras


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


v2016.04

 * Python 3 matplotlib library added
 * Set Jupyter notebook default kernel to py2 to support udacity notebooks
 * New versioning
 * Disable warning in jupyter notebook


v0.03

 * Added Python 3 libraries for Tensorflow, Keras (1.0.5), Scikit-learn
 * Added Jupyter Python 3 kernel
 * Added Tensorflow Magenta
 * Updated scikit-learn


v0.02

 * Added Keras python library
 * Added Tensorflow Magenta
 * Upgrade to cudnn 5
 * Upgrade to the latest version of boost 0.1.0+1518
 * Upgrade to the latest version of Tensorflow (0.9)


v0.01

 * Nvidia Driver Version 352
 * Cuda Toolkit Version 7.5
 * Nvidia cuDNN Version 4
 * bfboost 0.1.0+1402




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
