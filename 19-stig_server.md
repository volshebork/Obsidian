# STIG the Server

Apply the RHEL 9 STIG to the server.

> [!IMPORTANT]
> It is recommended to fully STIG a test client first, using [20-stig_clients.md](20-stig_clients.md), to find which rules to skip and any other configuration changes needed. Add those skips with [18-skip_rules.md](18-skip_rules.md) before STIGing the server, since rebuilding a client is much faster than rebuilding the server.

## Preview the STIG

Log in to the server's desktop as root.

1. Run the STIG playbook in check mode

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply_stig.yml --limit local --check
    ```

    - `--check` shows what the playbook would change, without changing anything
    - Some tasks may report errors in check mode, since they depend on changes earlier tasks would have made
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored
    - A `do_reboot` failure at the end is expected, since Ansible cannot reboot the server it is running on

2. Make sure every rule in the vars file is being skipped

    ```bash
    grep -A 1 "<rule number>" /root/ansible/ansible.log
    ```

    - Replace `<rule number>` with each rule number from `/root/ansible/playbooks/vars.yml`, such as `258090`
    - Each of the rule's tasks should show `skipping`

## Apply the STIG

> [!IMPORTANT]
> It is recommended to fully STIG a test client first, using [20-stig_clients.md](20-stig_clients.md), to find which rules to skip and any other configuration changes needed. Add those skips with [18-skip_rules.md](18-skip_rules.md) before STIGing the server, since rebuilding a client is much faster than rebuilding the server.

Log in to the server's desktop as root.

1. Run the STIG playbook

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply_stig.yml --limit local
    ```

    - The play recap should show `failed=1`, from the `do_reboot` handler
    - The `do_reboot` failure  at the ned is expected, since Ansible cannot reboot the server it is running on, so the next step reboots it manually
    - Any other failure should be investigated before rebooting

2. Reboot the server
    - Several STIG settings only take effect after a reboot

    ```bash
    reboot
    ```

## Verify the Server

Log in to the server's desktop as root.

1. Confirm the repo is still published

    ```bash
    curl -s http://localhost/repos/BaseOS/repodata/repomd.xml | head -n 5
    ```

    - The output should be the start of an XML file

2. Confirm Ansible still runs

    ```bash
    cd /root/ansible
    ansible local -m ping
    ```

    - The output should return `"ping": "pong"`

3. Confirm SCC still launches

    ```bash
    /opt/scc/scc
    ```

4. Confirm STIG Viewer still launches

    ```bash
    /opt/stig_server/stig_viewer_3-linux-x64/STIG\ Viewer\ 3 --no-sandbox
    ```

## End State

At this point, the server

- Has the RHEL 9 STIG applied
- Still publishes the repos and runs Ansible, SCC, and STIG Viewer

Next: [20-stig_clients.md](20-stig_clients.md) to apply the STIG to the clients.
