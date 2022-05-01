# Deploy TARS Framework with Docker

In this section, we will discuss deploying the TARS framework with Docker. We will first explain how to create a Docker network. Next, we will talk about how to start MySQL. And then, we will go over the steps required to run tarscloud/framework Docker. You might prefer to deploy TarsNode with Docker because when you start the container, the framework service will start and update automatically instead of going through each step manually.

## Create a Docker Network

At this point, you should have Docker installed (if not, please refer to "Get Docker"), and you can create a Docker network to make containers communicate with each other. As we mentioned in the previous chapter, TARS can be run on physical machines, virtual machines, and containers, and it will work in the same way regardless of the environment that you choose.

For example, you can type this command in a terminal to create a Docker network (you also can modify the parameters for subnet and gateway):

## Create a bridge virtual network and set the name, subnet and gateway.
    docker network create -d bridge --subnet=172.25.0.0/16 --gateway=172.25.0.1 tars


## Start MySQL

MySQL is required to provide framework data to TARS. However, you can skip this step if your machine runs a different MySQL instance. As a side note, we highly suggest separating framework DB from application DB for the sake of data security.

To start MySQL, run the following command in your terminal:

    docker run -d \
        --net=tars \
        -e MYSQL_ROOT_PASSWORD="123456" \
        --ip="172.25.0.2" \
        -v /data/framework-mysql:/var/lib/mysql \
        -v /etc/localtime:/etc/localtime \
        --name=tars-mysql \
        mysql:5.6

        C:\Users\venky\github\tars-tutorials> docker ps
        CONTAINER ID   IMAGE       COMMAND                  CREATED              STATUS              PORTS      NAMES
        854df1263178   mysql:5.6   "docker-entrypoint.s…"   About a minute ago   Up About a minute   3306/tcp   tars-mysql


Please note that the parameters for MYSQL_ROOT_PASSWORD, ip, and Mysql can be modified as needed.

## Run tarscloud/framework Docker

You are now ready to deploy the TARS framework with Docker.

Start by pulling Docker image:

    docker pull tarscloud/framework:v2.4.0

Next, run Docker image:

### Mount timezone file /etc/localtime to container. Remove it if you don't have.
### expose port 3000 for web entry
### expose port 3001 for web authorization
    docker run -d \
        --name=tars-framework \
        --net=tars \
        -e MYSQL_HOST="172.25.0.2" \
        -e MYSQL_ROOT_PASSWORD="123456" \
        -e MYSQL_USER=root \
        -e MYSQL_PORT=3306 \
        -e REBUILD=false \
        -e INET=eth0 \
        -e SLAVE=false \
        --ip="172.25.0.3" \
        -v /data/framework:/data/tars \
        -v /etc/localtime:/etc/localtime \
        -p 3000:3000 \
        -p 3001:3001 \
        tarscloud/framework:v2.4.0

Let’s take a closer look at the above commands.

Parameter Interpretation

MYSQL_HOST
IP of MySQL DB instance.
MYSQL_ROOT_PASSWORD
Root password for MySQL.
MYSQL_USER
MySQL user uses root as default.
MYSQL_PORT
Self-explanatory: MySQL port.
REBUILD
This parameter is usually set to false. However, if there is an error in the intermediate installation and you want to reset the database, you can set it to true.
INET
The name of the network interface (such as eth0, as you can see in ifconfig) indicates the native IP bound by the framework (note that it cannot be 127.0.0.1 - it’s a rule written in the installation script).
SLAVE
It refers to a slave node for TARS framework. If you need TARS framework’s high-availability, you should run a second tars-framework container with SLAVE=ture (to learn more about the master/slave model, see Wikipedia).
Directory Interpretation

The directory of Docker (/data/tars) will be mapped to the host directory (/data/framework). Please check the /data/framework directory after starting Docker to confirm that the following directories have been created:

