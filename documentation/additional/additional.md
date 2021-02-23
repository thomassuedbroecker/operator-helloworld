# Additional information

### Relevant files in the `Asible Operator SDK` 

* `[PROJECT_ROOT]/watches.yaml`

By default it will create a single role, but you can certainly have many roles. Roles are mapped to the API endpoint of the CRD in the `watches.yaml` file.

* `[PROJECT_ROOT]/roles/hello/tasks/main.yml`

Defining the task for the role hello.

* `[PROJECT_ROOT]/config/samples/cache_v1_hello.yaml`

With this `YAML` file you create a custom resource in RedHat OpenShift.

* `[PROJECT_ROOT]/config/rbac/role.yaml`

This role `YAML` file containes the permissions we need to create and modify resources in RedHat OpenShift.

* `[PROJECT_ROOT]/config/default/kustomization.yaml`

This kustomization `YAML file contains the information for the container image we will create as our operator.

### Using `Podman` in the `Makefile`

If you use `Podman` you need to edit the Makefile replace `docker` with `podman`.

```make
...
# Build the docker image
docker-build:
	podman build . -t ${IMG}

# Push the docker image
docker-push:
	podman push ${IMG}
...
bundle-build:
	podman build -f bundle.Dockerfile -t $(BUNDLE_IMG) .
...
```

### Building a Ubuntu VirtualBox image

 Maybe this is useful when you build your own [Ubunto VirtualBox image](https://suedbroecker.net/2021/02/01/install-virtualbox-and-setup-a-virtual-machine-with-ubuntu-on-macos/) and with Podman.

### Understand Custom Resource definition

For more details visit Kubernetes Documentation for [Custom Resource Definitions](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions)

#### Step 1: Set project context

```sh
oc project default
```
#### Step 2: Get pod for this project

```sh
oc get pods
```

#### Step 3: Get pod custom resource

```sh
oc get awesome
```

* Example output:

```sh
error: the server doesn't have a resource type "awesome"
```

#### Step 4: Define custom resource

Create a file called `awesome-crd.yaml`and past in following content.

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  # name must match the spec fields below, and be in the form: <plural>.<group>
  name: awesomes.my.example.com
spec:
  # group name to use for REST API: /apis/<group>/<version>
  group: my.example.com
  # list of versions supported by this CustomResourceDefinition
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            host:
              type: string
            port:
              type: string
  # either Namespaced or Cluster
  scope: Namespaced
  names:
    # plural name to be used in the URL: /apis/<group>/<version>/<plural>
    plural: awesomes
    # singular name to be used as an alias on the CLI and for display
    singular: awesome
    # kind is normally the CamelCased singular type. Your resource manifests use this.
    kind: Awesome
    # shortNames allow shorter string to match your resource on the CLI
    shortNames:
    - as
```

```sh
oc create -f awesome-crd.yaml
```

* Example output:

```sh
customresourcedefinition.apiextensions.k8s.io/awesomes.my.example.com created
```

#### Step 5: Get existing resources

```sh
oc get awesomes 
```

* Example output:

```sh
No resources found in default namespace.
```

#### Step 6: Create a resource instance

Now we use our defined custom resource. 
Create a file called `awesome-crd-create.yaml`and past in following content.

```yaml
apiVersion: my.example.com/v1
kind: Awesome
metadata:
  name: my-awesome-instance
spec:
  user: my-awesome-user
```

```sh
oc create -f awesome-crd-create.yaml
```

* Example output:

```sh
awesome.my.example.com/my-awesome-instance created
```

#### Step 7: Get instance

```sh
oc get awesomes
```

* Example output:
```sh
NAME                  AGE
my-awesome-instance   38s
```

