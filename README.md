
# Installation Of K8s Dashboard(AWS)

## Step 1: 
The two-node cluster that we will be forming in this article will consist of a
Master node(T2.Medium instance) and a Slave node(T2.micro instance). Both these nodes need to
have Kubernetes installed on them. Therefore, follow the steps described below to install
Kubernetes on both the Ubuntu nodes.

* Update the OS
   ```
   $sudo yum update -y       
   ```
### First you need to install Docker contair for orchestration purpose
   a) Add the docker repo in yum.repod \n
      ```
      sudo yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo     
      ```
   b) enables the nightly repository.
      ```
      $sudo yum-config-manager --enable docker-ce-nightly      
      ```
   c) Install the Docker utility on both the nodes by
      running the following command as sudo in the Terminal
      of each node.
      ```
      $sudo yum install docker-ce docker-ce-cli containerd.io
      ```
   d) Check the docker version.
      ```
      $docker --version
      ```
   e) Start Docker.
      ```
      $ sudo systemctl start docker
      ```
   f) Verify that Docker Engine is installed correctly by running the ```hello-world``` image.
      ```
      $ sudo docker run hello-world
      ```
      This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits. Docker Engine is installed and             running.     You need to use ```sudo``` to run Docker commands.
## Step2:
### K8S Deployment
* Prerequisits


