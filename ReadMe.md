# ReadMe.md

[Podman](https://podman.io) is a free, open-source, drop-in [Docker](https://www.docker.com/) replacement for running containers. These examples deploy containers in podman using [Ansible](https://ansible.com).

There are many registries with repositories of official container images:

- docker.io (Docker)
- quay.io (Red Hat)
- ghcr.io (GitHub)
- azurecr.io (Microsoft Azure)

## Installation

### Podman

#### macOS

For Mac, Podman is provided through [Homebrew](https://brew.sh/). Once you have set up brew, you can use the `brew install` command to install Podman:

```sh
brew install podman
brew install podman-desktop       # manage containers via UI
brew install podman-compose       # optional for dockerfiles and Kubernetes
```

Create and start your first Podman machine:

```sh
podman machine init               # default --disk-size=100GB --memory=2GB
podman machine init --cpus=1 --disk-size=50G --memory=2G --volume $HOME:$HOME
podman machine start
```

Verify the installation using:

```sh
podman info
```

Launch an NGINX web server container:

```sh
cat > index.html <<EOF
<html>
<title>Hello, World!</title>
<h1 style="text-align: center;">Hello, World!</h1>
</html>
EOF

podman run \
  --detach --tty --rm \
  --publish 8080:80 \
  --volume $PWD:/usr/share/nginx/html \
  --name nginx \
  nginx:latest

curl http://localhost:8080
```

Uninstall at any time :

```sh
podman machine rm -f podman-machine-default --force || true
brew uninstall podman podman-compose podman-desktop
```

### Ansible

1. Create the Python virtual development environment and install Ansible:

  ```sh
  python3 -m ensurepip --upgrade
  python3 -m pip install --upgrade pip # use a virtual development environment
  pipenv install                       # use Python 3.11 (--python 3.11) or later
  pipenv install -r requirements.txt   # install required Python packages
  ```

1. Run the virtual environment:

  ```sh
  pipenv shell
  ```

1. Verify the list of installed Python packages includes Ansible:

  ```sh
  pip list
  ```

1. Verify the Ansible collections includes `ansible.posix` and `containers.podman` :

  ```sh
  ansible-galaxy collection list
  ```

### Podman Container Inventory

Use the [`community.docker.docker_containers`](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_containers_inventory.html) inventory plugin with Podman!

1. Run Podman Desktop
2. Verify the Docker socket is redirected to `podman.sock` :

  ```sh
  ls -la /var/run/docker.sock
  ```

3. If not, enable Docker Compatibilty in the status bar at the bottom of the window which installs the `podman-mac-helper` and redirects `/var/run/docker` to the `podman.sock` location
4. Restart Podman Desktop
5. Copy this `docker.yml` file to your Ansible `inventory` directory or specify it using the `-i {inventory_file}` command line option :

  ```yaml
  plugin: community.docker.docker_containers
  docker_host: unix://var/run/docker.sock
  ```

6. Run `ansible-inventory` to see your Podman containers!

  ```sh
  ansible-inventory --graph
  ansible-inventory -i inventory/docker.yml --graph
  ```

## Run

### podman.yaml

Example podman commands for managing containers via Ansible :

```sh
ansible-playbook podman.yaml --tags containers
ansible-playbook podman.yaml --tags containers -v
ansible-playbook podman.yaml --tags images
ansible-playbook podman.yaml --tags volumes
```

### grafana.yaml

Use [Grafana](https://grafana.com) for monitoring and observability from many different data sources :

```sh
ansible-playbook grafana.yaml
```

Open your web browser to <https://localhost:3000> and login with the default username `admin` and password `admin`.

### it-tools.yaml

Use IT Tools at <https://it-tools.tech> or run your own local instance:

```sh
ansible-playbook it-tools.yaml
```

Use the local container instance at <https://localhost:8080>.

### ubuntu.yaml

```sh
ansible-playbook ubuntu
```
