Day 5 — AWS VPC CNI and Pod IP Allocation
==========================================

Today we'll learn how an EKS Pod gets its IP address, 
how AWS network interfaces are involved, and why running out of IP addresses can prevent Pods from starting.

* How does an EKS Pod get an IP address?

      In a typical Amazon EKS cluster using the AWS VPC CNI, Pods receive IP addresses from your VPC network.
  
<img width="967" height="530" alt="image" src="https://github.com/user-attachments/assets/8522ad06-dd2a-43b1-91fb-121a72968e74" />

The flow is:

      1. Kubernetes schedules a Pod onto a worker node.
      2. The AWS VPC CNI configures the Pod's network.
      3. It allocates an available IP address and sets up the necessary networking.
      4. The Pod can use that IP to communicate, subject to routing and network security rules.

* The VPC CNI is responsible for networking setup and IP allocation; Kubernetes scheduling alone does not allocate the Pod's VPC IP.

What is an ENI, and why does it matter?

* An ENI (Elastic Network Interface) is a virtual network interface attached to an EC2 instance. It has a private IP address and can have additional private IP addresses.

In the standard AWS VPC CNI setup:

      The worker node has a primary network interface.
      The CNI can use additional IP addresses on network interfaces for Pods.
      The number of network interfaces and IP addresses available depends on the EC2 instance type and CNI configuration.

* For example, imagine your worker node has only a few available Pod IP addresses. If those addresses are exhausted and the CNI cannot obtain more, new Pods may fail to start.

* One important detail: AWS VPC CNI can also use prefix delegation, where it assigns a block of IP addresses rather than allocating only individual secondary IPs. We'll explore that later.

What happens when Pod IP addresses run out?

* Imagine you deploy five replicas of an application, but your worker nodes cannot allocate IP addresses for two of them.

You might see:

      - Some Pods running successfully.
      - Other Pods stuck in ContainerCreating.
      - Events or CNI logs showing IP allocation or network setup failures.

* The exact symptom can vary, so don't assume every ContainerCreating problem is IP exhaustion.

Practical lab — inspect your EKS networking:

* Run these read-only commands if you have access to an EKS cluster.

Step 1: Check Pod IP addresses

      kubectl get pods -A -o wide

* Look at the IP and NODE columns. These show the assigned Pod IP and the worker node hosting each Pod.

Step 2: Inspect worker node details

      kubectl get nodes -o wide
      kubectl describe node <node-name>

* Review the node's conditions, allocated resources, and events. These help distinguish IP/network setup problems from other scheduling issues.

Step 3: Check the AWS VPC CNI Pods

      kubectl get pods -n kube-system -o wide
Look for Pods named aws-node. The AWS VPC CNI runs as a DaemonSet, normally with an agent on each applicable worker node.

To inspect its logs:

      kubectl logs -n kube-system <aws-node-pod-name> -c aws-node --tail=100

* Look for messages mentioning IP allocation, IPAM, or network setup failures.

* If you don't currently have an EKS cluster, you can still learn the concepts now and practise the commands later. Avoid changing CNI settings during this lesson.

  
