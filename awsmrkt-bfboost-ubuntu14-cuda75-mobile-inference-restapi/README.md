```
    ###############################################################################
    ##                                                                           ##
    ## Find this AMI useful and think others may as well? Refer a colleague or a ##
    ##         friend and receive $250 in AWS infrastructure credits.            ##
    ##                                                                           ##
    ##              http://www.bitfusion.io/aws-product-referral/                ##
    ##                                                                           ##
    ###############################################################################
```

Bitfusion Ubuntu 14 Deep Learning Service AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01GSSXSV0)


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

GPU REST ENGINE API
===============================================================================
The AMI leverages NVIDIA's GPU rest engine: https://github.com/NVIDIA/gpu-rest-engine
and is designed to run on the following GPU enabled EC2 instances:

  * g2.2xlarge
  * g2.8xlarge

GPUs are efficient parallel processors that can deliver low-latency response
times for services responding to arbitrary incoming requests.

The NVIDIA GPU REST Engine (GRE) is a critical component for developers
building low-latency web services. GRE includes a multi-threaded HTTP server
that presents a RESTful web service and schedules requests efficiently across
multiple NVIDIA GPUs. The overall response time depends on how much processing
you need to do, but GRE itself adds very little overhead and can process
null-requests in as little as 10 microseconds.

Several trained example models are provided on the system and can be queried upon boot.
There are multiple instances of the GPU rest engine running, each on their own port and
with a different model.

 * CaffeNet port 8000
 * AlextNet port 8001
 * GoogleNet port 8002

### Example Curl Calls

