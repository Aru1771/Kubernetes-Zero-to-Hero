Service will below 3 things
1. Load balancing
2. service discovery
3. Exposing app to external world



Why we need service in K8S ?

A) if we don't have service future in k8s we have to share our pod's ip to access the application.
   but after some time the will crash for some reasons and replication set will create a new prod with other IP.
   in this case with the old pod Ip we can not access the app.

   to over come the above issue insted of accessing the application with pod ip's we will create a 
   service on top of it.

   this service acts like a loadbalancer.

   one you have created a service we will see that service like: 
   Eg: payments.defaults.svc
      service.namespace.svc

   by using this service IP we will access the application which are running in pods.

   once request recieaved the service then kubeproxy will distribute those requests to pods.

   Hear we have one doubt:
   ======================
if the new pod was created the IP will be change so service also will face same issue.how it 
will be over comed.

it will comes with Service Discovery process:
* means this services will track the applications with labeles not with IP address.
* afert a pod creash new pod will be created with new IP. but the lable will be the same for all pods.


to expose our app to outside world. we have 3 main types.
1. cluster ip: if you use this we can access app only inside the cluster
2. nodepot: if we use this we can access the app with in the organization. simply how have access to vpc.
3. load balacer: by using this we can get one elastic ip addreess. with the ip we can access it from out side.


we can expose our application in 3 ways:
=========================================

1. cluster IP: we can access app inside the cluster.
2. NodePort: we can access the app with in the VPC.
3. LoadBalancer: we can access the app from the external world.

if we are using minikune for nodeport we have to do port forwording. 

Kubeshark to see the traffic flow in the our cluster



 



 


   

   

   

   

    

   
  
