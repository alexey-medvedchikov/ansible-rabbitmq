rabbitmq
======

This role configures RabbitMQ cluster. This playbook is derived from alexeymedvedchikov.rabbitmq but adds the ability to use FQDN.

Requirements
------------

Clustering requires proper short hostname resolution on every node in cluster.
Use some /etc/hosts setup role for this. All other requirements are listed in
metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

	# Set /var/lib/rabbitmq/.erlang.cookie
	rabbitmq_erlang_cookie: 1qa2ws3ed

	# Set max open files for rabbit process 
	rabbitmq_ulimit_open_files: 1024

	# Use cluster or no
	rabbitmq_create_cluster: no

	# Short hostname of cluster master, on master node it must be specified too
	rabbitmq_cluster_master: localhost

	# Vhosts to create
	rabbitmq_vhosts: []

	# Enabled plugins
	rabbitmq_plugins:
	  - rabbitmq_management

	# Users to create, self explained parameters
	rabbitmq_users:
	  - user: admin
	    password: admin
	    vhost: /
	    configure_priv: .*
	    read_priv: .*
	    write_priv: .*
	    tags: administrator

	# Users to remove
	rabbitmq_users_removed:
	  - guest

       # Use FQDN
       rabbitmq_use_longname: 'true'

Examples
--------

First of all you need configure /etc/hosts:

	[all]
	hosts:
	  - { ip: '10.0.0.1', domain: 'mq1' }
	  - { ip: '10.0.0.2', domain: 'mq2' }
	  - { ip: '10.0.0.3', domain: 'mq3' }
	  - { ip: '10.0.0.4', domain: 'mq4' }

	[rabbidz]
	mq1.local rabbitmq_cluster_master=mq2
	mq2.local rabbitmq_cluster_master=mq2
	mq3.local rabbitmq_cluster_master=mq3
	mq4.local rabbitmq_cluster_master=mq3

	[rabbidz:vars]
	rabbitmq_create_cluster=yes

or you can use group variables (preferably)

	[rabbidz1]
	mq1.local
	mq2.local

	[rabbidz1:vars]
	rabbitmq_create_cluster=yes
	rabbitmq_cluster_master=mq3

	[rabbidz2]
	mq3.local
	mq4.local

	[rabbidz2:vars]
	rabbitmq_create_cluster=yes
	rabbitmq_cluster_master=mq3

License
-------

LGPL

Author Information
------------------

- Alexey Medvedchikov, 2GIS, LLC

