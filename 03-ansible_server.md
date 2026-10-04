# Ansible Server

Install Ansible on the server, set up an Ansible directory with DISA's RHEL 9 STIG role, and create the playbooks that configure and STIG clients.

## Install Ansible and the collections

Log in to the desktop as root.

1. Install Ansible

    ```bash
    dnf install -y ansible-core
    ```

2. Verify the install

    ```bash
    ansible --version
    # The output should show ansible-core 2.14
    ```

3. Install the Ansible collections

    ```bash
    ansible-galaxy collection install /opt/disa/downloads/community-general-12.0.1.tar.gz /opt/disa/downloads/ansible-posix-2.1.0.tar.gz
    ```

4. Verify the collections

    ```bash
    ansible-galaxy collection list
    ```

    - `ansible.posix` 2.1.0 and `community.general` 12.0.1 should be listed

## Extract the RHEL 9 STIG role

Log in to the desktop as root.

1. Extract the DISA zip and the role inside it
    - The DISA zip contains a second zip with the role inside, so both need extracting

    ```bash
    # Install unzip if it is not already installed
    dnf install -y unzip

    # Extract the DISA zip
    unzip /opt/disa/downloads/U_RHEL_9_V2R9_STIG_Ansible.zip -d /opt/disa/ansible

    # Extract the role
    unzip /opt/disa/ansible/rhel9STIG-ansible.zip -d /opt/disa/ansible/rhel9STIG-ansible
    ```

    - `U_RHEL_9_V2R9_STIG_Ansible_Documentation.pdf` in `/opt/disa/ansible` explains the role's variables

## Create the Ansible directory

Log in to the desktop as root.

1. Create the directory structure and copy in the role

    ```bash
    # Create the directory structure
    mkdir -p /root/ansible/inventory /root/ansible/playbooks /root/ansible/roles

    # Copy the role
    cp -r /opt/disa/ansible/rhel9STIG-ansible/roles/rhel9STIG /root/ansible/roles/
    ```

    - Only the role is copied
    - DISA's `site.yml`, `enforce.sh`, and `ansible.cfg` are replaced by the files created below

2. Create the Ansible configuration file

    ```bash
    touch /root/ansible/ansible.cfg
    vim /root/ansible/ansible.cfg
    ```

    - Press `i` to enter insert mode, then add the following content

    ```ini
    [defaults]
    inventory = /root/ansible/inventory/inventory.ini
    roles_path = /root/ansible/roles
    # ansible output will be appended to this log. You may want eventually mv the file so the file doesn't get too long.
    log_path = /root/ansible/ansible.log

    # DISA's callback that writes an XCCDF report of the playbook results
    callback_plugins = /root/ansible/roles/rhel9STIG/callback_plugins
    callbacks_enabled = stig_xml
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit
    - Ansible only reads this file when commands are run from `/root/ansible`

3. Create the inventory file

    ```bash
    touch /root/ansible/inventory/inventory.ini
    vim /root/ansible/inventory/inventory.ini
    ```

    - Press `i` to enter insert mode, then add the following content

    ```ini
    [local]
    # The server itself, so it can run playbooks on itself
    localhost ansible_connection=local

    [clients]
    # Add clients here, one per line, as they are created
    # Format: <client-name> ansible_host=<client-ip>
    # Example: client01 ansible_host=192.168.1.21

    [clients:vars]
    # Settings applied to every host in [clients]
    # ansible_user is the account Ansible logs in to clients with
    # every client will need an identical admin account named "ansible"
    ansible_user=ansible
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit

## Create the STIG playbook

Log in to the desktop as root.

