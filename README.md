# Ansible_files
## Single DB install

* configuare Ansible inventory file.

```
cat /etc/ansible/hosts

[mysql]
10.110.19.70
10.110.19.71

```

* configuare entrance file.

```

cat /etc/ansible/main.yml

---
- hosts: mysql
  become: yes
  vars:
    test_var: "test_value"

  roles:
    - role: percona-server-5.7.44
      when: 
        - ansible_distribution == 'CentOS'
        - ansible_distribution_major_version  == '7'

```

* Running install

```
ansible-playbook -i hosts main.yml  \
-e mysql_deploy_dir=/data/mysql  \
-e mysql_port=3306 \
-e mysql_instance_name=test \
-e mysql_mode=single 
```


## Mastr Slave install 

* configuare Ansible inventory file

```
cat /etc/hosts

[mysql]
10.110.19.70 mysql_role=master
10.110.19.71 mysql_role=slave

```

* configuare entrance file.

```
cat /etc/ansible/main.yml

---
- hosts: mysql
  become: yes
  vars:
    test_var: "test_value"

  roles:
    - role: percona-server-5.7.44
      when: 
        - ansible_distribution == 'CentOS'
        - ansible_distribution_major_version  == '7'

```

* Running install

```
ansible-playbook -i hosts main.yml  \
-e mysql_deploy_dir=/data/mysql  \
-e mysql_port=3306 \
-e mysql_instance_name=test \
-e mysql_mode=master_slave \
-e mysql_replication_gtid=1 

```
