# TARS Servers, TarsWeb, and Development Environment Setup

## Core Framework Servers: Linux/Mac

The TARS framework is ultimately made up of several core framework servers: tarsregistry, tarsAdminRegist, tarsconfig, tarsnode, tarsnotify, tarsproperty, tarsqueryproper, tarsquerystat, tarslog, tarspatch.

You can use the $ ps -ef | grep app/tars | grep -v grep command to check your TARS servers’ status.

Here is an output from

    $ ps -ef | grep app/tars | grep -v grep when servers run successfully：

    [root@VM-0-7-centos deploy]# ps -ef | grep app/tars | grep -v grep

    root       368     1  0 09:20 pts/0    00:00:25
    /usr/local/app/tars/tarsregistry/bin/tarsregistry
    --config=/usr/local/app/tars/tarsregistry/conf/tars.tarsregistry.config.conf
    root      9245 32687  0 09:29 ?        00:00:13
    /usr/local/app/tars/tarsstat/bin/tarsstat
    --config=/usr/local/app/tars/tarsnode/data/tars.tarsstat/conf/tars.tarsstat.config.conf
    root     32585      1 0 09:20 pts/0    00:00:10
    /usr/local/app/tars/tarsAdminRegistry/bin/tarsAdminRegistry
    --config=/usr/local/app/tars/tarsAdminRegistry/conf/tars.tarsAdminRegistry.config.conf
    root      32588     1 0 09:20 pts/0    00:00:20
    /usr/local/app/tars/tarslog/bin/tarslog
    --config=/usr/local/app/tars/tarslog/conf/tars.tarslog.config.conf
    root      32630     1 0 09:20 pts/0    00:00:07
    /usr/local/app/tars/tarspatch/bin/tarspatch
    --config=/usr/local/app/tars/tarspatch/conf/tars.tarspatch.config.conf
    root      32653     1 0 09:20 pts/0    00:00:14
    /usr/local/app/tars/tarsconfig/bin/tarsconfig
    --config=/usr/local/app/tars/tarsconfig/conf/tars.tarsconfig.config.conf
    root      32687     1 0 09:20 ?        00:00:22
    /usr/local/app/tars/tarsnode/bin/tarsnode
    --locator=tars.tarsregistry.QueryObj@tcp -h 172.16.0.7 -p 17890
    --config=/usr/local/app/tars/tarsnode/conf/tars.tarsnode.config.conf
    root      32695     1 0 09:20 pts/0    00:00:09
    /usr/local/app/tars/tarsnotify/bin/tarsnotify
    --config=/usr/local/app/tars/tarsnotify/conf/tars.tarsnotify.config.conf
    root      32698     1 0 09:20 pts/0    00:00:14
    /usr/local/app/tars/tarsproperty/bin/tarsproperty
    --config=/usr/local/app/tars/tarsproperty/conf/tars.tarsproperty.config.conf
    root      32709     1 0 09:20 pts/0    00:00:12
    /usr/local/app/tars/tarsqueryproperty/bin/tarsqueryproperty
    --config=/usr/local/app/tars/tarsqueryproperty/conf/tars.tarsqueryproperty.config.conf
    root      32718     1 0 09:20 pts/0    00:00:12
    /usr/local/app/tars/tarsquerystat/bin/tarsquerystat
    --config=/usr/local/app/tars/tarsquerystat/conf/tars.tarsquerystat.config.conf

As you can see from the output, servers such as tarsregistry, tarsAdminRegist, and tarsconfig are currently running.

In case one or more servers fail to start, you should add monitor.sh to crontab, which automatically performs checks on the servers. If a server fails, crontab will automatically reboot the server.

Add the following line to crontab：

    * * * * * /usr/local/app/tars/tarsnode/util/monitor.sh


## Core Framework Servers: Windows

TARS is ultimately made up of several core modules as shown below:

    c:\tars\cpp\deploy>busybox.exe ps | busybox.exe grep tars

    14592 14400  0:00  5:05   tarsAdminRegist.exe
    8892 14520  0:00  5:02   tarsregistry.exe
    8524  7916  0:00  5:01   tarsconfig.exe
    9400  9624  0:01  4:59   tarsnode.exe
    10488  5348  0:00  4:57   tarsnotify.exe
    10460  8356  0:01  4:56   tarsproperty.exe
    8536  4384  0:00  4:48   tarsqueryproper.exe
    1268 13708  0:00  4:45   tarsquerystat.exe
    7308 10560  0:00  4:37   tarsstat.exe
    11760 14236  0:00  4:34   tarslog.exe
    4180 14428  0:00  4:32   tarspatch.exe

For the master framework node, TarsNode and TarsWeb must be alive to ensure that other TARS servers will be automatically pulled up by TarsNode.

If the master node crashes, the slave node will be the backup plan to ensure the servers function properly.
To ensure that core framework servers are started, it can be controlled through c:\tars-install\tars\tarsnode\util\monitor.bat, set to Windows scheduled task.


## TarsWeb Description: Windows

After your TARS framework is deployed, the TarsWeb will be installed on the master node. The TarsWeb is implemented by Node.js and consists of two modules which are discussed later in this section.

PM2 is a process manager that we can use to start, stop, restart and delete processes. To check all the running processes, and view the TarsWeb modules, use the command below:

pm2 list

You should see the following output:

C:\tars-install\web> pm2.cmd list
┌─────┬─────────────────────┬─────────────┬─────────┬─────────┬──────────┬────────┬──────┬───────────┬──────────┬──────────┬──────────┬──────────┐
│ id  │ name                │ namespace   │ version │ mode    │ pid      │ uptime │ ↺    │ status    │ cpu      │ mem      │ user     │ watching │
├─────┼─────────────────────┼─────────────┼─────────┼─────────┼──────────┼────────┼──────┼───────────┼──────────┼──────────┼──────────┼──────────┤
│ 0   │ tars-node-web       │ default     │ 2.4.0   │ fork    │ 9700     │ 19m    │ 0    │ online    │ 1%       │ 63.4mb   │ user     │ disabled │
│ 1   │ tars-user-system    │ default     │ 2.0.1   │ fork    │ 8612     │ 19m    │ 0    │ online    │ 1%       │ 54.4mb   │ user     │ disabled │
└─────┴─────────────────────┴─────────────┴─────────┴─────────┴──────────┴────────┴──────┴───────────┴──────────┴──────────┴──────────┴──────────┘

Similar to Linux/Mac, Windows TarsWeb consists of two modules:

tars-node-web: TarsWeb homepage service, default binding 3000 port, source code corresponding web directory.
tars-user-system: The authority management service is responsible for managing all relevant authorities, and is bound to port 3001 by default, source code corresponding web/demo directory.
Note that tars-node-web calls tars-user-system to complete the relevant permission verification.

The web is implemented by Node.js + Vue. The final installation and operation directory is as follows:

c:\tars-install\web

If pm2.cmd list shows that tars-node-web and tars-user-system fail to start, you can enter the directory to locate the problem:

cd c:\tars-install\web\demo; npm.cmd run start
cd c:\tars-install\web; npm.cmd run start

Next, run npm.cmd run start. We can observe the output of the console. The commands are:

pm2.cmd start tars-node-web
pm2.cmd start tars-user-system

