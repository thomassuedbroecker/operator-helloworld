# Operator Framework Ansible Training

In this training you will automate the deployment of a `Hello World!` application to OpenShift, with the `Hello World` operator you will build.

The automation contains:

* the deployment of the application 
  * Create deployment
  * Create service
  * Create route
    * Therefor we will collect application domain information

* Verify the existance of the "helloworld" name space

This training will show you how to setup a development environment and deploy your first Operator written in Ansible. Upon completing this training you will learn the following:

* Setup Ansible Operator Development Environment
* Create Operator Scaffolding and CRD/CR (APIs)
* Test and Debug Operators
* Read parameter inputs from CRs into Ansible facts
* Read and create k8s objects using the k8s Ansible Module
* Deploy Application

## Prerequisites

* A fedora 30 or higher system with access to the Internet
* OpenShift 4.x Cluster environment and a cluster admin account

