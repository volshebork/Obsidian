# Baseline Scan

Scan the server and clients before the STIG is applied. These results are the baseline that the post-STIG scan is compared against.

## Confirm the Server Can Reach Every Host

Log in to the server's desktop as root.

1. Ping every host with Ansible

    ```bash
    cd /root/ansible
    ansible all -m ping
    ```

    - Every host, including `localhost`, should return `"ping": "pong"`
    - Warnings that `community.general` and `ansible.posix` do not support Ansible version 2.14.18 are expected and can be ignored

## Scan the Server

Log in to the server's desktop as root.

1. Launch SCC

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type and select Local Scan

3. In the Content pane, confirm RHEL_9_STIG is the only content checked

4. Click Start Scan
    - The scan takes several minutes

## Add the Clients to SCC

Log in to the server's desktop as root.

1. Launch SCC if it is not already open

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type and select UNIX SSH Remote Scan

3. Click Edit/Select UNIX Hosts

4. If this is the first time, create the SCC master password when prompted
    - At least 15 characters, with uppercase, lowercase, a number, and a special character
    - It expires after 60 days, so record when it was set
    - It is required before every remote scan

5. If this is the first client, add the ansible account as a credential
    - Click Add New Credential
    - SSH Username: `ansible`
    - Nickname/Comment: `ansible account`
    - SSH and/or sudo Password: the `ansible` password
    - Leave the remaining fields blank and click Save

6. Add each client as a host
    - Click Add New Host
    - DNS Name/IP Address: the client's IP address
    - Description: the client's name from the Ansible inventory
    - SSH Port: `22`
    - Authentication Type: SSH as non-root, then Sudo: With Password
    - Select Credential: `ansible`
    - Click Test SSH, Save and Close

7. Confirm each client's SSH Connection column shows Success and its checkbox is checked

8. Click Close

## Scan the Clients

Log in to the server's desktop as root.

1. Launch SCC if it is not already open

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type and select UNIX SSH Remote Scan

3. In the Content pane, confirm RHEL_9_STIG is the only content checked

4. Click Start Scan
    - The scan takes several minutes per client

## Save the Baseline Checklists

Log in to the server's desktop as root.

1. Create the baseline directory

    ```bash
    mkdir -p /root/stig_checklists/baseline
    ```

2. List the scan sessions

    ```bash
    ls /root/SCC/Sessions
    ```

    - Each scan creates a session folder named for its date and time
    - The server scan and the client scan each have their own session

3. Copy the checklists from each session

    ```bash
    cp /root/SCC/Sessions/<session>/Results/SCAP/Checklists/*.cklb /root/stig_checklists/baseline/
    ```

    - Replace `<session>` with a session folder name, and repeat for each session
    - SCC names each checklist with the host's name and scan date

4. Confirm there is one checklist per host

    ```bash
    ls /root/stig_checklists/baseline
    ```

## Review the Baseline in STIG Viewer

Log in to the server's desktop as root.

1. Launch STIG Viewer

    ```bash
    /opt/stig_server/stig_viewer_3-linux-x64/STIG\ Viewer\ 3 --no-sandbox
    ```

2. In the Checklists row, click Open

3. Browse to `/root/stig_checklists/baseline` and select a host's `.cklb` file
    - Rules SCC could not check automatically are marked Not Reviewed

## End State

At this point

- The server and every client have been scanned with SCC
- Each host has a baseline checklist in `/root/stig_checklists/baseline`

Next: [15-skip_rules.md](15-skip_rules.md) to skip any STIG rules before the STIG is applied.
