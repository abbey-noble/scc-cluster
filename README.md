# SCC training cluster

Ansible configuration for the SCC training cluster.

## Inventory

`inventory/hosts.yaml` lists the nodes in the `cluster` group and tells Ansible which hostname, port, and user to use when connecting over SSH.

| Ansible host | Internal IP | SSH endpoint |
|---|---|---|
| `scc-01` | `10.10.80.11` | `sc26.eu-west-2.cloud:22201` |
| `scc-02` | `10.10.80.12` | `sc26.eu-west-2.cloud:22202` |

Ansible connects as the `scc` user.

## Playbooks

Playbooks define the configuration Ansible applies to the nodes.

- `accounts.yaml`: creates user accounts and installs their SSH public keys
- `install-iperf3.yaml`: installs `iperf3` for network testing

Both playbooks target the `cluster` inventory group and require sudo access.

## Setup/Run

Clone the repository and enter its directory:

```bash
git clone https://github.com/abbey-noble/scc-cluster.git
cd scc-cluster
```

Install the required Ansible collections:

```bash
ansible-galaxy collection install -r requirements.yaml
```

Check that Ansible can connect to both nodes ( ping pong :-) ):

```bash
ansible cluster -m ansible.builtin.ping
```

Preview a playbook without changing the nodes:

```bash
ansible-playbook accounts.yaml --check --ask-become-pass
```

Apply a playbook:

```bash
ansible-playbook accounts.yaml --ask-become-pass
```

Run a playbook on one node:

```bash
ansible-playbook accounts.yaml \
  --limit scc-01 \
  --ask-become-pass
```
