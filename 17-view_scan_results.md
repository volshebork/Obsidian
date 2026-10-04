# View Scan Results in STIG Viewer

Open the checklists SCC created during a scan in STIG Viewer to review each host's findings.

## Find the Scan Session

Log in to the server's desktop as root.

1. List the scan sessions

    ```bash
    ls /root/SCC/Sessions
    ```

    - Each scan creates a session folder named for its date and time
    - The most recent scan is listed last

## Open the Checklists

Log in to the server's desktop as root.

1. Launch STIG Viewer

    ```bash
    /opt/stig_server/stig_viewer_3-linux-x64/STIG\ Viewer\ 3 --no-sandbox
    ```

    - Type `/opt/stig_server/stig_viewer_3-linux-x64/S` and press `Tab` to complete the rest of the path

2. In the Checklists row, click Open

3. Browse to `/root/SCC/Sessions/<session>/Results/SCAP/Checklists` and select a host's `.cklb` file
    - Replace `<session>` with the session folder name
    - SCC names each checklist with the host's name and scan date
    - Rules SCC could not check automatically are marked Not Reviewed

4. Repeat for each host

## End State

At this point

- Each scanned host's checklist is open in STIG Viewer

Next: [18-skip_rules.md](18-skip_rules.md) to skip any STIG rules before the STIG is applied.
