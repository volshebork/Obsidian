# SCC Server

Install SCC and the RHEL 9 STIG benchmark to scan clients, and STIG Viewer to review the scan results. No scans are run in this document.

## Install SCC

Log in to the desktop as root.

1. Extract SCC and the benchmark

    ```bash
    # Install unzip if it is not already installed
    dnf install -y unzip

    # Extract SCC
    unzip /opt/disa/downloads/scc-5.15_linux_x86_64_bundle.zip -d /opt/disa/scc

    # Extract the benchmark
    unzip /opt/disa/downloads/U_RHEL_9_V2R9_STIG_SCAP_1-4_Benchmark-enhancedV13-signed.zip -d /opt/disa/benchmark
    ```

2. Install SCC

    ```bash
    dnf install -y /opt/disa/scc/scc-5.15_linux_x86_64/scc-5.15.linux.x86_64.rpm
    ```

    - `dnf` is used instead of `rpm` so any dependencies are installed from the local repo

3. Launch SCC

    ```bash
    /opt/scc/scc
    ```

4. Install the benchmark in SCC
    - In the Content pane, click Install
    - Select Content File(s) to Install
    - Browse to `/opt/disa/benchmark` and select `U_RHEL_9_V2R9_STIG_SCAP_1-4_Benchmark-enhancedV13-signed.xml`
    - Confirm Red Hat Enterprise Linux 9 STIG appears in the Content pane

5. Install the UNIX Remote Scanning Plugin
    - Click Choose a Scan Type
    - Select UNIX SSH Remote Scan
    - Click Install UNIX Remote Scan Plugin
    - Browse to `/opt/disa/downloads` and select `SCC_5.15_UNIX_Remote_Scanning_Plugin.scc`
    - Click OK on the Key Exchange Algorithm Changed message

## Install STIG Viewer

Log in to the desktop as root.

1. Extract STIG Viewer and the STIG

    ```bash
    # Extract STIG Viewer and rename its folder
    unzip /opt/disa/downloads/U_STIGViewer-linux-x64-3-8-1.zip -d /opt/disa
    mv /opt/disa/stig_viewer_3-linux-x64 /opt/disa/stigviewer

    # Extract the STIG
    unzip /opt/disa/downloads/U_RHEL_9_V2R9_STIG.zip -d /opt/disa/stig
    ```

2. Launch STIG Viewer

    ```bash
    /opt/disa/stigviewer/STIG\ Viewer\ 3 --no-sandbox
    ```

    - `--no-sandbox` is required to run STIG Viewer as root

3. Open the STIG in STIG Viewer
    - In the STIG Viewer row, click Open
    - Browse to `/opt/disa/stig/U_RHEL_9_V2R9_Manual_STIG` and select `U_RHEL_9_STIG_V2R9_Manual-xccdf.xml`
    - Confirm the Red Hat Enterprise Linux 9 STIG opens and is listed under the STIG Viewer row on the home screen

## End State

At this point, the server

- Has SCC installed with the RHEL 9 STIG benchmark loaded
- Has STIG Viewer installed with the RHEL 9 STIG opened

Next: `05-onboard-a-client.md` sets up a client so the server can reach it with Ansible and SCC.
