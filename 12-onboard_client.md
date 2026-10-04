# Onboard a Client

Copy the server's SSH key to the client and add the client to the Ansible inventory, so the server can manage the client with Ansible.

## Copy the SSH Key to the Client

Log in to the server's desktop as root.

1. Copy the server's SSH key to the client's ansible account

    ```bash
    ssh-copy-id ansible@<client-ip>
    ```

    - Replace `<client-ip>` with the client's IP address
    - Type `yes` to accept the client's host key
    - Enter the client's `ansible` password when prompted

## Add the Client to the Inventory

Log in to the server's desktop as root.

1. Open the inventory file

    ```bash
    vim /root/ansible/inventory/inventory.ini
    ```

2. Press `i` to enter insert mode, then add a line under `[clients]` in this format

   ```ini
    <client-name> ansible_host=<client-ip>
    ```

    - Replace `<client-name>` with a name for the client, such as its hostname
    - Replace `<client-ip>` with the client's IP address
    - For example, `rhel-client ansible_host=192.168.56.20`

3. Press `Esc`, then type `:wq` and press `Enter` to save and exit

## Verify the Client

Log in to the server's desktop as root.

1. Confirm the SSH key works

    ```bash
    ssh ansible@<client-ip> hostname
    ```

    - The output should be the client's hostname, with no password prompt

2. Confirm Ansible can reach the client

    ```bash
    cd /root/ansible
    ansible <client-name> -m ping
    ```

    - The output should return `"ping": "pong"`
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

## End State

At this point, the client

- Accepts SSH logins from the server's key
- Is in the server's Ansible inventory

Next: [13-connect_client_to_repo.md](13-connect_client_to_repo.md) to point the client at the server's repos.
