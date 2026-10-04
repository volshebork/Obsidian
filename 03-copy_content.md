# Copy Content to the Server

Copy the content from [01-download_content.md](01-download_content.md) from the storage device to the server.

## Copy the Content

Log in to the desktop as root.

1. Insert the storage device

2. Mount the storage device

    ```bash
    mount /dev/sr0 /mnt
    ```

    - `/dev/sr0` is the DVD drive
    - For a USB drive, run `lsblk` to find its device name, then mount that instead, (e.g. `/dev/sdb1`)
    - A "mounted read-only" warning is expected for a DVD

3. Create the content directory

    ```bash
    mkdir -p /opt/stig_server/downloads
    ```

4. Copy the downloaded files

    ```bash
    cp /mnt/* /opt/stig_server/downloads/
    ```

    - An "omitting directory 'ansible'" message is expected, since the next step copies it

5. Copy the ansible directory

    ```bash
    cp -r /mnt/ansible /opt/stig-server/ansible
    ```

6. Unmount the storage device

    ```bash
    umount /mnt
    ```

7. Remove the storage device

## Verify the Content

Log in to the desktop as root.

1. Confirm the downloaded files

    ```bash
    ls -1 /opt/stig-server/downloads/
    ```

    - The output should show

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

2. Confirm the ansible directory

    ```bash
    find /opt/stig-server/ansible -type f
    ```

    - The output should show

    ```txt
    /opt/stig-server/ansible/ansible.cfg
    /opt/stig-server/ansible/inventory/inventory.ini
    /opt/stig-server/ansible/playbooks/apply_stig.yml
    /opt/stig-server/ansible/playbooks/repo_clients.yml
    /opt/stig-server/ansible/playbooks/vars.yml
    /opt/stig-server/ansible/roles/repo_clients/files/local.repo
    /opt/stig-server/ansible/roles/repo_clients/tasks/main.yml
    ```

## End State

At this point, the server

- Has every downloaded file in `/opt/stig-server/downloads`
- Has the ansible directory in `/opt/stig-server/ansible`

Next: [04-create_local_repo.md](04-create_local_repo.md) to create the local repo from the RHEL 9.6 DVD.
