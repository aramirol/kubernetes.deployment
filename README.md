# kubernetes.deployment

[![Platform](https://img.shields.io/badge/kubernetes-v.1.26.3-blue?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![OS](https://img.shields.io/badge/centos-Stream_9-blue?logo=centos&logoColor=white)](https://centos.org/)
[![License](https://img.shields.io/github/license/aramirol/kubernetes.deployment)](https://github.com/aramirol/kubernetes.deployment/blob/main/LICENSE)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/aramirol/kubernetes.deployment?color=orange&logo=git&logoColor=white)](https://github.com/aramirol/kubernetes.deployment/releases)

This role is ready to deploy a basic Kubernetes platform over CentOS-9 pre-installed nodes. Just go through the variable files and modify them if necessary.

<img src="Kubernetes_Logo.png" data-canonical-src=".Kubernetes_Logo.png" width="20%" height="20%" />


## Requirements

You must have the VMs provisioned before using this role.
- 1 or more **Control-Plane** nodes (*Recommended: 3 VMs / 2 CPUs / 4GB Ram*)
- 1 or more **Compute** nodes (*Recommended: 2 VMs / 2 CPUs / 4GB Ram*)

You must have a **Load Balancer** to switch traffic to Control-Plane nodes or/and Compute nodes. For example [HAProxy](https://www.haproxy.org/).

Please, use the files from [examples folder](./examples/) to create your inventory file, ansible config file and more.

### Inventory file

Your inventory file must have the following groups:

- **`[control-plane-primary]`**: This is the first Control-Plane node that is used to initialize the kubernetes cluster and deploy the additional components.
- **`[control-plane-secondary]`**: This group contains the rest of the **Control-Plane nodes** that make up the kubernetes cluster and will be joined once the primary node has initiated.
- **`[control-plane-nodes:children]`**: This group contains the two previous groups.
- **`[compute-nodes]`**: This group contains all the Compute nodes that will be joined to the cluster.

## Role variables

- *vars/main.yml*

  | Variable | Description |
  |---|---|
  | `primary_cp_node` | Name of the remote hosts where first Control-Plane node will be deployed by the role |
  | `deploy_cluster_control_plane` | **False** if you want to deploy a single node Control-Plane. **True** to deploy as a cluster.
  | `lb_name` | Load Balancer NAME to redirect inbound traffic. Required for deployment with multiple Control-Plane nodes. |
  | `lb_ip` | Load Balancer IP to redirect inbound traffic. Required for deployment with multiple Control-Plane nodes. |
  | `lb_port` | Load Balancer PORT to redirect inbound traffic. Required for deployment with multiple Control-Plane nodes. |
  | `dns_name` | DNS NAME to resolve hosts. If not, use /etc/hosts on all nodes. |
  | `dns_ip` | DNS IP to resolve hosts. If not, use /etc/hosts on all nodes. |
  | `networking_url` | Kubernetes network based URL (example: calico, flannel, weave...). |
  | `deploy_ingress` |  **True** if you want to deploy a NGINX Ingress Controller |
  | `deploy_storage` |  **True** if you want to deploy a Rook Ceph Storage |


## Dependencies

N/A

## Example Playbook

```yml
$ ansible-galaxy install -r requirements.yml -p <<roles_path>>

# requirements.yml
---
- scm: git
  src: https://github.com/aramirol/kubernetes.deployment.git
  version: v1.0.0
  name: kubernetes.deployment
```

```yml
$ ansible-playbook playbook.yml

# playbook.yml
---
- hosts: all
  gather_facts: true
  
  roles:
   - kubernetes.deployment
```

## License

[MIT License](https://github.com/aramirol/kubernetes.deployment/blob/main/LICENSE)

## Author Information

https://github.com/aramirol
