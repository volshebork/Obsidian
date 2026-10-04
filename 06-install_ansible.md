# Install Ansible

Install Ansible and the Ansible collections the RHEL 9 STIG role needs.

## Install Ansible and the Collections

Log in to the desktop as root.

1. Install Ansible

    ```bash
    dnf install -y ansible-core
    ```

2. Install the community.general collection

    ```bash
    ansible-galaxy collection install /opt/stig_server/downloads/community-general-12.0.1.tar.gz
    ```

3. Install the ansible.posix collection

    ```bash
    ansible-galaxy collection install /opt/stig_server/downloads/ansible-posix-2.1.0.tar.gz
    ```

## Verify the Install

Log in to the desktop as root.

1. Confirm the Ansible version

    ```bash
    ansible --version
    ```

    - The output should show `ansible [core 2.14.18]`

2. Confirm both collections are installed

    ```bash
    ansible-galaxy collection list
    ```

    - `community.general` 12.0.1 and `ansible.posix` 2.1.0 should be listed

## End State

At this point, the server

- Has Ansible installed
- Has the community.general and ansible.posix collections installed

Next: [07-set_up_ansible_directory.md](07-set_up_ansible_directory.md) to set up the Ansible directory and the RHEL 9 STIG role.
