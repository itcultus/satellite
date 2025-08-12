# satellite_sync_repos

This role synchronizes individual repositories, or all the repositories of a product. The role is created mainly for 
a fresh Satellite installation because a Product, or an individual repository, should have a synchronization plan defined
which will ensure the synchronization of the repositories.  
In the future, it might be possible to force the synchronization of a product or a repository but at the time of this writing
we only support the initial synchronization. 

# Role Variables

This role supports the [Common Role Variables](https://github.com/theforeman/foreman-ansible-modules/blob/develop/README.md#common-role-variables).

The main data structure for this role is the list of `satellite_host_collections`. Each `collection` requires the following fields:

- `name`: The name of the location.
- `organization`: Organization that the entity is in
- `product`: Product to which the repository lives in

For all other fields, see the [`theforeman.foreman.repository_sync`](https://theforeman.github.io/foreman-ansible-modules/latest/plugins/repository_sync_module.html#ansible-collections-theforeman-foreman-repository-sync-module) module.

## Example Playbook

Create the 'Lab Servers' host collection in the ITCultus organization.

```yaml
- hosts: localhost
  roles:
    - role: satellite_sync_repos
      vars:
        satellite_server_url: https://satellite.example.com
        satellite_username: "admin"
        satellite_password: "changeme"
        satellite_products:
          - name: Red Hat Enterprise Linux for x86_64
            repository_sets:
              - name: Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)
                releasever: 8
              - name: Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
                releasever: 8
```

## License

GPLv3

## Author Information

Peter Tselios  
ITCultus LTD
