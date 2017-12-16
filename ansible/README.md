# Ansible Installer

The ansible installer will allow you to configure multiple nodes simultaneously.

Configure the hosts files with your list of nodes eg.

```
[all:vars]
email=test

[zen-nodes]
node1.com fqdn=test stakeaddr=test region=sea
node2.com fqdn=test2 stakeaddr=test2 region=sea
```

Now run the installer with:

```
ansible-playbook -i hosts main.yml
```

When the installation completes, you can check the output of all your
installed nodes will be stored in /tmp/zen-node-results

```
cat /tmp/zen-node-results
```

It will list results similar to this:

```
localhost:

Shield Address:
zcNBdJxZnhTZMdiSQABYiW1wY2A8Swrq8TsauYLiyaShKD91GrZvn1dqAkhZ8USmAHoKNxhokeoYJZwJAtKjyeWN4BMNM6v

Balance:
  transparent: 0.00
  private: 0.00
  total: 0.00
```

You may now send your 1 ZEN to this shield address.

### Upgrading

The installer has a handy way to update all of your zen-nodes:

```
ansible-playbook -i hosts upgrade.yaml
```

This will upgrade restart the required services. It is recommended you subscribe to
[announcement tracking](https://github.com/WhenLamboMoon/docker-zen-node/issues/28) to receive email notifications.
