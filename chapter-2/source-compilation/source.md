# Deploy TARS Framework with Source Compilation

In this section, we will discuss how to deploy the TARS framework using the Source Compilation approach. Although the steps to deploy by source are the same for all OSs, the methods and commands used in Linux/Mac as opposed to Windows are slightly different because of the inherent differences in the operating systems. Therefore, the Source Compilation approach has two sets of instructions, one for Linux/Mac users and the other for Windows users.

- The main steps to deploy TARS Framework:
- Check your dependent environment
- Download and compile TarsFramwork source code
- Download TarsWeb and deploy TarsFramework
- Deploy TarsNode

## Check Your Dependent Environment: Linux/Mac Requirements

Please check if your dependent environment satisfies TARS deployment requirements, as shown in the below table.

 
Hardware/Software

Requirements

Hardware

A machine running Linux or Mac
Linux kernel

2.6.18 or later
(Dependent OS)
GCC

4.8.2 or later; glibc-devel
(Dependent C++ framework tools）
Bison

2.5 or later
(Dependent C++ framework tools）
Flex

2.5 or later
(Dependent C++ framework tools）
CMake

3.2 or later
(Dependent C++ framework tools）
nvm

0.35.1 or later
(Dependent web management system, auto-install while deploying）
node

12.13.0 or later
(Dependent web management system, auto-install while deploying）
git

Any version
 

Before you can compile a source, you must install the following tools: gcc, glibc-devel, bison, flex, cmake, which, psmisc, ncurses-devel, and zlib-devel. These are dependent libraries that are required for the framework to operate successfully. You have the options below to install these tools.

Here we provide instructions for CentOS, Ubuntu, and macOS as a demonstration.

For CentOS, run:

yum install glibc-devel gcc bison flex cmake which psmisc ncurses-devel zlib-devel

For Ubuntu:

sudo apt-get install build-essential bison flex cmake psmisc libncurses5-dev zlib1g-dev

Please note that Linux released different versions of tools for different distributions (e.g., Ubuntu, CentOS or Red Hat), which resulted in variations in the naming of the packages (e.g., zlib-devel vs zlib1g-dev), but the files are mostly the same.

For macOS:

Please install brew first (for more information on how to install brew, see the Homebrew Documentation).

brew install bison flex cmake nvm node

Some of the packages mentioned above (e.g., gcc) are not listed in this command because they are installed on Mac by default.


## Check Your Dependent Environment: Windows Requirements


    Windows >=win7
    CMake 3.2 or later
    (Dependent c++ framework tools）

    Git Any version

    nvm 0.35.1 or later

    Node.js 12.13.0 or later


## Download and Compile TarsFramework Source Code

Linux/Mac and Windows users can download TarsFramework source code using the same method. We first need to build a development environment using the following steps.

Download the source code of the repo tarsframework:

    cd ${source_folder}
    git clone https://github.com/TarsCloud/TarsFramework.git --recursive

Enter the directory of build：

    cd TarsFramework
    git submodule update --remote --recursive
    cd build
    cmake ..
    make -j4

By default, when we compile TARS, it downloads MySQL-client source code and automatically compiles the libmyqlclient.a file.

Change to root and create an installation directory:

    cd /usr/local
    sudo mkdir tars
    sudo mkdir app

Return to TarsFramework/build/ and run the commands below:

    cd build
    sudo make install

After you finish downloading, you should see the installation package in the directory /usr/local/tars/cpp where the compiled framework and the installation script are also located. The default path for TarsFramework after the installation is /usr/local/app.

If you want to install on different paths, you need to:

    modify tarscpp/cmake/Common.cmake

    modify TARS_PATH in tarscpp/servant/makefile/makefile.tars

    modify TARS_PATH in tarscpp/servant/makefile/tars-tools.cmake

    modify DEMO_PATH in tarscpp/servant/script/create_tars_server.sh


Please follow below links to deploy Tars Framework on Linux/mac and Windows.

[Deploy TarsFramework: Linux/Mac Users
](./linux.md)

[Deploy TarsFramework: Windows Users](./windows.md)




## TARS Control Scripts

if you deploy TARS Framework using the Source Compilation approach, you can control the start and stop of the servers using the below scripts (for Docker approach, you can do the same thing in Docker).

Start TARS Framework servers: /usr/local/app/tars/tars-start.sh

Stop TARS Framework servers: /usr/local/app/tars/tars-stop.sh

Start & Stop one server:
- /usr/local/app/tars/xxxx/util/start.sh
- /usr/local/app/tars/xxxx/util/stop.sh

*xxxx: fill in the directory where you locate your servers.

Note:

In the core servers of the framework, TarsNode must be alive as it will monitor other servers. If a server crashes, it will be automatically rebooted.

Pm2 monitors TarsWeb.

After the machine that has deployed the framework restarts, it can execute /usr/local/app/tars/tars-start.sh to restart all servers.

TarsNode monitoring can be performed regularly in crontab: /usr/local/app/tars/tarsnode/util/check.sh

The application node with TarsNode deployed needs to monitor only the TarsNode.