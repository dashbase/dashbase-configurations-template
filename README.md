# dashbase-config-template
A template used to make a config repo for a deployment

Dashbase is comprised of a set of services: API, Table, Web, and Auth. The config for each service exists in its own directory. The 
dashbase.yml file contains references to the configurations for each service in the Dashbase deployment. It also contains connection info
and mapping of machines to the services that should be installed on each.

Dashbase is controlled by a CLI. The machine running the CLI is referred to as the "master machine". The master machine also
contains the config directory for the deployment.

├── dashbase-api
│   └── conf
│       ├── config.yml
│       └── env-config.yml
├── dashbase-tables
│   ├── json
│   │   ├── conf
│   │   │   ├── config.yml
│   │   │   └── env-config.yml
│   │   └── data
│   │       ├── nginx.json
│   │       └── test_json.log
│   └── regex
│       ├── conf
│       │   ├── config.yml
│       │   └── env-config.yml
│       └── data
│           └── nginx.log
├── dashbase-web
│   └── conf
│       ├── config.yml
│       └── env-config.yml
└── dashbase.yml
