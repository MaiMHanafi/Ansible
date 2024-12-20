# Ansible Playbook: Install and Configure Apache Web Server

## Repository: [Ansible_Install_APACHE](https://github.com/MaiMHanafi/Ansible_Install_APACHE)

---

## Overview
This repository contains an Ansible playbook for automating the installation and configuration of the Apache web server on Linux hosts restrcting the allowed IPs to connect to the web server. The playbook ensures Apache is installed, configured, and running with a specified document root for hosting web applications.

---

## Key Features
- Installs Apache HTTP Server.
- Ensures the service is enabled and running.
- Configures a custom `DocumentRoot` for serving web content.
- Supports multiple hosts (e.g., web servers).
- Restricting some IPs.
---

## Prerequisites

### 1. Control Node Requirements
Ensure the control node meets the following criteria:
- Ansible is installed (v2.9+ recommended).
- SSH access to target nodes is configured.

### 2. Managed Nodes Requirements
- Linux-based operating systems (e.g., Ubuntu, CentOS).
- `sudo` privileges for the user executing the playbook.
- Open port `80` for HTTP traffic.

---

## File Structure
The repository contains the following:

```plaintext
Ansible_Install_APACHE/
├── inventory                 # Inventory file defining target hosts
├── playbook.yml              # Ansible playbook
├── roles/
│   └── apache/
│       ├── tasks/
│       │   └── main.yml      # Tasks for installing and configuring Apache
│       ├── defaults/
│       │   └── main.yml      # Default variables
│       ├── templates/
│       │   └── httpd.conf.j2 # Apache configuration template (if applicable)
│       └── handlers/
│           └── main.yml      # Handlers for reloading/restarting Apache
└── README.md                 # Documentation
```

---

## Usage

### 1. Clone the Repository
```bash
git clone https://github.com/MaiMHanafi/Ansible_Install_APACHE.git
cd Ansible_Install_APACHE
```

### 2. Update the Inventory File
Edit the `inventory` file to define the target hosts. Example:
```ini
[webservers]
webserver01 ansible_host=192.168.1.10 ansible_user=your_user ansible_private_key_file=~/.ssh/id_rsa
webserver02 ansible_host=192.168.1.11 ansible_user=your_user ansible_private_key_file=~/.ssh/id_rsa
```

### 3. Configure Variables
Edit `roles/apache/defaults/main.yml` to customize the `apache_document_root` and other variables:
```yaml
apache_document_root: /var/www/html
```

### 4. Run the Playbook
Execute the playbook to install and configure Apache:
```bash
ansible-playbook -i inventory playbook.yml
```

If a `sudo` password is required, add the `--ask-become-pass` flag:
```bash
ansible-playbook -i inventory playbook.yml --ask-become-pass
```

---

## Example Playbook Execution
```bash
PLAY [Install and Configure Apache] ****************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************
ok: [webserver01]
ok: [webserver02]

TASK [Install Apache] ******************************************************************************************************************
changed: [webserver01]
changed: [webserver02]

TASK [Ensure Apache is Enabled and Running] ********************************************************************************************
ok: [webserver01]
ok: [webserver02]

PLAY RECAP *****************************************************************************************************************************
webserver01                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
webserver02                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

---

## Variables
The playbook uses the following variables:

| Variable               | Description                               | Default Value       |
|------------------------|-------------------------------------------|---------------------|
| `apache_document_root` | Directory for serving web content         | `/var/www/html`     |
| `apache_server_name`   | Apache Server name                        |  `mywebsite.local`  |
| `allowed_ips`          | The allowed (whitelisted) IPs             |    - 192.168.1.10   |
|                        |                                           |    - 192.168.1.20   |
|                        |                                           |    - 192.168.1.30   |
--------------------------------------------------------------------------------------------


## Customization
To customize the Apache configuration:
1. Modify the template file in `roles/apache/templates/httpd.conf.j2`.
2. Adjust the variables in `defaults/main.yml`.

---

## Troubleshooting
### Common Issues
1. **Permission Denied (Public Key):**
   - Ensure the SSH key is copied to the target hosts:
     ```bash
     ssh-copy-id -i ~/.ssh/id_rsa your_user@target_host_IP
     ```

2. **Sudo Password Required:**
   - Run the playbook with the `--ask-become-pass` flag.
   - In the Target hosts, in the /etc/sudoers file ,Add this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL) NOPASSWD:ALL #if EC2 > Group (sudo)
%wheel  ALL=(ALL) NOPASSWD:ALL #if VM > Group (sudo)


3. **Port 80 Already in Use:**
   - Stop conflicting services using port `80`:
     ```bash
     sudo systemctl stop nginx
     ```

---

## Contributing
Contributions are welcome! Please submit a pull request or open an issue to suggest improvements.

---


## Author
Created by [Mai M. Hanafi](https://github.com/MaiMHanafi).
Email: maihanafi34@gmail.com
LinkedIn Profile: linkedin.com/in/mai-mohamed-hanafi-388b131b5
