AWS Security Groups vs Kubernetes NetworkPolicy vs NACL:
===========================================================

* Today we'll understand an important production topic for an AWS DevOps Engineer:

* How do AWS and Kubernetes control network traffic in an EKS cluster?

Imagine you have an EKS cluster with:

    - Frontend Pod
    - Backend Pod
    - Database Pod
    - Worker nodes running on EC2 instances

* You want frontend to access backend, backend to access database, and unwanted traffic to be blocked.

There are three important security mechanisms to understand.

Kubernetes NetworkPolicy:
-------------------------

* NetworkPolicy controls communication between Pods, based on rules such as Pod labels, namespace labels, IP ranges and ports.

Example:

    - Allow frontend Pod to access backend Pod on port 8080.
    - Block other unwanted Pods from accessing backend.

    spec:
      podSelector:
        matchLabels:
          app: backend
      policyTypes:
        - Ingress
      ingress:
        - from:
            - podSelector:
                matchLabels:
                  app: frontend
          ports:
            - protocol: TCP
              port: 8080


Remember: NetworkPolicy enforcement requires a network plugin that supports it. A policy object alone does not guarantee traffic filtering.

AWS Security Groups (SG):
-------------------------

* A Security Group is a virtual firewall associated with AWS network interfaces (ENIs), commonly attached to EC2 worker nodes.

It controls:

    - Inbound traffic — traffic coming into the ENI.
    - Outbound traffic — traffic leaving the ENI.
Example:

    Suppose your worker node's Security Group allows inbound TCP traffic on port 8080 from a trusted source.

* That Security Group rule controls traffic reaching the protected network interface. It does not select Kubernetes Pods using labels.

Important characteristics:

    - Security Groups are stateful: return traffic for an allowed connection is automatically permitted.
    - Security Groups support allow rules, not explicit deny rules.
    - In a standard EKS VPC CNI setup, Pod traffic commonly uses the node's network interfaces and is subject to the applicable AWS network controls.
    - EKS also supports Security Groups for Pods in supported configurations, allowing security groups to be associated with Pod networking.


AWS Network ACL (NACL):
-----------------------

* A Network ACL is a firewall associated with a VPC subnet.

* It controls traffic entering and leaving that subnet.

For example:

    - Allow inbound TCP port 8080.
    - Deny traffic from a particular IP range.
    - Allow outbound traffic on selected ports.
Important characteristics:

    - NACLs are stateless.
    - You must consider both inbound and outbound rules.
    - Rules are evaluated in ascending rule-number order; the first matching rule determines the result.
    - NACLs support both allow and deny rules.
<img width="1032" height="446" alt="image" src="https://github.com/user-attachments/assets/fcf97500-dca0-4bcd-868a-88660caabd0c" />

<img width="957" height="455" alt="image" src="https://github.com/user-attachments/assets/1888e6a9-9812-4f86-8b01-7ae64c256ef4" />

Production scenario — Frontend cannot access Backend:
-----------------------------------------------------

Imagine:

    - Frontend Pod: 10.0.1.10
    - Backend Pod: 10.0.2.20
    - Backend listens on TCP port 8080.
    - Both Pods run on different worker nodes.
Frontend tries:

    curl -v http://10.0.2.20:8080

But the connection times out.

How should you troubleshoot?

<img width="990" height="567" alt="image" src="https://github.com/user-attachments/assets/972da61b-4478-40d3-ae40-493462eb94b6" />

<img width="1041" height="833" alt="image" src="https://github.com/user-attachments/assets/531d4785-a492-4907-b33b-e7ce2aec2af4" />

