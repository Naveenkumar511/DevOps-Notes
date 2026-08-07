# 1. Ping All Servers

**Description:** Checks connectivity between the control node and
managed hosts.

``` yaml
---
- hosts: all
  gather_facts: no
  tasks:
    - ping:
```

# 2. Update Ubuntu Servers

**Description:** Updates apt cache and upgrades packages.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        update_cache: yes
    - apt:
        upgrade: yes
```

# 3. Install Nginx

**Description:** Installs Nginx.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        name: nginx
        state: present
```

# 4. Start & Enable Nginx

**Description:** Starts and enables Nginx.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - service:
        name: nginx
        state: started
        enabled: yes
```

# 5. Install Multiple Packages

**Description:** Installs common packages.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        name: [git,curl,unzip,nginx]
        state: present
```

# 6. Create User

**Description:** Creates a user.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - user:
        name: devops
        state: present
```

# 7. Copy File

**Description:** Copies a file.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - copy:
        src: index.html
        dest: /var/www/html/index.html
```

# 8. Create Directory

**Description:** Creates a directory.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - file:
        path: /opt/app
        state: directory
```

# 9. Create File

**Description:** Creates empty file.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - file:
        path: /tmp/sample.txt
        state: touch
```

# 10. Delete File

**Description:** Deletes a file.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - file:
        path: /tmp/sample.txt
        state: absent
```

# 11. Restart Service

**Description:** Restarts nginx.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - service:
        name: nginx
        state: restarted
```

# 12. Execute Shell

**Description:** Runs shell command.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - shell: uptime
      register: out
    - debug:
        var: out.stdout
```

# 13. Download File

**Description:** Downloads file.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - get_url:
        url: https://example.com/file.zip
        dest: /tmp/file.zip
```

# 14. Install Docker

**Description:** Installs Docker.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        name: docker.io
        state: present
```

# 15. Create Multiple Users

**Description:** Creates users with loop.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - user:
        name: '{{ item }}'
        state: present
      loop:
        - dev1
        - dev2
        - dev3
```

# 16. Install Apache

**Description:** Installs Apache.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        name: apache2
        state: present
```

# 17. Reboot Server

**Description:** Reboots server.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - reboot:
```

# 18. Display Host Information

**Description:** Shows hostname.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - command: hostname
      register: h
    - debug:
        var: h.stdout
```

# 19. Install Git

**Description:** Ensures Git installed.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        name: git
        state: present
```

# 20. Deploy Simple Website

**Description:** Installs nginx and copies website.

``` yaml
---
- hosts: all
  become: yes
  tasks:
    - apt:
        name: nginx
        state: present
    - copy:
        src: index.html
        dest: /var/www/html/index.html
```