> Currently the inference api only supports JPG images.  You can easily convert any images from from one format to another using ImageMagick or [Bitfusion's image manipulation service API](https://github.com/bitfusionio/imagemagick_api])

Using a simple curl call we will submit a picture of an Anogra rabbit
to each of the API services. You can see that the results from each model provides slightly
different confidence levels and results.  The results have been piped into JQ for readability.

#### CaffeNet
```
$ host="EC2 instance IP"
$ curl \
   --verbose \
  -X POST \
   --data-binary @angorarabbit.jpg \
   http://${host}:8000/api/classify | jq

# Concatenated output.....

[
  {
    "confidence": 0.9999,
    "label": "n02328150 Angora, Angora rabbit"
  },
  {
    "confidence": 0,
    "label": "n02123394 Persian cat"
  },
  {
    "confidence": 0,
    "label": "n02111889 Samoyed, Samoyede"
  },
  {
    "confidence": 0,
    "label": "n02326432 hare"
  },
  {
    "confidence": 0,
    "label": "n02325366 wood rabbit, cottontail, cottontail rabbit"
  }
]
```

#### AlexNet
```
$ host="EC2 instance IP"
$ curl \
   --verbose \
  -X POST \
   --data-binary @angorarabbit.jpg \
   http://${host}:8001/api/classify | jq

# Concatenated output.....


[
  {
    "confidence": 0.9993,
    "label": "n02328150 Angora, Angora rabbit"
  },
  {
    "confidence": 0.0005,
    "label": "n02123394 Persian cat"
  },
  {
    "confidence": 0.0001,
    "label": "n01877812 wallaby, brush kangaroo"
  },
  {
    "confidence": 0.0001,
    "label": "n02111889 Samoyed, Samoyede"
  },
  {
    "confidence": 0,
    "label": "n02325366 wood rabbit, cottontail, cottontail rabbit"
  }
]

```

#### GoogleNet
```
$ host="EC2 instance IP"
$ curl \
   --verbose \
  -X POST \
   --data-binary @angorarabbit.jpg \
   http://${host}:8002/api/classify | jq

# Concatenated output.....

[
  {
    "confidence": 1,
    "label": "n02328150 Angora, Angora rabbit"
  },
  {
    "confidence": 0,
    "label": "n02111889 Samoyed, Samoyede"
  },
  {
    "confidence": 0,
    "label": "n02123394 Persian cat"
  },
  {
    "confidence": 0,
    "label": "n02326432 hare"
  },
  {
    "confidence": 0,
    "label": "n02112018 Pomeranian"
  }
]
```

### Stoping and Starting the Service

Starting the GPU Rest Engine Workers
```
$ sudo service gre-workers start
$ docker ps -a

CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
8ed8bfd1a951        bitfusion/gpurestengine   "inference caffe/mode"   41 hours ago        Up 2 hours          0.0.0.0:8002->8000/tcp   gpurestengine-googlenet
c16e9edd7dc4        bitfusion/gpurestengine   "inference caffe/mode"   41 hours ago        Up 2 hours          0.0.0.0:8001->8000/tcp   gpurestengine-alexnet
6eee880622a4        bitfusion/gpurestengine   "inference caffe/mode"   41 hours ago        Up 2 hours          0.0.0.0:8000->8000/tcp   gpurestengine-caffenet
```

Stopping the GPU Rest Engine Workers
```
$ sudo service gre-workers start
$ docker ps -a
docker ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS                     PORTS               NAMES
8ed8bfd1a951        bitfusion/gpurestengine   "inference caffe/mode"   41 hours ago        Exited (2) 8 seconds ago                       gpurestengine-googlenet
c16e9edd7dc4        bitfusion/gpurestengine   "inference caffe/mode"   41 hours ago        Exited (2) 8 seconds ago                       gpurestengine-alexnet
6eee880622a4        bitfusion/gpurestengine   "inference caffe/mode"   41 hours ago        Exited (2) 9 seconds ago                       gpurestengine-caffenet
```


### Training your own model

You can use this AMI to train your own Caffe model as well. If you have trained
your own model simply create a container that points to your model collateral.


#### Create your container

```
export IMAGE='bitfusion/gpurestengine'
CONTAINER_NAME='gpurestengine-custom-model'
nvidia-docker run --name=${CONTAINER_NAME} -p 8000:8000 -d ${IMAGE} \
  inference "${CUSTOM_DEPLOY_PROTEXT}" "${CUSTOM_MODEL}" "${CUSTOM_MEAN}" "${CUSTOM_SYNSET_WORDS}"

```

#### Add the container name to list of container to be started

  1. Edit /etc/init/gre-workers.conf and append the container name to list of containers to be started

  ```
  sudo vim /etc/init/gre-workers.conf
  env CONTAINERS="gpurestengine-caffenet gpurestengine-alexnet gpurestengine-googlenet gpurestengine-custom-model"
  ```

  2. Restart the gre-workers service

  ```
  sudo service gre-workers stop
  sudo service gre-workers start
  ```

### Working with docker containers

#### What containers are running:

To view running containers or docker processes

```
$ docker ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
8ed8bfd1a951        bitfusion/gpurestengine   "inference caffe/mode"   40 hours ago        Up About an hour    0.0.0.0:8002->8000/tcp   gpurestengine-googlenet
c16e9edd7dc4        bitfusion/gpurestengine   "inference caffe/mode"   40 hours ago        Up About an hour    0.0.0.0:8001->8000/tcp   gpurestengine-alexnet
6eee880622a4        bitfusion/gpurestengine   "inference caffe/mode"   40 hours ago        Up About an hour    0.0.0.0:8000->8000/tcp   gpurestengine-caffenet
```

#### Making changes to the container:

Let's make a change to the "gpurestengine-caffenet" container:

```
# 1. Connect
$ docker exec -i -t gpurestengine-caffenet /bin/bash

# 2. Let's install vim and download a data set
root@ip-172-31-67-58:/#  apt-get update
root@ip-172-31-67-58:/#  apt-get install vim
root@ip-172-31-67-58:/#  wget http://vis-www.cs.umass.edu/lfw/lfw-deepfunneled.tgz
root@ip-172-31-67-58:/#  tar zxvf lfw-deepfunneled.tgz
root@ip-172-31-67-58:/#  exit

# 3 Commit your changes
$ docker commit 4378dd2a49ae bitfusion/gpurestengine
```

To verify your changes persisted, you can stop the container and start it back up with the following:

```
$ sudo stop gre name=gpurestengine-caffenet
$ sudo start gre name=gpurestengine-caffenet
$ docker exec -i -t gpurestengine-caffenet /bin/bash

# You are now in the container.  Let's check if the data we downloaded is still there.
root@ip-172-31-67-58:/#  ls -lah | grep lfw

drwxr-xr-x 5751 root root 204K Jun 22 21:59 lfw-deepfunneled
-rw-r--r--    1 root root 104M Aug 23  2013 lfw-deepfunneled.tgz

root@ip-172-31-67-58:/# exit
```

#### Training your own model with imagenet

To train your own model a good place to start is the following tutorial for training a model with imagenet: http://caffe.berkeleyvision.org/gathered/examples/imagenet.html


#### Training your own model with your own dataset:

To train on your own dataset you can follow the steps outlined here: https://github.com/BVLC/caffe/issues/550

### Starting the GPU Rest Engine manually

This would start the server with the bvlc reference caffe model trained with ilsvrc12 dataset and have the server
listening on port 8005
```
nvidia-docker run --name=gpurestengine -p 8005:8000 --rm \
  bitfusion/gpurestengine \
    inference \
      "caffe/models/bvlc_reference_caffenet/deploy.prototxt" \
      "caffe/models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel" \
      "caffe/data/ilsvrc12/imagenet_mean.binaryproto" \
      "caffe/data/ilsvrc12/synset_words.txt"
```



Supported AWS Instances
-------------------------------------------------------------------------------
```
g2.2xlarge	g2.8xlarge
```

Version History
-------------------------------------------------------------------------------


v2106.02

 * Bugfix - ensure containers start on reboot
 * Start multiple models at boot time


v0.01

 * Nvidia Driver Version 352
 * Cuda Toolkit Version 7.5
 * Nvidia cuDNN Version 4
 * bfboost 0.1.0+1402




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
