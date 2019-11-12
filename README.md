# morpheus_ha_ansible

This is a repo of Ansible roles that allows you to provision a High Availablility Morpheus application.

## HA Architecture

* 2 App servers
* Odd Number Elasticsearch nodes (will support additional nodes)
  * single node implementations not supported
* Odd Number RabbitMQ nodes (will support additional nodes)
  * single node implementations not supported
* Odd Number Percona XtraDB Cluster Nodes (will support additional nodes)
  * single node implementations not supported

![alt text](https://github.com/tadamhicks/morpheus_ha_ansible/blob/master/HA.jpg "Morpheus HA Architecture Example")

### Assumptions

* CentOS 7.x or Ubuntu 18.04.
* Entirely separated services.  They can reside on the same sever(s) or not, this is up to the user to follow best practices around Isolation.
* A valid loadbalancer/VIP for RabbitMQ has been configured.
  * Ports and server mappings for ports 5672 and 61613.
* A valid loadbalancer/VIP for application tier servers has been configured.
  * Ports and server mappings for ports 443 and 80.
* Shared storage for the application servers has been set up.  Future versions will configure this from NFS paths for you.

## Getting Started

Edit group_vars/all replacing values with pertinent data, such as `appliance_url` (value for the FQDN of the vip in front of appliance servers), `rabbitmq_lb` (value for the load balancer ip or FQDN in front of the Rabbit nodes), and any usernames, clusternames or password fields that should be changed.

The host groups for the `hosts` file should be edited for valid ips of target servers.

Here is an example of a valid `hosts` file including the requisite groups:

```text
[rabbit_cluster]
52.52.27.6 ansible_user=slimshady
13.56.104.200 ansible_user=slimshady
54.241.108.245 ansible_user=slimshady

[elastic_cluster]
13.56.44.24 ansible_user=slimshady
54.215.121.50 ansible_user=slimshady
13.57.169.119 ansible_user=slimshady

[db]
52.9.94.143 ansible_user=slimshady
52.9.226.166 ansible_user=slimshady
52.8.96.0 ansible_user=slimshady

[morpheus]
13.57.66.216 ansible_user=slimshady
52.52.2.76 ansible_user=slimshady
```

Once this has been configured it is very straightforward to run the `morpheus.yml` playbook on these hosts:

```text
slimshady@localhost:~$ ansible-playbook -i /path/to/hosts morpheus.yml
```
