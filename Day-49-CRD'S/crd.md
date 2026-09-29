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



| CRD                                               | CR                               |
| ------------------------------------------------- | -------------------------------- |
| CustomResourceDefinition                          | Custom Resource                  |
| Defines a resource type                           | Actual instance                  |
| Defines the schema                                | Provides values                  |
| Usually created by platform/operator installation | Created by users/apps            |
| `kind: CustomResourceDefinition`                  | `kind: Database`                 |
| Defines `Database`                                | Creates `payment-db`             |
| Defines allowed structure                         | Represents desired configuration |


CRD: 

        apiVersion: apiextensions.k8s.io/v1
        kind: CustomResourceDefinition
        
        metadata:
          name: databases.database.example.com
        
        spec:
          group: database.example.com
        
          names:
            kind: Database
            plural: databases
        
          scope: Namespaced
        
          versions:
            - name: v1
              served: true
              storage: true
        
              schema:
                openAPIV3Schema:
                  type: object
                  properties:
                    spec:
                      type: object
                      properties:
                        engine:
                          type: string
                        version:
                          type: string
                        storage:
                          type: string


Create a CR:


        apiVersion: database.example.com/v1
        kind: Database
        
        metadata:
          name: payment-db
          namespace: production
        
        spec:
          engine: postgres
          version: "16"
          storage: 20Gi

Controller:
-----------

* Controller will watch the desired state what we have mentioned in the CR.yaml file and reconcile it accordingly

* it will identify the diff b/w the actual state and desired state. then it will take action accordingly.

* Desired State: What the user says they want.
* Actual State: What currently exists in the cluster.

* So conceptually: controller will

            Observe
               ↓
            Compare
               ↓
            Act
               ↓
            Observe again

             
* Kubernetes controllers interact with the API Server and receive information about resource changes.

K8S-API Server Responsible for:

        Receiving API requests
        Validation
        Authentication/authorization
        Admission
        Reading/writing cluster state

Controller Responsible for:

        Watching resources
        Understanding desired state
        Comparing with actual state
        Taking corrective action
* Controllers react to both directions of change: at Actual state and desired state.

* Controller Doesn't Just "Create Things"
  
        Create
        Update
        Delete
* Controller Doesn't Directly Change etcd: The controller normally interacts with the Kubernetes API Server.

      Controller → API Server → etcd

* The Word "Reconciliation": 

        Reconciliation means bringing the actual state toward the desired state.

* Controller Does Not Guarantee Instant Correction

        The controller is not necessarily instantaneous.
        
        There can be a small delay:
