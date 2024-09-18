# Ansible Role: restic-exporter

This Ansible role is designed to deploy and configure **restic-exporter**, a Prometheus exporter for the **Restic** backup system. It manages the installation of the exporter, configuration of dependencies, secret management, and sets up a **systemd** service for process management.

### Prerequisites

- Ansible 2.9 or higher
- Python 3 installed on the target host
- A user with sudo privileges on the target host

### Main Features

- Clones the **restic-exporter** Git repository with customizable branch and repository options.
- Creates a Python virtual environment for isolating dependencies.
- Configures environment variables for **Restic**, **AWS**, and **Backblaze**.
- Stores the Restic password in a secured file with appropriate permissions.
- Creates and configures a **systemd** service to manage the **restic-exporter** process.

### Available Variables

The role is fully customizable using the following variables:

#### Repository Configuration

- `restic_exporter_repo`: The Git repository URL for **restic-exporter** (default: `"https://github.com/ngosang/restic-exporter.git"`).
- `restic_exporter_branch`: The Git branch to clone (default: `"main"`).
- `restic_exporter_path`: The local path to clone the project (default: `"/opt/restic-exporter"`).
- `restic_exporter_venv`: Path to the Python virtual environment (default: `"/opt/restic-exporter-venv"`).

#### Restic Configuration

- `restic_repository`: The Restic repository URL or local path (default: `"/data"`).
- `restic_password`: The password for the Restic repository (default: empty).
- `restic_password_file`: The file path containing the Restic password (default: `"{{ ansible_env.HOME }}/.restic_password"`).

#### AWS and Backblaze Configuration (Optional)

- `restic_aws_access_key_id`: AWS access key (default: empty).
- `restic_aws_secret_access_key`: AWS secret key (default: empty).
- `restic_b2_account_id`: Backblaze B2 account ID (default: empty).
- `restic_b2_account_key`: Backblaze B2 account key (default: empty).

#### restic-exporter Configuration

- `restic_refresh_interval`: Refresh interval in seconds for the metrics (default: `60`).
- `restic_listen_port`: The port for **restic-exporter** to listen on (default: `8001`).
- `restic_listen_address`: The address for **restic-exporter** to listen on (default: `"0.0.0.0"`).
- `restic_log_level`: The log level for **restic-exporter** (default: `"INFO"`).
- `restic_exit_on_error`: Whether to shut down on any Restic error (default: `false`).
- `restic_no_check`: Whether to skip the `restic check` operation (default: `false`).
- `restic_no_stats`: Whether to skip the collection of backup statistics (default: `false`).
- `restic_no_locks`: Whether to skip the collection of lock data (default: `false`).
- `restic_include_paths`: Whether to include snapshot paths in the metrics (default: `false`).
- `restic_insecure_tls`: Whether to skip TLS verification for self-signed certificates (default: `false`).

### Usage Example

Here is an example playbook using this role:

```yaml
---
- hosts: restic-exporter
  become: yes
  roles:
    - role: restic-exporter
      vars:
        restic_repository: "s3:s3.amazonaws.com/my-backup-bucket"
        restic_password: "my-secure-password"
        restic_aws_access_key_id: "my-aws-access-key-id"
        restic_aws_secret_access_key: "my-aws-secret-key"
        restic_refresh_interval: 120
        restic_listen_port: 9000
        restic_log_level: "DEBUG"
        restic_no_stats: true
        restic_insecure_tls: true
```

### systemd Commands

The role configures a **systemd** service for **restic-exporter**. You can manage the service using the following commands:

- **Start the service**: `systemctl start restic-exporter`
- **Stop the service**: `systemctl stop restic-exporter`
- **Restart the service**: `systemctl restart restic-exporter`
- **Check the status of the service**: `systemctl status restic-exporter`

### Security

The Restic password and other sensitive variables (such as AWS and Backblaze credentials) are stored securely with appropriate file permissions (`0600`). The **restic-exporter** service is run as **root** to ensure access to critical Restic configuration files.

### Contributions

Contributions are welcome. To report an issue or suggest improvements, please open an **issue** or submit a **pull request**.

### License

This role is licensed under the **MIT** License. Check the `LICENSE` file for more information.
