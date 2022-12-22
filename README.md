
<p align="center">
    <img src="source/app/static/assets/img/logo.ico" />
</p>

<p align="center">
  Incident Response Investigation System
  <br>
  <br>
</p>

# IRIS

[![License: LGPL v3](https://img.shields.io/badge/License-LGPL_v3-blue.svg)](./LICENSE.txt)   
IRIS is a web collaborative platform aiming to help incident responders sharing technical details during investigations. 

![demo_timeline](img/timeline_speed.gif)

## Table of contents
- [Getting Started](#getting-started)
  - [Run IrisWeb](#run-irisweb)
  - [Configuration](#configuration)
- [Showcase](#showcase)
- [Documentation](#documentation)
  - [Upgrades](#upgrades)
  - [API](#api)
- [Help](#help)
- [Considerations](#considerations)
- [License](#license)


## Getting started
It is divided in two main parts, IrisWeb and IrisModules.   
 - IrisWeb is the web application which contains the core of
Iris (web interface, database management, etc). 
 - IrisModules are extensions of the core that allow third parties to process
data via Iris (eg enrich IOCs with MISP and VT, upload and injection of EVTX into Splunk). 
 
IrisWeb can work without any modules though defaults ones are preinstalled. Head to ``Manage > Modules`` in the UI 
to configure and enable them. 

### Run IrisWeb 
Iris is split on 5 Docker services, each with a different role.

- ``app - iris_webapp``: The core, including web server, DB management, module management etc.
- ``db``: A PostgresSQL database
- ``RabbitMQ``: A RabbitMQ engine to handle jobs queuing and processing
- ``worker``: Jobs handler relying on RabbitMQ
- ``nginx``: A NGINX reverse proxy

Each service can be built independently, which can be useful when developing.

``` bash
#  Clone the iris-web repository
git clone https://github.com/dfir-iris/iris-web.git
cd iris-web

# Checkout to the last tagged version 
git checkout v2.0.0

# Copy the environment file 
cp .env.model .env
# [... optionally, do some configuration as specified below ...]

# Build the dockers
docker-compose build

# Run IRIS 
docker-compose up
```

Iris shall be available on the host interface, port 443, protocol HTTPS - ``https://<your_instance_ip>:443``.  
By default, an ``administrator`` account is created. The password is printed in stdout the very first time Iris is started. It won't be printed anymore after that.  
You can search for ``WARNING :: post_init :: create_safe_admin :: >>>`` in the logs of the `webapp` docker to find the password.  

### Configuration
There are three different options for configuring the settings and credentials: Azure Key Vault, Environment Variables and Configuration Files. This is also the order of priority, if a settings is not set it will fallback on the next option.
For all available configuration options see [CONFIGURATION.md](CONFIGURATION.md). 

## Showcase
For a more comprehensive overview of the case features, 
you can head to [tutorials](https://docs.dfir-iris.org/operations/tutorials/), we've put some videos there.  

## Documentation
A comprehensive documentation is available on [docs.dfir-iris.org](https://docs.dfir-iris.org).

### Upgrades
Please read the release notes when upgrading versions. Most of the time the migrations are handled automatically, but some
changes might require manual labor depending on the version. 

### API
The API reference is available in the [documentation](https://docs.dfir-iris.org/operations/api/#references) or [documentation repository](https://github.com/dfir-iris/iris-doc-src).

## Help
You can reach us on [Discord](https://discord.gg/76tM6QUJza) or by [mail](mailto:contact@dfir-iris.org) if you have any question, issue or idea!   
We are also on [Twitter](https://twitter.com/dfir_iris) and [Matrix](https://matrix.to/#/#dfir-iris:matrix.org).  

## Considerations
Iris is in its early stage. It can already be used in production, but please set backups of the database and DO NOT expose the interface on the Internet. We highly recommend using a private dedicated and secured network.

## License
The contents of this repository is available under [LGPL3 license](LICENSE.txt).

