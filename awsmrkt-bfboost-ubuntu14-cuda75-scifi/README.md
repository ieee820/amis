Bitfusion Ubuntu 14 Scientific Computing AMI
==============================================================================


#### Intro

This AMI is built primarly to support the scientific computing ecosystem
(Mathematics, science and engineering). Python 2 and Python 3 are both available,
**but key modules are currently in place for Python 2 only**.  Aside from the
python environment we have installed other popular tools:

  * Julia
  * Octave
  * R
  * R Studion Server (Web based IDE for R)
  * Jupyter with the following kernels:
    * Julia
    * Octave
    * R
    * Python 2
    * Python 3

#### Scientific Computing Tutorials:

  * https://web.stanford.edu/~arbenson/cme193.html
  * http://www.scipy-lectures.org/


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

You can see theavailable BlAS libraries and change the default with the
following command:

```
$ update-alternatives --config libblas.so.3

There are 3 choices for the alternative libblas.so.3 (providing /usr/lib/libblas.so.3).

  Selection    Path                                 Priority   Status
------------------------------------------------------------
* 0            /opt/openblas/lib/libopenblas.so.0    50        auto mode
  1            /opt/openblas/lib/libopenblas.so.0    50        manual mode
  2            /usr/lib/libblas/libblas.so.3         10        manual mode
  3            /usr/lib/openblas-base/libblas.so.3   40        manual mode

Press enter to keep the current choice[*], or type selection number:
```



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

#### R & BLAS Benchmarks

##### Download The Benchmark

```
wget http://r.research.att.com/benchmarks/R-benchmark-25.R
```

##### Using Libblas3

Swith the active BLAS library to libblas (pick option 2):

```
sudo update-alternatives --config libblas.so.3
```

Run the benchmark:

```
cat R-benchmark-25.R | time R --slave
```

Output:

```
    Loading required package: Matrix
    Loading required package: SuppDists
    Warning message:
    In library(package, lib.loc = lib.loc, character.only = TRUE, logical.return = TRUE,  :
      there is no package called ‘SuppDists’
    Warning messages:
    1: In remove("a", "b") : object 'a' not found
    2: In remove("a", "b") : object 'b' not found


       R Benchmark 2.5
       ===============
    Number of times each test is run__________________________:  3

       I. Matrix calculation
       ---------------------
    Creation, transp., deformation of a 2500x2500 matrix (sec):  0.826666666666667
    2400x2400 normal distributed random matrix ^1000____ (sec):  0.689333333333333
    Sorting of 7,000,000 random values__________________ (sec):  0.870666666666667
    2800x2800 cross-product matrix (b = a' * a)_________ (sec):  12.5423333333333
    Linear regr. over a 3000x3000 matrix (c = a \ b')___ (sec):  6.039
                          --------------------------------------------
                     Trimmed geom. mean (2 extremes eliminated):  1.63198360389782

       II. Matrix functions
       --------------------
    FFT over 2,400,000 random values____________________ (sec):  0.255666666666665
    Eigenvalues of a 640x640 random matrix______________ (sec):  0.932666666666667
    Determinant of a 2500x2500 random matrix____________ (sec):  4.04033333333334
    Cholesky decomposition of a 3000x3000 matrix________ (sec):  5.06866666666667
    Inverse of a 1600x1600 random matrix________________ (sec):  3.09833333333334
                          --------------------------------------------
                    Trimmed geom. mean (2 extremes eliminated):  2.26859653167043

       III. Programmation
       ------------------
    3,500,000 Fibonacci numbers calculation (vector calc)(sec):  0.680333333333332
    Creation of a 3000x3000 Hilbert matrix (matrix calc) (sec):  0.26800000000001
    Grand common divisors of 400,000 pairs (recursion)__ (sec):  0.442333333333342
    Creation of a 500x500 Toeplitz matrix (loops)_______ (sec):  0.404666666666676
    Escoufier's method on a 45x45 matrix (mixed)________ (sec):  0.456999999999994
                          --------------------------------------------
                    Trimmed geom. mean (2 extremes eliminated):  0.434097981814902


    Total time for all 15 tests_________________________ (sec):  36.616
    Overall mean (sum of I, II and III trimmed means/3)_ (sec):  1.17135069772039
                          --- End of test ---

    177.83user 0.83system 2:58.90elapsed 99%CPU (0avgtext+0avgdata 438428maxresident)k
    1680inputs+0outputs (9major+97155minor)pagefaults 0swaps
```

