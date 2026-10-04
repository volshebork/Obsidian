# Skip STIG Rules

Skip any STIG rules known to break something in your environment before the STIG is applied. It is safer to skip too many rules at first and enable them later than to break a working system.

## Find the Rule Number

Log in to the server's desktop as root.

1. Find the rule number for each rule to skip
    - The rule number is the number in the rule's Vuln ID, so V-258090 is rule 258090
    - Vuln IDs are shown for each rule in STIG Viewer

2. To look up a rule by its STIG ID instead, search the role's defaults

    ```bash
    grep "<stig-id>" /root/ansible/roles/rhel9STIG/defaults/main.yml
    ```

    - Replace `<stig-id>` with the rule's STIG ID, such as `RHEL-09-433015`
    - The output shows the rule number after `R-`, such as `# R-258090 RHEL-09-433015`

## Add the Rules to the Vars File

Log in to the server's desktop as root.

1. Open the vars file

    ```bash
    vim /root/ansible/playbooks/vars.yml
    ```

2. Press `i` to enter insert mode, then add a comment and a line for each rule in this format

    ```yaml
    # V-<rule number>: <why the rule is skipped>
    rhel9STIG_stigrule_<rule number>_Manage: False
    ```

    - Every skipped rule remains an open finding, so the comment should explain why

3. Press `Esc`, then type `:wq` and press `Enter` to save and exit

## Verify the Skipped Rules

Log in to the server's desktop as root.

1. List every skipped rule

    ```bash
    grep "Manage: False" /root/ansible/playbooks/vars.yml
    ```

    - Each rule added above should be listed
    - `rhel9STIG_stigrule_258090_Manage: False` is already skipped, since fapolicyd blocks STIG Viewer from launching

## End State

At this point, the server

- Has every rule to skip listed in the vars file

Next: [19-stig_server.md](19-stig_server.md) to apply the STIG to the server.
