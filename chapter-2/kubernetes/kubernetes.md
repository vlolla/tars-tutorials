# Deploy TARS Framework with Kubernetes

By incorporating some of Kubernetes’ unique characteristics, the TARS Framework can become an even more powerful development framework. Because Kubernetes and TARS are not fully compatible with each other, we have developed a solution to deploy TARS in a Kubernetes environment—k8stars.

[k8stars](https://github.com/TarsCloud/K8STARS) has the following characteristics:

Maintain the same functionality of TARS Framework.

Support TARS naming service and configuration management.

Support a smooth transfer of TARS services into container orchestrations such as Kubernetes.

Non-invasive, which means there isn’t a coupling between the deployment environment and the services

## Prerequisites

If you want to deploy TARS with Kubernetes, make sure you have the below requirements:

Install Kubernetes; you may use kubectl or other tools of your choice to control the clusters

An endpoint for docker commands

git clone https://github.com/TarsCloud/K8STARS.git


## Deploy tars_db

Instead of MySQL that is used in the Docker and Source methods, the k8stars solution uses tars_db to store TARS Framework’s data such as the server’s information and configuration. Please follow the below instructions to deploy tars_db.

// acquire relevant documents on deployment under baseserver file

    cd baseserver
    make deploy

// Create a MySQL database (in practice, a cloud database is recommended)

    kubectl apply -f yaml/db_all_in_one.yaml

// Acquire the names of the database with following command

    kubectl get pods | grep tars-db

// Edit the name in db/install_db_k8s.sh, and then import the data

    sh db/install_db_k8s.sh

For existing tars db, executing SQL files may erase original data. You only need to import the missing db.

## Deploy TarsRegistry

Use kubectl apply -f yaml/registry.yaml to deploy TarsRegistry. If you didn't use Kubernetes when creating the database, please edit the data path in the registry.yaml file accordingly.

## Deploy TarsWeb

Use

    kubectl apply -f yaml/tarsweb.yaml 

to deploy TarsWeb. By default, TarsWeb uses port 3000, but you may use a method of your choice. Check if the status of your browser is active. (At the moment, please use NodePort to map to the external network 30000 port. Assess Tarsweb via http://xxx.xxx.xxx.xxx:30000).

Note: because the current Tarsweb version is not yet compatible with Kubernetes’ scenarios, the restart/stop options on the webpage will not execute successfully.

## Deploy Other Framework Servers

For instance, to deploy tarsnotify please use kubectl apply -f yaml/tarsnotify.yaml. All database configurations can be edited as needed. All other services can be deployed in this fashion, as you may simply change tarsnotify to your desired service. Currently available services include:

- tarslog
- tarsconfig
- tarsproperty
- tarsstat
- tarsquerystat
- Tarsqueryproperty

You can access TARS at http://xx.xx.xx.xx:30000/. You will need to create a new password for your admin account when you try to log in for the first time.


Congratulations! You have completed the installation of TarsWeb and TarsFramework using the Kubernetes approach.

## TARS Deployment Directories

k8stars manages services based on the environment variables in TARS_PATH, and therefore it is important to understand the deployment directories. The function of each directory is as follows:

- ${TARS_PATH}/bin: to store start scripts and binary files,
- ${TARS_PATH}/conf: to store configuration files,
- ${TARS_PATH}/log: to store log files,
- ${TARS_PATH}/data: to provide execution status and cache files.


## Application Servers Deployment

Run the following command to deploy application servers:

    cd examples/simple && kubectl apply -f simpleserver.yaml

Parameters Description

- examples/simple/Dockerfile will create the container image, and cmd/tarscli/Dockerfile will build the base image.
- tarscli genconf in start.sh will be used to generate TARS servers’ starting configuration.
- The file _server_meta.yaml is used to configure servers’ metadata. You can refer to the structure of ServerConf in app/genconf/config.go for the field information. The endpoint is tcp -h ${local_ip} -p ${random_port} by default and supports automatically filling in IP and random ports.
- To confirm if deployment of application servers is successful, log in to db_tars and execute select * from t_server_conf\G. You should see that the simpleserver’s node information is registered in both the database and TarsWeb. The screenshot of TarsWeb is shown below.