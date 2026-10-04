# Install STIG Viewer

Install STIG Viewer and open the RHEL 9 STIG, so scan results can be reviewed.

## Install STIG Viewer on Server

Log in to the desktop as root.

1. Install unzip if needed

    ```bash
    dnf install -y unzip
    ```

2. Extract STIG Viewer

    ```bash
    unzip /opt/stig_server/downloads/U_STIGViewer-linux-x64-3-8-1.zip -d /opt/stig_server
    ```

3. Launch STIG Viewer

    ```bash
    /opt/stig_server/stig_viewer_3-linux-x64/STIG\ Viewer\ 3 --no-sandbox
    ```

    - `--no-sandbox` is required to run STIG Viewer as root
    - Type `/opt/stig_server/stig_viewer_3-linux-x64/S` and press `Tab` to complete the rest of the path

4. Launch STIG Viewer

    ```bash
    /opt/stig_server/stigviewer/STIG\ Viewer\ 3 --no-sandbox
    ```

    - `--no-sandbox` is required to run STIG Viewer as root

## Open the RHEL 9 STIG

Log in to the desktop as root.

1. Extract the STIG

    ```bash
    unzip /opt/stig_server/downloads/U_RHEL_9_V2R9_STIG.zip -d /opt/stig_server/stig
    ```

2. Launch STIG Viewer if it is not already open

    ```bash
    /opt/stig_server/stig_viewer_3-linux-x64/STIG\ Viewer\ 3 --no-sandbox
    ```

3. In the STIG Viewer row, click Open

4. Browse to `/opt/stig_server/stig/U_RHEL_9_V2R9_Manual_STIG` and select `U_RHEL_9_STIG_V2R9_Manual-xccdf.xml`

5. Confirm the RHEL 9 STIG opens
    - Return to the home screen
    - The RHEL 9 STIG should be listed in the STIG Viewer row

## End State

At this point, the server

- Has STIG Viewer installed
- Has the RHEL 9 STIG opened in STIG Viewer

Next: [11-install_rhel_client.md](11-install_rhel_client.md) to build a client.