1. Create the vars file
    - Overrides DISA's defaults without editing the role
    - Skips V-258090, which enables fapolicyd and would block STIG Viewer from launching

    ```bash
    touch /root/ansible/playbooks/vars.yml
    vim /root/ansible/playbooks/vars.yml
    ```

    - Press `i` to enter insert mode, then add the following content

    ```yaml
    # V-258090: do not enable or start fapolicyd
    rhel9STIG_stigrule_258090_Manage: False
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit
    - To skip another rule, add a line in the same format
        - `rhel9STIG_stigrule_<rule number>_Manage: False`
        - Rule numbers are listed in `/root/ansible/roles/rhel9STIG/defaults/main.yml`, each with a comment showing its STIG ID
        - Add a comment above each line explaining why the rule is skipped

2. Create the STIG playbook

    ```bash
    touch /root/ansible/playbooks/apply-stig.yml
    vim /root/ansible/playbooks/apply-stig.yml
    ```

    - Press `i` to enter insert mode, then add the following content

    ```yaml
    ---
    - name: Apply RHEL 9 STIG
      hosts: all
      gather_facts: no
      become: yes
      vars_files:
        - vars.yml
      roles:
        - rhel9STIG
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit
    - `hosts: all` covers every host in the inventory, so each run uses `--limit` to choose which clients to STIG

## Create the repo client role and playbook

Log in to the desktop as root.

1. Create the role's directories

    ```bash
    mkdir -p /root/ansible/roles/repo_clients/files /root/ansible/roles/repo_clients/tasks
    ```

2. Create the repo file that clients will receive

    ```bash
    touch /root/ansible/roles/repo_clients/files/local.repo
    vim /root/ansible/roles/repo_clients/files/local.repo
    ```

    - Press `i` to enter insert mode, then add the following content

    ```ini
    [local-baseos]
    name=Local BaseOS
    baseurl=http://<repo-server-ip>/repos/BaseOS
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

    [local-appstream]
    name=Local AppStream
    baseurl=http://<repo-server-ip>/repos/AppStream
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
    ```

    - Replace both instances of `<repo-server-ip>` with the server's IP address
    - Press `Esc`, then type `:wq` and press `Enter` to save and exit
    - This matches the server's own `local.repo`, but points at the server over HTTP instead of the local disk

3. Create the role's tasks

    ```bash
    touch /root/ansible/roles/repo_clients/tasks/main.yml
    vim /root/ansible/roles/repo_clients/tasks/main.yml
    ```

    - Press `i` to enter insert mode, then add the following content

    ```yaml
    ---
    # Copy the repo file so clients install packages from the repo server
    - name: Deploy the repo file
      ansible.builtin.copy:
        src: local.repo
        dest: /etc/yum.repos.d/local.repo
        owner: root
        group: root
        mode: "0644"
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit

4. Create the repo client playbook

    ```bash
    touch /root/ansible/playbooks/repo-clients.yml
    vim /root/ansible/playbooks/repo-clients.yml
    ```

    - Press `i` to enter insert mode, then add the following content

    ```yaml
    ---
    - name: Point clients at the repo server
      hosts: clients
      become: yes
      roles:
        - repo_clients
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit

## Generate the server's SSH key

Log in to the desktop as root.

1. Generate the SSH key

    `ssh-keygen -t ed25519`

    - Press `Enter` to accept the default location
    - Press `Enter` twice to leave the passphrase empty, so Ansible and SCC can reach clients without prompting
    - Ansible and SCC use this key to reach clients, and each host receives it when onboarded

## Verify the setup

Log in to the desktop as root.

1. Run the checks from the Ansible directory

    ```bash
    cd /root/ansible

    # Confirm Ansible can reach the server itself
    ansible local -m ping

    # Confirm both playbooks have valid syntax
    ansible-playbook playbooks/apply-stig.yml --syntax-check
    ansible-playbook playbooks/repo-clients.yml --syntax-check
    ```

    - `ansible local -m ping` should return `"ping": "pong"`
    - Each syntax check should print the playbook name with no errors
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

## End State

At this point, the server

- Has Ansible installed
- Has an Ansible playbook to configure clients to point to the server for packages
- Has an Ansible playbook to apply STIGs
- Has an SSH key ready to copy to hosts when they are onboarded
- Has **not** had any stigs applied

Next: `04-scc-server.md` installs SCC and STIG Viewer to scan hosts and review the results.
