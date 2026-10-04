# Scan the Server

Confirm the server can reach every host, then scan the server before the STIG is applied.

## Confirm the Server Can Reach Every Host

Log in to the server's desktop as root.

1. Ping every host with Ansible

    ```bash
    cd /root/ansible
    ansible all -m ping
    ```

    - Every host, including `localhost`, should return `"ping": "pong"`
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

## Scan the Server to Get a Baseline

Log in to the server's desktop as root.

1. Launch SCC

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type and select Local Scan

3. In the Content pane, confirm RHEL_9_STIG is the only content checked

4. Click Start Scan
    - Click Continue on the Unanswered Manual Questions Found warning, which marks manual checks as Not Reviewed
    - The scan takes several minutes

5. Confirm the scan completed
    - The results window should list the server with a score and 0 errors

## End State

At this point, the server

- Can reach every host with Ansible
- Has been scanned with SCC before the STIG is applied

Next: [15-add_clients_to_scc.md](15-add_clients_to_scc.md) to add the clients to SCC for remote scanning.
