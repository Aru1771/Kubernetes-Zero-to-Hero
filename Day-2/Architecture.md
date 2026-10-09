Kubernets Architecture divide into 2 parts:
============================================

1. Control Plain components
2. Data Palin Componenets

Before going to components concept we have understand one thing if you want to Run a container we need 
a container Run time without this run time our container will not Run.

In Docker we have Docker shim as Container run time.

Kubernets is adavnaced than Docker it will support most of the advanced Enterprise level concepts like
Auto scaling, Auto healing, load balancing, clustring with master and worker nodes.

in production we always multi master and multi worker structure.

in k8s our request will not directly go to worker node(Data plane). it will go through master node(control plane).

in k8s the lowest level of deployment is POD. Pod is just like a wraper to our container with advances features.






1. Control plain components:
   -------------------------

   1. API-server:
      ----------
      * it will acts as a core component in k8s.
      * it will decide pod creation.
      * it will take all incomming requests from out side from the cluster.
      * it will expose k8s to external world.
     
      * Eg: user is trying to create a Pod. he reaches API server with a command so this API server will decide which worker node is free to deploy this Pod. But to schedule that Pod on thet worker node we will use Scheduler
        
    2. Scheduler:
       ---------
        * it will responsible for scheduling a Pods and resources in K8S.

    3. ECTD:
       -----
       * It will acts like DB of our K8s cluster.
       * it will store in Key=value pair.
    
     4. Controller Manager:
        ------------------
        * Controller Manager is the brain that watches and fixes things inside the cluster.

        * It runs many controllers.

        📌 Think of it like a supervisor 👨‍💼

            Watches everything

            Takes action when something is wrong

      🔁 What does Controller Manager do?

          It continuously:

           Checks current state

           Compares with desired state

      Takes action to fix it
   🧩 Important Controllers inside Controller Manager
  🔹 Node Controller

       Checks node health

       If a node goes down → marks it NotReady

  🔹 ReplicaSet Controller

      Ensures correct number of pods

      Example:

      Desired: 3 pods

      One pod crashes → creates a new pod

   🔹 Deployment Controller

        Handles rolling updates

        Ensures zero downtime deployments

  🔹 Job / CronJob Controller

      Runs jobs

  5. Cloud Controller Manager
     ------------------------

     Cloud Controller Manager handles cloud-specific tasks only.

     It talks to:

         Cloud APIs

         Load balancers

         Volumes

          Cloud networking

  🧩 What Cloud Controller Manager controls?
      🔹 Node Controller (Cloud version)

           Detects when cloud VM is deleted

           Removes node from cluster

  🔹 Service Controller

       Creates cloud Load Balancer

   Example:

       type: LoadBalancer


  Automatically creates AWS ELB / Azure LB / GCP LB

   🔹 Route Controller

        Manages cloud routing rules

   🔹 Volume Controller

       Attaches / detaches cloud disks

       Example: EBS, Azure Disk, Persistent Disk

   🧠 Easy way to remember

   🧠 Controller Manager

    “Fixes Kubernetes things”

   ☁️ Cloud Controller Manager

       “Talks to the cloud”

   🏢 Enterprise understanding (simple)

      Controller Manager = core Kubernetes logic

      Cloud Controller Manager = cloud integration layer

  Separation improves:

    Portability

     Security

    Cloud neutrality


  🏁 One-line summary

     👉 Controller Manager keeps the cluster healthy
     👉 Cloud Controller Manager connects Kubernetes to the cloud



    
2 . Data plain components:
   ----------------------
   1. Kubelet:
      -------
      * kubelet is responsible component for maintianing the deployed pod in the worker node.
      * it will check the Pod is running or not. if it is not running it will tells K8s control plain.

   2. Container Runtime:
      ------------------
       * in k8s we will use "Docker shim", Container D, Cri-O or any other container runtimes which implement k8s container interface.
         
   3. Kube-Proxy:
       -----------
