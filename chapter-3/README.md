# Introduction

Chapter 2 focused on installing TARS Framework’s components on your machine and briefly explained these items from a deployment point of view. In this chapter, we will discuss the fundamental concepts behind the TARS Framework and gain a deeper understanding of TARS’ development environment.

This chapter will give more detailed explanations of the rules and conventions that would help you during your development process in the TARS Framework. After you have a firm grasp on these basic concepts, we will dive deeper into TarsWeb by illustrating its structure and the development workflow. Through this chapter, we hope that you can become more familiar with TARS and manage your services on the platform with ease.

By the end of this chapter, you should be able to:

- Understand TARS basic concepts.
- Describe the structure of the TarsWeb.
- Understand how to work in the TarsWeb.

## TARS Concepts

In this section, we will introduce some fundamental concepts that you need to understand to develop your project in the TARS framework. We will also provide the rules and conventions used in the framework you should follow to ensure a smooth TARS experience. The topics covered here include service naming rules, template configuration, startup configuration, tars file directory specification, server development mode, client development mode, and development and debugging release.

## TARS Concepts: Service Naming Rules

Before you get started with the development process, it is crucial to understand the three-layer structure of a service’s full name. This structure can avoid any conflict when the server and servant names adopted by different developers overlap. Please keep this in mind when devising your development plan accordingly.

A full name of a service using the TARS framework has three components:

APP
Server
Servant
Let’s discuss them in more detail.

APP refers to the application name, which identifies a group of services. Developers can define it according to their needs, usually based on the functionality of the service/project. Please keep in mind the following rules:

The application name must be unique.
As a convention, the application name corresponds to a namespace or package name in the code.
Please note that the application name "tars" is used by the framework and should not be used for business services.
Server refers to the service name, the name of the process that provides services. Please keep in mind the following rules:

Usually, the server name is named according to the business service function. It will be displayed on the left service tree of the TarsWeb as shown below.

Server name
A server must belong to an APP, and each server name under the APP is unique.
Follow this format: xxServer. For example, LogServer and TimerServer.
A server represents an independent program, is bound to at least one IP, and implements a set of related interfaces.
Servant is a service provider, which provides one or more specific interfaces. Please keep in mind the following rules:

Servant corresponds to a class in the service code, which inherits from the interface in the tars protocol file.
A servant must belong to a server. The servant names under the server are unique.
Servant needs an obj name, such as HelloObj. When provided to the client, the full name is shown in the following format, App.Server.Servant (e.g., Test.HelloServer.HelloObj).
Each servant refers to an independent port of the server.
When the client calls the server, it only needs to specify the name of the servant to complete remote communication.