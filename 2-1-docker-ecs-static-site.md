## Deploy a static website using a Docker container on ECS

In this module you will deploy a static website using a [docker container](https://www.docker.com/what-container) on Amazon Elastic Container Service ([Amazon ECS](https://aws.amazon.com/ecs/)). The main aim of this configuration is that you can use it to run Docker applications on a scalable cluster that allows you, for instance, to run the applications easily by eliminating the need for you to install, operate, and scale your own cluster management infrastructure or to scale your containers up or down to meet your application's capacity requirement.

If you are imagining how could be do, should be something like this:


Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

This page describes the commands you can use in a Dockerfile. When you are done reading this page, refer to the Dockerfile Best Practices for a tip-oriented guide.