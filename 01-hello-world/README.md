# Ansible - A "hello world" Playbook

Playbooks are written in yaml format, and you can actually choose where to store your playbooks. In my case I\'ll create a folder called playbooks for storing my playbooks, and I\'ll create this in the root user\'s home directory:

```bash
[root@controller ~]# pwd
/root
[root@controller ~]# mkdir playbooks
[root@controller ~]# cd playbooks
[root@controller playbooks]# pwd
/root/playbooks
```

In this directory, I'll then create a yaml file called HelloWorld.yml:

```yaml
---
- name: This is a hello-world example
  hosts: ansibleclient01.local
  tasks:
    - name: Create a file called '/tmp/testfile.txt' with the content 'hello world'.
      copy:
        content: hello world\n
        dest: /tmp/testfile.txt
```

This playbook is designed to create the file /tmp/testfile.txt on the client ansibleclient01.local using ansible\'s [copy module](http://docs.ansible.com/ansible/copy_module.html). You can think of this playbook as a script that you can execute. You can execute this playbook using the ansible-playbook command:

```text
[root@controller playbooks]# ansible-playbook HelloWorld.yml

PLAY [This is a hello-world example] *******************************************

TASK [setup] *******************************************************************
ok: [ansibleclient01.local]

TASK [Create a file called '/tmp/testfile.txt' with the content 'hello world'.]
changed: [ansibleclient01.local]

PLAY RECAP *********************************************************************
ansibleclient01.local : ok=2 changed=1 unreachable=0 failed=0

```

This results in the following file being created on the client:

```bash
[root@ansibleclient01 /]# cat /tmp/testfile.txt
hello world
```

Now let's delete the testfile.txt. Then return back to the controller. Now let\'s create testfile.txt in a different way, this time using a static file. we do this by first creating a static file and placing it in a logical location. In my case I created a folder called files and placed the testfile.txt in it:

```bash
[root@controller playbooks]# tree
.
├── files
│     └── testfile.txt
└── HelloWorld.yml
1 directory, 2 files

[root@controller playbooks]# cat files/testfile.txt
hello world
```

In the HelloWorld.yml file I then changed the copy module's **content** attribute to **src**:

```yaml
---
- name: This is a hello-world example
  hosts: ansibleclient01.local
  tasks:
    - name: Create a file called '/tmp/testfile.txt' with the content 'hello world'.
    copy:
      src: ./files/testfile.txt
      dest: /tmp/testfile.txt
```

 Notice, I think the src path is relative to where the playbook\'s location. You can use absolute path but that will make it inflexible to move the static file to another location. <http://docs.ansible.com/ansible/playbooks_intro.html>
