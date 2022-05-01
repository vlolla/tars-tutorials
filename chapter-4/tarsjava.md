# Build Your Service in TarsJava

This section provides the instructions to develop a Hello World service in TarsJava, and it is split into three main steps:

- Define the tars file
- Server development
- Client development

Since TARS employs the server-client model for development, we will first define the tars file that describes the protocol communication interface, then we will build the server and create the client.

Plesae refer more for the examples on Java -> https://github.com/TarsCloud/TarsJava



# Build Your Service in TarsJava: Define the Tars File

As the first of server development in TarsJava, we will define the tars file that specifies request methods and field parameters. Let’s create the hello.tars file in the src/main/resources directory and fill the file with the following content:

    module TestApp
    {
        interface Hello
        {
            string hello(int no, string name);
        };
    };

Next, we can compile the tars file with the below steps. Add the following content into tars-maven-plugin for generating Java file configuration:

    <plugin>
        <groupId>com.tencent.tars/<groupId>
        <artifactId>tars-maven-plugin/<artifactId>
        <version>1.6.1</version>
        <configuration>
            <tars2JavaConfig>
                <!-- tars file locate-->
                <tarsFiles>

    <tarsFile>${basedir}/src/main/resources/hello.tars/<tarsFile>
                    </tarsFiles>
                    <!-- source encoding-->
                    <tarsFileCharset>UTF-8</tarsFileCharset>
                    <!-- true:create server source code, false: create client source code-->
                    <servant>true</servant>
                    <!-- generated source file encoding-->
                    <charset>UTF-8</charset>
                    <!-- generated source file locate -->
                    <srcPath>${basedir}/src/main/java</srcPath>
                    <!-- package prefix -->

    <packagePrefixNamecom.qq.tars.quickstart.server./packagePrefixName>
            </tars2JavaConfig>
        </configuration>
    </plugin>

After executing mvn tars:tars2java in the project root directory, the following lines of code will be generated automatically in HelloServant.java:

    @Servant
    public interface HelloServant {

        public String hello(int no, String name);
    }

The tars file is completed and we can move to create the server.

## Build Your Service in TarsJava: Server Development

Developing a server in TarsJave requires three steps, which include interface implementation, server configuration, and server compilation and packaging.

First, we need to create a HelloServantImpl.java file to implement the interface named HelloServant.java. In order to do so, please run the below commands:

    public class HelloServantImpl implements HelloServant {

        @Override
        public String hello(int no, String name) {
            return String.format("hello no=%s, name=%s, time=%s", no, name, System.currentTimeMillis());
        }
    }

Next, we need to create a services.xml configuration file under resources/. Please fill services.xml with the following:

    <?xml version="1.0" encoding="UTF-8"?>
    <servants>
        <servant name="HelloObj">

    <home-api>com.qq.tars.quickstart.server.testapp.HelloServant</home-api>

    <home-class>com.qq.tars.quickstart.server.testapp.impl.HelloServantImpl</home-class>
        </servant>
    </servants>

After the service is written, it needs to load the configuration exposure service when the process starts.

For server compilation and packaging, we need to execute the mvn package in the project root directory to generate the war package, which can be later published by the TARS management system.

## Build Your Service in TarsJava: Client Development

After server development is completed, we will develop the client. First, you need to create a client project. Then, add dependency:

    <dependency> 
        <groupId>com.tencent.tars</groupId>
        <artifactId>tars-client</artifactId>
        <version>1.6.1</version>
        <type>jar</type>
    </dependency>

In the next step you need to add plugins:

    <plugin>
        <groupId>com.tencent.tars</groupId>
        <artifactId>tars-maven-plugin</artifactId>
        <version>1.6.1</version>
        <configuration>
            <tars2JavaConfig>
                    <tarsFiles>

    <tarsFile>${basedir}/src/main/resources/hello.tars/<tarsFile>
                    </tarsFiles>
                    <tarsFileCharset>UTF-8</tarsFileCharset>
                    <!-- create source code，PS: Client call, must be false here -->
                    <servant>false</servant>
                    <charset>UTF-8</charset>
                    <srcPath>${basedir}/src/main/java</srcPath>

    <packagePrefixName>com.qq.tars.quickstart.client.</packagePrefixName>

            </tars2JavaConfig>
        </configuration>
    </plugin>

Generate the code for the client according to the service tars interface file:

    @Servant
    public interface HelloPrx {

        public String hello(int no, String name);

            public String hello(int no, String name, @TarsContext java.util.Map<String, String> ctx);

            public void async_hello(@TarsCallback HelloPrxCallback callback, int no, String name);

            public void async_hello(@TarsCallback HelloPrxCallback callback, int no, String name, @TarsContext java.util.Map<String, String> ctx);
    }


Run the following commands to set up a synchronous call:

public static void main(String[] args) {
     CommunicatorConfig cfg = new CommunicatorConfig();
        //create Communicator
        Communicator communicator = CommunicatorFactory.getInstance().getCommunicator(cfg);
        //create proxy by Communicator
        HelloPrx proxy = communicator.stringToProxy(HelloPrx.class, "TestApp.HelloServer.HelloObj");
        String ret = proxy.hello(1000, "HelloWorld");
        System.out.println(ret);
}

Run the commands below to set up an asynchronous call:

public static void main(String[] args) {
     CommunicatorConfig cfg = new CommunicatorConfig();
        //create Communicator
        Communicator communicator = CommunicatorFactory.getInstance().getCommunicator(cfg);
        //create proxy by Communicator
        HelloPrx proxy = communicator.stringToProxy(HelloPrx.class, "TestApp.HelloServer.HelloObj");
        proxy.async_hello(new HelloPrxCallback() {

           @Override
           public void callback_expired() {
           }

           @Override
           public void callback_exception(Throwable ex) {
           }

           @Override
           public void callback_hello(String ret) {
                System.out.println(ret);
           }
        }, 1000, "HelloWorld");
}

Tips: Communicator manages the client's resources, so using a global object would be better.

Good job! You’ve successfully built a Hello World service in TarsJava. We encourage you to develop an HTTP server on your own in TarsJava. Feel free to reference the procedures about HTTP server development from the previous sections on TarsGo, TarsPHP, or Tars.js.

