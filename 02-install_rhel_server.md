# Install RHEL on the Server

Install RHEL 9.6 from the installation DVD using these settings.

## Install RHEL

1. Boot the server from the RHEL 9.6 DVD
2. Set Software Selection to Server with GUI
3. Set a static IP address
4. Set the hostname
5. Set a root password
6. Leave Lock root account unchecked
7. Create an admin user and check Make this user administrator
8. Begin the installation and reboot when it finishes
9. Keep the RHEL 9.6 DVD on hand, since `04-create_local_repo.md` copies the repos from it

## Verify the Install

Log in to the desktop as root.

1. Confirm the RHEL version, hostname, IP address, and admin account

```bash
cat /etc/redhat-release
hostname
ip -4 addr
id <admin-user>
```

- The release should show 9.6, and `id` should include `wheel`

## End State

At this point, the server

- Has RHEL 9.6 installed with a GUI
- Has a static IP address and hostname
- Allows root to log in to the desktop
- Has an admin account in `wheel`

Next: `03-copy_content.md` to copy the downloaded content to the server.
