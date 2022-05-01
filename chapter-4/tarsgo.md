# Build Your Service in TarsGo

This section provides the instructions to develop a Hello World service using Go in TARS, and it is split into three main steps:

- Define the tars file
- Server creation and development
- Client development

Since TARS employs the server-client model for development, we will first define the tars file that describes the protocol communication interface, build the server, and create the client. After successfully developing a service, we will show how to develop a HTTP server, which is a feature supported by TarsGo.

Please refer -> https://github.com/TarsCloud/TarsGo


## Build Your Service in TarsGo: Define the Tars File

As we’ve mentioned in Chapter 1, the TARS framework supports agile software development. This feature is evident when we use TarsGo to develop a service, as we will run the script create_tars_server.sh to automatically create all the required files to create the server. With these files auto-generated, we only need to focus on the logic of the program. Please run the below command to execute create_tars_server.sh:

    sh $GOPATH/src/github.com/TarsCloud/TarsGo/tars/tools/create_tars_server.sh [App] [Server] [Servant]

For example:

    sh $GOPATH/src/github.com/TarsCloud/TarsGo/tars/tools/create_tars_server.sh TestApp HelloGo SayHello

If there is a syntax error during execution, try to first use the dos2unix create_tars_server.sh file to transcode (the dos2unix command is a DOS/Mac to UNIX text file format converter).

After the command is executed, the code will be generated to GOPATH, and the directory will be named in the format of APP/Server. The specific path will also be prompted in the generated code. Here is the output of the above example code:

    [root@1-1-1-1 ~]# sh $GOPATH/src/github.com/TarsCloud/TarsGo/tars/tools/create_tars_server.sh TestApp HelloGo SayHello
    [create server: TestApp.HelloGo ...]
    [mkdir: $GOPATH/src/TestApp/HelloGo/]
    >>>Now doing:./start.sh >>>>
    >>>Now doing:./Server.go >>>>
    >>>Now doing:./Server.conf >>>>
    >>>Now doing:./ServantImp.go >>>>
    >>>Now doing:./makefile >>>>
    >>>Now doing:./Servant.tars >>>>
    >>>Now doing:client/client.go >>>>
    >>>Now doing:vendor/vendor.json >>>>
    # runtime/internal/sys
    >>> Great！Done! You can jump in $GOPATH/src/TestApp/HelloGo
    >>> After editing the tars file, use the following to automatically generate the go file
    >>>       $GOPATH/bin/tars2go *.tars

## Build Your Service in TarsGo: Server Development

Now that we have the required files ready, we can move on to define the tars file, which specifies the request method, parameter field type, etc. The client will rely on the tars file to call the server. As the output from the previous page suggests, we need to enter the $GOPATH/src/TestApp/HelloGo directory and edit the tars file.

We can then test our definition of an echoHello interface, with the client request parameter being a short string such as "tars" and the service response “Hello tars”.

The content of the $GOPATH/src/TestApp/HelloGo/SayHello.tars file will be:

    module TestApp{
    interface SayHello{
        int echoHello(string name, out string greeting);
    };
    };

Note: The keyword out in the above parameter identifies the output parameter.

Next, convert the tars file into the form of golang language using the $GOPATH/bin/tars2go SayHello.tars command.

At this point, we’ve finished defining the tars file, and we can start to implement the logic of the server. Our Hello, World program will have the following logic: the client sends a name → the server responds to Hello and the name sent by the client.

Navigate to $GOPATH/src/TestApp/HelloGo/SayHelloImp.go, and modify the file with the following content:

    package main

    type SayHelloImp struct {
    }

    func (imp *SayHelloImp) EchoHello(name string, greeting *string) (int32, error) {
        *greeting = "hello " + name
        return 0, nil
    }

Note: The function name should be capitalized, following the syntax of golang.

