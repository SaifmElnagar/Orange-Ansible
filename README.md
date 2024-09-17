
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

This inventory setup provides a clear structure for groups and servers while maintaining organized and readable configurations.

