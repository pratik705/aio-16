# aio-16(all-in-one Red Hat Open Stack - 16)

The intention of this repository is to  deploy simple single-node Red Hat OpenStack Platform 16.1 in a test environment.

This repo contains two roles. "virt-setup" and "aio-rhosp". The usage can be controlled with the help of ansible tags.
  - virt-setup: It can be used to create RHEL8 virtual machine. Ansible tag: "deploy_vm"
  - aio-rhosp: It will deploy all-in-one Red Hat Open Stack on the server. Ansible tag: "deploy_aio_rhosp"

If the tags are omitted then by-default it creates a virtual machine and further deploys all-in-one Red Hat OpenStack on top of it.


## Usage:
- **To build a RHEL8 virtual machine and all-in-one Red Hat OpenStack:**
  - Pre-requsite:
    - RHEL8 qcow2 disk image is pre-downloaded on the baremetal server(rhel8_qcow2_file: /root/rhel-8.2-x86_64-kvm.qcow2)
    - Baremetal server where you want to build a virutal machine for all-in-one Red Hat OpenStack should have access to the Red Hat base packages(through Red Hat Network/Satellite/local repo) OR should have following packages installed:
      - virt-install
      - libvirt-python
      - virt-install
      - libvirt-client
      - qemu-kvm
      - qemu-img
      - libguestfs-tools
      - libguestfs-xfs
    - Virtual machine should be able to subscribe to the following Red Hat repositories:
      - rhel-8-for-x86_64-baseos-eus-rpms
      - rhel-8-for-x86_64-appstream-eus-rpms
      - rhel-8-for-x86_64-highavailability-eus-rpms
      - ansible-2.9-for-rhel-8-x86_64-rpms
      - openstack-16.1-for-rhel-8-x86_64-rpms
      - fast-datapath-for-rhel-8-x86_64-rpms
      - rhel-8-for-x86_64-highavailability-rpms

    - You have Red Hat credentials and pool ID to register a RHEL8 virtual machine to Red Hat Network and login Red Hat docker registry
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
    - You have Red Hat credentials and pool ID to register a RHEL8 virtual machine to Red Hat Network and login Red Hat docker registry
    OR
    - Server is already registered to Red Hat
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
