# Install [Docker](https://www.docker.com)

### Step 1: Install Docker 17.03+

Add the docker ce repositories. Understanding [DNF](https://fedoraproject.org/wiki/DNF?rd=Dnf)) on Fedora. 

_Note:_ You can also use `podman` and `buildah` instead of `Docker`. For example if you want to run the training on a [Ubuntu](https://ubuntu.com) machine. 

```sh
sudo dnf -y install dnf-plugins-core
```

```sh
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

### Step 2: Install docker-ce

```sh
sudo dnf -y install docker-ce docker-ce-cli
```

```sh
sudo systemctl start docker
```

```sh
sudo systemctl enable docker
```