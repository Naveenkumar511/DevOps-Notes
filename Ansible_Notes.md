# ANSIBLE NOTES (Beginner) - Notepad Format

# 1. What is Configuration Management Tool?

## Definition

A Configuration Management (CM) tool is used to automatically configure, manage, and maintain multiple servers in a consistent state.

Instead of logging into each server manually, a CM tool performs the same task on all servers automatically.

## Why do we need Configuration Management?

Without CM Tool:
- Login to each server manually
- Install software one by one
- Configure services individually
- High chance of human errors
- Time consuming

With CM Tool:
- Manage hundreds of servers at once
- Faster deployment
- Consistent configuration
- Easy maintenance
- Less human error

## Real-Time Example

Suppose a company has **100 Linux servers**.

Task:
- Install Nginx
- Start Nginx Service
- Enable Service
- Copy Website Files

Without CM Tool:
- Login to all 100 servers
- Execute commands manually

With CM Tool:
- Run one automation script
- All servers are configured automatically

## Popular Configuration Management Tools

- Ansible
- Puppet
- Chef
- SaltStack

# 2. What is Ansible?

## Definition

Ansible is an **Open Source Configuration Management and Automation Tool** developed by **Red Hat**.

It is used to:
- Configure Servers
- Install Software
- Deploy Applications
- Manage Users
- Automate Repetitive Tasks
- Provision Cloud Infrastructure

Ansible communicates with remote servers using **SSH**.

No agent installation is required on managed servers.

## Advantages of Ansible

- Open Source
- Easy to Learn
- Agentless (No agent required)
- Uses SSH
- YAML-based Playbooks
- Idempotent
- Fast Deployment
- Scalable
- Cross Platform
- Reusable Playbooks

# 3. Ansible Installation on Ubuntu Server

```bash
sudo apt update
sudo apt install ansible -y
ansible --version
```

Inventory:
```text
/etc/ansible/hosts
```

Generate SSH key:
```bash
ssh-keygen
ssh-copy-id ubuntu@<server-ip>
ssh ubuntu@<server-ip>
```
Ansible Configuration:
======================

Create Directory
 mkdir ansible-project
 cd ansible-project
 vim ansible.cfg
   
[defaults]

inventory = ./inventory
remote_user = ubuntu
private_key_file = ~/.ssh/id_rsa
host_key_checking = False

[privilege_escalation]

become = True
become_method = sudo
become_user = root

vim inventory

[prod]
5.5.6.5

copy the pem key




Test connection:
```bash
ansible all -m ping
```

# 4. Ansible Ad-Hoc Commands

Syntax:
```bash
ansible <host> -m <module> -a "<arguments>"
```

Examples:
```bash
ansible all -m ping
ansible all -a "hostname"
ansible all -a "uptime"
ansible all -a "df -h"
ansible all -m apt -a "name=nginx state=present"
ansible all -m service -a "name=nginx state=started"
ansible all -m copy -a "src=index.html dest=/tmp/index.html"
```

# 5. Ansible Playbook

Example:

```yaml
---
- name: Install Nginx
  hosts: web
  become: yes

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx
      service:
        name: nginx
        state: started
```

Run:
```bash
ansible-playbook install-nginx.yml
ansible-playbook install-nginx.yml --syntax-check
ansible-playbook install-nginx.yml --check
```

Examples:
=========
# Copy file from control node to managed node
```yaml
---
- name: Copy HTML File
  hosts: web
  become: yes

  tasks:

    - name: Copy index.html
      copy:
        src: index.html
        dest: /var/www/html/index.html

```

# Copy files already in managed node

```yaml
---
- name: Copy Remote File
  hosts: all
  become: yes

  tasks:

    - name: Copy remote file
      copy:
        src: /tmp/app.conf 
        dest: /etc/app.conf
        remote_src: yes

```


# Deploy and Configure Web Servers

```yaml
---
- name: Deploy and Configure Web Servers
  hosts: webservers
  become: true  # Runs tasks as root/sudo, required for installing packages

  tasks:
    - name: Ensure Apache is installed (Ubuntu/Debian)
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Deploy custom index.html landing page
      copy:
        content: "<h1>Welcome to my Ansible-automated web server!</h1>"
        dest: /var/www/html/index.html
        owner: www-data
        group: www-data
        mode: '0644'

    - name: Ensure Apache is running and enabled on boot
      service:
        name: apache2
        state: started
        enabled: yes
```
========================================
#  Ansible Variables

What are Variables?

Variables are used to store values that can be reused in Ansible playbooks.

Instead of hardcoding values like package names, usernames, or ports, you can store them in variables.

This makes playbooks:

