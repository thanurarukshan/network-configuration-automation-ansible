# Network Configuration Automation with Ansible

## Overview
This project provides an automated approach to configuring, managing, and monitoring network and system settings on multiple Linux servers using Ansible. The playbook is designed to ensure consistency and reliability across environments.

## Features
- **Network Configuration** - Set up network interfaces, IP addressing, and routing.
- **User Management** - Create and manage user accounts.
- **Package Management** - Install and update required packages.
- **System Tuning** - Optimize system settings for performance.
- **Monitoring** - Set up monitoring tools such as Prometheus, Grafana, and SNMP.
- **Security** - Configure SELinux and firewall settings.
- **Time Synchronization** - Set up Chrony for accurate timekeeping.
- **Server Partitioning** - Automate disk partitioning.
- **System Updates** - Keep the system up-to-date with Yum updates.

## Prerequisites
- **Ansible Installed** - Ensure Ansible is installed on the control node.
- **SSH Access** - The control node must have SSH access to target servers.
- **Sudo Privileges** - The user running the playbook should have sudo privileges.

## Project Structure
```sh
network-configuration-automation-ansible/
│── ansible.cfg
│── inventory/
│   ├── dev.ini
│── playbooks/
│   ├── site.yml
│── roles/
│   ├── connectivity-test/
│   ├── network-configuration-set/
│   ├── user-management/
│   ├── package-management/
│   ├── system-tuning/
│   ├── monitoring/
│   ├── server-partitioning/
│   ├── grafana/
│   ├── node_exporter/
│   ├── prometheus_installation/
│   ├── snmp_notifier/
│   ├── chrony_config/
│   ├── selinux-configuration/
│   ├── systemctl_runlevel/
│   ├── yum_update_and_sync/
│   ├── scan/
```

## Installation
Clone the repository:
```sh
git clone https://github.com/thanurarukshan/network-configuration-automation-ansible.git
```

Navigate into the project directory:
```sh
cd network-configuration-automation-ansible
```

Install Ansible if not already installed:
```sh
sudo apt update && sudo apt install ansible -y  # For Debian-based systems
sudo yum install ansible -y  # For RedHat-based systems
```

## Usage
Update the `inventory/dev.ini` file with your target servers.

Run the playbook:
```sh
ansible-playbook -i playbooks/dev.ini playbooks/site.yml --ask-become-pass
```

Monitor execution and check logs for any failures.

## Roles Explained
Each role is modular and serves a specific purpose in automating network and system management:
- **connectivity-test** - Ensures target servers are reachable.
- **network-configuration-set** - Configures network interfaces.
- **user-management** - Manages user creation and access.
- **package-management** - Installs necessary software packages.
- **system-tuning** - Optimizes system performance.
- **monitoring** - Deploys monitoring tools like Prometheus and Grafana.
- **server-partitioning** - Automates disk partitioning.
- **security** - Configures firewall and SELinux policies.
- **time-sync** - Ensures accurate time synchronization using Chrony.
- **scan** - Scans system configurations and generates reports.

## Contribution
Feel free to fork the repository, create a feature branch, and submit a pull request. Issues and suggestions are welcome!

## Contact
For any queries or contributions, reach out to thanurarukshan2000@gmail.com.