* it will provide networking, IP address to pods and load balacing in k8s. it will usese Ip tables in linux machine.
* kube-proxy acts as a data-plane controller that writes the routing rules on each node so that the Linux kernel handles the actual traffic routing.
* kube-proxy runs on every single node in your cluster. It constantly watches the Kubernetes API server for two things:
  
         1. When a new Service is created (and assigned a ClusterIP).
         2. When Pods (Endpoints) are added, removed, or change health status.
  
* The Rule Maker (Data Plane Role)
  
          When kube-proxy detects a change, it updates the low-level networking rules on its host node. Depending on your cluster configuration, it uses either:
         • IPVS (IP Virtual Server): L4 load balancing built into the Linux kernel (standard for modern/large clusters).
         • iptables: A Linux packet filtering system.

* It translates a high-level concept like "Send traffic for 10.96.0.1 to these 3 pods" into low-level firewall/routing rules.

* The Traffic Flow (What happens to your request)

         Because kube-proxy already configured the node's kernel rules ahead of time:
         1. Your application sends a packet to the Service IP (payments.default.svc.cluster.local).
         2. The packet hits the Linux kernel of the host node.
         3. The kernel looks at the iptables or IPVS rules created by kube-proxy.
         4. The kernel intercepts the packet, changes the destination IP from the Service IP to one of the healthy Pod IPs (performing DNAT / Destination Network               Address Translation), and load-balances the request.
         5. The packet goes directly to the Pod
               

   
      
Scenario 2: Pod-to-Service Communication
            
When a Pod behind Service 1 tries to send a request to payments.default.svc.cluster.local, the traffic never leaves the Kubernetes network engine. It              all happens at the Linux kernel level:

            1. The Request Leaves Pod 1: Pod 1 sends an HTTP request addressed to the Service 2 ClusterIP (e.g., 10.96.0.1).
            2. Hitting the Host Kernel: The packet leaves the Pod's virtual network interface and enters the host node's Linux kernel.
            3. The Kernel Inspection: The kernel checks its iptables or IPVS rules (which kube-proxy previously wrote). It sees a rule that says: "If traffic is                  destined for 10.96.0.1, intercept it."
            4. DNAT (Destination Network Address Translation): The kernel randomly selects one of the healthy backend Pod IPs belonging to Service 2 (e.g.,                       192.168.1.45) based on its load-balancing algorithms. It alters the packet header, changing the destination from the Service IP to the specific                    Pod IP.
            5. Direct Delivery: The packet is then routed across the cluster network directly to the node hosting that specific Service 2 Pod.
            

Actual Flow:

   
      1. The Request Hits the Kernel: The packet arrives, addressed to the virtual Service IP.
      2. Consulting the Rules: The kernel checks the iptables or IPVS rules (programmed by kube-proxy).
      3. Choosing the Pod (Load Balancing): The rules tell the kernel which Pod IPs are currently healthy. The kernel randomly (or via round-robin) picks the best           target Pod IP.
      4. Performing DNAT: Now the kernel rewrites the packet header, swapping the virtual Service IP out and putting the chosen Pod IP in.
      5. Routing the Traffic: The kernel routes the packet directly to its final destination.



4. CoreDNS


* The Watcher (Control Plane)

         Just like kube-proxy, CoreDNS constantly watches the Kubernetes API server.
         • Whenever a new Service is created, CoreDNS automatically generates a DNS record for it.
         • It follows a strict naming convention: <service-name>.<namespace>.svc.<cluster-domain>.

* The Resolution Flow (The Step-by-Step)

         When Service 1 wants to talk to Service 2, CoreDNS steps in before any network packets are even routed:
         1. The Question: Inside Pod 1, your application code tries to connect to http://cluster.local.
         2. The Phone Call: The Pod's operating system sends a standard DNS query to the cluster's central DNS server, which is CoreDNS (running as a set of                    standard pods in the kube-system namespace).
         3. The Answer: CoreDNS looks at its internal database, finds the record for payments.default, and replies back to the Pod with the Service ClusterIP                   (e.g., 10.96.0.1).
         4. The Hand-off: Now that Pod 1 has the IP address, it can finally create the actual data packet.




      
