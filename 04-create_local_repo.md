# Create the Local Repo

Copy the BaseOS and AppStream repos from the RHEL 9.6 DVD to the server, and point the server at them.

## Copy the Repos From the DVD

Log in to the desktop as root.

1. Insert the RHEL 9.6 DVD

2. Mount the DVD

    ```bash
    mount /dev/sr0 /mnt
    ```

    - A `mounted read-only` warning is expected

3. Create the repo directory

    ```bash
    mkdir -p /var/www/html/repos
    ```

4. Copy the BaseOS repo

    ```bash
    cp -r /mnt/BaseOS /var/www/html/repos/
    ```

5. Copy the AppStream repo

    ```bash
    cp -r /mnt/AppStream /var/www/html/repos/
    ```

    - The copies take several minutes

6. Set the SELinux labels so Apache can read the repos

    ```bash
    restorecon -R /var/www/html/repos
    ```

7. Unmount the DVD

    ```bash
    umount /mnt
    ```

8. Remove the DVD

## Point the Server at the Local Repo

Log in to the desktop as root.

1. Copy the repo file

    ```bash
    cp /opt/stig_server/downloads/local.repo /etc/yum.repos.d/local.repo
    ```

    - The server reads its repo straight from disk, so it keeps working even if Apache is stopped

## Verify the Local Repo

Log in to the desktop as root.

1. Confirm both repos are listed

    ```bash
    dnf clean all
    dnf repolist
    ```

    - `local-baseos` and `local-appstream` should be listed
    - A `This system is not registered` message is expected and can be ignored

2. Confirm the SELinux labels

    ```bash
    ls -Zd /var/www/html/repos/BaseOS /var/www/html/repos/AppStream
    ```

    - Both should show `httpd_sys_content_t`

## End State

At this point, the server

- Has the BaseOS and AppStream repos in `/var/www/html/repos`
- Installs packages from its own local repo

Next: [05-publish_repo.md](05-publish_repo.md) to host the repos over HTTP for clients.
