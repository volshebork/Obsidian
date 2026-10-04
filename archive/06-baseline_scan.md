# Baseline Scan

Confirm the server can reach every client, then scan the server and clients before any STIG is applied. These results are the baseline that the post-STIG scan is compared against.

## Confirm the server can reach every host

Log in to the server's desktop as root.

1. Ping every host with Ansible

    ```bash
    cd /root/ansible
    ansible all -m ping
    ```

    - Every host, including `localhost`, should return `"ping": "pong"`
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

## Scan the server

Log in to the server's desktop as root.

1. Launch SCC

    ```bash
    /opt/scc/scc
    ```

2. Confirm Red Hat Enterprise Linux 9 STIG is the only content checked in the Content pane

3. Start a local scan of the server
    - Click Start Scan
    - The scan takes several minutes

## Scan the clients

Log in to the server's desktop as root.

1. Launch SCC

    ```bash
    /opt/scc/scc
    ```

2. Open the UNIX host list
    - Click Choose a Scan Type
    - Select UNIX SSH Remote Scan
    - Click Edit/Select UNIX Hosts

3. If this is the first client, create the SCC master password when prompted
    - At least 15 characters, with uppercase, lowercase, a number, and a special character
    - It expires after 60 days, so record when it was set
    - It protects the client credentials SCC stores, and is required before every remote scan

4. Add the ansible account as a credential
    - This is only needed once, since every client uses the same ansible account
    - Click Add New Credential
    - SSH Username
        - `ansible`
    - Nickname/Comment
        - `ansible account`
    - SSH and/or sudo Password
        - The ansible account's password
    - Leave the remaining fields blank, then click Save

5. Add each client as a UNIX host
    - Click Add New Host
    - DNS Name/IP Address
        - The client's IP address
    - Description
        - The client's name from the Ansible inventory
    - SSH Port
        - `22`
    - Authentication Type
        - SSH as non-root, then Sudo: With Password
    - Select Credential
        - `ansible`
    - Leave the remaining fields as they are
    - Click Test SSH, Save and Close
    - Confirm the SSH Connection column shows Success and the host's checkbox is checked

## Review the results in STIG Viewer

Log in to the server's desktop as root.

1. Copy each host's checklist to the baseline directory

    ```bash
    # Create the baseline directory
    mkdir -p /root/stig-checklists/baseline

    # Copy the STIG Viewer 3 checklists from the scan session
    cp /root/SCC/Sessions/<session>/Results/SCAP/Checklists/*.cklb /root/stig-checklists/baseline/
    ```

    - Replace `<session>` with the scan's session folder, named for the date and time of the scan
    - SCC names each checklist with the host's name and scan date, so no renaming is needed
    - SCC also writes a `.ckl` file for the older STIG Viewer 2, which is not needed

2. Launch STIG Viewer

    ```bash
    /opt/disa/stigviewer/STIG\ Viewer\ 3 --no-sandbox
    ```

3. Open each host's checklist
    - In the Checklists row, click Open
    - Browse to `/root/stig-checklists/baseline` and select the host's `.cklb` file
    - Rules SCC could not check automatically are marked Not Reviewed

## End State

At this point

- The server can reach every client with Ansible
- The server and every client have been scanned with SCC
- Each host has a baseline checklist in `/root/stig-checklists/baseline`

Next: `07-apply-stig.md` applies the STIG to the server and clients, then rescans to confirm the results.
