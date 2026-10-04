# Install RHEL on a Client

Install RHEL 9.6 on the client however its purpose requires, with the settings below. These are the only settings the STIG process depends on.

## Install RHEL

1. Set a static IP address on the same network as the server
2. Create a user named `ansible` and check Make this user administrator
3. Use the same `ansible` password on every client
4. Begin the installation and reboot when it finishes

## Verify the Client

Log in to the client's desktop as root.

1. Confirm the ansible account is in `wheel`

    ```bash
    id ansible
    ```

    - The output should include `wheel`

2. Confirm SSH is running

    ```bash
    systemctl is-active sshd
    ```

    - The output should be `active`

3. Confirm the client can reach the server

    ```bash
    ping -c 3 <server-ip>
    ```

    - Replace `<server-ip>` with the server's IP address
    - All three packets should be received

## End State

At this point, the client

- Has RHEL 9.6 installed with a static IP address
- Has an `ansible` account in `wheel`
- Can reach the server on the network

Next: [12-onboard_client.md](12-onboard_client.md) to let the server manage the client with Ansible.
