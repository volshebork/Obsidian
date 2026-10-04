# Connect a Client to the Repo

Point the client at the server's BaseOS and AppStream repos, so the client installs packages from the server.

## Run the Repo Client Playbook

Log in to the server's desktop as root.

1. Run the repo client playbook against the client

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/repo_clients.yml --limit <client-name> -K
    ```

    - Replace `<client-name>` with the client's name from the inventory
    - At the `BECOME password` prompt, enter the client's `ansible` password
    - The play recap should show `failed=0`

## Verify the Client Can Install Packages

Log in to the client's desktop as root.

1. Confirm both repos are listed

    ```bash
    dnf clean all
    dnf repolist
    ```

    - `local-baseos` and `local-appstream` should be listed
    - A `This system is not registered` message is expected and can be ignored

2. Install a test package

    ```bash
    dnf install -y zsh
    ```

    - The install should complete without errors

3. Remove the test package

    ```bash
    dnf remove -y zsh
    ```

## End State

At this point, the client

- Installs packages from the server's repos

Next: [14-baseline_scan.md](14-baseline_scan.md) to scan the server and clients before the STIG is applied.
