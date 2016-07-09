
{{ ami_title }} AMI Readme
==========================


To get started, launch an AWS instances using this AMI from the EC2
Console. If you are not familiar with this process please review the AWS
documentation provided here:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html

Accessing the instance via SSH:

```
ssh -i <path to your pem file> ubuntu@<EC2 Instance Public IP>
```

Once you have logged in into the instance you are instantly ready to launch any
Tensorflow scripts you may have.



Jupyter Notebook - http://{EC2 Instance Public IP}:8888
-------------------------------------------------------

### Logging In

You can login to the notebook at:

  http://{EC2 Instance Public IP}:8888

  The login PASSWORD is set to the Instance ID.

Use the following command to get the instance ID (Jupyter Notebook Password):

```
  ec2metadata --instance-id
    
```
    
### Updating the HASHED Password:

**It is highly recommended that you change the Jupyter login password.**
    
You can update the hashed password using the function notebook.auth.security.passwd():

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
  service ipython-notebook restart
```
    
### Notebook Location
    
The default notebook directory is /home/ubuntu/pynb.  This directory is
required for Jupyter to function.  If it is deleted you will need
recreate it and ensure it is owned by the ubuntu user.
