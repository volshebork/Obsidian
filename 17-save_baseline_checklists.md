# Save the Baseline Checklists

Copy each host's baseline checklist out of SCC's scan sessions and review it in STIG Viewer.

## Save the Baseline Checklists for Server and Client

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

    - Type `/opt/stig_server/stig_viewer_3-linux-x64/S` and press `Tab` to complete the rest of the path

2. In the Checklists row, click Open

3. Browse to `/root/stig_checklists/baseline` and select a host's `.cklb` file
    - Rules SCC could not check automatically are marked Not Reviewed

4. Repeat for each host

## End State

At this point

- Each host has a baseline checklist in `/root/stig_checklists/baseline`

Next: [18-skip_rules.md](18-skip_rules.md) to skip any STIG rules before the STIG is applied.
