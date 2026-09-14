How we can took backup by using velero to s3
============================================


* Before taking backup we have to install velero
* Velero has two major components to install
  1. Client--> which we have to install on laptop
  2. server --> which we have to install on cluster (master node)
 
1. Velero Client installtion:
   -------------------------

   * Go to velero official page velero for aws --> https://velero.io/docs/v1.0.0/aws-config/
   * Go to *official releases* in the page and scrool down and get the wget velero-v1.18.2-linux-amd64.tar.gz --tar.gz link.
   * untar it will tar -zxvf velero-v1.18.2-linux-amd64.tar.gz
   * we will get velero directory
   * cd velero directory and cp the velero folder to /usr/local/bin folder.

2. create s3 bucket as instrcted in the offficial docs.
3. create iam user, create policy and attch the policy to user.
4. Create an access key for the user
5. install the velero step in that cmd we have pass one flag called **--plugins velero/velero-plugin-for-aws:*version we have to metion**


To take a back up by using a velero:
-------------------------------------

CMD to take namespace backup in k8s:

         velero backup create <backup_name> --include-namespaces <name_Sapce_name>
to see the list of backups:

          velero get backups
to restore a backup:

          velero restore create --from-backup <backup_name>
