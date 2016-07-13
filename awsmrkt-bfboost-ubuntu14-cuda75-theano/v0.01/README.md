
Bitfusion Boost Ubuntu 14 Theano AMI Readme
==============================================================================


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

Theano
-------------------------------------------------------------------------------

From the Theano Documentation:

> Theano is a Python library that allows you to define, optimize, and evaluate mathematical expressions involving multi-dimensional arrays efficiently. Theano features:

> * tight integration with NumPy – Use numpy.ndarray in Theano-compiled functions.
> * transparent use of a GPU – Perform data-intensive calculations up to 140x faster than with CPU.(float32 only)
> * efficient symbolic differentiation – Theano does your derivatives for function with one or many inputs.
> * speed and stability optimizations – Get the right answer for log(1+x) even when x is really tiny.
> * dynamic C code generation – Evaluate expressions faster.
> * extensive unit-testing and self-verification – Detect and diagnose many types of errors.

> Theano has been powering large-scale computationally intensive scientific
> investigations since 2007. But it is also approachable enough to be used in the
> classroom (University of Montreal’s deep learning/machine learning classes).

#### Theano - Tutorials & References

* Official: http://deeplearning.net/software/theano/tutorial/index.html
* http://www.marekrei.com/blog/theano-tutorial/
* http://ir.hit.edu.cn/~jguo/docs/notes/a_simple_tutorial_on_theano.pdf
* http://outlace.com/Beginner-Tutorial-Theano/

#### Theano - Minimal Examples

#### Example 1

The example is taken from http://www.marekrei.com/blog/theano-tutorial/

```
  import theano
  import numpy

  x = theano.tensor.fvector('x')
  W = theano.shared(numpy.asarray([0.2, 0.7]), 'W')
  y = (x * W).sum()

  f = theano.function([x], y)

  output = f([1.0, 1.0])
  print output
```

##### Example 2

This example is taken from: http://deeplearning.net/software/theano/tutorial/adding.html

```
  import numpy
  import theano.tensor as T
  x = T.dscalar('x')
  y = T.dscalar('y')
  z = x + y
  numpy.allclose(z.eval({x : 16.3, y : 12.1}), 28.4)
```

#### Theano - Verifying GPU Usage

If you are an AWS G2 EC2 instance we have included a test script to
confirm that Theano is using the GPU.  The script comes from the Theano
documentation:

  http://deeplearning.net/software/theano/tutorial/using_gpu.html


To verify GPU Usage:


```
   python2 theano_gpu_check.py
```



Keras
-------------------------------------------------------------------------------

This AMI has Keras set to use Theano as it's backend.   Keras is compatible
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


#### Keras - Getting Started:

Getting started: 30 seconds to Keras:

  http://keras.io/#getting-started-30-seconds-to-keras

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


v0.01

 * Nvidia Driver Version  352
 * Cuda Toolkit Version   7.5
 * Nvidia cuDNN Version   5
 * theano Version         0.82
 * bfboost                0.1.0+1532




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.iouest to support@bitfusion.io

