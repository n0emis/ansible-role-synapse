# Ansible role for Matrix Synapse

Ansible role for installation and configuration of matrix synapse

## Requirements

* Database (recommend: PostgreSQL) 
* (optional) An e-mail server

## Role Variables

| Variable Name | Function | Default value | Comment |
| ------------- | -------- | ------------- | ------- |
| `synapse_user` | User created for running the systemd service | `matrix-synapse` |
| `synapse_group` | Group for the user created for the systemd service | `{{ synapse_user }}` |
| `synapse_pip_version` | Version of matrix-synapse that is going to be installed | `1.15.1` | 
| `synapse_systemd_service_name` | The name of the systemd service file | `matrix-synapse` |
| `synapse_config_path` | Path of synapse configuration directory | `/etc/matrix-synapse` |
| `synapse_venv_path` | Path of python virtualenv directory | `/opt/venvs/matrix-synapse` |
| `synapse_data_directory` | Path of synapse data directory | `/var/lib/matrix-synapse` |
| `synapse_media_store_path` | Path of synapse media_store directory | `{{ synapse_data_directory }}/media-store` |

### Secrets
When first running this role, these secrets get automatically generated. You can find them at the following locations:
 
* `synapse_registration_shared_secret`: /etc/matrix-synapse/homeserver.yaml, Variable: registration_shared_secret
* `synapse_macaroon_secret_key`: /etc/matrix-synapse/homeserver.yaml, Variable: macaroon_secret_key
* `synapse_form_secret`: /etc/matrix-synapse/homeserver.yaml, Variable: form_secret
* `synapse_signing_key`: The whole content of the file /etc/matrix-synapse/{{ synapse_server_fqdn_matrix }}.signing.key

### homeserver.yaml

#### Other
You can define any other options for homeserver.yaml in the ansible variable `synapse_homeserver_config_extra_options`

## Dependencies
This role does not have any dependencies

## Example Playbook
This example playbook depends on [geerlingguy.postgresql](https://github.com/geerlingguy/ansible-role-postgresql)
```yaml
- hosts: servers
  roles:
    - role: ../geerlingguy.postgresql
      vars:
        postgresql_users:
          - name: "{{ synapse_database_user }}"
            password: "{{ synapse_database_password }}"
        postgresql_databases:
          - name: "{{ synapse_database_database }}"
            owner: "{{ synapse_database_user }}"
            lc_collate: 'C'
            lc_ctype: 'C'
    - role: .
  vars:
    synapse_database_host: "localhost"
    synapse_database_database: "synapse"
    synapse_database_user: "synapse"
    synapse_database_password: "synapse-db-passwd"
    synapse_allow_public_rooms_over_federation: true
    synapse_enable_group_creation: true
    synapse_enable_registration: false
    synapse_domain: "example.com"
    synapse_server_fqdn_matrix: "matrix.{{ synapse_domain }}"

    # Leave these out on first run:
    #synapse_registration_shared_secret: ""
    #synapse_macaroon_secret_key: ""
    #synapse_form_secret: ""
    #synapse_signing_key: ""
```
## License

MIT
