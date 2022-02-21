
# Download TarsWeb and Deploy TarsFramework: Linux/Mac Users

Run the following command to download TarsWeb:

    git clone https://github.com/TarsCloud/TarsWeb.git

Copy TarsWeb to /usr/local/tars/cpp/deploy/ and rename it:

    mv TarsWeb web
    sudo cp -rf web /usr/local/tars/cpp/deploy/

Let’s see what files does /usr/local/tars/cpp/deploy consist of:

    ubuntu@VM-0-14-ubuntu:/usr/local/tars/cpp/deploy$ ls -l
    total 10304
    -rw-r--r-- 1 root root 443392 Apr 3 17:22 busybox.exe
    -rw-r--r-- 1 root root 1922 Apr 3 17:22 centos7_base.repo
    -rw-r--r-- 1 root root 1395 Apr 3 17:22 Dockerfile
    -rwxr-xr-x 1 root root 3260 Apr 4 11:31 docker-init.sh
    -rwxr-xr-x 1 root root 319 Apr 3 22:13 docker.sh
    drwxr-xr-x 7 root root 4096 Apr 3 17:57 framework
    -rwxr-xr-x 1 root root 4537 Apr 4 11:31 linux-install.sh
    -rwxr-xr-x 1 root root 9820288 Apr 3 22:16 mysql-tool
    -rwxr-xr-x 1 root root 811 Apr 4 11:31 tar-server.sh
    -rwxr-xr-x 1 root root 16449 Apr 3 17:22 tars-install.sh
    -rwxr-xr-x 1 root root 320 Apr 4 11:31 tars-stop.sh
    drwxr-xr-x 2 root root 4096 Apr 3 17:57 tools
    drwxr-xr-x 12 root root 4096 Apr 3 21:07 web
    -rwxr-xr-x 1 root root 3590 Apr 3 17:22 web-install.sh
    -rwxr-xr-x 1 root root 1476 Apr 3 17:22 windows-install.sh

In the deploy directory cd /usr/local/tars/cpp/deploy, we can run the script linux-install.sh and automatically deploy TARS (access permissions can be edited as needed).

    chmod a+x linux-install.sh

    ./linux-install.sh MYSQL_HOST MYSQL_ROOT_PASSWORD INET REBUILD(false[default]/true) SLAVE(false[default]/true) MYSQL_USER MYSQL_PORT

Parameter Interpretation

MYSQL_HOST
MySQL IP address.

MYSQL_ROOT_PASSWORD

MySQL root password. Note that the password should not contain special characters, such as exclamation marks, which will cause issues in the shell script recognition.

INET

The name of the network interface (such as eth0, which you can find out for your machine by running ifconfig) indicates the native IP bound by the framework (note that it cannot be 127.0.0.1 - it’s a rule written in the installation script).

REBUILD

This parameter determines whether to rebuild the database, and it is usually set to false. However, if there is an error in the intermediate installation and you want to reset the database, you can set it to true.

SLAVE

A slave TARS framework node.

You also have the option to deploy a server on a master framework node or slave framework node. To set it up, install two TARS framework nodes and one MySQL. For example:

    TARS master [192.168.7.151]
    TARS slave [192.168.7.152]
    MySQL [192.168.7.153]
    Execute on the master framework node (192.168.7.151):

    chmod a+x linux-install.sh
    ./linux-install.sh 192.168.7.153 tars2015 eth0 false false

Execute on the slave framework node (192.168.7.152):

    chmod a+x linux-install.sh
    ./linux-install.sh 192.168.7.153 tars2015 eth0 false true



Note:

In the master framework node, TarsNode and TarsWeb must be kept alive so that other TARS servers work automatically. If TarsNode and TarsWeb are not kept alive, you can reboot these servers through the monitoring process in crontab.

If the master node crashes, TarsNode in the slave node can be kept alive to make other TARS servers work. Essentially, the slave node can act as a backup plan to ensure that all programs work properly.

The linux-install.sh script will log in to the database using MYSQL_USER and MYSQL_PASSWORD. It will then create an account, TarsAdmin, and authorize the framework to use databases about TARS.

If it is Ubuntu, we should use sudo ./linux-install.sh ...

After execution, you can check whether the Node.js environment variable is running using the following command: node --version

After installation, the Node.js-related environment variables will be written in /etc/profile. If it doesn't work, execute it manually with source /etc/profile. If it's Ubuntu, please pay attention to the permission settings.

If everything goes well, the output should look as follows:

    2019-10-31 11:06:13 INSTALL TARS SUCC: http://xxx.xxx.xxx.xxx:3000/ to open the tars web.

    2019-10-31 11:06:13 If in Docker, please check your host IP and port.

    2019-10-31 11:06:13 You can start tars web manual: cd /usr/local/app/web; npm run prd

And you should be able to access TarsWeb, as shown in the below image, with this URL: http://xxx.xxx.xxx.xxx:3000/. Now, you need to create a new password for your admin account.

