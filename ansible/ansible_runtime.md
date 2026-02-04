# Ansible Runtime Cheat Sheet

## Table of content

* [Runtime Options](#runtime-options)
  * [Check](#check)
  * [Ask for SSH and Sudo Password](#ask-for-ssh-and-sudo-password)
    * [Ask for SSH Password](#ask-for-ssh-password)
    * [Ask for Sudo Password](#ask-for-sudo-password)
    * [Combining to ask for both passwords](#combining-to-ask-for-both-passwords)
* [Extra Options/Variables](#extra-optionsvariables)
* [Defining Runtime Variables](#defining-runtime-variables)
  * [Runtime Variable Example](#runtime-variable-example)
* [List all task without running the playbook](#list-all-task-without-running-the-playbook)

## Runtime Options

### Check

The option `--check` will run the playbook in check mode, simulating the ansible run without doing any changes.

### Ask for SSH and Sudo Password

You can tell ansible to ask for the SSH password and the sudo password (become password). This comes in handy when you freshly installed a
server and have not setup any key authentication for SSH, yet.

#### Ask for SSH Password

To tell ansible to ask for the password you can simply append `-k` at the end or use the option `--ask-pass`:

```text
ansible-playbook -i inventory/ playbooks/my_playbook.yml -k
```

#### Ask for Sudo Password

To tell ansible to ask for the sudo password you can simply append `-K` or the option `--ask-become-pass`:

```text
ansible-playbook -i inventory/ playbooks/my_playbook.yml -K
```

#### Combining to ask for both passwords

Of course you can (and sometimes have to) combine both options to ask for both passwords like this:

```text
ansible-playbook -i inventory/ playbooks/my_playbook.yml --ask-pass --ask-become-pass
```

## Extra Options/Variables

When you have freshly installed a server it may become necessary that yoiu pass extra options to your playbook, because usually you do not
have any of your desired configuration values on the server, yet.

To use extra options use the following syntax:

```text
ansible-playbook -i inventory/ playbooks/my_playbook.yml -e "ansible_port=44" -e "ansible_user=iamansible"
```

## Defining Runtime Variables

Of course it is possible to define your own runtime variables. This will come in handy if you need to stop the playbook at a specific point
to then run it without specific options. For example after the SSH port changed and the SSH keys are provisioned.

### Runtime Variable Example

To have the variable `initial_setup` defined and being used with `true` or `false` you can set the variable like this in your ansible task/playbook:

```yaml
- name: Stop Playbook for SSH transition
  ansible.builtin.pause:
    prompt: |

      SSHD PORT SET & KEYS PROVISIONED
      The initial configuration is done.

      To prevent connection issues, please:
      1. Press 'Ctrl+C' then 'A' to abort this run.
      2. Re-run the playbook without the following options: '-e "initial_setup=true"'

  when: initial_setup | default(false) | bool  # IMPORTANT!!! You need the default value to be "false" otherwise your playbook will always run as initial_setup!
  check_mode: false  # pause doesn't run in check mode by default, so we force it
```

Then call the ansible playbook with the extra option `-e "initial_setup=true"`:

```text
ansible-playbook -i inventory/ playbooks/my_playbook.yml -e "ansible_port=44" -e "ansible_user=iamansible" -e "initial_setup=true"
```

## List all task without running the playbook

Sometimes you want to verify if your new task(s) will be run before you just start an ansible run. Thankfully you can simply append the option
`--list-tasks` like this:

```text
ansible-playbook -i inventory 01_base_deployment.yml --list-tasks
```

The output will show you a list of the tasks like this:

```text
$:> ansible-playbook -i inventory 01_base_deployment.yml --list-tasks

playbook: 01_base_deployment.yml

  play #1 (ionos): Base Deployment IONOS Servers        TAGS: []
    tasks:
      base : Set base_hostname  TAGS: []
      base : Configure Locales  TAGS: []
      base : Create swap file   TAGS: []
      base : Ensure swap file has correct permissions   TAGS: []
      base : Run mkswap on the swap file        TAGS: []
      base : Add swap to /etc/fstab     TAGS: []
      base : Install APT Packages       TAGS: []
      base : Install Extra APT Packages TAGS: []
      base : Purge Unwanted APT Packages        TAGS: []
      base : Create Users       TAGS: []
      base : Setup Sudoers for {{ base_user }}  TAGS: []
      base : Ensure .ssh Directory Exists       TAGS: []
      base : Add authorized_keys        TAGS: []
      base : Replace .bashrc File       TAGS: []
      base : Replace bash + vim files   TAGS: []
      base : Replace root bashrc        TAGS: []
      base : Place root .bash_aliases   TAGS: []
      base : Setup base_sshd_config     TAGS: []
```

Here you can now verify if all your tasks are run by the playbook as you want them.
