# Exercise 2

In this exercise you will complete the following:

* Expand [Ansible role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) to get the cluster domain name and save that in a fact
* Expand the [Ansible role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) to deply a helloworld application
* Test the operator using the [ansible-runner](https://github.com/ansible/ansible-runner)
* Deploy operator to namespace so it runs without the [ansible-runner](https://github.com/ansible/ansible-runner)

### Step 1: Update Ansible role to get cluster domain name and save as a fact

Here we will learn to use the k8s Ansible module to gather information we want to use later in our automation. In this case the cluster domain name. Append the following tasks to the Ansible role.

We going to following [Asible](https://docs.ansible.com/) 'keywords':

* [k8s_info](https://docs.ansible.com/ansible/latest/collections/community/kubernetes/k8s_info_module.html)
* [set_fact](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/set_fact_module.html)
* debug

```sh
nano roles/hello/tasks/main.yml
```

Content to addd to the `main.yml`:

```yml
- name: Get Application Domain from Cluster Ingress
  k8s_info:
    api_version: config.openshift.io/v1
    kind: Ingress
    name: cluster
  when: application_domain is undefined
  register: ingress

- name: Set Application Domain
  set_fact:
    application_domain: "{{ ingress.resources[0].spec.domain }}"
  when: application_domain is undefined

- name: Print application domain
  debug:
    msg: "Application domain is {{ application_domain }}"
```

### Step 2: Run Operator using ansible-runner

Each time Operator is started or something changes with our CRD the Ansible role will run and our changes will of course be executed.

```sh
ansible-operator run local
```

Example output:

```sh
TASK [Print application domain] ********************************
ok: [localhost] => {
    "msg": "Application domain is apps.ocp4.keithtenzer.com"
}
```

### Step 3: Update Ansible role to deploy hellowoworld application

Now we will learn to use the k8s Ansible module to deploy an application. We will deploy a helloworld application that prints to STDOUT. Notice the route is using the cluster domain we gathered in the previous step. In this step we will create a deployment, service and route for our helloworld application. Append the following tasks to the Ansible role.

```sh
nano roles/hello/tasks/main.yml
```

* We use `k8s:definition:` to define the `Service`, `Deployment` and `Route

```yml
- name: Deploy helloworld service
  k8s:
    definition:
      apiVersion: v1
      kind: Service
      metadata:
        namespace: "{{ ansible_operator_meta.namespace }}"
        labels:
          app: helloworld
        name: helloworld
      spec:
        ports:
        - port: 8080
          targetPort: 8080
        selector:
          app: helloworld
          name: helloworld
      status:
        loadBalancer: {}

- name: Deploy helloworld app
  k8s:
    definition:
      kind: Deployment
      apiVersion: apps/v1
      metadata:
        name: helloworld
        namespace: "{{ ansible_operator_meta.namespace }}"
        labels:
          app: helloworld
      spec:
        replicas: 1
        strategy:
          type: RollingUpdate
        selector:
          matchLabels:
            app: helloworld
        template:
          metadata:
            labels:
              app: helloworld
              name: helloworld
          spec:
            containers:
            - image: openshift/hello-openshift
              imagePullPolicy: Always
              name: helloworld
              readinessProbe:
                httpGet:
                  path: /
                  port: 8080
                initialDelaySeconds: 60
                periodSeconds: 10
                timeoutSeconds: 60
              livenessProbe:
                httpGet:
                  path: /
                  port: 8080
                initialDelaySeconds: 120
                periodSeconds: 10
              ports:
              - containerPort: 8080
            restartPolicy: Always
        triggers:
        - type: ConfigChange

- name: Deploy helloworld route
  k8s:
    definition:
      apiVersion: route.openshift.io/v1
      kind: Route
      metadata:
        namespace: "{{ ansible_operator_meta.namespace }}"
        annotations:
          openshift.io/host.generated: "true"
        name: helloworld
      spec:
        host: "hello-{{ ansible_operator_meta.namespace }}.{{application_domain}}"
        to:
          kind: Service
          name: helloworld
          weight: 100
        port:
          targetPort: 8080
        wildcardPolicy: None
```

### Step 4: Update role permissions

Since we are creating a service and route we need to add those permissions to the role.

Add services so we can create them.

```sh
nano config/rbac/role.yaml
```

* More about [Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)

Permissions to add:

```yml
  - apiGroups:
      - ""
    resources:
      - secrets
      - pods
      - pods/exec
      - pods/log
      - services
```

Append routes and ingress api groups so we can also manage those objects.

```yml
  - apiGroups:
    - ""
    - config.openshift.io
    resources:
    - ingresses
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    - route.openshift.io
    resources:
    - routes
    - routes/custom-host
    verbs:
    - create
    - delete
    - deletecollection
    - get
    - list
    - patch
    - update
    - watch
```

### Run Operator using ansible-runner

This time we should see the application being deployed. A single pod should start and a service/route should be created.

```$ ansible-operator run local```

### Test our deployment

To test simply use curl against the route URL. It does take a minute or so to start application.

* pods:

```sh
oc get pods
```

Example output:

```sh
NAME                         READY   STATUS    RESTARTS   AGE
helloworld-f9d964dcc-jgcmn   1/1     Running   0          70s
```
* routes:

```sh
oc get routes
```

Example output:

```sh
NAME         HOST/PORT                                             PATH   SERVICES     PORT   TERMINATION   WILDCARD
helloworld   hello-operator-helloworld.apps.ocp4.keithtenzer.com          helloworld   8080
```

* Invoke `YOUR` URL with curl or just open a browser and insert the URL

```
$ curl http://hello-operator-helloworld.apps.ocp4.keithtenzer.com
Hello OpenShift!
```