Then, navigate to $GOPATH/src/TestApp/HelloGo/HelloGo.go and fill it with the following:

    package main

    import (
        "github.com/TarsCloud/TarsGo/tars"

        "TestApp"
    )

    func main() { //Init servant
        imp := new(SayHelloImp)
    //New Imp
        app := new(TestApp.SayHello)
    //New init the A JCE
        cfg := tars.GetServerConfig()
    //Get Config File Object
        app.AddServant(imp, cfg.App+"."+cfg.Server+".SayHelloObj")
    //Register Servant
        tars.Run()
    }

After we finish writing SayHelloImp.go and HelloGo.go, we can compile HelloGo to build the executable file. Then, we need to make a package to release the service. Run the below command to generate the executable file HelloGo and the package HelloGo.tgz.

    cd $GOPATH/src/TestApp/HelloGo/ && make && make tar

We have now finished writing the server.

## Build Your Service in TarsGo: Client Development

The next step is to set up the client. First, we will navigate to client.go and fill it with the following:

    package main

    import (
            "fmt"
            "github.com/TarsCloud/TarsGo/tars"

            "TestApp"
    )

    //Only need to initialize once. It is global
    var comm *tars.Communicator
    func main() {
            comm = tars.NewCommunicator()
            obj := "TestApp.HelloGo.SayHelloObj@tcp -h 127.0.0.1 -p 3002 -t 60000"
            app := new(TestApp.SayHello)
            /*
            // if your service has been registered at tars registry
            comm = tars.NewCommunicator()
            obj := "TestApp.HelloGo.SayHelloObj"
            // tarsregistry service at 192.168.1.1:17890
            comm.SetProperty("locator", "tars.tarsregistry.QueryObj@tcp -h 192.168.1.1 -p 17890")
            */

            comm.StringToProxy(obj, app)
            reqStr := "tars"
            var resp string
            ret, err := app.EchoHello(reqStr, &resp)
            if err != nil {
                    fmt.Println(err)
                    return
            }
            fmt.Println("ret: ", ret, "resp: ", resp)
    }

Parameter Interpretation:

- import/TestApp, 
It is a library generated by tars2go.
- obj, 
It specifies the IP port of the server.

-If the server is not registered in the TarsRegistry, you need to know the IP and port of the server to specify it in obj. In the above example, the protocol is TCP, the address of the server is 127.0.0.1, and the port is 3002. If there are multiple servers, you can write TestApp.HelloGo.SayHelloObj@tcp -h 127.0.0.1 

-p 9985:tcp -h 192.168.1.1 -p 9983 so that the request can be distributed to multiple ends.

-If the server has been registered in TarsRegistry, you do not need to write the server IP and port, but you need to specify the TarsRegistry address when initializing the communicator.
- comm, 
It means communicator. For communication with the server, it needs to be a global variable.
Next, we compile and test the client by running the following commands:

        #go build client.go
        #/client
        ret: 0 resp: hello tars

Great job! You’ve finished building a Hello World program in TarsGo. In this section of TarsGo, we’ve introduced how to create a service based on the tars protocol and deploy it on the TARS platform. Next, we will provide an example of developing a HTTP server TarsGo.

## TARS HTTP Server Development in TarsGo

Recall that the TARS platform does not have any restrictions on communication protocols. This allows us to run a Go server that provides the HTTP protocol on the TARS platform. Although data coding and decoding and the package protocol are different from the tars protocol, the features such as process management, monitoring and reporting, and log collection are still available for use.

The provided script create_tars_server.sh that we used in the beginning to create a server also enables an easy processing of HTTP requests in TarsGo, which is encapsulated in Go native. The instructions to develop a HTTP server are as follows:

To develop a HTTP server, write the below content in $GOPATH/src/TestApp/HelloGo/HelloGo.go:

    package main

    import (
        "net/http"
        "github.com/TarsCloud/TarsGo/tars"
    )

    func main() {
        mux := &tars.TarsHttpMux{}
        mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
            w.Write([]byte("Hello tars"))
        })
            cfg := tars.GetServerConfig()
        tars.AddHttpServant(mux, cfg.App+"."+cfg.Server+".HttpSayHelloObj") //Register http server
        tars.Run()
    }

In addition, you can directly call other TARS servers using steps outlined in the "Client Development" section.