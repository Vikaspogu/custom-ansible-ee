# Custom Ansible Execution Environment

This repository is to build a custom execution image for managing SOPS secrets, OPNSense router in Ansible Automation platform.

## Getting Started

Install [ansible-builder](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html#install-ansible-builder) package

(Optional) To bootstrap a new Ansible builder structure

```bash
mkdir custom-ansible-ee
ansible-builder create
ansible-builder build -v3 -t quay.io/<USER>/<IMAGE_NAME>:<IMAGE_TAG>
```

Create the image using the podman command

```bash
git clone https://github.com/Vikaspogu/custom-ansible-ee.git
cd custom-ansible-ee
ansible-builder create
podman build -f context/Containerfile -t quay.io/<USER>/<IMAGE_NAME>:<IMAGE_TAG> --variant=x86_64
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
