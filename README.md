# VSCode Ansible Role

Installs VSCode

## Requirements

This role requires Ansible 2.0 or higher.

## Role Variables

### Defaults

None


### Vars

These are not meant to be modified

    - name: vscode_package
      desc: Target package for installation
      value: code

    - name: vscode_requisite_packages
      desc: Package dependencies required for installation
      value:
        - apt-transport-https

    - name: vscode_gpg_url
      desc: URL for vscode gpg key
      value: 'https://packages.microsoft.com/keys/microsoft.asc'

    - name: vscode_repo_url
      desc: URL for vscode debian package
      value: 'http://packages.microsoft.com/repos/vscode'

    - name: vscode_repo
      desc: Apt sources repo definition
      value: 'deb [arch=amd64] {{ vscode_repo_url }} stable main'

## Dependencies

None

## Example Playbook

Install Visual Studio Code

    - hosts: all
      roles:
        - ansible-role-vscode

## Testing

Tests can be executed with local vagrantfile using the command. This mounts the role directory in the guest and executes the role via the playbook in `tests/` folder.

```shell
vagrant up
```
