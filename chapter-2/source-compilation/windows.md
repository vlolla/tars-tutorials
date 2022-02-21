# Download TarsWeb andDownload TarsWeb and Deploy TarsFramework: Windows Users

Note:

When installing TARS, it will automatically download Node.js, NPM, PM2 and other dependencies and set the environment variables to ensure that Node.js works.

By default, Node.js v12.13.0 is downloaded.

If you have an older version of Node.js already installed, it is recommended to uninstall it first.

Download TarsWeb from GitHub into a directory c:\tars\cpp\deploy with the following commands:

    cd c:\tars\cpp\deploy
    git clone https://github.com/TarsCloud/TarsWeb.git web

After you retrieve and download files from the TarsWeb repo, check c:\tars\cpp\deploy and make sure you have the following files ready.

    2020/03/31  17:20    <DIR>          .
    2020/03/31  17:20    <DIR>          ..
    2020/03/24  11:38           443,392 busybox.exe
    2020/03/24  11:38             1,981 centos7_base.repo
    2020/03/28  15:43             3,273 docker-init.sh
    2020/03/28  15:43               319 docker.sh
    2020/03/28  15:43             2,365 Dockerfile
    2020/03/28  14:33    <DIR>          framework
    2020/03/30  12:45         4,099,584 libmysql.dll
    2020/03/28  15:43             4,829 linux-install.sh
    2020/03/30  12:51           110,592 mysql-tool.exe
    2020/03/28  15:43             1,349 tar-server.sh
    2020/03/30  12:52           635,904 tars-client.exe
    2020/03/30  11:28            16,885 tars-install.sh
    2020/03/24  11:38               322 tars-stop.sh
    2020/03/30  14:23    <DIR>          tools
    2020/03/30  14:14    <DIR>          web
    2020/03/28  15:43             3,732 web-install.sh
    2020/03/28  15:43             1,543 windows-install.sh

To deploy TarsFramework on Windows, first enter c:\tars\cpp\deploy and run the following command:

    .\busybox.exe sh ./windows-install.sh MYSQL_HOST MYSQL_ROOT_PASSWORD HOSTIP REBUILD(false[default]/true) SLAVE(false[default]/true) MYSQL_USER MYSQL_PORT

Parameter Interpretation

MYSQL_HOST
MySQL IP address.

MYSQL_ROOT_PASSWORD
MySQL root password. Note that the password should not contain special characters, such as exclamation marks, which can cause issues in the shell script recognition.

HOSTIP
It’s a localhost IP (note that it cannot be 127.0.0.1).

REBUILD
This parameter determines whether to rebuild the database, and it is usually set to false. However, if there is an error in the intermediate installation and you want to reset the database, you can set it to true.

SLAVE
A slave TARS framework node.

Like Linux/Mac, you can deploy a server on a master framework node or slave framework node. To set it up, install two TARS framework nodes and one MySQL. For example,

TARS master [192.168.7.151]

TARS slave [192.168.7.152]

MySQL [192.168.7.153]

Execute on the master framework node (192.168.7.151)

    .\busybox.exe sh ./windows-install.sh 192.168.7.153 tars2015 192.168.7.151 false false root 3306

Execute on the slave node (192.168.7.152)

    busybox.exe sh ./windows-install.sh 192.168.7.153 tars2015 192.168.7.152 false true root 3306

Note:

For the master framework node, TarsNode and TarsWeb must be kept alive to ensure other TARS servers work automatically. If your servers are not alive, you can set c:\tars-install\tars\tarsnode\util\monitor.bat to Windows scheduled task to reboot the dead servers.

If the master node crashes, TarsNode in the slave node can be kept alive to make other TARS servers work. Essentially, the slave node can act as a backup plan to ensure that all programs work properly.

The windows-install.sh script will log in to the database using MYSQL_USER and MYSQL_PASSWORD. It will then create an account, TarsAdmin, and authorize the framework to use databases about TARS.

Now you should be able to access TarsWeb and TarsFramework with this URL: http://xxx.xxx.xxx.xxx:3000/. You need to create a new password for your admin account.