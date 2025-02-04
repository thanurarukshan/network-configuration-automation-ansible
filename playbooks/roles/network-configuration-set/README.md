1Ô∏è‚É£ Configure Network Interface (Static/DHCP)
yaml
Copy
Edit
- name: Configure network interface (Static/DHCP)
  ansible.builtin.template:
    src: network_config.j2
    dest: "/etc/sysconfig/network-scripts/ifcfg-{{ network_interface }}"
  notify: Restart Network
  when: not use_dhcp
Ì†ΩÌ¥π What it does:
Uses an Ansible Jinja2 template (network_config.j2) to configure a network interface.
If use_dhcp: false, it applies a static IP configuration to the network interface file.
The network service is restarted via a handler (Restart Network) to apply changes.
If DHCP is enabled, this task is skipped.
2Ô∏è‚É£ Set Hostname
yaml
Copy
Edit
- name: Set hostname
  ansible.builtin.hostname:
    name: "{{ hostname }}"
  when: set_hostname
Ì†ΩÌ¥π What it does:
Changes the system hostname using the hostname module.
Only runs if set_hostname: true.
3Ô∏è‚É£ Update /etc/hosts File
yaml
Copy
Edit
- name: Update /etc/hosts
  ansible.builtin.lineinfile:
    path: /etc/hosts
    line: "127.0.1.1 {{ hostname }}"
    state: present
  when: set_hostname
Ì†ΩÌ¥π What it does:
Ensures that the new hostname is added to /etc/hosts for local resolution.
This prevents issues where the system fails to resolve its own name.
Runs only if set_hostname: true.
4Ô∏è‚É£ Configure DNS Servers
yaml
Copy
Edit
- name: Configure DNS servers
  ansible.builtin.template:
    src: resolv.conf.j2
    dest: /etc/resolv.conf
  when: dns_servers is defined
Ì†ΩÌ¥π What it does:
Uses a template (resolv.conf.j2) to configure DNS settings in /etc/resolv.conf.
If dns_servers is defined, it writes the provided DNS IP addresses to the configuration file.
5Ô∏è‚É£ Configure SSH - Disable Root Login
yaml
Copy
Edit
- name: Configure SSH settings
  ansible.builtin.lineinfile:
    path: /etc/ssh/sshd_config
    regexp: "^PermitRootLogin"
    line: "PermitRootLogin no"
  notify: Restart SSH
  when: disable_root_login
Ì†ΩÌ¥π What it does:
Edits /etc/ssh/sshd_config to disable root login via SSH.
Uses the lineinfile module to modify an existing configuration line.
Notifies a handler (Restart SSH) to restart SSH for changes to take effect.
Runs only if disable_root_login: true.
6Ô∏è‚É£ Configure SSH - Change SSH Port
yaml
Copy
Edit
- name: Set SSH port
  ansible.builtin.lineinfile:
    path: /etc/ssh/sshd_config
    regexp: "^#?Port "
    line: "Port {{ ssh_port }}"
  notify: Restart SSH
Ì†ΩÌ¥π What it does:
Updates the SSH daemon configuration to use a custom SSH port instead of the default port 22.
The regular expression ^#?Port ensures that even if the port is commented out (#Port 22), it will be modified.
SSH service is restarted after the change.
7Ô∏è‚É£ Enable and Start Firewall
yaml
Copy
Edit
- name: Enable and start firewall
  ansible.builtin.service:
    name: firewalld
    state: started
    enabled: yes
  when: firewall_enable
Ì†ΩÌ¥π What it does:
Ensures the firewall service (firewalld) is running and enabled on boot.
Runs only if firewall_enable: true.
8Ô∏è‚É£ Open Required Firewall Ports
yaml
Copy
Edit
- name: Open required firewall ports
  ansible.builtin.firewalld:
    port: "{{ item }}/tcp"
    permanent: yes
    state: enabled
  with_items: "{{ open_ports }}"
  when: firewall_enable
Ì†ΩÌ¥π What it does:
Loops through the list of open_ports and enables them in firewalld.
Ensures ports are opened permanently, so they remain open after reboots.
Runs only if firewall_enable: true.
9Ô∏è‚É£ Enable and Start NTP Service
yaml
Copy
Edit
- name: Enable and configure NTP
  ansible.builtin.service:
    name: chronyd
    state: started
    enabled: yes
  when: enable_ntp
Ì†ΩÌ¥π What it does:
Ensures the NTP (chronyd) service is enabled and running.
Runs only if enable_ntp: true.
Ì†ΩÌ¥ü Configure NTP Servers
yaml
Copy
Edit
- name: Configure NTP servers
  ansible.builtin.lineinfile:
    path: /etc/chrony.conf
    line: "server {{ item }} iburst"
  with_items: "{{ ntp_servers }}"
  notify: Restart NTP
  when: enable_ntp
Ì†ΩÌ¥π What it does:
Loops through ntp_servers and adds them to /etc/chrony.conf.
Uses iburst for faster synchronization.
Notifies a handler (Restart NTP) to restart the service.
Runs only if enable_ntp: true.
Handlers (handlers/main.yml)
These handlers ensure changes take effect by restarting services when needed.

yaml
Copy
Edit
- name: Restart Network
  ansible.builtin.service:
    name: NetworkManager
    state: restarted

- name: Restart SSH
  ansible.builtin.service:
    name: sshd
    state: restarted

- name: Restart NTP
  ansible.builtin.service:
    name: chronyd
    state: restarted

