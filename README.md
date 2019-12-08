# ODROKUBE

A Kubernetes cluster ([k3s](https://github.com/rancher/k3s)) on [odroid](https://www.hardkernel.com/shop/odroid-c2/) boards.

![My odrokube](./img/odrokube.jpg)

This project hosts the code used to create my fancy odroid cluster running k3s.
The boards are running [Armbian Bionic 4.19](https://www.armbian.com/odroid-c2/)

## Ansible inventory

In my setup, I configured the DNS of my ISP to assign DNS anmes to the boards based on their static
addresses. It allows to create the ansible inventory `ansible/inv.ini`:

```
[odroid]
odroid01
odroid02
odroid03
odroid04

[kube-masters]
odroid02

[kube-workers]
odroid01
odroid03
odroid04
```

The `kube-masters` defines the boards where k3s will be configured to act as a `master` nodes.
The `kube-wokers` defines the boards where k3s will be configured to act as a `worker` nodes.

## Initialize the boards

Root login is enabled by default on Armbian so the first execution will have to be
run with the `root` user, default password is `1234`.

```
$ ansible-playbook -i ansible/inv.ini ansible/init-odroid.yml -u root --ask-pass
```

After the first execution, the `ansible` user can be used with ssh key authentication:

```
$ ansible-playbook -i ansible/inv.ini ansible/init-odroid.yml -u ansible
```

## Accessing cluster from outside

Copy /etc/rancher/k3s/k3s.yaml on your machine located outside the cluster as ~/.kube/config.
Then replace "localhost" with the IP or name of your k3s server. kubectl can now manage your k3s cluster.
