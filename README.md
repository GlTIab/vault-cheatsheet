# Hashicoprt Vault Cheatsheet

## Installation and Setup:

https://developer.hashicorp.com/vault/install#linux

## Start Vault Server:

```vault server -dev```

## Initialize Vault:

```vault operator init```

## Unseal Vault:

```vault operator unseal <key>```

## Authenticate to Vault:

``` export VAULT_TOKEN=<token>```

# Basic Operations:

## Write Data to Vault:

```vault kv put <secret/path> <key>=<value>```

## Read Data from Vault:

```vault kv get <secret/path>```

## Delete Data from Vault:

```vault kv delete <secret/path>```

# Authentication Methods:

## Token Authentication:

```vault login <token>```

## LDAP Authentication:

```vault auth enable ldap```

```vault write auth/ldap/config <config>```

## GitHub Authentication:

```vault auth enable github```

```vault write auth/github/config <config>```

# Secrets Engines:

## Enable Key/Value Secrets Engine:

```vault secrets enable -path=<path> kv```

## Database Secrets Engine (MySQL):

```vault secrets enable database```

```
vault write database/config/<name> plugin_name=mysql-database-plugin \
  connection_url="{{username}}:{{password}}@tcp(localhost:3306)/" \
  allowed_roles="readonly"
```

# Policies and Access Control:

## Autogenerate Policy:

```vault read -output-policy kv/<secret>```

## Create Policy:

```vault policy write <name> <policy.hcl>```

## Associate Policy with Token:

```vault token create -policy=<policy>```

## Token Roles:

```vault write auth/token/roles/my-role allowed_policies=my-policy```

# Advanced Features:

## AWS
```vault write aws/config/root access_key=<access_key> secret_key=<secret_key>```

```vault read aws/creds/my-role```

### Database:
```vault write database/roles/my-role db_name=<name> creation_statements="CREATE ROLE ..."```

```vault read database/creds/my-role```

## Transit Secrets Engine (Encryption as a Service):
```vault secrets enable transit```

```vault write -f transit/keys/<name>```

```vault write transit/encrypt/<name> plaintext=<data>```

## Token Renewal:

```vault token renew <token>```

# High Availability (HA):
## Enable HA Mode:

```vault operator raft join <address>```

## Check HA Status:

```vault status -format=json | jq .ha```

# Maintenance:

## Seal/Unseal Vault:
```vault operator seal```

```vault operator unseal```

# Backup and Restore:

## Backup:
```vault operator raft snapshot save <file_path>```

## Restore:

```vault operator raft snapshot restore <file_path>```

## Audit Logs:

```vault audit enable file file_path=<path>```

# Monitoring and Metrics:

## Enable Metrics:
```vault server -dev -dev-listen-address=<address>:<port>```

## Prometheus Metrics:

```vault status -format=json | jq .metrics```

# Feel free to contribute!




