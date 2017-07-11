## dashbase-config-template
A template used to make a config repo for a deployment

## Background
Dashbase is comprised of a set of services: API, Table, Web, and Auth. The config for each service exists in its own directory, which you will find here in the configuration template. The dashbase.yml file contains references to the configurations for each service in the Dashbase deployment. It also contains connection info
and mapping of machines to the services that should be installed on each.

Dashbase is controlled by a CLI. The machine running the CLI is referred to as the "master machine". The master machine also
contains the config directory for the deployment.


## Start a Dashbase deployment
The following will help you start a deployment of 1 remote machine, controlled by your macbook, the "master machine".

The remote machine should be 1 m4 or r4 type ec2 instance, started from ami-0ea4886e and Security Group "Dashbase Single Instance"

The work to make Dashbase run is something like this:

```
[ec2-user@ip-172-31-10-146 ~]$ sudo pip install dashbase
[ec2-user@ip-172-31-10-146 ~]$ sudo pip uninstall dashbase
[ec2-user@ip-172-31-10-146 ~]$ sudo chown -R ec2-user /usr/local/bin/
[ec2-user@ip-172-31-10-146 ~]$ pip install dashbase (so that cli can operate on dashbase without sudo)
[ec2-user@ip-172-31-10-146 ~]$ dashbase-cli config show (Home should be ~/.dashbase. all jars should be None)
[ec2-user@ip-172-31-10-146 ~]$ dashbase-cli version
[ec2-user@ip-172-31-10-146 ~]$ dashbase-cli install zookeeper
[ec2-user@ip-172-31-10-146 ~]$ dashbase-cli start zookeeper
[ec2-user@ip-172-31-10-146 ~]$ dashbase-cli ps (Should output that no Dashbase services are found/running)

alexmunk$ pip install dashbase
alexmunk$ dashbase-cli config show
alexmunk$ dashbase-cli version
alexmunk$ git clone https://github.com/dashbase/dashbase-config-template.git
alexmunk$ mkdir dashbase-alexs-deployment
alexmunk$ cp -r dashbase-config-template/ dashbase-alexs-deployment/
alexmunk$ cd dashbase-alexs-deployment
alexmunk$ rm -rf .git
alexmunk$ rm -rf .gitignore
alexmunk$ scp -i ~/.ssh/dashbase_alex_keypair.pem dashbase-tables/json/data/nginx.json ec2-user@remote-host-ip:/data/input/
alexmunk$ vi dashbase.yml
- set prefix to your chosen name of the deployment. i.e. prefix: “alexs-deployment”
- set hosts.host1.hostname: remote-host-IP
- set hosts.host1.username: ec2-user
- set hosts.host1.private_key: ~/.ssh/your_ssh_key.pem
- delete hosts.host2 configuration block
- change all cases of host2 to host1
- set all cases of MONITOR_URL: remote-host-IP:9888 (there's probably one in api and another in json table)
- set web.env.API_HOST: remote-host-IP
- set web.env.API_PORT: 9876 (to match the configured api port in the same file)

alexmunk$ dashbase-cli start cluster --config ~/Dev/dashbase-alexs-deployment/dashbase.yml all

[ec2-user@ip-172-31-10-146 ~]$ dashbase-cli ps (should list all Dashbase services defined in dashbase.yml)
```

Visit: http://remote-host-IP:8080/search


