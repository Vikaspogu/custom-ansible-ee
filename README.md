# Homelab Ansible Execution Image Builder

This repository is to build a custom execution image for managing SOPS secrets, OPNSense router in Ansible Automation platform.

## Getting Started

Install [ansible-builder](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html#install-ansible-builder) package

(Optional) To bootstrap a new Ansible builder structure

```bash
mkdir homelab-ansible-execution-builder
ansible-builder create
```

Create the image using the podman command

```bash
git clone https://github.com/Vikaspogu/homelab-ansible-execution-builder.git
cd homelab-ansible-execution-builder
podman build -f context/Containerfile -t quay.io/<USER>/<IMAGE_NAME>:<IMAGE_TAG> context
```

Pushing image to a registry

```bash
podman push quay.io/<USER>/<IMAGE_NAME>:<IMAGE_TAG>
```

Using execution environment in [Jobs](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html#use-an-execution-environment-in-jobs)

Mount the SOPS keys to decrypt using [Execution environment mount options](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html#execution-environment-mount-options)

```bash
sudo mkdir -p /var/lib/awx/{.sops-key,.opn-key}
sudo mv keys.txt /var/lib/awx/.sops-key
sudo mv opn.key /var/lib/awx/.opn-key
```

## Troubleshooting

To exec into a Job container

```bash
sudo su - awx
podman ps 
podman exec -it <CONTAINER_ID> <COMMAND>
```
