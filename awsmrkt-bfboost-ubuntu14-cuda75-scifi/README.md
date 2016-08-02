
Bitfusion Ubuntu 14 Scientific Computing AMI Readme
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


OpenBLAS
-------------------------------------------------------------------------------

Bitfusion has installed the latest version of OpenBLAS with dynamic ARCH
support.  By default we have set it as the default library to use.

#### Changing the default library

If you like to see available OpenBlAS libraries and change the default run
the following command:

```
$ update-alternatives --config libblas.so.3

There are 4 choices for the alternative libblas.so.3 (providing /usr/lib/libblas.so.3).

  Selection    Path                                     Priority   Status
------------------------------------------------------------
* 0            /opt/openblas/lib/libopenblas.so.0        50        auto mode
  1            /opt/openblas/lib/libopenblas.so.0        50        manual mode
  2            /usr/lib/atlas-base/atlas/libblas.so.3    35        manual mode
  3            /usr/lib/libblas/libblas.so.3             10        manual mode
  4            /usr/lib/openblas-base/libopenblas.so.0   40        manual mode

Press enter to keep the current choice[*], or type selection number:
```


# Openblas performance


Octave Script:
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


#### libblas3 (Default for Octave)

Switch to libblas3 (pick option 2):
```
sudo update-alternatives --config libblas.so.3
```

Run the test:
```
time /usr/bin/octave -qf octave_example.m
/usr/bin/octave -qf octave_example.m
octave: X11 DISPLAY environment variable not set
octave: disabling GUI features
Elapsed time is 54.1769 seconds.
 53.344
 2.6987

```

### openblas (Bitfusion compiled)

Switch to OpenBlas (pick option 0):
```
sudo update-alternatives --config libblas.so.3
```

Run the test:
```
/usr/bin/octave -qf octave_example.m
octave: X11 DISPLAY environment variable not set
octave: disabling GUI features
Elapsed time is 2.16559 seconds.
 1.3306
 108.19
```

#### Octave Benchmark Results
<div>
    <a href="https://plot.ly/~schhibber/4/" target="_blank" title="Octave Benchmark Runtime" style="display: block; text-align: center;"><img src="https://plot.ly/~schhibber/4.png" alt="Octave Benchmark Runtime" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="schhibber:4"  src="https://plot.ly/embed.js" async></script>
</div>


Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.nano     t2.micro    t2.medium   t2.large
m3.medium	  m3.large	  m3.xlarge   m3.2xlarge
m4.large	  m4.xlarge	  m4.2xlarge	m4.4xlarge	m4.10xlarge
c3.large	  c3.xlarge	  c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large	  c4.xlarge	  c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large	  r3.xlarge	  r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge	  i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge	  d2.2xlarge	d2.4xlarge	d2.8xlarge
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
 * Pandas 0.18.1
 * sympy 1.0
 * matplotlib 1.5.1




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
