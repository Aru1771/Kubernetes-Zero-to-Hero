what is an Operator?
=====================

An Operator is software that extends Kubernetes to automate the deployment, management, and lifecycle of a particular application or system.

An Operator is a Kubernetes-native application that uses the controller/reconciliation pattern and application-specific knowledge
to automate the lifecycle and operational tasks of a particular application or system.

For example, imagine PostgreSQL.

A normal controller might know:

    "Keep 3 Pods running."

A PostgreSQL Operator can understand things like:

    How to deploy PostgreSQL
    How to configure it
    How to create storage
    How to create users
    How to perform backups
    How to handle upgrades
    How to detect failures
    How to restore a database
    How to manage replicas
    How to update the database safely

That's why we call it application-specific operational knowledge.


           Operator
                │
       ┌────────┴────────┐
       │                 │
    CRD/CR          Controller
       │                 │
       └────────┬────────┘
                ↓
           Reconciliation
                ↓
       Application resources


Why not just use a normal Controller?

This is the important question.

Suppose you have a custom resource:

Database

A basic custom controller could do:

    Database CR
        ↓
    Create StatefulSet
        ↓
    Create Service
        ↓
    Create PVC

But a real database has many operational requirements.

For example:

    Database
       ↓
    Install
       ↓
    Configure
       ↓
    Backup
       ↓
    Restore
       ↓
    Failover
       ↓
    Upgrade
       ↓
    Scale
       ↓
    Monitor
       ↓
    Recover

An Operator automates these day-2 operational tasks.

That's the major reason Operators are useful.

Operator = Kubernetes automation for the full lifecycle of an application.


Controller vs Operator
-----------------------

    | Controller                      | Operator                        |
    | ------------------------------- | ------------------------------- |
    | General control-loop concept    | Application-specific automation |
    | Watches resources               | Watches resources               |
    | Reconciles desired vs actual    | Reconciles desired vs actual    |
    | Can manage Kubernetes resources | Manages application lifecycle   |
    | May be simple                   | Usually more application-aware  |
    | Example: Deployment Controller  | Example: PostgreSQL Operator    |


Important:

      Controller
          =
      Observe + Reconcile
      
      Operator
          =
      Observe + Reconcile
              +
      Application knowledge
              +
      Lifecycle automation

An Operator is not a completely different mechanism from a Controller.

An Operator uses the controller pattern.

Real-world Operators
---------------------

cert-manager

    Automates TLS certificates.
    
    Certificate CR
          ↓
    cert-manager
          ↓
    Certificate
          ↓
    Secret
Prometheus Operator

    Manages Prometheus monitoring resources.
    
    Prometheus CR
          ↓
    Prometheus Operator
          ↓
    Prometheus deployment/configuration
Strimzi

    Manages Apache Kafka on Kubernetes.
    
    Kafka CR
       ↓
    Strimzi Operator
       ↓
    Kafka cluster
