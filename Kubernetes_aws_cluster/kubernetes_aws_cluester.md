<!-- Using Aws_Linux  with 3 nodes 1 as master named 'master' and 2 child nodes names 'node1' and 'node2'-->
sudo su
yum install docker
systemctl start docker
 
<!-- Install Kubernetes from https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/  -->
<!-- Also check -->
yum repolist
kubeadm init 
<!-- After running this command we will see -> To start using cluster you need to run the following as a regular user  -->
<!-- We have to copy this and paste it in command prompt of master node-->
<!-- and one more command we will get -> Alternatively, if you are root user u can run -->
<!-- We will also copy this to the terminal of same master node-->
<!-- Now we will find -> Then you can join any number of nodes by running the following on each root  -->
<!-- Copy this to worker nodes terminal -->


<!-- Then for communication between pods we need to install calico for kubernetes -->