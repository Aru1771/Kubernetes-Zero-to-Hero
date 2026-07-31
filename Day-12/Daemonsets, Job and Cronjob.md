Daemonsets, Job and Cronjob
============================

1. Daemonsets:
   -----------

* By using Daemonsets we can create a new pod in a new node. like one pod will be created by a single node.
* For Daemonsets we no need to mention replicas. the Daemonsets will create a single pod in every node and after some time if a new node was added automatically
  new Daemonsets pod will be create automatically in a new node. once node was deleted the Daemonsets pod aslo delete automatically.

  Use cases:
  ----------
  1. monitoring agents
  2. login agents
  3. kube-proxy
  4. weave-net(CNI)
  5. calico (CNI)
  6. flannel (CNI)
 
Daemonsets dm.yaml file:
------------------------
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-dm
spec:
  template:
    metadata:
      name: my-demo-ds
      lables: 
        app: demo-ds
    spec:
      containers:
        - name: my-demo-ds
          image: nginx
          ports:
          - containerPort: 80
  selector:
    matchLables:
      app: demo-ds 


cmd: kubectl create -f dm.yaml


* once we create the dm the dm pods will be created in all the worker nodes.  But it will not schedule any dm pod in control plane because  control plan will only allow
  control plane only allow control plane components. but this dm set which we created now is our cr so control plane taint will not tollerence this.

if you want to see dm:
-----------------------

cmd: kubectl get dm

if you want to see dm in all ns:
-----------------------------------

cmd: kubectl get ds -A 

-A: means all the ns





2. Cronjob:
   --------

   cronjob have syantax with 5 *

   eg: * * * * *

   * = min in a hour (0-59)
   * = hours in a day (0-24)
   * = day in a month (1-31)
   * = months in year (1-12)
   * = days in a week (0-6)  (0= sunday to 6= saturday)


case:1 
------
* if you want to run this cronjob at Every saturday 01:40 AM

  syantax: 40 1 * * 6

use cases:
----------
1. to generate a reports everyday, weekly, montly.
2. clean up logs.





3. Job:
   ---

it will run only once not like a cronjob

refer the docks from more



  
