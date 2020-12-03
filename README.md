
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
   * Add the docker repo in yum.repod \n
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
   * Start Docker.
      ```
      $ sudo systemctl start docker
      ```
   * Verify that Docker Engine is installed correctly by running the ```hello-world``` image.
      ```
      $ sudo docker run hello-world
      ```
      This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits. Docker Engine is installed and             running.     You need to use ```sudo``` to run Docker commands.
## Step2: K8S Deployment

### Prerequisits
* To disable swap memory on both the nodes as Kubernetes does not perform properly on a system that is using swap memory.
   ```
   sudo swapoff -a 
   ```

* On Master to give itʼs unique hostname.
   ```
   sudo hostnamectl set-hostname master-node 
   ```
* On Slave to give itʼs unique hostname.
   ```
   sudo hostnamectl set-hostname slave-node
   ```

   * Installing kubeadm
   * Letting iptables see bridged traffic
   ```
   cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
   net.bridge.bridge-nf-call-ip6tables = 1
   net.bridge.bridge-nf-call-iptables = 1
   EOF
   ```
   ```
   sudo sysctl --system
   ```
 ### K8S installation
 * Installing kubeadm, kubelet and kubectl
   ```
   cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
   [kubernetes]
   name=Kubernetes
   baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
   enabled=1
   gpgcheck=1
   repo_gpgcheck=1
   gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
   exclude=kubelet kubeadm kubectl
   EOF

* Set SELinux in permissive mode (effectively disabling it)
   ```
   sudo setenforce 0
   ```
   ```
   sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
   ```
   ```
   sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
   ```
   ```
   sudo systemctl enable --now kubelet
   ```
### Notes:
   * Setting SELinux in permissive mode by running setenforce 0 and sed ... effectively disables it. This is required to allow containers to access the host filesystem, which is      needed by pod networks for example. You have to do this until SELinux support is improved in the kubelet.

   * You can leave SELinux enabled if you know how to configure it but it may require settings that are not supported by kubeadm.
      The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.
      
 * Initialise kubernetes on master node.
   ```
   sudo kubeadm init --pod-network-cidr=172.31.0.0/16
   ```
 ### The output of above command will give this   
 ```
      To start using your cluster, you need to run the following as a regular user:

      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config

      You should now deploy a pod network to the cluster.
      Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
      https://kubernetes.io/docs/concepts/cluster-administration/addons/

      Then you can join any number of worker nodes by running the following on each as root:

      kubeadm join 172.31.38.20:6443 --token t1au9e.bcim6d05dw9wcz48 \
      --discovery-token-ca-cert-hash sha256:bb3e1779c680c52d5df975285362b5f1e4f1f3b08c751b9ab058af4a7759ff52
 ```
 ### you need execute above commands
 ```
 mkdir -p $HOME/.kube
 ```
 ```
 sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
 ```
 ```
 sudo chown $(id -u):$(id -g) $HOME/.kube/config
 ```
 ### Then you can join any number of worker nodes by running the following on each worker node as a root:
 ```
 kubeadm join 172.31.38.20:6443 --token t1au9e.bcim6d05dw9wcz48 \--discovery-token-ca-cert-hash sha256:bb3e1779c680c52d5df975285362b5f1e4f1f3b08c751b9ab058af4a7759ff52
 ```
 ### After Executing above
 * Deploy a pod network to the cluster on master node.
    ```
     kubectl apply -f https://docs.projectcalico.org/v3.10/manifests/calico.yaml
    ```
 * on Master Node - this will show total nodes connected to the Master.
   ```
   kubectl get nodes
   ```
 * On Master Node – This will show the pods.
   ```
   kubectl get pods --all-namespaces
   ```
   
 * Basic Command
 ```kubectl attach centos -c centos -i -t
kubectl run <name> --image=<required image name> -it 
kubectl get pods
kubectl get node
kubectl get all
kubectl run redhat --image=nginx --port=80
kubectl run --help
curl http://18.221.198.111:80
kubectl get ns
kubectl create ns hadoop
kubectl get pods -n hadoop
kubectl run ram --image=centos -it -n hadoop
```
 
 
 
      



