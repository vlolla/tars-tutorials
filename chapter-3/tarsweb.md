# Working in TarsWeb

## Service Deployment

After the development stage is completed, we need to deploy the service into TARS and the instructions are as follows.

Click Operation > Deploy Service in the main menu to see the interface for service deployment. You should see the interface shown below.

Let’s walk through the main menu and what you should put in each field.

- APP, 
The application your service belongs to, e.g., TestApp.
- Server Name, 
The name of your service program, e.g., HelloServer.
- Server Type, 
The language your service program is written in, e.g., java uses tars_java.
- Template, 
The template configuration set by your server.
- Node,
The machine IP where the server is deployed.
- Set,
The set grouping information about a server. Set information includes three parts: set name, set region and set group name. The default is "disable".
- OBJ,
Servant name, such as HelloObj.
- OBJ Endpoint,
The machine IP bound by the server, which is generally the same as the node.
- Port,
The servant bound to the port.
- Port Type,
Can be TCP or UDP.
- Protocol,
The communication protocol used by the application layer, and the TARS service uses the tars protocol by default.
- Threads, 
The number of server processing threads.
- Max Connections, 
Max connections of the server.
- Max Queue, 
The size of the request receiving queue.
- Queue Timeout, 
The timeout of the request receiving queue.

Click “Submit” to deploy your service into TARS. If successful, the name of HelloServer will appear in the TestApp application under the left service tree, and the information about your new server program will be seen on the right, as shown below.

## Service Publishing

To publish your service, go to the management system’s service tree and find the server you deployed and click to enter the service page.

Select “Publish”, select the node to publish, click the Publish Node button, click “upload release package”, and select your compiled package, as shown in the following image.

< insert image>

After uploading the package, click the "release version" drop-down box to display the packages you uploaded, and select the top one (the latest one uploaded). Click "Publish" to publish the server. You will see the following interface if you have successfully published the server.


< insert image>

If the publishing fails, you might want to check if it is naming issues, upload issues, and other environmental issues.

