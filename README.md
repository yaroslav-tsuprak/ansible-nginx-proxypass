# Ansible Role: Nginx Proxypass

Adding nginx.conf file
Adding new virtual hosts for proxying to docker container

## Requirements

Server with installed Nginx

## HowTo

1. Go to /ansible/playbooks/host_vars/proxypass.yml
   and add to file new virtual host

2. In /ansible/playbooks/roles/nginx_proxypass/templates/
   copy any vhost-* file with new name
   then change variable name in "server_name" line

3. In /ansible/playbooks/roles/nginx_proxypass/vars/
   copy any file with new hostname.yml
   change variables:
      nginx_server_name
      nginx_set_upstream_port
      nginx_error_log
      nginx_access_log
      nginx_vhosts_filename

4. Edit /ansible/playbooks/roles/nginx_proxypass/tasks/vhosts.yml
   copy/past part of code and change variables name:
      - name: Include *** variables
        include_vars: "{{ ***_vhost_file_name }}.yml"

      - name: Add *** vhost config file
        template:
          src: "vhost-{{ ***_vhost_file_name }}.conf.j2"
          dest: "{{ nginx_vhost_path }}/{{ ***_vhost_file_name }}.conf"
          mode: "{{ nginx_conf_mode }}"
        notify: Reload Nginx

5. Start playbook by command: ansible-playbook playbook.yml --ask-become-pass
   enter sudo password

## Author

Yaroslav Tsuprak
