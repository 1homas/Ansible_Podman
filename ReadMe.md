# ReadMe.md

[Podman](https://podman.io) is a free, open-source, drop-in [Docker](https://www.docker.com/) replacement for running containers. These examples deploy containers in podman using [Ansible](https://ansible.com).

There are many registries with repositories of official container images:

- docker.io (Docker)
- quay.io (Red Hat)
- ghcr.io (GitHub)
- azurecr.io (Microsoft Azure)

See <https://hub.docker.com/> for more images.

## Installation

### Podman

#### macOS

For Mac, Podman is provided through [Homebrew](https://brew.sh/). Once you have set up brew, you can use the `brew install` command to install Podman:

```sh
brew install podman
```

Create and start your first Podman machine:

```sh
podman machine initpodman machine start
```

Verify the installation using:

```sh
podman info
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

## Run

### podman.yaml

Example podman commands via Ansible

```sh
ansible-playbook podman.yaml --tags containers
ansible-playbook podman.yaml --tags containers -v
ansible-playbook podman.yaml --tags images
ansible-playbook podman.yaml --tags volumes
```

### grafana.yaml

```sh
ansible-playbook grafana.yaml
```

### it-tools.yaml

```sh
ansible-playbook it-tools.yaml
```

### ubuntu.yaml

```sh
ansible-playbook ubuntu
```