
---

# Lab-1

## 1. Establishing Local Connection Instead of SSH
In Ansible, to establish a local connection rather than using SSH, you can use the following inventory parameters:
- **ansible_connection=local**: This sets up a connection to the local system rather than using SSH.

### Example:
```ini
localhost ansible_connection=local
```

## 2. Connecting to a Windows Server
To connect to a Windows server using Ansible, you must set the `ansible_connection` parameter to **winrm**.

### Example:
```ini
[windows]
windows_host ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!
```
# Exercise 

1. **Adding a New Server and Aliases**

   First, add `server4.company.com` to your inventory and create an alias `db1` for it:

   ```bash
   # Sample Inventory File

   # Web Servers
   web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
   web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
   web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

   # Database Servers
   db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!
   ```

2. **Adding a Group for Database Servers**

   Update your inventory to include the `db_servers` group:

   ```bash
   # Sample Inventory File

   # Web Servers
   web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
   web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
   web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

   # Database Servers
   db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!

   [web_servers]
   web1
   web2
   web3

   [db_servers]
   db1
   ```

3. **Creating a Group of Groups**

   Add a new group called `all_servers` which includes `web_servers` and `db_servers`:

   ```bash
   # Sample Inventory File

   # Web Servers
   web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
   web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
   web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

   # Database Servers
   db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!

   [web_servers]
   web1
   web2
   web3

   [db_servers]
   db1

   [all_servers:children]
   web_servers
   db_servers
   ```

4. **Updating Inventory Based on the New Table**

   - **Group Definitions:**
     - `db_nodes`: `sql_db1`, `sql_db2`
     - `web_nodes`: `web_node1`, `web_node2`, `web_node3`
     - `boston_nodes`: `sql_db1`, `web_node1`
     - `dallas_nodes`: `sql_db2`, `web_node2`, `web_node3`
     - `us_nodes`: `boston_nodes`, `dallas_nodes`

   - **Updated Inventory File:**

   ```bash
   # Sample Inventory File

   # Database Servers
   sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
   sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

   # Web Servers
   web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
   web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
   web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass

   [db_nodes]
   sql_db1
   sql_db2

   [web_nodes]
   web_node1
   web_node2
   web_node3

   [boston_nodes]
   sql_db1
   web_node1

   [dallas_nodes]
   sql_db2
   web_node2
   web_node3

   [us_nodes:children]
   boston_nodes
   dallas_nodes
   ```

# Lab-2

Here's a comprehensive answer to each of your questions:

### 1. Ansible Package Installation

**Question:** On which hosts will Ansible install the `httpd` package using the given playbook?

**Answer:** The `httpd` package will be installed on the hosts listed under the `[webserver]` group, which are `web1` and `web2`.

### 2. Running Ansible Playbook

**Question:** What commands can you use to run an Ansible playbook named `install.yaml`?

**Answer:**
To run the `install.yaml` playbook, you can use the following command:

```bash
ansible-playbook install.yaml
```

### 3. Running Playbook in Check Mode

**Question:** What command would you use to run the `update_service.yml` playbook in check mode?

**Answer:**

```bash
ansible-playbook update_service.yml --check
```

### 4. Check Mode Result

**Question:** After running the `update_service.yml` playbook in check mode against the same server, which tasks would result in a changed status?

**Answer:** 

B. Update the service


### 5. Running Playbook in Both Check and Diff Modes

**Question:** What command would you use to run the `configure_database.yml` playbook in both check mode and diff mode?

**Answer:**

```bash
ansible-playbook configure_database.yml --check --diff
```

### 6. Syntax Error and Linting Commands

**Question:** What is the command to check syntax errors in the `configure_database.yml` playbook? What command would you use to run `ansible-lint` on the `database_setup.yml` playbook?

**Answer:**

To check syntax errors:

```bash
ansible-playbook configure_database.yml --syntax-check
```

To run `ansible-lint`:

```bash
ansible-lint database_setup.yml
```

### 7. `ansible-lint` Issues

**Question:** After running `ansible-lint` on the `database_setup.yml` playbook, which issues might you expect to see? (Select two choices)

**Answer:**

B. Deprecated 'apt' module  
C. Missing 'name' attribute for a task

### 8. Actions Based on `ansible-lint` Feedback

**Question:** Which of the following is NOT a recommended action based on feedback about indentation, deprecated modules, and missing name attributes?

**Answer:**

C. Ignoring the feedback and proceeding with playbook execution.

### 9. No Output from `ansible-lint`

**Question:** If `ansible-lint` provides no output after checking a playbook, what does it indicate?

**Answer:**

C. The playbook adheres to best practices and has no style-related issues.

### 10. Fix Issues in Playbook

**Question:** Solve issues in the given playbook.

**Answer:** The provided playbook is missing a proper YAML structure, and it needs a colon after the `become` keyword. Here's the corrected version:

```yaml
---
- hosts: localhost
  become: yes
  tasks:
    - name: Execute a date command on localhost
      command: date
```

### 11. Ansible Playbook for Nginx

**Question:** Create an Ansible playbook to install Nginx on a group of servers, ensuring the service is started and enabled at boot.

**Answer:** Here is a sample Ansible playbook for installing Nginx:

```yaml
# README.md

## Ansible Playbook for Installing Nginx

This playbook installs Nginx on a group of servers, starts the service, and ensures it is enabled at boot.

```yaml
---
- name: Install and configure Nginx
  hosts: group-name
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start Nginx service
      service:
        name: nginx
        state: started

    - name: Ensure Nginx is enabled at boot
      service:
        name: nginx
        enabled: yes
```
# Lab-3

# Ansible Playbooks

- **Write the playbook to add a task to start httpd service on all nodes defined in the inventory file.**
  - Hint: Use the service module.

![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p1/Pasted%20image.png)

- **Create a playbook that uses the file module to create a directory structure on multiple hosts.** 
  - Ensure the directories have specific permissions, ownership, and that certain files are absent in these directories.
 
![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p2/Pasted%20image.png)

- **Use the lineinfile module to ensure that a specific line exists in a configuration file on all hosts.** 
  - For example, add a line to `/etc/hosts` only if it’s not already there.
 
![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p3/Pasted%20image.png)

- **Write a playbook that uses the docker_container module to start a container with a specific image (e.g., nginx).**
  - Ensure the container is always running.
 
![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p4/Pasted%20image.png)

- **Create a role named nginx_setup that installs and configures Nginx on any system.**
  - The role should ensure that the `nginx.conf` file is properly set up, the service is running, and the firewall allows traffic on port 80.
 
![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p1/Pasted%20image.png)

- **Create a role that configures a basic LAMP stack (Linux, Apache, MySQL, PHP).**
  - Ensure each service is installed and properly configured. For example, MySQL should have a database and user created, and Apache should serve a test PHP file.
 
![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p6/Pasted%20image.png)

- **Given the following sample Ansible playbook, write a handler that will correctly restart the web server when the configuration file is changed.**
  ```yaml
  - name: Update web server configuration
    hosts: webservers
    tasks:
      - name: Copy web server configuration file
        copy:
          src: /path/to/webserver.conf
          dest: /etc/webserver/webserver.conf
        notify: Restart web server

![Output](https://github.com/SaifmElnagar/Orange-Ansible/blob/main/p7/Pasted%20image.png)




