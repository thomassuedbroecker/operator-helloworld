# Operator Framework Ansible Workshop

### Introduction

In this workshop you will automate the deployment of an `Hello World!` application to OpenShift, with an `Hello World` operator, implemented with [Ansible](https://www.ansible.com/).

The automation of your simple operator will do for you:

* the deployment of the application 
    * Create deployment
    * Create service
    * Create route
        * Therefor we will collect application domain information

Your operator will have capability **Level 1** as you see in the image below.

_Note:_ The image resource you can find on the [Operator Framework: What?](https://operatorframework.io/what/) page.

![](./images/capability-model-operatorframework.png)

### Objectives

This workshop will show you how to setup a development environment and deploy your first Operator written in Ansible. 

Upon completing this workshop you will learn the following:

* Setup Ansible Operator Development Environment
* Create Operator Scaffolding and [Custom Resource Definition (CRD/RD)](https://docs.openshift.com/container-platform/4.5/rest_api/extension_apis/customresourcedefinition-apiextensions-k8s-io-v1.html)
* Test and Debug Operators
* Read parameter inputs from [Custom Resource Definition (CRD/RD)](https://docs.openshift.com/container-platform/4.5/rest_api/extension_apis/customresourcedefinition-apiextensions-k8s-io-v1.html) into [Ansible facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html)
* Read and create k8s objects using the [k8s Ansible Module](https://docs.ansible.com/ansible/latest/collections/community/kubernetes/k8s_info_module.html)
* Deploy Application

### Estimated time and level

|  Time | Level |  
| - | - | 
| 60 min | beginner to intermediate  | 

> _Note:_ The installation of all prerequistes isn't included.

### Prerequisites

* A fedora 30 or higher system with access to the Internet
* OpenShift 4.x Cluster environment and a cluster admin account

It would be good, if you are basicly familar with ...

* ... [Podman](https://podman.io/) or [Docker](https://www.docker.com/get-started)
* ... using container registries like [Quay.io](https://quay.io/) 
* ... handle `YAMLs`
* ... deployments of containers to [RedHat OpenShift](https://www.openshift.com/)

### Technology/Frameworks Used

* [Ansible](https://www.ansible.com/)
* [Operator Framework](https://operatorframework.io/)
* [Ansible Runner](https://github.com/ansible/ansible-runner)

_Additional information:_ 

Following tools are needed to be installed on your local machine:

* [Python](https://www.python.org/)
* [Podman](https://podman.io/) or [Docker](https://www.docker.com/get-started)
* [curl](https://curl.se)
* [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started) (depending on your cloud provider)
* [OpenShift CLI (oc)](https://docs.openshift.com/container-platform/4.5/welcome/index.html)
* [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/reference/kubectl/kubectl/)
* [GO](https://golang.org/)
* [make](https://en.wikipedia.org/wiki/Make_(software))
* [0perator SDK](https://sdk.operatorframework.io/) using [Ansible](https://www.ansible.com/)

### Credits

* [Keith Tenzer](http://keithtenzer.com) (creater of the initial version of the workshop)
* [Thomas Südbröcker](https://twitter.com/tsuedbroecker)

### Additional resources

YouTube "How it does work?":

* [Building Kubernetes Operators with the Operator Framework and Ansible (Keith Tenzer)](https://youtu.be/5XZZxhwb_xs)
* [Kubernetes Operators Explained](https://youtu.be/i9V4oCa5f9I)
* [What is Ansible?](https://youtu.be/fHO1X93e4WA)
* [Operators on OpenShift Container Platform 4.x](https://youtu.be/JMrxPyv9nxQ)

Operator Resources:

* [Operator SDK](https://sdk.operatorframework.io/)
* [Operator Hub (OpenShift operators)](https://operatorhub.io/?category=OpenShift+Optional)
* [Operator Hub (IBM operators)](https://operatorhub.io/?keyword=IBM)

Internal OperatorHub in your RedHat OpenShift cluster.

![](./images/operatorhub.png)



