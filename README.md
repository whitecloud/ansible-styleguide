# Ansible Styleguide


## Environment 

When provisioning a server with environment variables add the environment variables to /etc/environment with lineinfile. Do this from the ansible role that is associated with the service or application that is being installed. For example for tomcat installation the CATALINA_HOME environment variable is often used to reference the folder that contains tomcat and its associated webapps. 

```yaml
- name : add line CATALINA_HOME to /etc/environment
  lineinfile :
    dest : "/etc/environment"
    line : "CATALINA_HOME={{ tomcat_home }}"
    state : present
  sudo : true

```

## Booleans

There are many different ways to specify a boolean value in ansible, True/False, true/false, yes/no, 1/0. While it is cute to see all those options we prefer to stick to one : true/false. The main reasoning behind this is that java and javascript have similar designations for boolean values. 

```yaml
# bad
- name : start sensu-client
  service:
    name    : sensu-client
    state   : restarted
    enabled : 1
  become : yes
 
# good
- name: start sensu-client
  service:
    name: sensu-client
    state: restarted
    enabled: true
  become: true
```

### Why?

## Colons maps and key value pairs

Use only one space after the colon when designating a key value pair

```yaml
# bad
- name : start sensu-client
  service:
    name    : sensu-client
    state   : restarted
    enabled : true
  become : true


# good
- name: start sensu-client
  service:
    name: sensu-client
    state: restarted
    enabled: true
  become: true
```
## Sudo
Use the new become syntax when designating that a task needs to be run with sudo

```
#bad
- name: template client.json to /etc/sensu/conf.d/
  template: src=client.json.j2 dest=/etc/sensu/conf.d/client.json
  sudo: true
 
# good
- name: template client.json to /etc/sensu/conf.d/
  template: src=client.json.j2 dest=/etc/sensu/conf.d/client.json
  become: true
```
### Why?
> Because sudo was deprecated at version 1.9.1
