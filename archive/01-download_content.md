# Download Content

Download everything the STIG server needs on a Windows PC with internet access, burn it to a DVD, then copy it to the server. This is the only step that needs internet access.

This document was written for RHEL 9.6, using the file versions listed below. When newer versions are released, update the filenames and versions in this document to match.

## Download the content

1. Click each link below to download the file. The versions listed have been confirmed to work together.

    | Item | File |
    | --- | --- |
    | RHEL 9 STIG | [U_RHEL_9_V2R9_STIG.zip](https://ncp.nist.gov/checklist/1072/download/18717) |
    | RHEL 9 STIG for Ansible | [U_RHEL_9_V2R9_STIG_Ansible.zip](https://ncp.nist.gov/checklist/1072/download/18718) |
    | RHEL 9 STIG SCAP 1.4 Enhanced Benchmark | [U_RHEL_9_V2R9_STIG_SCAP_1-4_Benchmark-enhancedV13-signed.zip](https://ncp.nist.gov/checklist/1072/download/18720) |
    | SCC (SCAP Compliance Checker) | [scc-5.15_linux_x86_64_bundle.zip](https://ncp.nist.gov/checklist/1072/download/18721) |
    | SCC UNIX Remote Scanning Plugin | `SCC_5.15_UNIX_Remote_Scanning_Plugin.scc` from the SCAP Tools section of [DoD Cyber Exchange SCAP](https://www.cyber.mil/stigs/SCAP) |
    | STIG Viewer | `U_STIGViewer-linux-x64-3-8-1.zip` from [DoD Cyber Exchange SRG / STIG Tools](https://www.cyber.mil/stigs/srg-stig-tools) |
    | community.general collection | [community-general-12.0.1.tar.gz](https://galaxy.ansible.com/ui/repo/published/community/general/?version=12.0.1) |
    | ansible.posix collection | [ansible-posix-2.1.0.tar.gz](https://galaxy.ansible.com/ui/repo/published/ansible/posix/?version=2.1.0) |

    - The full DISA checklist can be found at [Red Hat Enterprise Linux 9 Ver 2, Rel 9 Checklist](https://ncp.nist.gov/checklist/1072), but you do not need it for this guide
    - Newer versions of the Ansible collections may not work with the RHEL 9 STIG for Ansible role

2. Burn all downloaded files to a DVD

## Copy the content to the server

Log in to the desktop as root.

1. Insert the DVD and copy the content

    ```bash
    # Mount the DVD
    mount /dev/sr0 /mnt

    # Create the downloads directory
    mkdir -p /opt/disa/downloads

    # Copy the content
    cp /mnt/* /opt/disa/downloads/

    # Unmount and eject the DVD
    umount /mnt
    eject
    ```

2. Verify all eight files were copied

    ```bash
    ls -1 /opt/disa/downloads/
    ```

    - The output should show
        - `ansible-posix-2.1.0.tar.gz`
        - `community-general-12.0.1.tar.gz`
        - `scc-5.15_linux_x86_64_bundle.zip`
        - `SCC_5.15_UNIX_Remote_Scanning_Plugin.scc`
        - `U_RHEL_9_V2R9_STIG_Ansible.zip`
        - `U_RHEL_9_V2R9_STIG_SCAP_1-4_Benchmark-enhancedV13-signed.zip`
        - `U_RHEL_9_V2R9_STIG.zip`
        - `U_STIGViewer-linux-x64-3-8-1.zip`

## End State

At this point, the server has all the files needed to

- Host Ansible to configure and STIG hosts
- Host SCC to scan hosts
- Host STIG Viewer to review scan results

Next: `02-repo-server.md` copies the AppStream and BaseOS repos from the RHEL 9.6 DVD and hosts them for other hosts on the network.
