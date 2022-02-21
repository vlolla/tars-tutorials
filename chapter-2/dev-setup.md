# Development Environment Setup

In this section, we will go through the environment requirements for developing in the TARS Framework. As we have mentioned in Chapter 1, there are different programming languages versions of TARS, which include TarsGo, TarsJava, TarsCPP, TarsPHP, and Tars.js. Their development environments are accordingly different while the development mode is similar across the languages.

## General Development Mode

The development mode of service is as follows.

- TARS provides communication protocols between services with customized syntax.
- Each version of TARS provides a set of libraries that can quickly implement services based on the communication protocols mentioned earlier.
- Services written in different programming languages can call each other with the same protocol.
- All services can be managed on TarsWeb.



## TarsCPP

If TARS has been compiled and deployed on the machine, TarsCPP will work automatically. The environment requirements are shown in the table:

 


    linux kernel>=2.6.18

    gcc version>=4.8, glibc-devel

    bison version>=2.5

    flex version>=2.5

    CMake version>=2.8.8

    MySQL version>=4.1.17
 

Please make sure your machine fulfills these requirements. Below, we demonstrate the steps to install TarsPP in CentOS7 (commands should be similar for other OSs). 

Input the following command and run it.

    yum install glibc-devel gcc gcc-c++ bison flex cmake

Next, make the compilation environment of TarsCPP.

    git clone https://github.com/TarsCloud/TarsCpp.git --recursive
    cd TarsCpp
    cmake .
    make
    make install

If you want to enable SSL & http2, try the following commands:

    cmake .. -DTARS_SSL=ON -DTARS_HTTP2=ON
    make
    make install

To disable SSL & http2, run the below commands:

    cmake .. -DTARS_SSL=OFF -DTARS_HTTP2=OFF
    make
    make install

Now your environment is set up, and you can develop application servers in C++.


## TarsGo

TarsGo requires golang = 1.9. X or above. Please follow the steps below to configure your golang environment.

Install TARS:

    go get github.com/TarsCloud/TarsGo/tars

Compile tars2go, which generates code based on interfaces of the Tars file:

    cd $GOPATH/src/github.com/TarsCloud/TarsGo/tars/tools/tars2go && go build .

    cp tars2go $GOPATH/bin

After confirming that the installation of TARS is successful under GOPATH, we can now develop services in golang.

## TarsJava

TarsJava requires JDK >= 1.8 and Maven >= 2.2.1. Please follow these steps to build a project and configure dependency.

Create a Maven web project through IDE or Maven.

Take Eclipse as an example, click File > New > Project > Maven Project > maven-archetype-webapp.

Enter the groupid and artifactid.

After executing, the directory structure will be as follows.

    ├── pom.xml
    └── src
    ├── main
    │   ├── java
    │   │   └── tars
    │   │       └── test
    │   │           ├── HelloServant.java
    │   │           └── HelloServantImpl.java
    │   ├── resources
    │   │   └── servants.xml
    │   └── webapp
    └── test
        ├── java
        │   └── tars
        │       └── test
        │           └── TarsTest.java
        └── resources

Add a dependent jar package in pom.xml of the built project.

The framework dependency configuration and plugin dependency configuration should be as shown below.

<dependency> 
     <groupId>com.tencent.tars</groupId> 
     <artifactId>tars-server</artifactId> 
     <version>1.6.1</version> 
     <type>jar</type> 
</dependency> 

<plugin> 
      <groupId>com.tencent.tars</groupId> 
      <artifactId>tars-maven-plugin</artifactId> 
      <version>1.6.1</version> 
      <configuration> 
           <tars2JavaConfig> 
                <tarsFiles> 

<tarsFile>${basedir}/src/main/resources/hello.tars</tarsFile> 
                 </tarsFiles> 
                 <tarsFileCharset>UTF-8</tarsFileCharset> 
                 <servant>true</servant> 
                 <srcPath>${basedir}/src/main/java</srcPath> 
                 <charset>UTF-8</charset> 

<packagePrefixName>com.qq.tars.quickstart.server.</packagePrefixName> 
           </tars2JavaConfig>
      </configuration> 
</plugin>