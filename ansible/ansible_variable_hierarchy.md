# Ansible Variable Hierarchy

Here is a short explanation of how the variable hierarchy works in ansible.

!!! Warning
    Be careful with **role variables** because they are very high in the hierarchy - they are hard to override - **prefer using defaults** instead!

## The hierarchy in order

1. Default Var (defined in the role at `defaults/main.yml`)
Can be overriden by group_vars & host_vars
2. Task Var (defined on the individual task)
A variable defined within a task always beats any other variable
3. Block Var (defined in the block)
A variable defined in a block overrides all variables below.
4. Role Var
A variable defined in a role overrides group_vars & host_vars
5. Host Var
A variable defined in the host_vars overrides the group_vars
6. Group Var
Defines variables for groups and can basically overriden by any variable above

## Example

Defined in `roles/nginx/defaults.yml`:

```yaml
http_port: 123  # Can be overriden by host_vars & group_vars - best alternative to role variables
```

Defined in `group_vars/webservers.yml`:

```yaml
http_port 80
```

Derfined in `host_vars/special-server.yml`:

```yaml
http_port: 8080  # Overrides group_vars
```

Defined in `roles/nginx/vars/main.yml`:

```yaml
http_port: 443  # Overrides group_vars & host_vars
```

Defined inside a `block` statement (we do not use them so far):

```yaml
- name: Maintenance Tasks
  block:
    vars:
      http_port: 9090  # Overrides role vars, group_vars & host_vars
```

Defined within a task `roles/nginx/tasks/force_http_port.yml`:

```yaml
- name: Force custom http_port
  vars:
    http_port: 2222 # Overrides EVERYTHING above!
```

## So which variable is used at the end?

If all the different variables above would be used at the same time for one single task, here is what the value of `http_port` would be in
the end:

| Level | Value | Why? |
| --- | --- | --- |
| group_vars | 80 | Generic starting point |
| host_vars | 8080 | Specific for that host |
| role vars | 443 | Applied in the logic of the role - considered to be very intentional |
| block var | 9090 | Only applied within this small section of code |
| task var | 2222 | The **absolute winner** - defined right at the point of creation |
