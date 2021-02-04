# Exercise 3

In this exercise you will complete the following:

* Create a [Quay.io](https://quay.io/) account
* Build image of our Operator and push it to [Quay.io](https://quay.io/)
* Deploy Operator to OpenShift cluster

### Step 1: Create Quay.io Account

Quay.io us a container registry provided by Red Hat. You can create your own account push container images to it. Each image can be public or private. To make images available to OpenShift you will need to make them public.

Go to [Quay.io](https://quay.io/) and create your own account if you don't have one.

### Step 2: Build Operator image and push to quay.io

```sh
sudo make docker-build docker-push IMG=quay.io/ktenzer/operator-helloworld:latest
```

Make the `operator-helloworld` image in your quay.io account public. Log into quya.io, click on the image. Under settings (on the left) there is option to make the image public.

### Step 3: Deploy Operator to OpenShift Cluster

By default the operator will be deployed to a project called operator-helloworld-system. You can change this by editing the `config/default/kustomization.yaml` file.

```sh
make deploy IMG=quay.io/ktenzer/operator-helloworld:latest
```

Check Operator Deployment

```sh
oc get deployment -n operator-helloworld-system
```

Example output:

```sh
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
helloworld-controller-manager   1/1     1            1           37s
```

### Step 4: Deploy Helloworld Application using Operator

Using the Operator we just deployed into the `operator-helloworld-system` namespace we will now deploy the application using CR.

* Create the operator

```sh
oc create -f config/samples/cache_v1_hello.yaml -n operator-helloworld-system
```

* Get the deploment information

```sh
oc get deployment -n operator-helloworld-system
```

Example output:

```
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
helloworld                      1/1     1            1           12m
helloworld-controller-manager   1/1     1            1           12m
```

### Step 5: Cleanup Application
Removing the CR will delete everything that was created by it since the objects are linked to the CR.

```sh
oc delete hello hello-sample -n operator-helloworld-system
```

### Step 6: Cleanup Operator

This will remove the Operator, CRD and all the roles.

```sh
make undeploy
```

**Congrats**, if you got this far you are ready to write your own Operators in Ansible! :smiley:
