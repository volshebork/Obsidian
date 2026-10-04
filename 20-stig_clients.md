# STIG the Clients

Apply the RHEL 9 STIG to clients.

> [!IMPORTANT]
> STIG a single test client first, to find which rules to skip and any other configuration changes needed. Add those skips with [18-skip_rules.md](18-skip_rules.md), then rebuild the test client and repeat this document until it is STIGed without problems.

## Preview the STIG

Log in to the server's desktop as root.

1. Run the STIG playbook in check mode

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply_stig.yml --limit <client-name> --check -K
    ```

    - Replace `<client-name>` with the client's name from the inventory, or use `clients` to preview every client
    - At the `BECOME password` prompt, enter the clients' `ansible` password
    - `--check` shows what the playbook would change, without changing anything
    - Some tasks may report errors in check mode, since they depend on changes earlier tasks would have made
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

2. Make sure every rule in the vars file is being skipped

    ```bash
    grep -A 1 "<rule number>" /root/ansible/ansible.log
    ```

    - Replace `<rule number>` with each rule number from `/root/ansible/playbooks/vars.yml`, such as `258090`
    - Each of the rule's tasks should show `skipping`

## Apply the STIG

Log in to the server's desktop as root.

1. Run the STIG playbook

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply_stig.yml --limit <client-name> -K
    ```

    - Replace `<client-name>` with the client's name from the inventory, or use `clients` to STIG every client
    - At the `BECOME password` prompt, enter the clients' `ansible` password
    - This will reboot the client at the end of the run
    - Ansible waits for the client to come back up before finishing
    - The play recap should show `failed=0`

## Verify the Client

Log in to the server's desktop as root.

1. Confirm Ansible can still reach the client

    ```bash
    cd /root/ansible
    ansible <client-name> -m ping
    ```

    - The output should return `"ping": "pong"`

2. Confirm SCC can still connect to the client
    - Launch SCC, click Choose a Scan Type, and select UNIX SSH Remote Scan
    - Click Edit/Select UNIX Hosts
    - Right-click the client and test its SSH connection
    - The SSH Connection column should show Success

3. Log in to the client's desktop as root and confirm it can still install packages

    ```bash
    dnf install -y zsh
    dnf remove -y zsh
    ```

## End State

At this point, the client

- Has the RHEL 9 STIG applied
- Can still be reached by Ansible and SCC
- Can still install packages from the server's repos

Next: [21-rescan.md](21-rescan.md) to scan the STIGed hosts and compare against the baseline.
