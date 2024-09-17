
---

# Ansible Inventory Configurations

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

Here’s the conversion of your inventory configuration into a README file format:

---

# Exerecises 

## 1. Adding Server to Inventory and Aliases
We have a sample inventory file with the servers `web1`, `web2`, and `web3`. We will add another server called `server4.company.com` with the alias `db1`.

### Initial Inventory:
```bash
web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
```

### Updated Inventory:
```bash
web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
db1  ansible_host=server4.company.com
```

## 2. Inventory with SSH and WinRM Connections
The web servers (`web1`, `web2`, and `web3`) are Linux-based and use SSH. The database server (`db1`) is a Windows machine, and we use WinRM for the connection.

### Updated Inventory with Connection and Authentication Details:
```bash
# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Server
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!
```

## 3. Grouping Servers
We have created a group called `web_servers` for the web servers. Now, we will add another group called `db_servers` for the database servers.

```bash
[web_servers]
web1
web2
web3

[db_servers]
db1
```

## 4. Group of Groups
We will create a new group called `all_servers` and add both `web_servers` and `db_servers` under it.

```bash
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

## 5. Representing Table Data in Ansible Inventory Format
We will represent the following server details in Ansible inventory format:

### Table:
| Server Alias |  Server Name  |  OS    |     User      | Password |
|--------------|---------------|--------|---------------|----------|
| sql_db1      | sql01.xyz.com | Linux  |     root      | Lin$Pass |
| sql_db2      | sql02.xyz.com | Linux  |     root      | Lin$Pass |
| web_node1    | web01.xyz.com | Win    | administrator | Win$Pass |
| web_node2    | web02.xyz.com | Win    | administrator | Win$Pass |
| web_node3    | web03.xyz.com | Win    | administrator | Win$Pass |

### Inventory Format:
```bash
# Database Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Web Servers
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
```

## 6. Grouping Servers
We will group the servers together as per the following details:

### Groupings:
|    Group         |  Members                          |
|------------------|-----------------------------------|
|    db_nodes      |  sql_db1, sql_db2                 |
|   web_nodes      |  web_node1, web_node2, web_node3  |
|    boston_nodes  |  sql_db1, web_node1               |
|    dallas_nodes  |  sql_db2, web_node2, web_node3    |
|   us_nodes       |  boston_nodes, dallas_nodes       |

### Grouped Inventory:
```bash
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

---

This README format reflects the complete inventory configuration and grouping based on your provided details.
