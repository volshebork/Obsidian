# Add Clients to SCC

Add each client to SCC as a UNIX host, so the server can scan it over SSH.

## Create the SCC Master Password

Skip this section if the master password already exists.

Log in to the server's desktop as root.

1. Launch SCC

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type and select UNIX SSH Remote Scan

3. Click Edit/Select UNIX Hosts

4. Create the master password when prompted
    - At least 15 characters, with uppercase, lowercase, a number, and a special character
    - It expires after 60 days, so record when it was set
    - It is required before every remote scan

## Add the ansible Credential

Skip this section if the `ansible` credential already exists.

Log in to the server's desktop as root.

1. In the UNIX host list, click Add New Credential

2. Fill in the credential
    - SSH Username
        - `ansible`
    - Nickname/Comment
        - `ansible account`
    - SSH and/or sudo Password
        - The `ansible` password

3. Leave the remaining fields blank and click Save

## Add Each Client

Log in to the server's desktop as root.

1. In the UNIX host list, click Add New Host

2. Fill in the host
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

3. Click Test SSH, Save and Close

4. Repeat for each client

## Verify the Clients

Log in to the server's desktop as root.

1. In the UNIX host list, confirm each client
    - The SSH Connection column shows Success
    - The checkbox is checked

2. Click Close

## End State

At this point, the server

- Has every client saved in SCC, ready to scan

Next: [16-scan_clients.md](16-scan_clients.md) to scan the clients before the STIG is applied.
