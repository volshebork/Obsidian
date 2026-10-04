# Install SCC

Install SCC and the UNIX Remote Scanning Plugin, which lets SCC scan clients over SSH.

## Install SCC on Server

Log in to the desktop as root.

1. Install unzip if needed

    ```bash
    dnf install -y unzip
    ```

2. Extract SCC

    ```bash
    unzip /opt/stig_server/downloads/scc-5.15_linux_x86_64_bundle.zip -d /opt/stig_server/scc
    ```

3. Install SCC

    ```bash
    dnf install -y /opt/stig_server/scc/scc-5.15_linux_x86_64/scc-5.15.linux.x86_64.rpm
    ```

4. Launch SCC

    ```bash
    /opt/scc/scc
    ```

5. Confirm the RHEL 9 STIG content is installed
    - In the Content pane, expand Linux
    - RHEL_9_STIG should be listed with version 002.009.013
    - SCC 5.15 includes this content, so no separate install is needed

## Install the UNIX Remote Scanning Plugin

Log in to the desktop as root.

1. Launch SCC if it is not already open

    ```bash
    /opt/scc/scc
    ```

2. Click Choose a Scan Type

3. Select UNIX SSH Remote Scan

4. Click Install UNIX Remote Scan Plugin

5. Browse to `/opt/stig_server/downloads` and select `SCC_5.15_UNIX_Remote_Scanning_Plugin.scc`

6. Click OK on the Key Exchange Algorithm Changed message
    - This only affects hosts saved in older SCC versions, so it does not apply to a new install

7. Confirm the plugin is installed
    - Click Options
    - Under SSH Remote Scanning Options, the Uninstall button next to Uninstall Remote UNIX SSH Plugin should be clickable

## End State

At this point, the server

- Has SCC installed with the RHEL 9 STIG content
- Has the UNIX Remote Scanning Plugin installed

Next: [10-install_stig_viewer.md](10-install_stig_viewer.md) to install STIG Viewer.
