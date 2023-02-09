## Setup

I had to tweak Python. mainly, ensuring that ansible and brew used the same version / packages.

```
# add the following to your `~/zshrc` file:

# Homebrew
eval "$(/opt/homebrew/bin/brew shellenv)"

# Homebrew: Python
export PATH="/opt/homebrew/opt/python@3.10/libexec/bin/:${PATH}"
```

Steps:

```
# install ansible
brew install ansible

# install community/vmware module
ansible-galaxy collection install community.vmware

# install community requirements

pip install -r ~/.ansible/collections/ansible_collections/community/vmware/requirements.txt
```

Update `inventory.yml` for your specific hosts and variables.

## Run playbooks

Create the DC/cluster/hosts:

```
ansible-playbook 01-vcsa-and-esxi-init.yml
```

Create the vSS to vDS migration for networking:

```
ansible-playbook 02-networking.yml
```

Flip USB NIC and onboard NIC so we can have all 2.5G traffic:

```
ansible-playbook 02.5-networking.yml
```

Add NFS and iSCSI targets to all hosts:

```
ansible-playbook 03-storage.yml
```

Create a folders and "other":

```
ansible-playbook 04-on-top.yml
```

## Extras

Enable SSH on all hosts (not reliant on vCenter):

```
# there's also 99-disable-ssh.yml
ansible-playbook 99-enable-ssh.yml
```

Ensure VCSA VM appliance is running:

```
ansible-playbook 99-power-on-vcsa-vm.yml
```