
# Provisioner Options

key | default value | Notes
----|---------------|--------
ansible_version | latest | Desired version, only affects `apt-get` installs
ansible_sudo | true | Determines whether `ansible-playbook` is executed as root or as the current authenticated user
sudo_command | sudo -E | `sudo` command; change to `sudo -E -H` to be consistent with Ansible
ansible_platform | Naively tries to determine | OS platform of server
require_ansible_repo | true | Set if installing Ansible from a `yum` or `apt` repo
ansible_apt_repo | ppa:ansible/ansible | `apt` repo; see `https://launchpad.net` `/~ansible/+archive/ubuntu/ansible` or `rquillo/ansible`
ansible_yum_repo | nil | `yum` repo for EL platforms
ansible_binary_path | NULL | If specified this will override the location where `kitchen` tries to run `ansible-playbook` from, i.e. `ansible_binary_path: /usr/local/bin`
enable_yum_epel | false | Enable the `yum` EPEL repo
ansible_sles_repo | `http://download.opensuse.org/repositories` `/systemsmanagement/SLE_12` `/systemsmanagement.repo` | Zypper SuSE Ansible repo
python_sles_repo | `http://download.opensuse.org/repositories` `/devel:/languages:/python/SLE_12` `/devel:languages:python.repo` | Zypper SuSE python repo
require_ansible_omnibus | false | Set to `true` if using Omnibus Ansible `pip` install
ansible_omnibus_url | `https://raw.githubusercontent.com` `/neillturner/omnibus-ansible` `/master/ansible_install.sh` | Omnibus Ansible install location
ansible_omnibus_remote_path | /opt/ansible | Server installation location of an Omnibus Ansible install
http_proxy | nil | Use HTTP proxy when installing Ansible, packages and running Ansible
https_proxy | nil | Use HTTPS proxy when installing Ansible, packages and running Ansible
no_proxy | nil | List of URLs or IPs that should be excluded from proxying
roles_path | roles | Ansible repo roles directory
group_vars_path | group_vars | Ansible repo group_vars directory
host_vars_path | host_vars | Ansible repo hosts directory
library_plugins_path | library | Ansible repo library plugins directory
callback_plugins_path | callback_plugins | Ansible repo `callback_plugins` directory
filter_plugins_path | filter_plugins | Ansible repo `filter_plugins` directory
lookup_plugins_path | lookup_plugins | Ansible repo `lookup_plugins` directory
additional_copy_path | | Arbitrary array of files and directories to copy into test environment, relative to the current dir, e.g. vars or included playbooks
extra_vars | Hash.new | Hash to set the `extra_vars` passed to `ansible-playbook` command
playbook | default.yml | Playbook for `ansible-playbook` to run
modules_path | | Ansible repo manifests directory
ansible_verbose | false | Extra information logging
ansible_verbosity | 1 | Sets the verbosity flag appropriately, e.g.: `1 => '-v', 2 => '-vv', 3 => '-vvv' ...`. Valid values are: `1, 2, 3, 4` or `:info, :warn, :debug, :trace`
ansible_check | false | Sets the `--check` flag when running Ansible
ansible_diff | false | Sets the `--diff` flag when running Ansible
update_package_repos | true | Update OS repository metadata
ansiblefile_path | | Path to Ansiblefile
requirements_path | | Path to Ansible Galaxy requirements
ansible_vault_password_file | | Path to Ansible Vault password file
ansible_connection | local | use `ssh` if the host is not `localhost` (Linux) or `winrm` (Windows) or `none` if defined in inventory
hosts |  | Create Ansible hosts file for localhost with this server group
ansible_inventory |  | Static or dynamic inventory file or directory or none if defined in `ansible.cfg`
ansible_limit |  | Further limits the selected host/group patterns
ansible_extra_flags |  | Additional options to pass to ansible-playbook, e.g. `'--skip-tags=redis'`
ansible_playbook_command | | Override the Ansible playbook command
require_ruby_for_busser | false | Install Ruby to run Busser for tests
require_chef_for_busser | true | Install Chef to run Busser for tests. NOTE: kitchen 1.4 only requires Ruby to run Busser so this is not required.
chef_bootstrap_url | `https://www.getchef.com/chef/install.sh` | The Chef install
require_ansible_source | false | Install Ansible from source using method described [here](http://docs.ansible.com/intro_installation.html#running-from-source). Only works on Debian/Ubuntu at present
ansible_source_rev | | Branch or tag to install Ansible source
ansible_host_key_checking | true | Strict host key checking in ssh
private_key | | ssh private key file for ssh connection
idempotency_test | false | Enable to test Ansible playbook idempotency
ssh_known_hosts | | List of hosts that should be added to ~/.ssh/known_hosts
kerberos_conf_file | | Path of krb5.conf file using in Windows support
require_windows_support | false | Install [Windows support](http://docs.ansible.com/ansible/intro_windows.html)

## Configuring Provisioner Options

The provisioner can be configured globally or per suite, global settings act as defaults for all suites, you can then customise per suite, for example:

```yaml
---
driver:
  name: vagrant

provisioner:
  name: ansible_playbook
  roles_path: roles
  hosts: tomcat-servers
  require_ansible_repo: true
  ansible_verbose: true
  ansible_verbosity: 2
  ansible_diff: true

platforms:
- name: nocm_ubuntu-12.04
  driver_plugin: vagrant
  driver_config:
    box: nocm_ubuntu-12.04
    box_url: http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box

suites:
 - name: default
```

### Per-suite Structure

It can be beneficial to keep different Ansible layouts for different suites. Rather than having to specify the roles, modules, etc for each suite, you can create the following directory structure and they will automatically be found:

```
$kitchen_root/ansible/$suite_name/roles
$kitchen_root/ansible/$suite_name/modules
$kitchen_root/ansible/$suite_name/Ansiblefile
```
