# Obsidian

This repo was written for RHEL 9.6.

These documents are meant to be read and followed from this repo. Downloading these files may change the behavior of relative links.

> [!IMPORTANT]
> The STIG settings and configurations in this repo have not been tested against the software running on your clients, and may conflict with configurations required by your organization.

The repo walks you through building a server that will allow clients to install packages and have STIGs applied. You will also be able to run Ansible playbooks from this server to any clients. You will no longer need to move files to each workstation you build as long as this server is maintained.

In the end you will have one server acting as:

- The repo server
- An Ansible server
- An SCC server to scan clients
- A STIG Viewer server to view the scans of clients

## Documents

Follow the documents in order if you are starting from no server. Afterwards, you can reference any document as needed.

### Prepare

1. [01-download_content.md](01-download_content.md)
2. [02-install_rhel_server.md](02-install_rhel_server.md)
3. [03-copy_content.md](03-copy_content.md)

### Repo server

4. [04-create_local_repo.md](04-create_local_repo.md)
5. [05-publish_repo.md](05-publish_repo.md)

### Ansible server

6. [06-install_ansible.md](06-install_ansible.md)
7. [07-set_up_ansible_directory.md](07-set_up_ansible_directory.md)
8. [08-generate_ssh_key.md](08-generate_ssh_key.md)

### SCC server

9. [09-install_scc.md](09-install_scc.md)
10. [10-install_stig_viewer.md](10-install_stig_viewer.md)

### Clients

11. [11-install_rhel_client.md](11-install_rhel_client.md)
12. [12-onboard_client.md](12-onboard_client.md)
13. [13-connect_client_to_repo.md](13-connect_client_to_repo.md)

### Baseline scan

14. [14-scan_server.md](14-scan_server.md)
15. [15-add_clients_to_scc.md](15-add_clients_to_scc.md)
16. [16-scan_clients.md](16-scan_clients.md)
17. [17-view_scan_results.md](17-view_scan_results.md)

### Apply the STIG

18. [18-skip_rules.md](18-skip_rules.md)
19. [19-stig_server.md](19-stig_server.md)
20. [20-stig_clients.md](20-stig_clients.md)
21. [21-rescan.md](21-rescan.md)

## Validation

Documents 01 through 18 were tested on RHEL 9.6.

Documents 19 and 20 were tested through their `--check` runs only. The STIG was not applied, so the rules that need to be skipped in your environment have not been identified. Start with a test client, as recommended in those documents.

## Not Covered

- Apache STIGs
  - The server runs Apache to publish the repos, so it also falls under the Apache Server 2.4 UNIX Server and Site STIGs
  - SCC includes content for both, but these documents do not scan or harden Apache
- SCC Manual Questions answer files
  - SCC marks rules it cannot check automatically as Not Reviewed
  - SCC can answer those rules automatically from an answer file, which saves time when the same answers apply to every client
- Scripting
  - Most steps in these documents can be scripted, and readers are welcome to do so

```txt
 ____  _____ ____  ____   _    _ _ _ 
|  _ \| ____|  _ \|  _ \ / \  | | | |
| |_) |  _| | |_) | |_) / _ \ | | | |
|  __/| |___|  __/|  __/ ___ \|_|_|_|
|_|   |_____|_|   |_| /_/   \_(_|_|_)
```
