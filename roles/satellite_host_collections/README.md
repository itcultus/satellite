# satellite_host_collections

This role creates and manages Host Colelctions.

# Role Variables

This role supports the [Common Role Variables](https://github.com/theforeman/foreman-ansible-modules/blob/develop/README.md#common-role-variables).

The main data structure for this role is the list of `satellite_host_collections`. Each `collection` requires the following fields:

- `name`: The name of the location.
- `organization`: Organization that the entity is in

For all other fields, see the `host_collection` module.

## Example Playbook

Create the 'Lab Servers' host collection in the ITCultus organization.

```yaml
- hosts: localhost
  roles:
    - role: satellite_host_collections
      vars:
        satellite_server_url: https://satellite.example.com
        satellite_username: "admin"
        satellite_password: "changeme"
        satellite_host_collections:
          - name: Lab Servers
            organisation: ITCultus
            description: All ITCultus lab servers
```
