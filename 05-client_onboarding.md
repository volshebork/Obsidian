# Onboard a Client

Set up a new client so the server can reach it with Ansible and SCC, then point the client at the server's repos. Repeat this document for every new client.

## Prepare the client

Log in to the client's desktop as root.

1. Confirm the ansible account exists

    `id ansible`

    - The output should include `wheel` in the groups list
    - If the account was created when the client was built, skip to step 3

2. Create the ansible account if it does not exist

    ```bash
    # Create the account and add it to wheel so it can use sudo
    useradd -G wheel ansible

    # Set the password
    passwd ansible
    ```

    - Use the same ansible password on every client, so Ansible can use one password for all clients

3. Confirm SSH is running

    ```bash
    systemctl is-active sshd
    ```

    - The output should be `active`

## Copy the server's SSH key to the client

Log in to the server's desktop as root.

1. Copy the SSH key

    `ssh-copy-id ansible@<client-ip>`

    - Replace `<client-ip>` with the client's IP address
    - Type `yes` to accept the client's host key
    - Enter the client's ansible password when prompted

2. Verify the key works

    `ssh ansible@<client-ip> hostname`

    - The output should be the client's hostname, with no password prompt

## Add the client to the inventory

Log in to the server's desktop as root.

1. Add the client under `[clients]`

    `vim /root/ansible/inventory/inventory.ini`

    - Press `i` to enter insert mode, then add a line under `[clients]` in this format

    ```ini
    <client-name> ansible_host=<client-ip>
    ```

    - Replace `<client-name>` with a name for the client, such as its hostname
    - Replace `<client-ip>` with the client's IP address
    - Press `Esc`, then type `:wq` and press `Enter` to save and exit

2. Verify Ansible can reach the client

    ```bash
    cd /root/ansible
    ansible <client-name> -m ping
    ```

    - The output should return `"ping": "pong"`
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

## Point the client at the repo server

Log in to the server's desktop as root.

1. Run the repo client playbook

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/repo-clients.yml --limit <client-name> -K
    ```

    - At the `BECOME password` prompt, enter the client's ansible password
    - `-K` is needed because the playbook uses `sudo` on the client

## Verify the client can install packages from the server

Log in to the client's desktop as root.

1. Confirm both repos are listed

    ```bash
    dnf clean all
    dnf repolist
    ```

    - `local-baseos` and `local-appstream` should be listed
    - A "This system is not registered" message is expected and can be ignored

2. Install a test package, then remove it

    ```bash
    dnf install -y zsh
    dnf remove -y zsh
    ```

    - Both commands should complete without errors

## End State

At this point, the client

- Accepts SSH logins from the server with the server's key
- Is listed in the server's Ansible inventory
- Installs packages from the server's repos

Next: `06-baseline-scan.md` confirms the server can reach every client, then scans the server and clients before any STIG is applied.
