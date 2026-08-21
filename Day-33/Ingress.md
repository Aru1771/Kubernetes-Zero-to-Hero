 Kubernetes Ingress:
 --------------------

 1) why we are using ingress to expose our application ?

* in k8s we have services to expose our app to external world like NodePort and LoadBalncer.
* if By using this Load Balncer service type we can expose our app to external user so users will able to access the application.
* But we have few draw backs hear this Load balancer type will not support enterpress level Load balancing.
  1. Ratio based, Sticky sections, Path, Host, Domain, Whitelisting IP address and black listing IP address , TLS and web app Firewall, Ddos Attacks.
  2. if cloud Provider will not support the our K8s Cloud-controller manger it will not work.
  3. if we use LoadBalancer service type it is very costly.
  4. if we use 10 services we have to create 10 LB in cloud.


* To overcome all these issue we have ingress.
* in ingress we have 3 main things.
  1. Ingress--------------------> Yaml file.
  2. Ingress controller --------> it will read the ingress yaml files.
  3. Load Balancer -------------> it is created by the Ingress controller.
  

 
