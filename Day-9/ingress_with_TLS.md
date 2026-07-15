K8S-ingress
============


why we need ingress ?

service is offering

1. Discovery
2. Load balancing
3. Exposing the app to outer world

this services offering load balancing it works with round robin mechanism.

this offering very simple LB. But when we compared with the Enterprise Load Balancing that offer Ratio based, Sticky sections, Path, Host, Domain, Whitelisting and black listing, TLS and Firewall.


But k8s Service LB not offering these many Enterprises stranded things.

for our application we have 10 micro services and for all 10 services we crated 10 services with LB type in cloud provider 10 static Public ip address will be crated so it will charge more for 10 static ip address.



to overcome all the issues K8S comes with Ingress
                                          --------

K8s Will not directly support this ingress it need help from ingress controller this ingress controller will be provided by other companies like Nginx, F5, Ambassador, HAproxy.


in K8S we will write ingress manifest file with set of rules to implement this we need to deploy controllers in our K8S.

What is this ingress controller ?

it is Load Balancer + API Gateway which provides Enterprise standards in K8S. 



some ingress controllers sit inside a cluster inside like (nginx) we have to deploy them like a PODS.

but few ingress controller sit outside the cluster like (F5) and send the traffic via VXLAN tunnels.





 
