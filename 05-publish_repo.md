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

Client access to the repos is verified in [13-connect_client_to_repo.md](13-connect_client_to_repo.md), once a client is built.

## End State

At this point, the server

- Hosts the BaseOS and AppStream repos over HTTP

Next: [06-install_ansible.md](06-install_ansible.md) to install Ansible and the collections.
