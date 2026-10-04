# Generate the SSH Key

Generate the server's SSH key, which Ansible uses to log in to clients.

## Generate the Server's SSH Key

Log in to the desktop as root.

1. Generate the SSH key

    ```bash
    ssh-keygen -t ed25519 -N "" -f /root/.ssh/id_ed25519
    ```

    - `-N ""` sets an empty passphrase, so Ansible can log in to clients without prompting
    - Each client receives this key when it is onboarded

## Verify the SSH Key

Log in to the desktop as root.

1. Confirm both key files exist

    ```bash
    ls /root/.ssh/id_ed25519*
    ```

    - The output should show `/root/.ssh/id_ed25519` and `/root/.ssh/id_ed25519.pub`

## End State

At this point, the server

- Has an SSH key ready to copy to clients when they are onboarded

Next: [09-install_scc.md](09-install_scc.md) to install SCC and the UNIX Remote Scanning Plugin.
