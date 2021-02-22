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

### Add created Operator to a custom catalog in your RedHat OpenShift cluster

Documentation [OLM - Managing Custom Catalogs](https://docs.openshift.com/container-platform/4.5/operators/admin/olm-managing-custom-catalogs.html#olm-managing-custom-catalogs-bundle-format)


[Installing `opm`](https://docs.openshift.com/container-platform/4.5/operators/admin/olm-managing-custom-catalogs.html#olm-installing-opm_olm-managing-custom-catalogs)

#### Step 1: Prepare to save the credentials

```sh
REG_CREDS=${XDG_RUNTIME_DIR}/containers/auth.json
```

#### Step 2: Logon to the RedHat Registry

```sh
podman login registry.redhat.io
```

#### Step 3: create a folder called catalog

```sh
mkdir catalog
```

#### Step 4: Change to the newly created directory

```sh
cd catalog
```

#### Step 5: Download the `opm` Operator Manager CLI

```sh
oc image extract registry.redhat.io/openshift4/ose-operator-registry:v4.5 \ -a ${REG_CREDS} \ --path /usr/bin/opm:. \ --confirm
```

* Example output:

```sh
error: directory . must be empty, pass --confirm to overwrite contents of directory
```

#### Step 6: Verify the CLI is downloaded

```sh
ls
```

Example output:

```sh
opm
```

#### Step 7:  Change the execution rights

```sh
chmod +x ./opm
```

#### Step 8:  Move the CLI to the `bin`directory

```sh
sudo mv ./opm /usr/local/bin/
```

#### Step 9: Verify the client CLI installation

```sh
opm version
```

Example output:

```sh
Version: version.Version{OpmVersion:"1.12.3", GitCommit:"", BuildDate:"2020-07-01T23:18:58Z", GoOs:"linux", GoArch:"amd64"}
```

### Create an index image

#### Step 1: Login to the Container registry with the operators we want to use

```sh
docker login quay.io
```

#### Step 2: Login to RedHat OpenShift 

```sh
oc login --token=[YOUR_TOKEN] --server=https://[YOUR_URL].us-south.containers.cloud.ibm.com:[YOUR_PORT]
```

#### Step 3: Creating an index image

[Problem](https://github.com/operator-framework/operator-registry/issues/453#issuecomment-760779372)

```sh
opm index add --bundles quay.io/tsuedbroecker/operator-helloworld:latest --tag quay.io/tsuedbroecker/operator-helloworld-catalog:latest --container-tool podman
```

