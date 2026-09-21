Custome Resource Defination(CRD'S):
-------------------------------------


Topic:
------

    why we need CRD's in K8S
    What is the use of CRD's

CRD: Custome resource defination

why we need CRD's in K8S
----------------------------

* To extent the k8s api based up on our requirment.
* By applying the CRD k8s will learn about new type of resources and types.
* it will start support CR types in k8s.

Take an example:
-----------------

* if you are deploying an application by writing a deployment.yaml file.
* we will provide all the required configration and we will apply.
* At the time of appling it K8S will validate all the configration with Deployment Resource Defination.
* Simple consider this definations like a templates
* So we cannot add custome filed hear. we can not put image some other palce in yaml file. all these will be mentioned as per the resource defination.


Now we can understand about CRD:
--------------------------------

* if you create any Custome resources k8s will validate it with CRD'S.
* Simple consider this definations like a template.

what this CRD (template) contains ?
-----------------------------------

* It contains Name
* Fields of the custome resources.
* How it shoud be validate
* Data type of those fields
* Once we create the custome resource defination.
* Now we are able to create a Custome resource by using a manifest file/ json files / imparative way
* when we are apply are creating the these CR k8s will validate all the fields in CR with CRD's.

Three Main Objects in when we are creating CR:
-----------------------------------------------

1. CRD
2. CR
3. Custome Controller --> it will manages the lifecycle of Custome resource.

Before know about Custome Controllers:

* first we have to understand what controllers will do in k8s.
* Take deployment controller as an example.
* The Deployment controller will take care of the deployment resource and it lifecycle managment.

* in the same way when we are creating a custome resource we have an option called custome controller.
* we can use that custome resource and ask that controller to manage our custome resource.
* this cutome controller is optional but it is recomended way in prod.
* 


Note: 


  For more deatails refer cks2024-repo---> day -49 folder 








