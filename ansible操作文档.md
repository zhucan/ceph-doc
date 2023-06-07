# ansible操作文档

## 安装

### CentOS Stream release 8

```SQL
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
sudo dnf install ansible
```

### MacOS 

```SQL
sudo easy_install pip
sudo pip install ansible
```

### CentOS  / Fedora

```SQL
sudo yum install ansible
```

**注意****：**可能需要在某些版本的CentOS，RHEL和Scientific Linux上添加EPEL-Release存储库。

### ubuntu

```SQL
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

## 配置SSH免密

### 准备config.yml配置文件

```SQL
---
id_rsa_file: "/root/.ssh/id_rsa"
passphrase: "root"
root_password: "password"
nodes:
  - 1.2.3.4
  - 5.6.7.8
  - 9.10.11.12
```

- id_rsa_file： 密钥路径
- passphrase: 用户名
- root_password :密码
- nodes： 节点ip

### 准备password-less-ssh.yml用于执行task

```SQL
---
- hosts: localhost
  tasks:
    - stat:
        path: "{{ id_rsa_file }}"
      register: op

    - name: Generating ssh key pair
      command: ssh-keygen -t rsa -b 4096 -f "{{ id_rsa_file }}" -q -N "{{ passphrase }}"
      when: op.stat.exists == false

    - debug: 
        msg: "Key pair already exists. Using the same key."
      when: op.stat.exists

    - name: Copy public key to the nodes
      command: sshpass -p "{{ root_password }}" ssh-copy-id -i "{{ id_rsa_file }}" root@"{{ item }}" -f -o StrictHostKeyChecking=no
      with_items:
        - "{{ nodes }}"

  vars_files:
    - config.yml 
```

- vars_files：指定config.yml路径

### 执行ansible-playbook

```SQL
[root@k8s-1 ansible]# ansible-playbook password-less-ssh.yml 
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [localhost] ********************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************
ok: [localhost]

TASK [stat] *************************************************************************************************************************************************************
ok: [localhost]

TASK [Generating ssh key pair] ******************************************************************************************************************************************
skipping: [localhost]

TASK [debug] ************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Key pair already exists. Using the same key."
}

TASK [Copy public key to the nodes] *************************************************************************************************************************************
changed: [localhost] => (item=10.9.9.147)
changed: [localhost] => (item=10.9.9.148)
changed: [localhost] => (item=10.9.9.149)

PLAY RECAP **************************************************************************************************************************************************************
localhost                  : ok=4    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0 
```