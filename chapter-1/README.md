
# Introduction

n this chapter, we will start off by discussing differences between monolithic and microservice architecture. Next we will introduce the TARS Project and we will deep dive into TARS’ microservice ecosystem. We will also compare TARS with other microservice frameworks and discuss some of the advantages of TARS. Last but not least, we will explain what platforms built with TARS consist of and how the service interaction flow looks like.

### By the end of this chapter, you should be able to:

- Have a good understanding of the differences between monolithic and microservice architecture.
- Discuss what TARS is and what its goals are.
- Describe the TARS Project microservice ecosystem.
- Compare different microservice frameworks.
Explain what platforms built with TARS consist of.
- Describe service interaction flow.


# What is TARS

TARS is a new generation distributed microservice applications framework that was created in 2008. It provides developers and enterprises with a complete set of solutions to build, release, deploy and maintain stable and reliable applications that run at scale.

In June 2018, TARS joined the Linux Foundation umbrella and became one of its projects. On March 10th, 2020, it was announced that the TARS Project will transition into the TARS Foundation,

“a neutral home for open source microservices projects that empower any industry to quickly turn ideas into applications at scale”.

The TARS Foundation’s goal is to address the most common problems related to microservices application, including solving multi-programming language interoperability issues, mitigating transfer issues, maintaining consistency of data storage, and ensuring high performance while supporting a growing number of requests.

TARS framework has been successfully used by many companies from diverse industries such as fintech, esports, edge computing, online streaming, e-commerce and education, to name a few.

Check out this video to find out more about TARS applications.


https://www.linuxfoundation.org/blog/the-tars-foundation-the-formation-of-a-microservices-ecosystem/

## Ecosystem Components: Infrastructure

TARS is working in PaaS. It can be run on physical machines, virtual machines and containers, including Docker and Kubernetes.

## Ecosystem Components: Storage

TARS service data can be stored in cache, DB or a file system based on your status.

- For cache, you can use Redis or DCache (a distributed, in-memory NoSQL system which is based on the TARS framework), etc.
- For DB, you can use MySQL, MariaDB, etc.
- For file systems, you can use Ceph, HDFS, etc.