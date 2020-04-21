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
| `synapse_pip_version` | Version of matrix-synapse that is going to be installed _(required)_ | | 
| `synapse_systemd_service_name` | The name of the systemd service file | `matrix-synapse` |
| `synapse_config_path` | Path of synapse configuration directory | `/etc/matrix-synapse` |
| `synapse_venv_path` | Path of python virtualenv directory | `/opt/venvs/matrix-synapse` |
| `synapse_data_directory` | Path of synapse data directory | `/var/lib/matrix-synapse` |
| `synapse_media_store_path` | Path of synapse media_store directory | `{{ synapse_data_directory }}/media-store` |

### homeserver.yaml

#### Other
You can define any other options for homeserver.yaml in the ansible variable `synapse_homeserver_config_extra_options`

## Dependencies
This role does not have any dependencies

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yaml
- hosts: servers
  roles:
     - { role: n0emis.synapse }
```
## License

MIT