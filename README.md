## Ansible Role: secret

**IMPORTANT**
_This role is a fork of the [debops.secret](https://github.com/debops/debops/tree/master/ansible/roles/debops.secret)
Ansible role. It's not supported by the DebOps project nor anyone else. Don't
open any issues! For the original postfix role checkout the [DebOps](https://github.com/debops/debops)
repository._

`debops.secret` role enables you to have a separate directory on the Ansible
Controller (different than the playbook directory and inventory directory)
which can be used as a handy "workspace" for other roles.

Some usage examples of this role in [DebOps](https://debops.org/) include:

- password lookups, either from current role, or using known location of
  passwords from other roles, usually dependencies (for example
  `debops.mariadb` role can manage an user account in the database with
  random password and other role can lookup that password to include in
  a generated configuration file);

- secure file storage, for example for application keys generated on remote
  hosts (`debops.boxbackup` role retrieves client keys for backup
  purposes), for that reason secret directory should be protected by an
  external means, for example encrypted filesystem (currently there is no
  encryption provided by default);

- secure workspace (`debops.boxbackup` role, again, uses the secret directory
  to create and manage Root CA for backup servers – client and server
  certificates are automatically downloaded to Ansible Controller, signed and
  uploaded to destination hosts);

- simple centralized backup (specific roles like `debops.sshd`,
  `debops.pki` and `debops.monkeysphere` have separate task lists that
  are invoked by custom playbooks to allow backup and restoration of ssh host
  keys and SSL certificates. Generated .tar.gz files are kept on Ansible
  Controller in secret directory).


### Documentation

More information about `debops.secret` can be found in the
[official debops.secret documentation](https://docs.debops.org/en/latest/ansible/roles/ansible-secret/docs/).


### Example Playbook

```yaml
- name: Configure secret storage space
  hosts: all
  become: True

  environment: '{{ inventory__environment | d({})
                   | combine(inventory__group_environment | d({}))
                   | combine(inventory__host_environment  | d({})) }}'

  roles:

    - role: debops.secret
      tags: [ 'role::secret' ]
```


### Authors and license

- Maciej Delmanowski | [e-mail](mailto:drybjed@gmail.com) | [Twitter](https://twitter.com/drybjed) | [GitHub](https://github.com/drybjed)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)
