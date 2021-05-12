# Ansible playbook to prepare Windows OS image for OpenShift (using Packer)

Note: This playbook was based on this repo: https://github.com/openshift/windows-machine-config-operator/tree/master/docs/vsphere_ci

## Goals
This playbook uses `Packer` to prepare a Windows image to be used with Windows Nodes on OpenShift.

![Prepare Windows Image](imgs/run.gif)
## How to use

You should define the following variables to run it:
- vcenter:
-
    admin_username: User with Admin privileges in vCenter
    admin_password: Admin user's password
    server: vCenter FQDN or IP
    datastore: vCenter Datastore name
    network: Network Interface Card type (e.g.: vmxnet3, e1000e, etc.).
    datacenter: vCenter Datacenter name
    cluster: vCenter Cluster name
    folder: vCenter Folder Name used to place the template
    win_iso_path: The path to your Windows 2019 ISO e.g. (e.g.: "[datastore1] ISOs/WinServer2019.iso").
- windows:
-  
    vm_name: Windows Template Name
    vm_cpu: Number of vCPU
    vm_mem: Amount of Memory in Mb (e.g.: 16384)
    vm_disk: Hard Drive Size in Mb (e.g.: 120000)
    admin_passwd: admin123
    input_locale: Input Locale (e.g.: en-US, en-ES)
    system_locale: System Locale (e.g.: en-US, en-ES)

- ssh\_pub\_key: Path to SSH Public Key. (e.g.: ~/.ssh/id_rsa.pub).

Instructions to use it:

1. Clone this repo into your workspace (it needs to have access to vCenter API URL).

```shell
$ git clone git@github.com:cptmorgan-rh/ocp-windows-image-prepare.git
```

2. Edit the variables according to your environment. This can be done by modifying the `group_vars\all.yml` file:

```yaml
ssh_pub_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
vcenter:
  admin_username: administrator@vsphere.local
  admin_password: Password1!
  server: 10.0.0.200
  datastore: ds0
  network: 10.0.0.x-24
  network_card: vmxnet3
  datacenter: dc0
  cluster: LAB
  folder: Templates
  win_iso_path: "[ds0] ISOs/Windows.2019.iso"
  vmwaretools_iso_path: "[] /usr/lib/vmware/isoimages/windows.iso"
download:
  packer: https://releases.hashicorp.com/packer/1.6.6/packer_1.6.6_linux_amd64.zip
windows:
  vm_name: win-ocp-image-template
  vm_cpu: 4
  vm_mem: 16384
  vm_disk: 120000
  admin_passwd: admin123
  input_locale: en-US
  system_locale: en-US
```

## Notes
Consider the following notes when trying to use this playbook:

- This playbook has been tested with Red Hat Enterprise Linux 8 and Fedora 33, it may not work with other Linux distributions not based on RHEL. If you intent to try it using other distributions and have suggestion to make it more portable, please feel free to propose a PR - I will be happy to analyze and approve it as soon as I can! :)