##### Using Openblas (Bitfusion compiled)

Swith the active back to using Openblas (pick option 0):

```
sudo update-alternatives --config libblas.so.3
```

```
cat R-benchmark-25.R | time R --slave
```

Output:
```
    Loading required package: Matrix
    Loading required package: SuppDists
    Warning message:
    In library(package, lib.loc = lib.loc, character.only = TRUE, logical.return = TRUE,  :
      there is no package called ‘SuppDists’
    Warning messages:
    1: In remove("a", "b") : object 'a' not found
    2: In remove("a", "b") : object 'b' not found


       R Benchmark 2.5
       ===============
    Number of times each test is run__________________________:  3

       I. Matrix calculation
       ---------------------
    Creation, transp., deformation of a 2500x2500 matrix (sec):  0.83
    2400x2400 normal distributed random matrix ^1000____ (sec):  0.689666666666667
    Sorting of 7,000,000 random values__________________ (sec):  0.869666666666667
    2800x2800 cross-product matrix (b = a' * a)_________ (sec):  0.734000000000001
    Linear regr. over a 3000x3000 matrix (c = a \ b')___ (sec):  0.409
                          --------------------------------------------
                     Trimmed geom. mean (2 extremes eliminated):  0.748981566820251

       II. Matrix functions
       --------------------
    FFT over 2,400,000 random values____________________ (sec):  0.271666666666667
    Eigenvalues of a 640x640 random matrix______________ (sec):  2.52433333333333
    Determinant of a 2500x2500 random matrix____________ (sec):  0.407333333333334
    Cholesky decomposition of a 3000x3000 matrix________ (sec):  0.411333333333334
    Inverse of a 1600x1600 random matrix________________ (sec):  0.38
                          --------------------------------------------
                    Trimmed geom. mean (2 extremes eliminated):  0.399309047890929

       III. Programmation
       ------------------
    3,500,000 Fibonacci numbers calculation (vector calc)(sec):  0.680333333333335
    Creation of a 3000x3000 Hilbert matrix (matrix calc) (sec):  0.275333333333331
    Grand common divisors of 400,000 pairs (recursion)__ (sec):  0.424666666666669
    Creation of a 500x500 Toeplitz matrix (loops)_______ (sec):  0.389333333333335
    Escoufier's method on a 45x45 matrix (mixed)________ (sec):  0.338999999999999
                          --------------------------------------------
                    Trimmed geom. mean (2 extremes eliminated):  0.382698259008479


    Total time for all 15 tests_________________________ (sec):  9.63566666666667
    Overall mean (sum of I, II and III trimmed means/3)_ (sec):  0.485525736392257
                          --- End of test ---

    62.53user 10.94system 0:55.07elapsed 133%CPU (0avgtext+0avgdata 451160maxresident)k
    4376inputs+0outputs (17major+99018minor)pagefaults 0swaps
```


##### Results

Using OpenBlas shows signifgent improvements due to it's ability to use multiple
cores:

Timings Results (shorter is better):

```
    libopenblas  55s
    libblas     178s
```


Octave & OpenBLAS Benchmarks
-------------------------------------------------------------------------------

#### Octave Script

Save it as octave_benchmark.m

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

Run the benchmark script:

```
time /usr/bin/octave -qf octave_benchmark.m
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

Run the benchmark script:

```
/usr/bin/octave -qf octave_example.m
octave: X11 DISPLAY environment variable not set
octave: disabling GUI features
Elapsed time is 2.16559 seconds.
 1.3306
 108.19
```

#### Octave Benchmark Results

Timings Results (lower is better):

```
  libopenblas  2.17s
  libblas     54.18s
```

<div>
    <a href="https://plot.ly/~schhibber/4/" target="_blank" title="Octave Benchmark Runtime" style="display: block; text-align: center;"><img src="https://plot.ly/~schhibber/4.png" alt="Octave Benchmark Runtime" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="schhibber:4"  src="https://plot.ly/embed.js" async></script>
</div>



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


Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
