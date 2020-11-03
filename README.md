# aio-16(all-in-one Red Hat Open Stack - 16)

The intention of this repository is to  deploy simple single-node Red Hat OpenStack Platform 16.1 in a test environment.

This repo contains two roles. "virt-setup" and "aio-rhosp". The usage can be controlled with the help of ansible tags.
  - virt-setup: It can be used to create RHEL8 virtual machine. Ansible tag: "deploy_vm"
  - aio-rhosp: It will deploy all-in-one Red Hat Open Stack on the server. Ansible tag: "deploy_aio_rhosp"

If the tags are omitted then by-default it creates a virtual machine and further deploys all-in-one Red Hat OpenStack on top of it.

- **Tested the installation on RHEL7.8 KVM host.**




## Usage:
- **To build a RHEL8 virtual machine and all-in-one Red Hat OpenStack:**
  - Pre-requsite:
    - RHEL8 qcow2 disk image is pre-downloaded on the baremetal server(rhel8_qcow2_file: /root/rhel-8.2-x86_64-kvm.qcow2)
    - KVM host where you want to build a virutal machine for all-in-one Red Hat OpenStack should have access to the Red Hat base packages(through Red Hat Network/Satellite/local repo) **OR** should have following packages installed:
      - virt-install
      - libvirt-python
      - virt-install
      - libvirt-client
      - qemu-kvm
      - qemu-img
      - libguestfs-tools
      - libguestfs-xfs

    - You have Red Hat credentials and pool ID to register a RHEL8 virtual machine to Red Hat Network and login Red Hat docker registry. You will be prompted during the playbook execution.
    - Virtual machine should be able to subscribe to the following Red Hat repositories:
      - rhel-8-for-x86_64-baseos-eus-rpms
      - rhel-8-for-x86_64-appstream-eus-rpms
      - rhel-8-for-x86_64-highavailability-eus-rpms
      - ansible-2.9-for-rhel-8-x86_64-rpms
      - openstack-16.1-for-rhel-8-x86_64-rpms
      - fast-datapath-for-rhel-8-x86_64-rpms
      - rhel-8-for-x86_64-highavailability-rpms
  ```
  # ansible-playbook main.yaml -e @vars/virt-setup.yaml -e @vars/aio-rhosp.yaml
  ```
---
- **To build a RHEL8 virtual machine:**
  - Pre-requsite:
    - RHEL8 qcow2 disk image is pre-downloaded on the baremetal server(rhel8_qcow2_file: /root/rhel-8.2-x86_64-kvm.qcow2)
  ```
  # ansible-playbook main.yaml -e @vars/virt-setup.yaml --tags deploy_vm
  ```
---
- **To build a all-in-one Red Hat OpenStack on existing server:**
  - Pre-requsite:
    - You have Red Hat credentials and pool ID to register a RHEL8 virtual machine to Red Hat Network and login Red Hat docker registry. You will be prompted during the playbook execution. **OR** The server is already registered to Red Hat
    - Server has access to the following Red Hat repositories:
      - rhel-8-for-x86_64-baseos-eus-rpms
      - rhel-8-for-x86_64-appstream-eus-rpms
      - rhel-8-for-x86_64-highavailability-eus-rpms
      - ansible-2.9-for-rhel-8-x86_64-rpms
      - openstack-16.1-for-rhel-8-x86_64-rpms
      - fast-datapath-for-rhel-8-x86_64-rpms
      - rhel-8-for-x86_64-highavailability-rpms
  ```
  # ansible-playbook main.yaml -e @vars/aio-rhosp.yaml  --tags deploy_aio_rhosp
  ```
---
## Variables:
- vars/virt-setup.yaml

| Variable Name  | Description  | Required  | Default |
|----------------|--------------|-----------|---------|
| network1_name  | Libvirt network name which will be used to access the virtual machine  | No  | osp_mgmt  |
| network2_name  | Libvirt Network name which will be used to deploy Red Hat Open Stack   | No  | osp       |
| rhel8_qcow2_file  | Absulte path of the RHEL8 qcow2 image   | Yes  | null       |
| baremetal_ssh_key  | KVM host SSH public key to access the virtual machine   | Yes  | null       |
| vm_directory  | The absulute path of the directory where the virtual machine disk will be stored   | No  | /opt      |
| vm_name  | Name of the virtual machine   | No  | aio-16      |
| vm_size  | Size of the virtual machine disk   | No  | 50G      |
| vm_memory_size  | Memory of the virtual machine  | No  | 20000      |
| vm_cpu_count  | CPU count of the virtual machine   | No  | 4      |

- vars/aio-rhosp.yaml

| Variable Name  | Description  | Required  | Default |
|----------------|--------------|-----------|---------|
| aio_hostname  | Hostname of the server  | No  | aio-16.example.com  |
| aio_domain | Domain name of the server   | No  | example.com       |
| aio_libvirt_type | Virtlization type of the server.<br><br>* qemu - If you are deploying aio RHOSP on a virtual machine<br>* kvm - If you are deploying aio RHOSP on physical node  | No  | qemu       |
| aio_interface | Interface name of the server for aio RHOSP deployment.<br><br>Stick with the default value if you are deploying aio RHOSP on a KVM with RHEL8 qcow2 downloaded from Red Hat portal   | No  | eth1       |
| aio_dns_server | DNS server IP/FQDN.<br><br>Stick with the default value if you are deploying a virtual machine and aio with the roles from this repository   | No  |   192.168.150.1     |
---
## Demo:
[![asciicast](https://asciinema.org/a/370199.svg)](https://asciinema.org/a/370199)
