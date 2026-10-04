# Apply STIG

Apply the RHEL 9 STIG to the server, then to the clients, and rescan to confirm the results. STIG the server first, so it is hardened before it pushes changes to any client.

## Skip rules before applying the STIG

Log in to the server's desktop as root.

Any rule known to break something in your environment should be skipped before the STIG is applied. It is safer to skip too many rules at first and enable them later than to break a working system.

1. Find the rule number for each rule to skip
    - The rule number is the number in the rule's Vuln ID, so V-258090 is rule 258090
    - To look up a rule by its STIG ID instead, search the role's defaults

    ```bash
    grep "RHEL-09-433015" /root/ansible/roles/rhel9STIG/defaults/main.yml
    ```

    - The output shows the rule number after `R-`, such as `# R-258090 RHEL-09-433015`

2. Add each rule to the vars file

    ```bash
    vim /root/ansible/playbooks/vars.yml
    ```

    - Press `i` to enter insert mode, then add a comment and a line for each rule in this format

    ```yaml
    # V-<rule number>: <why the rule is skipped>
    rhel9STIG_stigrule_<rule number>_Manage: False
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit
    - Every skipped rule remains an open finding, so the comment should explain why

3. Confirm the skipped rules

    ```bash
    cat /root/ansible/playbooks/vars.yml
    ```

    - V-258090 (fapolicyd) is already skipped from `03-ansible-server.md`

## STIG the server

Log in to the server's desktop as root.

1. Preview the changes without applying them

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply-stig.yml --limit local --check
    ```

    - `--check` shows what the playbook would change, without changing anything
    - Some tasks may report errors in check mode, since they depend on changes earlier tasks would have made

2. Run the STIG playbook against the server

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply-stig.yml --limit local
    ```

    - The playbook takes several minutes
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

3. Reboot the server
    - Several STIG settings only take effect after a reboot

    ```bash
    reboot
    ```

4. Log back in to the desktop as root and confirm the server still works

    ```bash
    # Confirm the repo is still published
    curl -s http://localhost/repos/BaseOS/repodata/repomd.xml | head -n 5

    # Confirm Ansible still runs
    cd /root/ansible
    ansible local -m ping
    ```

    - The `curl` command should print the start of an XML file
    - `ansible local -m ping` should return `"ping": "pong"`

5. Confirm SCC and STIG Viewer still launch

    ```bash
    /opt/scc/scc
    /opt/disa/stigviewer/STIG\ Viewer\ 3 --no-sandbox
    ```

## STIG the clients

Log in to the server's desktop as root.

1. Preview the changes without applying them

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply-stig.yml --limit clients --check -K
    ```

    - `--check` shows what the playbook would change, without changing anything
    - Some tasks may report errors in check mode, since they depend on changes earlier tasks would have made

2. Run the STIG playbook against the clients

    ```bash
    cd /root/ansible
    ansible-playbook playbooks/apply-stig.yml --limit clients -K
    ```

    - At the `BECOME password` prompt, enter the clients' ansible password
    - The playbook takes several minutes per client

3. Reboot the clients

    ```bash
    ansible clients -m ansible.builtin.reboot -b -K
    ```

    - Ansible waits for each client to come back up before finishing

4. Confirm Ansible can still reach the clients

    ```bash
    ansible clients -m ping
    ```

    - Every client should return `"ping": "pong"`

## Rescan

The server and clients were scanned before the STIG in `06-baseline-scan.md`. Scan them again the same way to confirm the playbook worked, and copy the new checklists to `/root/stig-checklists/post-stig` to compare against the baseline.

## End State

At this point

- The server and every client have the RHEL 9 STIG applied
- Each host has a post-STIG checklist to compare against its baseline
