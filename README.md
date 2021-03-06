# Ghost Application Cluster

This sets up a postgresql cluster consisting of one primary (master) server, one streaming replica server, a pgpool server that simply load balances (no health checks, yet), and an app server (I'm using [Ghost](https://ghost.org)). Since it is driven by ansible, you can also use it to deploy to your own stack (though modify as needed! If you're going to use as-is, you would modify the cluster-inventory file). To run on your own stack, you would run:

```
ansible-playbook -i ./cluster-inventory provision.yml --ask-pass --ask-sudo-pass
```

If you decide to use the above command with your vagrant setup, the password at both prompts is 'vagrant' (minus the quotes).

With a vagrant setup, you would access this at: http://10.0.0.2. You'll need to do final setup on the blog (to go to a http://10.0.0.2/admin for example) to get your account set up and ready to blog.

You can specify the password / sudo password in the inventory file...but that would be pretty insecure, wouldn't it? The best approach would be to use ssh keys instead so you don't rely on asking for passwords. If you use it for local development (ie with vagrant), it will automatically run the playbook when you run vagrant up and vagrant provision.

Next goal will be to do do more interesting things with pgpool to get a better understanding of promoting a replica to primary (and vice versa).

I've now also added the [ELK stack](https://www.elastic.co/webinars/introduction-elk-stack) as part of the whole server. Right now, it only processes auth.log, syslog but it is a start towards understanding centralized server logging. This can be accessed from the vagrant setup at http://kibana:kibana@10.0.0.6. You'll need to do a final setup for the index to get it to start displaying logs (select @timestamp as the time field name and click on 'Create' to create the index). Once that is done, you can click on discover to look at your logs.
