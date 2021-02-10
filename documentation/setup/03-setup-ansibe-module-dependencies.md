# Install Ansible and Module Dependencies

### Step 1: Install ansible

```sh
sudo dnf -y install ansible
```

The [Ansible runner](https://ansible-runner.readthedocs.io/en/stable/) and [http runner](https://github.com/ansible/ansible-runner-http) is used to run a local version of the operator. This is **very useful for development and testing**.

```sh
pip3 install --user ansible-runner
```

```sh
pip3 install --user ansible-runner-http
```

#### Install required python modules

```sh
pip3 install --user requests
``` 

```sh
pip3 install --user openshift
```