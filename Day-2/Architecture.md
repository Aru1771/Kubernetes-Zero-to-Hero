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
      
      

   
      
      
      









      
