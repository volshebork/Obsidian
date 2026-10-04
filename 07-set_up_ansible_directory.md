# Set Up the Ansible Directory

Copy the Ansible directory into place, add DISA's RHEL 9 STIG role, and point the repo client role at the server.

## Copy the Ansible Directory

Log in to the desktop as root.

1. Copy the Ansible directory to root's home directory

    ```bash
    cp -r /opt/stig_server/ansible /root/ansible
    ```

2. Install tree if needed

    ```bash
    dnf install -y tree
    ```

3. Confirm the directory was copied

    ```bash
    tree /root/ansible
    ```

    The output should look like this

    ```txt
    /root/ansible
    ├── ansible.cfg
    ├── inventory
    │   └── inventory.ini
    ├── playbooks
    │   ├── apply_stig.yml
    │   ├── repo_clients.yml
    │   └── vars.yml
    └── roles
        └── repo_clients
            ├── files
            │   └── local.repo
            └── tasks
                └── main.yml

    6 directories, 7 files
    ```

## Add the RHEL 9 STIG Role

Log in to the desktop as root.

1. Install unzip if needed

    ```bash
    dnf install -y unzip
    ```

2. Extract the DISA zip

    ```bash
    unzip /opt/stig_server/downloads/U_RHEL_9_V2R9_STIG_Ansible.zip -d /opt/stig_server/disa_ansible
    ```

3. Extract the role from the zip inside it

    ```bash
    unzip /opt/stig_server/disa_ansible/rhel9STIG-ansible.zip -d /opt/stig_server/disa_ansible/rhel9STIG-ansible
    ```

4. Copy the role into the Ansible directory

    ```bash
    cp -r /opt/stig_server/disa_ansible/rhel9STIG-ansible/roles/rhel9STIG /root/ansible/roles/
    ```

    - `U_RHEL_9_V2R9_STIG_Ansible_Documentation.pdf` in `/opt/stig_server/disa_ansible` explains the role's variables

5. Confirm the role was copied

    ```bash
    tree /root/ansible
    ```

    - The output should look like this

    ```txt
    /root/ansible
    ├── ansible.cfg
    ├── inventory
    │   └── inventory.ini
    ├── playbooks
    │   ├── apply_stig.yml
    │   ├── repo_clients.yml
    │   └── vars.yml
    └── roles
        ├── repo_clients
        │   ├── files
        │   │   └── local.repo
        │   └── tasks
        │       └── main.yml
        └── rhel9STIG
            ├── callback_plugins
            │   └── stig_xml.py
            ├── defaults
            │   └── main.yml
            ├── files
            │   └── U_RHEL_9_STIG_V2R9_Manual-xccdf.xml
            ├── handlers
            │   └── main.yml
            └── tasks
                └── main.yml

    12 directories, 12 files
    ```

## Point the Repo Client Role at the Server

Log in to the desktop as root.

1. Replace the placeholder with the server's IP address

    ```bash
    sed -i 's/<repo-server-ip>/<server-ip>/' /root/ansible/roles/repo_clients/files/local.repo
    ```

    - Leave `<repo-server-ip>` exactly as shown, it is the placeholder text being replaced
    - Replace `<server-ip>` with the server's IP address
    - For example, with a server at `192.168.56.10`

        ```bash
        sed -i 's/<repo-server-ip>/192.168.56.10/' /root/ansible/roles/repo_clients/files/local.repo
        ```

2. Verify the file (using 192.168.56.10 as an example)

    ```bash
    grep baseurl /root/ansible/roles/repo_clients/files/local.repo
    ```

    The baseurl lines should look like this

    ```txt
    baseurl=http://192.168.56.10/repos/BaseOS
    baseurl=http://192.168.56.10/repos/AppStream
    ```

## Verify the Ansible Directory

Log in to the desktop as root.

1. Confirm the repo client role has the server's IP address

    ```bash
    grep baseurl /root/ansible/roles/repo_clients/files/local.repo
    ```

    - Both lines should show the server's IP address

2. Confirm Ansible can reach the server

    ```bash
    cd /root/ansible
    ansible local -m ping
    ```

    - The output should return `"ping": "pong"`
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

3. Confirm both playbooks are valid

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply_stig.yml --syntax-check
    ansible-playbook playbooks/repo_clients.yml --syntax-check
    ```

    - Each check should print the playbook name with no errors

## End State

At this point, the server

- Has the Ansible directory at `/root/ansible`
- Has DISA's RHEL 9 STIG role in the Ansible directory
- Has a repo client role pointing at the server's repos

Next: [08-generate_ssh_key.md](08-generate_ssh_key.md) to generate the server's SSH key.
