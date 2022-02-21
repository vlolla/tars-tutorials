
# Description of TARS Framework

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

