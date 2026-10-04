# Publish the Repo

Host the server's BaseOS and AppStream repos over HTTP so clients can install packages from the server.

## Install and Start Apache

Log in to the desktop as root.

1. Install Apache

    ```bash
    dnf install -y httpd
    ```

2. Start Apache and enable it at boot

    ```bash
    systemctl enable --now httpd
    ```

3. Allow HTTP through the firewall

    ```bash
    firewall-cmd --permanent --add-service=http
    ```

4. Reload the firewall to apply the rule

    ```bash
    firewall-cmd --reload
    ```

## Verify on the Server

Log in to the desktop as root.

1. Confirm Apache is running

    ```bash
    systemctl is-active httpd
    ```

    - The output should be `active`

2. Confirm HTTP is allowed through the firewall

    ```bash
    firewall-cmd --list-services
    ```

    - `http` should be listed

3. Confirm both repos are published

    ```bash
    curl -s http://localhost/repos/BaseOS/repodata/repomd.xml | head -n 5
    curl -s http://localhost/repos/AppStream/repodata/repomd.xml | head -n 5
    ```

    - Each command should print the start of an XML file
    - An HTML error page means Apache cannot find or read the repos

## Verify From a Client

Log in to any client's desktop as root.

1. Confirm the client can reach the repo

    ```bash
    curl -s http://<repo-server-ip>/repos/BaseOS/repodata/repomd.xml | head -n 5
    ```

    - Replace `<repo-server-ip>` with the server's IP address
    - The output should be the start of an XML file, the same as on the server
    - Clients are pointed at the repo in a later step

## End State

At this point, the server

- Hosts the BaseOS and AppStream repos over HTTP
- Is reachable by clients on the network

Clients cannot install packages from the server yet, since their repo file is set up by Ansible.

Next: [06-install_ansible.md](06-install_ansible.md) to install Ansible and the collections.
