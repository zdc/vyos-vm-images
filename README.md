# vyos-vm-images

[Ansible](https://www.ansible.com/) playbooks to build VyOS VM images.

## Requirements

You need a machine with at least 20 GB free space with Debian 10 (bare-metal, virtual, Docker container with --privileged flag). Also, you need to install ansible and python packages:

```
sudo apt update
sudo apt install -y ansible python
```

If you want to build an OVA image, you also need `ovftool` from VMware. It should be downloaded from the [VMware site](https://code.vmware.com/tool/ovf). Also, you need a private key to sign an OVA file. It can be generated with the next command:

```
openssl req -x509 -nodes -sha256 -days 365 -newkey rsa:1024 -keyout myself.pem -out myself.pem
```

All other requirements will be installed by ansible-playbook.


## Prepare

You need to copy the ISO image with VyOS to /tmp/vyos.iso before running ansible-playbook. Resulting images also will be located inside /tmp/ directory.

## Supported Platforms

- QEMU

    ```
    ansible-playbook qemu.yml
    ```

- VMware

    ```
    ansible-playbook vmware.yml -e vyos_vmware_private_key_path=path_to_private_key
    ```

- Microsoft Hyper-V

    ```
    ansible-playbook hyperv.yml
    ```

- Vagrant libvirt

    ```
    ansible-playbook vagrant-libvirt.yml
    ```

## Additional (optional) parameters

- Path to local ISO image (default: /tmp/vyos.iso):

    ```
    -e iso_local=path
    ```

    Example:

    ```
    -e iso_local=/tmp/vyos/custom_image.iso
    ```

- Disk size (default: 10GB):

    ```
    -e disk_size=size
    ```

    Example for 2 GB:

    ```
    -e disk_size=2
    ```

- Enable Cloud-init (default: according to platform):

    ```
    -e cloud_init=true
    ```

- Configure custom Cloud-init datasources (default: according to platform):

    ```
    -e cloud_init_ds=datasources
    ```

    Example:
    ```
    -e cloud_init_ds=NoCloud,ConfigDrive,None
    ```

- Install guest agent. It can be `qemu`, `vmware` (default: none):

    ```
    -e guest_agent=agent
    ```

    Example:
    ```
    -e guest_agent=qemu
    ```

- Disable configuration stage modules in Cloud-init. Mostly useful when you are building for non-cloud environments, where Cloud-init meta-data is not available (default: false):
    ```
    -e cloud_init_disable_config=true
    ```

- Create an archive with files required to PXE boot (default: false):

    ```
    -e pxe=true
    ```

- Keep default `vyos` user with password `vyos` in configuration when building an image with Cloud-init (default: false):

    ```
    -e keep_user=true
    ```

- Create an image with empty configuration, do not add any default settings like eth0 address, SSH service (default: false):

    ```
    -e empty_config=true
    ```

- Select the default console in the boot menu. It can be `kvm` or `serial` (default: kvm)

    ```
    -e grub_console=kvm
    ```
