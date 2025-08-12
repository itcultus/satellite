# satellite_publish_cv

This role publishes and promotes Content and Composite Content Views in a Red Hat Satellite server.  
Policies for Satellite (composite) Content views varies among various organizations. With this role we try to 
automate one of the most common publication and promotion scenarios: Date-based publication and promotion of Content Views.  
The role is based on the following assumptions: 

- Content Views are publised into Library only
- Filters are already updated on each Content View/Composite Content View
- We promote to an existing version to the next environment. 
- We have a naming convention in place for the CVs/CCVs and we only publish and promote specific content views (not all of the
CVs/CCVs we have in Satellite)

In this scenario a new version of Content View is published to the Library every `X` number of days, for example 7 days.   
After some grace period, typically the same as `X`, the Composite Content Views are promoted to the next environment. 
Initial publishing of CVs/CCVs happens to all the environments. Thus, a CCV that is not published yet, will be published to all the
environments.  

Support the Composite Content View with the name `ccv_test`.
`ccv_test` v1.0 published to Library, Dev, Test and Production environments. Our organization published a new version of the CCV 
each 7 days.  
Thus, on day 7, the role will search for the above CCV and it will: 

* Publish version 2.0 to Library
* No action for the **Production** environment (Test and Production have the same version, v1.0)
* No action for the **Test** environment (Test and Production have the same version, v1.0)
* Promote version **2.0** from Library to the Dev environment

7 days later, we will have the following actions: 

* Publish version 3.0 to Library
* No action for the **Production** environment (Test and Production have the same version, v1.0)
* Promote to the `Test` environment the current version of the `Dev` environment (v2.0)
* Promote version **3.0** from Library to the Dev environment

Finally, 7 days later we will have the following actions: 

* Publish version 4.0 to Library
* Promote to the **Production** environment the current version of the Test environment, v2.0)
* Promote to the **Test** environment the current version of the `Dev` environment (v3.0)
* Promote version **4.0** from Library to the Dev environment

The role doesn't take any action on specific versions, it assumes that all the CV/CCV actions are performed in a timely manner.  

# Role Variables

This role supports the [Common Role Variables](https://github.com/theforeman/foreman-ansible-modules/blob/develop/README.md#common-role-variables).

The role uses the following variables:

| Variable name | Description | Type | Mandatory | Default Value |
|:----|:----|:---:|:---:|:---:|
| satellite_lifecycle_environments | The defined Lifecycle environments | List of dictionaries | Y | N/A |
| satellite_cv_search_pattern | The search pattern we will use | String | Y | N/A |
| spcv_min_publication_days | The minimum number of days between publication | Integer | Y |  7 |
| spcv_force_pub | Force publication of CVs/CCVs even if less than `spcv_min_publication_days` days passed since last publication | Boolean | N |  false |

## Example Playbook

Create the 'Lab Servers' host collection in the ITCultus organization.

```yaml
- hosts: localhost
  roles:
    - role: satellite_publish_cv
      vars:
        satellite_server_url: https://satellite.example.com
        satellite_username: "admin"
        satellite_password: "changeme"
        
```

## License

GPLv3

## Author Information

Peter Tselios  
ITCultus LTD

