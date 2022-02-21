# TARS Environment Setup

## Introduction

Now that you have learned about TARS and its functionalities, it’s time to set it up on your machine. This chapter will discuss three methods to deploy TARS Framework—Framework Docker deployment, Source Compilation deployment, and Kubernetes deployment. Although the Source Compilation method can understand the fundamentals of TARS Framework thoroughly, it requires you to go through more steps manually, which is more complicated and time-consuming. On the other hand, deployment through Docker is more simplified and suitable for TARS beginners who need to learn interface operations and the framework’s development process. Docker’s drawback is that it requires you to perform more steps when there is a service failure or a need to expand service capacity. In contrast, the Kubernetes deployment uses a container orchestration mechanism to enable automatic capacity expansion and failure recovery, but the method is not yet mature since it was recently open-sourced. If you are relatively new to TARS, we recommend the Framework Docker deployment to start your TARS journey.

We will demonstrate how to set up your TARS environment using Docker, Source Compilation, and Kubernetes. After you master these methods to install TARS, we will discuss the framework components, namely the core servers, TarsWeb, and TarsNode. Lastly, we will present the instructions to deploy your development environment in TarsGo, TarsJava, TarsCPP, TarsPHP, and Tars.js, which correspond to the different programming languages TARS supports.

## Description of TARS Framework

To reiterate, TARS is a high-performance RPC framework based on name service and Tars protocol, as well as an integrated administration platform, which implements hosting-service via a flexible schedule.

The TARS Framework can be deployed on single or multiple machines, and it uses a Master-Slave architecture to manage distributed builds. The master is your main node that primarily handles your executions, while the slave node serves as a backup plan for the master node. Therefore, you can have one master node and multiple slave nodes on each machine that deploys the TARS Framework. Note, however, that you can never have many master nodes and one slave node.

In theory, it is sufficient to have one master and one slave node. However, if you want to expand a node on a new machine, execute linux-install.sh for Linux/Mac or windows-install.sh for Windows. When building the new node, remember that the parameter REBUILD must be false. Otherwise, the database will be reset.

In practice, even if the master and slave nodes are dead, running servers on the framework will still be as usual, but publishing will be affected. In addition, the master node should have TarsAdminRegistry, TarsPatch, TarsWeb, and TarsLog, which are not installed on the slave node(s).

The reasons why these services should not be duplicated on another node are the following. TarsAdminRegistry provides publishing admin status, and TarsLog must collect the remote logs on one machine (it will be bad if the logs are scattered on multiple machines!). Suppose you deploy TarsPatch and TarsWeb on multiple machines, the service won’t be published successfully unless the directory /usr/local/app/patches (or c:\tars-install\patches in Windows) is shared among multiple computers, say, through NFS. In other words, you can avoid this step by not deploying TarsPatch and TarsWeb on multiple machines. Another tip: to prevent hard disk overload, you can later deploy the TarsLog to a large hard disk server.

After the installation is complete, there will be five databases created:

db_tars
It is the core database for TARS, consisting of server info, template configuration, service configuration, etc.
db_tars_web
It is the core database for TarsWeb.
db_user_system
It refers to the user auth system for TarsWeb.
tars_stat
It refers to data about status monitoring.
tars_property
It is the database-stored property monitoring data.

## TARS Environment Setup

### Deployment Requirements

Hardware/Software

Requirements

Additional Information

Hardware

Physical Machine or Virtual Machine with more than 2G Memory	You can deploy TARS on a Cloud such as AWS or GCE, etc.
Operating System

Linux / Mac OS X / Windows	N/A
MySQL

>= 4.1.7

To download and install MySQL, go to "MySQL Community Downloads".	You can either deploy MySQL on a local machine, on a Docker, or a cloud such as AWS or GCE.

MySQL is used to store the data of the TARS framework (including servers’ information, configuration, and others).
Docker

For more information about Docker, see "Get Started with Docker".	It is required when you deploy TARS with Docker.
 
 Please note that these installation requirements apply to the Framework Docker deployment, the Source Compilation deployment, and the Kubernetes deployment.


[Deploy TARS Framework with Docker](./docker/docker.md)

[Deploy TARS Framework with Source Compilatoin](./source-compilation/source.md)

[Deploy TARS Framework with Kubernetes](./kubernetes/kubernetes.md)