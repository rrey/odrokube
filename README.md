# odrokube

## Initialize the boards

Root login is enabled by default on ubuntu so the first execution will have to be
run with the `root` user.

```
$ ansible-playbook -i ansible/inv.ini ansible/init-odroid.yml -u root --ask-pass
```

After the first execution, the `ansible` user can be used with ssh key authentication:

```
$ ansible-playbook -i ansible/inv.ini ansible/init-odroid.yml -u ansible
```
