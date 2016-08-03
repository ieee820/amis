Bitfusion Ubuntu 14 Scientific Computing AMI
==============================================================================


### Intro

This AMI is built to support the scientific computing ecosystem
(Mathematics, science, engineering, etc.) and comes with the following tools:

  * Python 2 with the following modules
    * Numpy 1.11.1
    * SciPy 0.18.0
    * SciKit-learn 0.17.1
    * Pandas 0.18.1
    * Sympy 1.0
    * Matplotlib 1.5.1
  * Python 3
  * Julia
  * Octave
  * R and RStudio Server (Web based IDE for R)
  * Jupyter with the following kernels:
    * Julia
    * Octave
    * R
    * Python 2
    * Python 3



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


R and RStudio Server - http://{ EC2 Instance Public IP }:8787
-------------------------------------------------------------------------------

#### Logging In To R Studio Server

You can access R Studio Server at http://{EC2 Instance Public IP}:8787 with
the following:

  * Username: ubuntu
  * The login PASSWORD is set to the Instance ID.

You can get the Instance ID (R studio Notebook Password) from the EC2 console by
clicking on the running instance, or if you are logged in via ssh you can obtain
it by executing the following command:

```
  ec2metadata --instance-id
```

#### Updating the R Studio Password:

**It is highly recommended that you change the R Studio Server password.**

When logged in via ssh you can update the password by running the following command:

```
  passwd
```

You will be prompted for the current password, the EC2 instance ID, followed
by a prompt for you new password.

Example below:
```
  $ passwd
  Changing password for ubuntu.
  (current) UNIX password:
  Enter new UNIX password:
  Retype new UNIX password:
  passwd: password updated successfully
```


  BLAS
-------------------------------------------------------------------------------

We have compiled and installed the latest version of OpenBLAS, and set it as the 
default BLAS library for all the installed to for oprimal performance. To change
the default BLAS library please follow the directoions below.

#### Changing the default BLAS library

You can see the available BLAS libraries and change the default library with the
following command:

```
$ sudo update-alternatives --config libblas.so.3

There are 3 choices for the alternative libblas.so.3 (providing /usr/lib/libblas.so.3).

  Selection    Path                                 Priority   Status
------------------------------------------------------------
* 0            /opt/openblas/lib/libopenblas.so.0    50        auto mode
  1            /opt/openblas/lib/libopenblas.so.0    50        manual mode
  2            /usr/lib/libblas/libblas.so.3         10        manual mode
  3            /usr/lib/openblas-base/libblas.so.3   40        manual mode

Press enter to keep the current choice[*], or type selection number:
```

##### R Benchmarks

Download the R Benchmark

```
wget http://r.research.att.com/benchmarks/R-benchmark-25.R
```

Run the R Benchmark:

```
cat R-benchmark-25.R | time R --slave
```

R Benchmark Results: m4.4xlarge (16vCPU: 8 cores - dual threaded)

Default BLAS:						178s  
Optimized BLAS:    71s


##### Octave Benchmarks

Octave Script - Save it as octave_benchmark.m

```
M = 256*256; N = 1048; K = 1048;
A = rand(K,M,"single"); B = rand(K,N,"single");

# Count total elapsed time
tic

#Assume matrices are stored columnwise
A = A / diag(norm(A,"columns")); B = B / diag(norm(B,"columns"));

# Time sgemm only
start = clock();
C = transpose(B)*A;
elapsedTime = etime(clock(), start);

# Find maximum value in each column and store in a vector
SAM=max(C);
toc
disp(elapsedTime);
gFlops = 2*M*N*K/(elapsedTime * 1e+9);
disp(gFlops);
```

Run the Octave script:

```
octave octave_example.m
```

Octave Benchmark Results: m4.4xlarge (16vCPU: 8 cores - dual threaded)

Default BLAS:						52 GFLOPS  
Optimized BLAS:			332 GFLOPS


Scientific Computing Tutorials:
-------------------------------------------------------------------------------

  * https://web.stanford.edu/~arbenson/cme193.html
  * http://www.scipy-lectures.org/


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

 * R 3.3.1
 * Octave 4.1.1
 * Julia 0.4.6
 * RStudion Server 0.99.903
 * Jupyter 4.1.0 (with R, Octave, Julia, Py2 and Py3 Kernels)
 * Numpy 1.11.1
 * SciPy 0.18.0
 * SciKit-learn 0.17.1
 * Pandas 0.18.1
 * sympy 1.0
 * matplotlib 1.5.1
 * OpenBlas 0.2.18




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
