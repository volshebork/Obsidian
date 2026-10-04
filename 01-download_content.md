# Download Content

Download the following content to a hard drive or burn to a DVD.

## STIG Files From the Internet

1. Click each link below to get to the download page. The versions listed have been confirmed to work together.

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

## Files From This Repo

If you are unable to download this repo from github, you can skp this step. You will need to manually recreate the directories and files on the server.

1. Download this repo as a zip
    - Go to [Obsidian](https://github.com/volshebork/Obsidian)
    - Click Code, then Download ZIP

2. Extract the zip
3. Copy the `ansible` directory to your DVD/storage device
4. Copy the local.repo file to your DVD/storage device

## Verify Files

You should have the following files downloaded:

```txt
ansible-posix-2.1.0.tar.gz
community-general-12.0.1.tar.gz
local.repo
scc-5.15_linux_x86_64_bundle.zip
SCC_5.15_UNIX_Remote_Scanning_Plugin.scc
U_RHEL_9_V2R9_STIG_Ansible.zip
U_RHEL_9_V2R9_STIG_SCAP_1-4_Benchmark-enhancedV13-signed.zip
U_RHEL_9_V2R9_STIG.zip
U_STIGViewer-linux-x64-3-8-1.zip
```

You should have the following directory downloaded:

```txt
ansible/
├── ansible.cfg
├── inventory/
│   └── inventory.ini
├── playbooks/
│   ├── apply-stig.yml
│   ├── repo-clients.yml
│   └── vars.yml
└── roles/
    └── repo_clients/
        ├── files/
        │   └── local.repo
        └── tasks/
            └── main.yml
```

## End State

At this point, your DVD or hard drive...

- Has the files needed for the server

Next: `02-install_rhel_server.md` to build the server.
