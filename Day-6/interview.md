"Deployment is a Kubernetes workload resource used to manage application Pods. 
When we create a Deployment, Kubernetes creates a ReplicaSet, and the ReplicaSet creates Pods based on the desired replica count. 
The ReplicaSet controller continuously monitors the actual and desired state. 
If a Pod fails, it creates a replacement to maintain the desired state. 
We can manually scale Pods or configure HPA for automatic scaling. 
For stable networking and load balancing across Pods, we use Kubernetes Services."


IMP CMD'S:

kubectl rollout status deployment nginx


 | Command                                                 | Purpose                          |
 | ------------------------------------------------------- | -------------------------------- |
 | `kubectl rollout status deployment nginx`               | Check rollout progress           |
 | `kubectl rollout history deployment nginx`              | View revision history            |
 | `kubectl rollout undo deployment nginx`                 | Roll back                        |
 | `kubectl rollout undo deployment nginx --to-revision=2` | Roll back to a specific revision |
 | `kubectl rollout pause deployment nginx`                | Pause rollout                    |
 | `kubectl rollout resume deployment nginx`               | Resume rollout                   |


<img width="961" height="352" alt="image" src="https://github.com/user-attachments/assets/cfd32c3d-a62e-4459-84f6-0fd2b8bc3fbb" />

<img width="912" height="496" alt="image" src="https://github.com/user-attachments/assets/31121840-3b53-46f2-be8c-81ae640ad9eb" />
