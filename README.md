rabbitmq
======

This role configures RabbitMQ cluster.

Requirements
------------

Clustering requires proper short hostname resolution on every node in cluster. All other requirements
are listed in metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

* `rabbitmq_erlang_cookie` - кукиса для erlang. В кластерной схеме - должна быть одинакова на всех машинах. 
 * Значение по умолчанию - **1qa2ws3ed**

* `rabbitmq_create_cluster` - Будет ли мы создавать кластер RabbitMQ или все инстансы будут независимыми
 * Значение по умолчанию - **false**

* `rabbitmq_cluster_master` - указывается Мастер-сервер rabbitmq, к которому будут подключаться другие RabbitMQ сервера при инициализации кластера
 * Значение по умолчанию - **localhost**

* `rabbitmq_use_longname` - использовать полные DNS имена. При настройке в 2Gis - требуется указывать 'true'
 * Значение по умолчанию(не используется) - **'false'**

`Доработать этот раздел`


Examples
--------

Плейбук:
```yml
---
- hosts: rabbitmq
  sudo: yes
  roles:
    - rabbitmq
```

Параметры:
```yml
rabbitmq_users:
  - user: admin
    password: passw0rd
    vhost: /
    configure_priv: .*
    read_priv: .*
    write_priv: .*
    tags: administrator


rabbitmq_erlang_cookie: ghwjkhguw324vhsdjk
rabbitmq_ulimit_open_files: 1024
rabbitmq_create_cluster: true
rabbitmq_cluster_master: mq1.rabbitmq.m1.nato
rabbitmq_enabled: yes
rabbitmq_use_longname: 'true'

rabbitmq_plugins:
  - rabbitmq_management
  - rabbitmq_management_agent
  - rabbitmq_shovel
  - rabbitmq_shovel_management
```

Dependencies
------------

None

License
-------

LGPL

Author Information
------------------

- Alexey Medvedchikov, 2GIS, LLC