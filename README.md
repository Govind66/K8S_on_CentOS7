# K8S_on_CentOS7

## Installation Of K8s Dashboard(AWS)

### Step 1: 
The two-node cluster that we will be forming in this article will consist of a
Master node(T2.Medium instance) and a Slave node(T2.micro instance). Both these nodes need to
have Kubernetes installed on them. Therefore, follow the steps described below to install
Kubernetes on both the Ubuntu nodes.

* Update the OS
 ```
 $sudo yum update -y       
 ```
 * Add the docker repo in yum.repod
 ```
 sudo yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo     
 ```
 * enables the nightly repository.
 ```
  $sudo yum-config-manager --enable docker-ce-nightly      
  ```
 * Install the Docker utility on both the nodes by
   running the following command as sudo in the Terminal
   of each node.
   ```
   $sudo yum install docker-ce docker-ce-cli containerd.io
   ```
   * Check the docker version.
   ```
   $docker --version
   ```
