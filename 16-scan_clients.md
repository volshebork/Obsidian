# Scan the Clients

Scan every client before the STIG is applied.

## Scan the Clients for a Baseline

Log in to the server's desktop as root.

1. Launch SCC

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type and select UNIX SSH Remote Scan

3. In the Content pane, confirm RHEL_9_STIG is the only content checked

4. Click Start Scan
    - Enter the SCC master password when prompted
    - Click Continue on the Unanswered Manual Questions Found warning, which marks manual checks as Not Reviewed
    - The scan takes several minutes per client

5. Confirm the scan completed
    - The results window should list each client with a score and 0 errors

## End State

At this point

- Every client has been scanned with SCC before the STIG is applied

Next: [17-view_scan_results.md](17-view_scan_results.md) to save and review the baseline checklists.
