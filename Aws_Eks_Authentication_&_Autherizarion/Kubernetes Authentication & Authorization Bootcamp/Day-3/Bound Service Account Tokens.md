Topic 2: Bound Service Account Tokens
=====================================

Learning Objectives
-------------------
By the end of this topic, you'll understand:

Why Bound Service Account Tokens were introduced
How Pods automatically receive tokens
Where the token is stored inside a Pod
How Kubernetes automatically rotates (refreshes) the token
Difference between old tokens and bound tokens
How this forms the foundation of IRSA

First, let's understand the problem.

Suppose we have a ServiceAccount:
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app-sa

And a Pod:
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  serviceAccountName: app-sa


Question:

How does this Pod authenticate to the Kubernetes API Server?

The Pod cannot simply say:

Hi API Server,
I'm app-sa.
Trust me.

The API Server won't trust it.

The Pod needs proof.

That proof is a JWT token.

Before Kubernetes v1.24
------------------------
When you created the ServiceAccount:

ServiceAccount

↓

Kubernetes automatically created Secret

↓

Permanent JWT Token

↓

Pod mounted Secret

Example:

ServiceAccount
      │
      ▼
Secret
app-sa-token-ab123

      │
      ▼
Mounted inside Pod

Problems:

Token never expired
Token stored as Secret
Easy to steal
No automatic rotation

Kubernetes Improved This
-------------------------
Instead of creating a Secret, Kubernetes now creates a Bound Service Account Token.

The flow is:

Pod Created

↓

API Server

↓

TokenRequest API

↓

Temporary JWT

↓

Mounted into Pod

↓

Automatically Refreshed
Notice something important.

The token is created because the Pod needs it.

It is bound to that specific Pod.

This is why it's called a Bound Service Account Token.

What does "Bound" mean?
-----------------------

This is the most common interview question.

Imagine two Pods.

Pod A
↓

ServiceAccount app-sa

and

Pod B

↓

ServiceAccount app-sa

Even though both use the same ServiceAccount,

they do not share the same token.

Instead:

Pod A

↓

JWT Token A
Pod B

↓

JWT Token B

Each Pod gets its own token.

That token is bound to that Pod.

If Pod A is deleted,

its token becomes invalid.

Complete Authentication Flow
------------------------------
Let's understand everything happening internally.

Suppose you apply:
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  serviceAccountName: app-sa

Internally Kubernetes performs:

Step 1

Pod Created

↓

Scheduler

Schedules Pod

↓

Kubelet

Starts Pod

↓

Kubelet notices

This Pod uses ServiceAccount app-sa

↓

Kubelet

Requests Token

↓

POST

TokenRequest API

↓

API Server

Generates JWT

↓

Returns JWT

↓

Kubelet mounts token

Inside Pod

↓

Application reads token

Authenticates to API Server

Where is the token stored?
--------------------------
Inside every Pod:

/var/run/secrets/kubernetes.io/serviceaccount/

Let's verify.

Create Pod:

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  serviceAccountName: app-sa

  containers:
  - name: nginx
    image: nginx

Login to Pod

kubectl exec -it nginx -- sh

Check:

cd /var/run/secrets/kubernetes.io/serviceaccount

List files:

ls

Output

ca.crt
namespace
token

Read token

cat token

You'll see

eyJhbGciOiJSUzI1Ni...

That is the JWT.

What are these files?
-----------------------
token

JWT token

Used to authenticate.

namespace

Contains

default

or

dev

So the application knows which namespace it is running in.

ca.crt

Certificate Authority certificate.

The application uses it to verify that it is really talking to the Kubernetes API Server and not an imposter.

Without ca.crt, the application cannot securely establish HTTPS communication with the API Server.

Automatic Token Rotation
-------------------------
Another major improvement.

Old token

Created

↓

Never Changed

New token

Created

↓

Expires

↓

Kubelet requests new token

↓

Replaces old token

↓

Application keeps working

Kubernetes refreshes the token before it expires.

No restart required.

Real Production Example

Imagine Prometheus running inside Kubernetes.

Prometheus

↓

Needs Pod Metrics

↓

Calls Kubernetes API

Every request includes:

Authorization:

Bearer <JWT Token>

If the token expires,

Kubelet gets a new one.

Prometheus continues working.

No manual action.

Hands-on Lab
Step 1

Create ServiceAccount

kubectl create sa app-sa
Step 2

Create Pod

apiVersion: v1
kind: Pod
metadata:
  name: nginx

spec:
  serviceAccountName: app-sa

  containers:
  - name: nginx
    image: nginx

Apply

kubectl apply -f pod.yaml
Step 3

Login

kubectl exec -it nginx -- sh
Step 4

Go to token directory

cd /var/run/secrets/kubernetes.io/serviceaccount
Step 5

List files

ls

Output

ca.crt
namespace
token
Step 6

Read namespace

cat namespace
Step 7

Read token

cat token

Old vs New
------------
Feature	Old ServiceAccount Token	Bound ServiceAccount Token
| Feature               | Old ServiceAccount Token | Bound ServiceAccount Token |
| --------------------- | ------------------------ | -------------------------- |
| Stored as Secret      | ✅ Yes                    | ❌ No                       |
| Permanent             | ✅ Yes                    | ❌ No                       |
| Expires               | ❌ Never                  | ✅ Yes                      |
| Auto Rotation         | ❌ No                     | ✅ Yes                      |
| Bound to Pod          | ❌ No                     | ✅ Yes                      |
| Uses TokenRequest API | ❌ No                     | ✅ Yes                      |

Real Company Scenario (EKS)
---------------------------
Suppose your application needs to:

Read Pods
Read Services
Read ConfigMaps

Instead of storing a permanent token inside the container image (which is insecure), Kubernetes automatically mounts a Bound Service Account Token into the Pod. The application reads that token whenever it calls the Kubernetes API.

The same mechanism is later used by IRSA to obtain temporary AWS credentials without storing AWS Access Keys in the Pod.

Interview Questions
--------------------
1. What is a Bound Service Account Token?

A Bound Service Account Token is a short-lived JWT generated by the Kubernetes API Server using the TokenRequest API. It is mounted into a Pod, automatically refreshed before expiration, and is bound to that Pod, making it more secure than the older long-lived ServiceAccount token Secrets.

2. Why is it called a "Bound" token?

Because the token is issued specifically for a Pod using a particular ServiceAccount. If that Pod is deleted, the token becomes invalid. Kubernetes does not create one permanent token shared across all Pods.

3. Where is the token mounted inside the Pod?
/var/run/secrets/kubernetes.io/serviceaccount/token
4. Who requests the token for the Pod?

The Kubelet requests a short-lived token from the Kubernetes API Server using the TokenRequest API and mounts it into the Pod as a projected volume.
