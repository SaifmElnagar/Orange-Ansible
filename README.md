# Orange-Ansible


```markdown
# Ansible Inventory Setup Guide

## 1. Inventory Parameters for Local Connection

To establish a local connection in Ansible, use the following inventory parameters:
- Set `ansible_connection` to `local` to connect to the local machine without using SSH.

## 2. Windows Server Connection

To connect to a Windows server in Ansible, you must set the `ansible_connection` parameter to `winrm`.

## 3. Updating Inventory File

We have a sample inventory file with the following servers:

```bash
web1 
web2
```

Now, add another server called `server4.company.com` with an alias `db1`:

```bash
web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
db1  ansible_host=server4.company.com
```

## 4. Inventory File with Details from the Table

The web servers are Linux-based, and the DB server is a Windows machine. Below is the updated inventory file with connection details:

```bash
# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!
```

## 5. Adding Group for Web Servers and DB Servers

We have added a group called `web_servers` for the web servers. Similarly, add a group called `db_servers` for the database servers:

```bash
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

## 6. Creating a Parent Group for All Servers

Now, create a group of groups called `all_servers`, and add the previously created groups `web_servers` and `db_servers` under it:

```bash
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

## 7. Inventory File Based on New Table Data

Using the details given in the table, update the inventory file as follows:

```bash
# DB Nodes
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Web Nodes
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


```
