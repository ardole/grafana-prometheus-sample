# Grafana and prometheus installation

... with Ansible.

- Install required roles

```
ansible-galaxy install -r roles/requirements.yml
```

- Running playbook

```
ansible-playbook playbook.yml -i inventory.yml
```