Reusable
Easy to maintain
Easy to update

# Example:

```yaml
---
- name: Install Nginx
  hosts: all
  become: yes

  vars:
    package_name: nginx
    service_name: nginx
    port: 80

  tasks:
    - name: Install package
      apt:
        name: "{{ package_name }}"
        state: present

    - name: Start service
      service:
        name: "{{ service_name }}"
        state: started
        enabled: yes
```

# Example:
```yaml
---
- name: Install Packages
  hosts: all
  become: yes

  vars:
    packages:
      - git
      - curl
      - unzip

  tasks:
    - name: Install packages
      apt:
        name: "{{ packages }}"
        state: present
```

====================================
# Ansible Handlers

What are Handlers?

Handlers are special tasks that run only when notified by another task.

They are mainly used to:

Restart services
Reload services
Reboot servers
Restart applications

Unlike normal tasks, handlers do not run every time. They run only if a task reports a change.

```yaml 
---
- name: Configure Nginx
  hosts: all
  become: yes

  tasks:

    - name: Copy nginx configuration
      copy:
        src: nginx.conf
        dest: /etc/nginx/nginx.conf
      notify:
        - Restart Nginx

  handlers:

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```
=====================================================

# What is an Ansible Role?

An Ansible Role is a way to organize playbooks, variables, tasks, handlers, templates, and files into a standard directory structure.

Instead of writing everything in one large playbook, roles split the project into reusable components.

Think of a role as a module for your Ansible project.

# Why Do We Use Roles?

Without Roles:

playbook.yml
│
├── 500+ lines of YAML
├── Variables
├── Tasks
├── Handlers
├── Templates
└── Files

Problems:

Difficult to read
Difficult to maintain
Difficult to reuse

With Roles:
playbook.yml
│
└── roles/
    ├── nginx/
    ├── docker/
    ├── mysql/
    └── jenkins/

Benefits:

Easy to manage
Easy to reuse
Better organization
Suitable for production environments

# Standard Role Directory Structure

roles/
└── nginx/
    ├── tasks/
    │   └── main.yml
    ├── handlers/
    │   └── main.yml
    ├── templates/
    │   └── nginx.conf.j2
    ├── files/
    │   └── index.html
    ├── vars/
    │   └── main.yml
    ├── defaults/
    │   └── main.yml
    ├── meta/
    │   └── main.yml
    ├── tests/
    └── README.md

# How to create role directory structure?

ansible-galaxy init role_name


# Folder Explanation
tasks/

Contains all tasks executed by the role.

Example:

# roles/nginx/tasks/main.yml
```yaml
---
- name: Install Nginx
  apt:
    name: nginx
    state: present

- name: Start Nginx
  service:
    name: nginx
    state: started
    enabled: yes
```

# handlers/

Contains handlers that run only when notified.

Example:

# roles/nginx/handlers/main.yml
---
- name: Restart Nginx
  service:
    name: nginx
    state: restarted

# files/

Contains static files copied to remote servers.

Example:

roles/nginx/files/
├── index.html
├── logo.png
└── app.conf

Use with:
```yaml
copy:
  src: index.html
  dest: /var/www/html/index.html
```

# vars/

Stores variables with high priority.

Example:

# roles/nginx/vars/main.yml
```yaml
---
package_name: nginx
service_name: nginx
```

# How to use roles?
```yaml
---
- name: configure webserver
  hosts: prod
  become: yes

  roles:
    - install-apache
    - copy-index
    - start-service

```
===================================================================

# 6. Ansible Vault

Used to encrypt:
- Passwords
- API Keys
- SSH Keys
- Database Credentials
- Secret Variables

Commands:

```bash
ansible-vault create secrets.yml
ansible-vault view secrets.yml
ansible-vault edit secrets.yml
ansible-vault encrypt secrets.yml
ansible-vault decrypt secrets.yml
ansible-vault rekey secrets.yml    # To change vault password
ansible-playbook site.yml --ask-vault-pass
```

## Best Practices

- Never store passwords in plain text.
- Use Vault for sensitive data.
- Keep vault passwords secure.
- Do not commit vault passwords to Git.



# Example: Docker Login

secrets.yml
```yaml
docker_user: myuser
docker_password: MyPassword@123
```

Playbook
```yaml
---
- hosts: all

  vars_files:
    - secrets.yml

  tasks:

    - name: Docker Login
      shell: |
        docker login -u {{ docker_user }} -p {{ docker_password }}
```

# Run the Playbook

If the vault file is encrypted:

  ansible-playbook playbook.yml --ask-vault-pass

Or use a password file:

  ansible-playbook playbook.yml --vault-password-file vault-password.txt



