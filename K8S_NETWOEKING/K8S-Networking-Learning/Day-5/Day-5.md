Day 5 — AWS VPC CNI and Pod IP Allocation
==========================================

Today we'll learn how an EKS Pod gets its IP address, 
how AWS network interfaces are involved, and why running out of IP addresses can prevent Pods from starting.

* How does an EKS Pod get an IP address?

      In a typical Amazon EKS cluster using the AWS VPC CNI, Pods receive IP addresses from your VPC network.
  
<img width="967" height="530" alt="image" src="https://github.com/user-attachments/assets/8522ad06-dd2a-43b1-91fb-121a72968e74" />
