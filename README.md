# Vagrant Example of Multi-node MySQL & HTTP Servers
This is a simple demo of a multi node [Vagrant](https://github.com/hashicorp/vagrant) setup which includes a [Ubuntu http-server](https://app.vagrantup.com/aphorise/boxes/ubuntu16-nginx) & an instance of  [MySQL](https://app.vagrantup.com/brownell/boxes/xenial64lemp) (images from [vagrantcloud](https://app.vagrantup.com/boxes/search)). See `Vagrantfile` for definition details of **port-forwarded** (**58181..N** NGiNX + **53306** MySQL).

```bash
vagrant up ; # or 'vagrant reload' when adjusting Vagrantfile.

vagrant global-status ; # should show running nodes
# id       name   provider   state   directory
# --------------------------------------------------------------------------------
# d69fd16  nginx1-mysql virtualbox running /home/auser/hashicorp.example_multinode2
# e86e530  nginx2-mysql virtualbox running /home/auser/hashicorp.example_multinode2
# d28f6d5  mysql-nginx  virtualbox running /home/auser/hashicorp.example_multinode2

vagrant port mysql-nginx ; # to see ports forwarded.

curl -v 127.0.0.1:58181 ; # check node1 with headers
curl -v 127.0.0.1:58182 ; # check node2 with headers
# ... should output default NGiNX home page.

nc -w 1 -z 127.0.0.1 53306  ; echo $?
# Connection to 127.0.0.1 port 53306 [tcp/*] succeeded!
# 0

# when done:
vagrant destroy -f nginx1-mysql nginx2-mysql mysql-nginx ; # ... destroy al
vagrant box remove -f brownell/xenial64lemp --provider virtualbox ; # ... deleting vbox images
vagrant box remove -f aphorise/ubuntu16-nginx --provider virtualbox ; # ... deleting vbox images
```

Documentation may be found at [the following link & tips section](https://www.vagrantup.com/docs/vagrantfile/tips.html).


**SETUP NOTE**: Newly setup & first time executors should consider setting the environment variable defining default providers - else the download images may occure.
```
VAGRANT_DEFAULT_PROVIDER='virtualbox' ;
# good to set global / export else no default image may be attempted.
```

