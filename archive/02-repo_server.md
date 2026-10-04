# Repo Server

Copy the BaseOS and AppStream repos from the RHEL 9.6 DVD to the server, then publish them over HTTP so other offline RHEL hosts can install packages from the server.

## Copy the repos from the DVD

Log in to the desktop as root.

1. Insert the RHEL 9.6 DVD, mount it, and copy the repos

    ```bash
    # Mount the DVD
    mount /dev/sr0 /mnt

    # Create the repo directory
    mkdir -p /var/www/html/repos

    # Copy the BaseOS and AppStream repos
    cp -r /mnt/BaseOS /var/www/html/repos/
    cp -r /mnt/AppStream /var/www/html/repos/

    # Set the SELinux labels so Apache can read the repos
    restorecon -R /var/www/html/repos

    # Unmount and eject the DVD
    umount /mnt
    eject
    ```

    - The copy takes several minutes
    - `/var/www/html` is Apache's default web folder, so the repos are ready to publish once Apache is installed

## Point the server at its local repo

Log in to the desktop as root.

1. Create the repo configuration file

    ```bash
    touch /etc/yum.repos.d/local.repo
    vim /etc/yum.repos.d/local.repo
    ```

    - Press `i` to enter insert mode, then add the following content

    ```ini
    [local-baseos]
    name=Local BaseOS
    baseurl=file:///var/www/html/repos/BaseOS
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

    [local-appstream]
    name=Local AppStream
    baseurl=file:///var/www/html/repos/AppStream
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
    ```

    - Press `Esc`, then type `:wq` and press `Enter` to save and exit
    - The server reads its repo straight from disk, so it keeps working even if Apache is stopped

2. Verify both repos are available

    ```bash
    dnf clean all
    dnf repolist
    ```

    - Both `local-baseos` and `local-appstream` should be listed
    - A "This system is not registered" message is expected and can be ignored

## Publish the repo over HTTP

Log in to the desktop as root.

1. Install Apache

    `dnf install -y httpd`

2. Start Apache and enable it at boot

    `systemctl enable --now httpd`

3. Allow HTTP through the firewall

    ```bash
    # Add the rule permanently, then reload so it takes effect now
    firewall-cmd --permanent --add-service=http
    firewall-cmd --reload

    # Verify http is listed
    firewall-cmd --list-services
    ```

4. Verify the repo is published

    ```bash
    curl -s http://localhost/repos/BaseOS/repodata/repomd.xml | head -n 5
    curl -s http://localhost/repos/AppStream/repodata/repomd.xml | head -n 5
    ```

    - Each command should print the start of an XML file
    - An HTML error page means Apache cannot find or read the repo

## Verify from another host

Log in to any host on the same network as the server.

1. Confirm the host can reach the repo

    ```bash
    curl -s http://<repo-server-ip>/repos/BaseOS/repodata/repomd.xml | head -n 5
    ```

    - Replace `<repo-server-ip>` with the server's IP address
    - The output should be the start of an XML file, the same as on the server
    - Pointing hosts at this repo is handled by the repo client playbook in `03-ansible-server.md`

## End State

At this point, the server

- Installs packages from its own local AppStream and BaseOS repos
- Hosts the AppStream and BaseOS repos over HTTP for clients on the network
- Clients **cannot** install packages from the server yet, since their repo configuration is set up by Ansible in `03-ansible-server.md`

Next: `03-ansible-server.md` sets up Ansible to configure and STIG hosts.
