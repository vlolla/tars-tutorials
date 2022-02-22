# Chapter 2:  TARS Environment Setup

Now that you have learned about TARS and its functionalities, it’s time to set it up on your machine. This chapter will discuss three methods to deploy TARS Framework—Framework Docker deployment, Source Compilation deployment, and Kubernetes deployment. Although the Source Compilation method can understand the fundamentals of TARS Framework thoroughly, it requires you to go through more steps manually, which is more complicated and time-consuming. On the other hand, deployment through Docker is more simplified and suitable for TARS beginners who need to learn interface operations and the framework’s development process. Docker’s drawback is that it requires you to perform more steps when there is a service failure or a need to expand service capacity. In contrast, the Kubernetes deployment uses a container orchestration mechanism to enable automatic capacity expansion and failure recovery, but the method is not yet mature since it was recently open-sourced. If you are relatively new to TARS, we recommend the Framework Docker deployment to start your TARS journey.

We will demonstrate how to set up your TARS environment using Docker, Source Compilation, and Kubernetes. After you master these methods to install TARS, we will discuss the framework components, namely the core servers, TarsWeb, and TarsNode. Lastly, we will present the instructions to deploy your development environment in TarsGo, TarsJava, TarsCPP, TarsPHP, and Tars.js, which correspond to the different programming languages TARS supports.

## Learning Objectives

By the end of this chapter, you should be able to:

- Understand the functionality of the components of TARS Framework.
- Explain the three primary ways to deploy TARS: Framework Docker deployment, Source Compilation deployment, and Kubernetes deployment.
- Discuss the TARS deployment requirements for all three approaches.
- Explain what TarsWeb and TarsNode are.
- Set up the appropriate development environment according to programming languages.



## Modules in this chapter.
[Tars Framework](./tars.md)

[Tars Environment Setup](./environment.md)

[TarsNode Deployment](./tarsnode.md)

[TARS Servers, TarsWeb, and Development Environment Setup, Completed](./dev-environment.md)

[Tars Development Environment Setup](./dev-setup.md)