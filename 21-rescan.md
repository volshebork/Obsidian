# Rescan

The server and clients were scanned before the STIG was applied. Scan them again to confirm the STIG worked, and compare the new results against the baseline.

## Rescan the Hosts

1. Scan the server with the Scan the Server section of [14-scan_server.md](14-scan_server.md)
2. Scan the clients with [16-scan_clients.md](16-scan_clients.md)
3. Open the new results with [17-view_scan_results.md](17-view_scan_results.md)
    - Each scan creates a new session folder, so the baseline results stay in their own session for comparison

## End State

At this point

- Every STIGed host has been rescanned
- Each host's results can be compared against its baseline in STIG Viewer
