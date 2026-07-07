 what is the diff b/w Container, Pod and Deployment ?
 ---------------------------------------------------

 container:
 ---------

  To running a container in docker we will use command in that command we will define 
  all the specifications.

  Pod:
  ----
  it is an Enterprice model hear we define all the specification of a container in yaml file.
  in pod will create single or multiple containers.
  Eg: sidecar containers called service mesh.

  Deployment:
  -----------

  By deployment we will get auto Healing and scaling capabilities.
  but pod don't have those capabilities.
  hear we are finally creating a Pob but not directly. for that we are using Deployment.

  If you create a deployment:
  --------------------------
  Deployement ----> Replica set ------> Pods
                      |
                      |
                    Controller

Replica set will create the Pods that we mentioned in the deployment.yaml file.
Replica set will make sure there is always defined replicas will run on the cluster.
Replica set controller will maintain the actual state same as desired state.

 






 
