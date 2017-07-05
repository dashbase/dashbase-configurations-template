# dashbase-config-template
A template used to make a config repo for a deployment

Dashbase is comprised of a set of services: API, Table, Web, and Auth. The config for each service exists in its own directory. The 
dashbase.yml file contains references to the configurations for each service in the Dashbase deployment. It also contains connection info
and mapping of machines to the services that should be installed on each.

Dashbase is controlled by a CLI. The machine running the CLI is referred to as the "master machine". The master machine also
contains the config directory for the deployment.

The following will help you start a cluster of Dashbase that looks like the following:
* master machine: macbook
* cluster machine: 1 ec2 instance, started from ami-0ea4886e (m4.xlarge recommended to get started)


The work to make Dashbase run is something like this:

```
[ec2-user@ip-172-31-10-146 ~]$ sudo mkdir /data/index
[ec2-user@ip-172-31-10-146 ~]$ sudo mkdir /data/input (grr. This should be changed)
[ec2-user@ip-172-31-10-146 ~]$ sudo chown -R ec2-user:ec2-user data
[ec2-user@ip-172-31-10-146 ~]$ sudo pip install dashbase (non-sudo fails)
[ec2-user@ip-172-31-10-146 ~]$ dashbase config
[ec2-user@ip-172-31-10-146 ~]$ install zookeeper
[ec2-user@ip-172-31-10-146 ~]$ start zookeeper

alexmunk$ git clone https://github.com/dashbase/dashbase-config-template.git
alexmunk$ cp -r dashbase-config-template/ dashbase-alexs-deployment/
alexmunk$ cd dashbase-alexs-deployment
alexmunk$ rm -rf .git
alexmunk$ rm -rf .gitignore
alexmunk$ scp -i ~/.ssh/dashbase_alex_keypair.pem dashbase-tables/json/data/nginx.json ec2-user@54.153.4.215:/data/input/
alexmunk$ vi dashbase.yml
- Delete host2
- add remote host IP to host1 config block
- Added path to ssh key: ~/.ssh/dashbase_alex_keypair.pem
- swap out all cases of host2 with host1
- Change all cases of MONITOR_URL: to <remote host IP>:9888
- Change web.API_HOST: to <remote host IP>
- Change web.API_PORT: to 9876 (to match the configured api port in the same file)
- Change prefix to name the deployment. i.e. “alexs-deployment”

alexmunk$ dashbase start cluster --config ~/Dev/dashbase-alexs-deployment/dashbase.yml all

[ec2-user@ip-172-31-10-146 ~]$ dashbase ps (should list all Dashbase services defined in dashbase.yml)
```

Visit: https://host-IP-where-Web-is-running:8080/search
