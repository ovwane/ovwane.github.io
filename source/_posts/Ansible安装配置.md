yum install -y epel-release
yum install -y python-pip sshpass

pip install --upgrade pip
pip install ansible

ansible --version

```
ansible 2.4.2.0
```

注：pip方式安装不会在/etc/ansible目录下生成默认的相关配置文件

ansible控制的主机
vim /etc/ansible/hosts 

ansible配置
vim /etc/ansible/ansible.cfg


## playbook
ansible playbook

```
vim ssh-addkey.yml 

# ssh-addkey.yml 
---
- hosts: s1
  gather_facts: no

  tasks:

  - name: install ssh key
    authorized_key: user=root 
                    key="{{ lookup('file', '/root/.ssh/id_rsa.pub') }}" 
                    state=present
```

运行playbook

ansible-playbook -i hosts ssh-addkey.yml