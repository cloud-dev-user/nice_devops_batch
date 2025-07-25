---

# Hands-On Lab: Ansible from Installation to Roles

---

## Goal

Set up Ansible environment, write a playbook to configure web servers (install Apache, deploy a simple HTML page), use variables, loops, handlers, and finally create and use a role from Ansible Galaxy.

---

# Step 1: Install & Configure Ansible

### On Ubuntu/Debian:

```bash
sudo apt update
sudo apt install -y ansible
ansible --version
```

### On CentOS/RHEL:

```bash
sudo yum install -y epel-release
sudo yum install -y ansible
ansible --version
```

### Configure SSH Access

* Ensure passwordless SSH to managed nodes (or localhost for testing)

```bash
ssh-keygen -t rsa
```
### ✅ Step 1: On the Control Node – Get Your Public Key

```bash
cat ~/.ssh/id_rsa.pub
````

Copy the full output (starts with `ssh-rsa`).

---

### ✅ Step 2: On the EC2 Instance – Add Public Key Manually

1. Create `.ssh` directory if it doesn't exist:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

2. Edit or create the `authorized_keys` file:

```bash
nano ~/.ssh/authorized_keys
# OR
vi ~/.ssh/authorized_keys
```

3. Paste the copied public key from **Step 1** as a new line in the file.

4. Save and exit the editor.

5. Set proper permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

### ✅ Step 3: From the Control Node – Test SSH Access

```bash
ssh ec2-user@<ec2-public-ip>
```

> Replace `ec2-user` with the appropriate default username for your AMI:
---

## Check file and folder permissions:

  ```bash
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/authorized_keys
  ```


## ✅ Result

You should now be able to SSH from the control node to the EC2 instance **without using a password or `.ppk` file**, using your SSH key instead.

```
ssh ec2-user@<ec2-public-ip>
```

### Create `hosts` inventory file and provide the public IP address of EC2 instance created:

```ini
[webservers]
192.168.1.100
192.168.1.101
```

Test connectivity:

```bash
ansible -i hosts webservers -m ping
```

---

# Step 2: Writing Your First Playbook

Create `site.yml`:

```yaml
---
- name: Configure Apache Web Servers
  hosts: webservers
  become: yes

  vars:
    apache_port: 80
    web_content: "<h1>Welcome to Ansible Managed Server</h1>"

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
      when: ansible_os_family == 'Debian'

    - name: Install httpd (RedHat based)
      yum:
        name: httpd
        state: present
      when: ansible_os_family == 'RedHat'

    - name: Ensure Apache is running and enabled
      service:
        name: "{{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }}"
        state: started
        enabled: true

    - name: Deploy custom index.html
      copy:
        content: "{{ web_content }}"
        dest: /var/www/html/index.html
      notify: Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: "{{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }}"
        state: restarted
```

Run playbook:

```bash
ansible-playbook -i hosts site.yml
```

---

# Step 3: Variables, Loops, and Conditions

Modify playbook snippet to create multiple HTML files:

```yaml
  vars:
    apache_port: 80
    pages:
      - home
      - about
      - contact

  tasks:
    - name: Create multiple HTML pages
      copy:
        content: "<h1>This is {{ item }} page</h1>"
        dest: "/var/www/html/{{ item }}.html"
      loop: "{{ pages }}"
```

---

# Step 4: Using Ansible Modules

Common useful modules demonstrated above:

* `apt` / `yum` for package management
* `service` for service control
* `copy` for file deployment

Other modules to explore: `file`, `template`, `command`, `user`, `git`, `shell`

---

# Step 5: Creating and Using Roles with Ansible Galaxy

### Generate a Role Skeleton:

```bash
ansible-galaxy init apache_role
```

This creates folder structure:

```
apache_role/
├── tasks/
│   └── main.yml
├── handlers/
│   └── main.yml
├── vars/
│   └── main.yml
├── defaults/
│   └── main.yml
├── templates/
├── files/
├── meta/
└── README.md
```

### Populate `tasks/main.yml`:

```yaml
---
- name: Install Apache package
  apt:
    name: apache2
    state: present
  when: ansible_os_family == 'Debian'

- name: Install Apache package (RedHat)
  yum:
    name: httpd
    state: present
  when: ansible_os_family == 'RedHat'

- name: Ensure Apache service is started
  service:
    name: "{{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }}"
    state: started
    enabled: true
```

### Populate `handlers/main.yml`:

```yaml
---
- name: Restart Apache
  service:
    name: "{{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }}"
    state: restarted
```

### Use the Role in your Playbook

Modify `site.yml`:

```yaml
- name: Configure Apache Web Servers with Role
  hosts: webservers
  become: yes
  roles:
    - apache_role
```

Run:

```bash
ansible-playbook -i hosts site.yml
```

---

# Bonus: Install a Role from Ansible Galaxy

```bash
ansible-galaxy install geerlingguy.apache
```

Use in playbook:

```yaml
roles:
  - geerlingguy.apache
```

---

