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

_Note:_ Maybe it is useful when you build your own [Ubunto VirtualBox image](https://suedbroecker.net/2021/02/01/install-virtualbox-and-setup-a-virtual-machine-with-ubuntu-on-macos/) and with Podman.