app_log
Log directory of framework servers.
tarsnode-data
/usr/local/app/tars/tarsnode/data is a directory within Docker that stores data about servers published to Docker. It ensures that data won’t be lost when we restart Docker.
web_log
These are the web logs of the tars-node-web module that are only available to the master framework node.
demo_log
These are the web logs of the tars-user-system module that are only available to the master framework node.
patchs
It uploads a release package and is only available for the master framework node.
If any of these directories are missing, you will have to create it manually and restart Docker.

Enter http://${your_machine_ip}:3000 to access your TarsWeb. 

TarsWeb is a web management system that monitors the runtime status of servers and starts, stops, deploys, and publishes servers. We will discuss TarsWeb more later in this chapter.

## Deploy TarsNode with Docker

When you have one or multiple application nodes, you also need to deploy a TarsNode process on each application node. TarsNode must be connected to the TarsRegistry to transmit information from the servers. A TarsNode acts as a bridge between the servers and the framework node.

The first step to deploy TarsNode with Docker is to pull an image:

docker pull tarscloud/tars-node:stable

Next, you need to run a service node image:

docker run -d \
    --name=tars-node \
    --net=tars \
    -e INET=eth0 \
    -e WEB_HOST="http://172.25.0.3:3000" \
    --ip="172.25.0.5" \
    -v /data/node:/data/app \
    -v /etc/localtime:/etc/localtime \
    -p 9000-9010:9000-9010 \
    tarscloud/tars-node:stable

Please note that the parameters in WEB_HOST and ip can be modified as needed.

As you can see above, ports 9000 to 9010 are available for your applications. You can also add more ports if necessary.

TarsNode will register itself to TarsRegistry 172.25.0.3. (if you used the parameters provided). You can find TarsNode by going to the Operation > Node Service page of your TarsWeb.

At this point, you have gone through deploying the TARS framework and TarsNode in the Docker approach. You are now able to access TarsWeb and start your projects in the TARS framework. You will see the following page and can create a new password for your admin account.

After you set the admin's password, the page below will be shown below every time you enter the URL.


## docker-compose

We provided a docker-compose.yml for you too. The subnet is set to 172.25.1.0/16 in case conflict with the guide in chapter 2 to 4

    version: "3"

    services:
    mysql:
        image: mysql:5.6
        container_name: tars-mysql
        ports:
        - "3307:3306"
        restart: always
        environment:
        MYSQL_ROOT_PASSWORD: "123456"
        volumes:
        - ./mysql/data:/var/lib/mysql:rw
        - ./source/Shanghai:/etc/localtime
        networks:
        internal:
            ipv4_address: 172.25.1.2
    framework:
        image: tarscloud/framework:v2.4.14
        container_name: tars-framework
        ports:
        - "3000:3000"
        restart: always
        networks:
        internal:
            ipv4_address: 172.25.1.3
        environment:
        MYSQL_HOST: "172.25.1.2"
        MYSQL_ROOT_PASSWORD: "123456"
        MYSQL_USER: "root"
        MYSQL_PORT: 3306
        REBUILD: "false"
        INET: eth0
        SLAVE: "false"
        volumes:
        - ./framework/data:/data/tars:rw
        - ./source/Shanghai:/etc/localtime
        depends_on:
        - mysql
    node:
        image: tarscloud/tars-node:latest
        container_name: tars-node
        restart: always
        networks:
        internal:
            ipv4_address: 172.25.1.5
        volumes:
        - ./node/data:/data/tars:rw
        - ./source/Shanghai:/etc/localtime
        environment:
        INET: eth0
        WEB_HOST: http://172.25.1.3:3000
        ports:
        - "9000-9010:9000-9010"
        depends_on:
        - framework
    networks:
    internal:
        driver: bridge
        ipam:
        config:
            - subnet: 172.25.1.0/16

Congratulations! You have completed the installation of TarsWeb and TarsFramework on your machine using the Docker approach.