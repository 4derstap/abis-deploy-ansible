# ABIS Installation Training

- [ABIS Installation Training](#abis-installation-training)
  - [ABIS Backend](#abis-backend)
  - [Deployment tools](#deployment-tools)
    - [Puppet/Puppet Bolt, Ansible)](#puppetpuppet-bolt-ansible)
    - [Managing configuration data](#managing-configuration-data)
  - [Deployment with Ansible](#deployment-with-ansible)
    - [Prerequisites](#prerequisites)
      - [Prepare db schemata](#prepare-db-schemata)
    - [Install collections](#install-collections)
    - [Prepare configuration data](#prepare-configuration-data)
    - [Apply the deployment configuration](#apply-the-deployment-configuration)
    - [Monitoring ABIS](#monitoring-abis)
  - [Preparing for failures](#preparing-for-failures)

## ABIS Backend

## Deployment tools

### Puppet/Puppet Bolt, Ansible)

### Managing configuration data

## Deployment with Ansible

### Prerequisites

- Install Ansible (ansible-core >= 2.10.0 is required).
- A database instance is prepared.

#### Prepare db schemata

Example:

```sql
ALTER SYSTEM SET DB_CREATE_FILE_DEST = '/u01/app/oracle/oradata/ORCLCDB';
create tablespace ABIS;
create tablespace ABIS_IDX;
create tablespace ABIS_LOB;

-- USER SQL --
CREATE USER "BIOMETRICSTORE" IDENTIFIED BY "biometricstore"
    DEFAULT TABLESPACE "ABIS"
    TEMPORARY TABLESPACE "TEMP";

-- QUOTAS --
ALTER USER "BIOMETRICSTORE" QUOTA UNLIMITED ON ABIS;
ALTER USER "BIOMETRICSTORE" QUOTA UNLIMITED ON ABIS_IDX;
ALTER USER "BIOMETRICSTORE" QUOTA UNLIMITED ON ABIS_LOB;

-- ROLES AND PERMISSIONS --
GRANT CREATE SESSION TO "BIOMETRICSTORE";
GRANT "CONNECT" to "BIOMETRICSTORE";
GRANT "RESOURCE" to "BIOMETRICSTORE";

-- USER SQL --
CREATE USER "WEBABIS" IDENTIFIED BY "webabis"
    DEFAULT TABLESPACE "ABIS"
    TEMPORARY TABLESPACE "TEMP";

-- QUOTAS --
ALTER USER "WEBABIS" QUOTA UNLIMITED ON ABIS;
ALTER USER "WEBABIS" QUOTA UNLIMITED ON ABIS_IDX;
ALTER USER "WEBABIS" QUOTA UNLIMITED ON ABIS_LOB;

-- ROLES AND PERMISSIONS --
GRANT CREATE SESSION TO "WEBABIS";
GRANT "CONNECT" to "WEBABIS";
GRANT "RESOURCE" to "WEBABIS";
```

### Install collections

```sh
ansible-galaxy install -r requirements.yml
```

### Prepare configuration data

The configuration data shall be prepared so that the services can work properly in the target environment.

==> Maintain data in `inventories`.

### Apply the deployment configuration

```sh
ansible-playbook -i inventories/abis_18.4.0 -i inventories/abis dermalog.abis_core.abis_install
```

### Monitoring ABIS

## Preparing for failures
