# Ansible Tutorial on Vagrant

## できること

ansibleで各リモートサーバー(vagrant)にLAMP環境を提供する

## 必要なもの

 - ansibleサーバー(1台)
 - リモートサーバー(2台)

## ansibleサーバー(1台)

```sh
$ cd ansible
$ vagrant up
$ vagrant ssh
[vagrant@localhost ~] ansible --version
[vagrant@localhost ~] ansible 2.9.9
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.6/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.6.8 (default, May 21 2019, 23:51:36) [GCC 8.2.1 20180905 (Red Hat 8.2.1-3)]
```

## リモートサーバー(2台)

```sh
$ cd server
$ vagrant up
$ vagrant ssh server1
$ vagrant ssh server2
```

## ansible環境からリモートサーバーにssh接続できるようにする

```
$ ssh-keygen -t rsa
$ ssh-copy-id vagrant@192.168.33.11
$ ssh-copy-id vagrant@192.168.33.12
```

## ansibleからリモートホストに接続できていることを確認する

```
$ ansible all -i inventory/hosts -m ping
192.168.33.11 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
192.168.33.12 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
```

## ansible playbookを実行する

```
ansible-playbook -i inventory/hosts playbook.yml
PLAY [all] **************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [192.168.33.11]
ok: [192.168.33.12]
..........中略
```

<img width="1281" alt="スクリーンショット 2020-05-23 16 10 47" src="https://user-images.githubusercontent.com/19683276/82725594-eaa9e480-9d18-11ea-9409-4f50e623158e.png">


## vagrantの状態をグローバルに確認する

```
vagrant plugin install vagrant-global-status
vagrant global-status
/Users/kosa3/ghq/github.com/kosa3/ansible-tutorial/ansible
  default      running      (virtualbox)   2020-05-23 11:40:48 +0900

/Users/kosa3/ghq/github.com/kosa3/ansible-tutorial/server
  server1      running      (virtualbox)   2020-05-23 12:07:23 +0900
  server2      running      (virtualbox)   2020-05-23 12:07:50 +0900
```
