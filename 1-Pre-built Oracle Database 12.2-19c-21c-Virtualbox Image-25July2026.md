Using the pre-built Oracle 19c VirtualBox image is the fastest way to start learning Oracle because Oracle Linux, Oracle Database, SQL Developer, and sample databases are already configured. 
The image we're referring to includes Oracle 19c (19.15) along with Oracle Linux and SQL Developer. ([DB Admin Blog][1])

Source: 
https://dbsysupgrade.com/pre-built-oracle-database-12-2-19c-21c-virtualbox-image/

## Step 1: Verify your PC

Minimum:

* Windows 10/11 (64-bit)
* 16 GB RAM (8 GB works but is slow)
* Intel VT-x / AMD-V enabled in BIOS
* 120 GB free SSD space
* Oracle VirtualBox 7.x (works better than older versions)

---

## Step 2: Download the VM

Download all multipart ZIP files from:

[Pre-built Oracle Database VirtualBox Image](https://dbsysupgrade.com/pre-built-oracle-database-12-2-19c-21c-virtualbox-image/?utm_source=chatgpt.com)

You'll get files similar to:

```
Oracle_Virtualbox_image_21c_Jun_2022.zip.001
Oracle_Virtualbox_image_21c_Jun_2022.zip.002
Oracle_Virtualbox_image_21c_Jun_2022.zip.003
```

Place them in the same folder.

---

## Step 3: Extract

Use **7-Zip**.

Right-click only on:

```
.zip.001
```

Select

```
Extract Here
```

7-Zip automatically combines the remaining parts.

Eventually you'll obtain

```
Oracle_VM.ova
```

(or a similarly named OVA file). ([Oracle][2])

---

## Step 4: Install VirtualBox

Download and install:

* VirtualBox
* VirtualBox Extension Pack

---

## Step 5: Import the VM

Open VirtualBox

```
File
    →
Import Appliance
```

Select

```
Oracle_VM.ova
```

Click

```
Next
Import
```

This may take 10–30 minutes.

---

## Step 6: Increase VM Resources

Before starting the VM:

Settings → System

Recommended:

| Setting      | Value   |
| ------------ | ------- |
| RAM          | 8 GB    |
| CPU          | 4 Cores |
| Video Memory | 128 MB  |

If your laptop has 32 GB RAM:

* Allocate 12 GB RAM
* Allocate 4–6 CPUs

---

## Step 7: Start the VM

Click **Start**.

Oracle Linux boots.

Login:

```
Username:
oracle

Password:
oracle
```

Root account:

```
Username:
root

Password:
oracle
```

([DB Admin Blog][1])

---

# Step 8: Open Terminal

Verify the databases.

```bash
ps -ef | grep pmon
```

You should see something similar to

```
ora_pmon_CDB2
```

---

## Step 9: Set Oracle Environment

```bash
. oraenv
```

Enter

```
CDB2
```

Verify:

```bash
echo $ORACLE_HOME
```

---

## Step 10: Connect to Oracle

```bash
sqlplus / as sysdba
```

You should see

```
SQL>
```

---

## Step 11: Verify Version

```sql
SELECT * FROM v$version;
```

Expected:

```
Oracle Database 19c Enterprise Edition
19.15
```

---

## Step 12: List Databases

```sql
SELECT name FROM v$database;
```

---

## Step 13: List PDBs

```sql
SHOW PDBS;
```

Example:

```
CDB2

PDB$SEED

PDB1
```

---

## Step 14: Connect to PDB

```sql
ALTER SESSION SET CONTAINER=PDB1;
```

Verify:

```sql
SHOW CON_NAME;
```

---

## Step 15: Connect with SQL Developer

SQL Developer is already installed inside the VM. ([DB Admin Blog][1])

Connection details:

```
Hostname:
localhost

Port:
1521

Service:
PDB1

Username:
system

Password:
oracle
```

---

# Step 16: Create a Practice User

```sql
CREATE USER robin IDENTIFIED BY Welcome123;

GRANT CONNECT, RESOURCE TO robin;

ALTER USER robin QUOTA UNLIMITED ON USERS;
```

---

# Step 17: Save a Snapshot

Once everything is working:

```
VirtualBox

Machine

Take Snapshot

Name:
Fresh Oracle19c
```

If you break the lab later, you can restore it in seconds.

---

# Recommended Learning Path

After the VM is running, follow this progression:

1. Oracle Linux basics
2. SQL*Plus
3. Oracle Architecture (Instance vs Database)
4. Multitenant (CDB/PDB)
5. Tablespaces
6. Users & Roles
7. Datafiles
8. Redo Logs
9. Control Files
10. Undo Management
11. RMAN Backup & Recovery
12. ARCHIVELOG mode
13. Oracle Net (Listener & TNS)
14. Data Pump (expdp/impdp)
15. Performance Views (V$ views)
16. Automatic Workload Repository (AWR)
17. Automatic Database Diagnostic Monitor (ADDM)
18. Oracle Patching (Release Updates)
19. Data Guard
20. RAC (advanced)

Focus first on Oracle architecture and administration concepts rather than SQL syntax. 
The biggest conceptual shifts are the Oracle memory architecture (SGA/PGA), tablespaces, redo/undo mechanisms, RMAN, and the multitenant CDB/PDB model.

[1]: https://dbsysupgrade.com/pre-built-oracle-database-12-2-19c-21c-virtualbox-image/?utm_source=chatgpt.com "Pre-built Oracle Database 12.2/19c/21c Virtualbox Image"
[2]: https://www.oracle.com/downloads/community/vts-hands-on-labs-downloads.html?utm_source=chatgpt.com "Oracle Database 19c (19.9.0) Upgrade and Migration hands-on Lab"
