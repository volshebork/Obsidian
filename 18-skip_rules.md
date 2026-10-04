# Skip STIG Rules

Skip any STIG rules known to break something in your environment before the STIG is applied. It is safer to skip too many rules at first and enable them later than to break a working system.

## Find the Rule's Manage Line

Log in to the server's desktop as root.

1. Search the role's defaults for the rule

    ```bash
    grep "<rule-id>" /root/ansible/roles/rhel9STIG/defaults/main.yml
    ```

    - Replace `<rule-id>` with either the number from the rule's Vuln ID, such as `258090`, or its STIG ID, such as `RHEL-09-433015`
    - Searching by STIG ID shows only the comment line, so add `-A 1` to also show the `Manage` line

2. Find the rule's `Manage` line in the output, for example

    ```txt
    # R-258090 RHEL-09-433015
    rhel9STIG_stigrule_258090_Manage: True
    rhel9STIG_stigrule_258090_fapolicyd_enable_Enabled: yes
    rhel9STIG_stigrule_258090_fapolicyd_start_State: started
    ```

    - The `Manage` line turns the rule on or off
    - `True` means the STIG role applies the rule
    - The other lines are the settings the rule applies, and do not need to change

3. Copy the rule's `Manage` line

4. Confirm the rule matches the finding in the scan results
    - The role and the scan content are updated separately, so a rule can differ between them
    - Compare the STIG ID and rule number against the finding in STIG Viewer before skipping it
    - If the rule is not found, the role does not apply it, and there is nothing to skip

## Add the Rule to the Vars File

Log in to the server's desktop as root.

1. Open the vars file

    ```bash
    vim /root/ansible/playbooks/vars.yml
    ```

2. Press `i` to enter insert mode, then add a comment explaining why the rule is skipped

   ```yaml
    # V-<rule number>: <why the rule is skipped>
    ```

    - Every skipped rule remains an open finding, so the comment should explain why

3. Paste the rule's `Manage` line below the comment

4. Change `True` to `False`, for example

    ```yaml
    # V-258090: do not enable or start fapolicyd, which blocks STIG Viewer from launching
    rhel9STIG_stigrule_258090_Manage: False
    ```

5. Press `Esc`, then type `:wq` and press `Enter` to save and exit

6. Repeat for each rule to skip

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
