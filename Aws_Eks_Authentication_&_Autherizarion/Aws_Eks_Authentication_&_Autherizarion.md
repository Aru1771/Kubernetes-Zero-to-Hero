In this section we can discuss:
1) Aws EKS Cluster Authentication & Autherizatio 
2) How to connected multiple clusters from User Mechine.

1)  Aws EKS Cluster Authentication & Autherizatio :
--------------------------------------------------
The below steps will we need to perform in the users mechine who need access to AWS eks cluster:
who create the EKS cluster by default that iam user will be add in Cluster "IAM ACCESS ENTRIES".
--------------------------------------------------------------------------------------------------
* First we have install AWS CLI and Kubectl in Users Mechine.
* After installing the Kubectl if we Run any command related to Kubectl, first it will search for a      file called "config" where this file will availble in our mechine in "root folder/.kube/config"-->  t
  this the actual path where the config file was available.
* what this config file contains ?
* In config file we have the info to which cluster we have to connect. if this file was not there in  our mechine then our kubectl request will not reach to our cluster.
* How we can create that Config file our mechine ?
* To create that config file we have to use a command:
  aws eks update-kubeconfig --region clutser_region --name cluster_name
* then it will throw an error message:
  YOU CAN CONFIGURE CREDENTIALS BY RUNNING "aws configure"
* So first we have to create a user in AWS IAM.
* At the time creating user in AWS IAM console what are permissions we are providing to user is only applyed at AWS console level. not applied to AWS EKS cluster.
* Once we have creted the user we have to create "Access key" and "secret access key".
* now again come to user mechine and configure these key's by using the command:
   Commad: aws configure
* once we have configure the Aws keys in our AWS cli now create the config file with the command:
    aws eks update-kubeconfig --region clutser_region --name cluster_name
  
* so the config file was cretaed with Clutser details and pointing to the cluster.

* in that config file we have our clutser data called:
   
  clutser certificate authority
  clutser ARN

* now we can run the kubectl command to see the output. But still we are unable to see cluster resouces. Why ?
* Because the IAM user have only access to AWS console so user can see the resources in the AWS console. But if he want to acces the Aws EKS cluster he can't.
* For Aws EKS clutser Access we have to add that IAM user in Cluster "IAM access entries"
* 
   




