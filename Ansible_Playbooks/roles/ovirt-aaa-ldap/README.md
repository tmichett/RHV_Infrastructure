Role Name
=========

This role deploy oVirt AAA LDAP configuration.

Requirements
------------

Just Ansible.

Role Variables
--------------

* aaa_profile_type: Define profile type, one of: 

   - 389ds
   - iplanet
   - rfc2307-389ds
   - rfc2307-generic
   - rfc2307-openldap
   - rfc2307-rhds
   - ad
   - ipa
   - openldap
   - rfc2307-edir
   - rhds

* aaa_user: User which should be used to search users for auhtorization.
* aaa_password: Password of the search user.
* aaa_profile_name: Name of the profile (visible in login page).
* aaa_ldap: List of ldap servers. If more servers is specified failover policy will be used.
* aaa_base_dn: Custom base DN in case user want to set special.
* aaa_sso_keytab: Path to keytab which store principal to use SSO. This parameter is required in case SSO should be deployed.

For example to obtain HTTP keytab for oVirt engine in IPA use following command
```bash
$ ipa-getkeytab --server=ipa.example.com --principal=HTTP/ovirt.example.com --keytab=ovirt.keytab
```

Dependencies
------------

No.

Example Playbook
----------------

This is example of how to deploy IPA with single server:

```yaml
    - name: Deploy oVirt AAA IPA
      hosts: localhost
      gather_facts: no
      vars:
        aaa_profile_type: ipa
        aaa_user: uid=search,cn=users,cn=accounts,dc=example,dc=com
        aaa_password: password
        aaa_profile_name: ipa
        aaa_ldap:
          - ldap.example.com
    
      roles:
        - machacekondra.ovirt-aaa-ldap
```

This is example of how to deploy IPA with failover servers:

```yaml
    - name: Deploy oVirt AAA IPA failover
      hosts: localhost
      gather_facts: no
      vars:
        aaa_profile_type: ipa
        aaa_user: uid=search,cn=users,cn=accounts,dc=example,dc=com
        aaa_password: password
        aaa_profile_name: ipa
        aaa_ldap:
          - ldap1.example.com
          - ldap2.example.com
    
      roles:
        - machacekondra.ovirt-aaa-ldap
```

This is example of how to deploy IPA with SSO:

```yaml
    - name: Deploy oVirt AAA IPA SSO
      hosts: localhost
      gather_facts: no
      vars:
        aaa_profile_type: ipa
        aaa_user: uid=search,cn=users,cn=accounts,dc=example,dc=com
        aaa_password: password
        aaa_profile_name: ipa
        aaa_ldap:
          - ldap1.example.com
          - ldap2.example.com
        aaa_sso_keytab: /path/to/ovirt.keytab
    
      roles:
        - machacekondra.ovirt-aaa-ldap
```

License
-------

BSD

Author Information
------------------

Ondra Machacek (@machacekondra)
