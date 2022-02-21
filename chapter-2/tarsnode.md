# Overview of Deployment Methods

Linux/Mac and Windows users have the same methods to deploy TarsNode. If we want the application server to be published to the application node, we will install TarsNode on the application node.

There are three ways to install TarsNode.

- TarsWeb online deployment
- Script deployment in application node
- Docker deployment, which we demonstrated before

Please note that the user running TarsNode may not be root. After installation, you can switch the directory to another user. For example, the current user is root, and you want to switch to the user tars. To do so, run the following commands:

#Under root, mask crontab and comment out tarsnode monitoring
#* * * * * /usr/local/app/tars/tarsnode/util/monitor.sh

#Stop tarsnode (note that crontab monitoring should also be shielded, otherwise it will be automatically pulled up)

    /usr/local/app/tarsnode/util/stop.sh

#Modify directory permissions

    chown -R tars:tars /usr/local/app/tarsnode

##Switch to tars users

    su tars

#Start

    /usr/local/app/tarsnode/util/start.sh

#Add crontab monitor

    * * * * * /usr/local/app/tars/tarsnode/util/monitor.sh

## Online Deployment

TarsWeb provides the function of installing TarsNode online. When installing, you need to input the IP, password, and other information of the application node to complete the installation of automatic TarsNode (you also need to add crontab to monitor TarsNode).

The tarsnode.tgz is automatically copied to the web/files directory by the installation script during the installation of TARS framework.
If not, you need to generate tarsnode.tgz by yourself. Here is how you can do this:

Run make install to compile TARS Framework, then run the following commands:

    cd /usr/local/tars/cpp/framework/servers
    tar czf tarsnode.tgz tarsnode
    cp tarsnode.tgz yourweb/files

Add a process monitoring in crontab to ensure that the TARS servers can be restarted after an exception occurs:

    * * * * * /usr/local/app/tars/tarsnode/util/monitor.sh

Note: The application node needs to support wget. Otherwise, the TarsNode cannot be downloaded from the TarsWeb.

## Script Deployment in the Application Node

TarsNode can also be installed automatically on the application node if it can normally access the TarsWeb.

Run commands below on application nodes to retrieve TarsNode package:

    wget http://webhost/get_tarsnode?ip=xxx&runuser=root
    chmod a+x get_tarsnode
    ./get_tarsnode

Parameter Description

- ip Local - IP

- runuser - Users running TarsNode

Complete the installation of TarsNode, and then add a process monitoring in crontab to ensure that the TARS servers can be restarted after an exception occurs.

    * * * * * /usr/local/app/tars/tarsnode/util/monitor.sh

After we successfully deploy TARS on our machines, let’s explore the critical components of the TARS Framework.

