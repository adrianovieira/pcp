# PCP
## [P]uppet [C]ommunity [P]latform

#### Table of contents

1. [Overview](#overview)
2. [Technologies](#technologies)
3. [Authors](#authors)
4. [Contributors](#contribuidores)
5. [Compatibility](#compatibility)
6. [Requirements](#requirements) 
7. [Setup](#setup)
8. [Environment](#environment)
9. [Structure](#structure)
10. [Mcollective](#mcollective)
11. [PuppetExplorer](#puppetexplorer)

## Overview

The PCP project aims to offer a complete Puppet virtual environment for testing and also development of Puppet modules.

This vagrantfile provides a easy way to install and integrate the main puppet tools.

## Technologies

* Puppet Server 2.3.2
* Puppet Agent 1.4.2
* Mcollective 2.8.8
* PuppetDB 4.0.2
* PostgreSQL 9.4.6
* Puppet Explorer 2.0.0
* ActiveMQ 5.13.2

Everything is installed and configured using Puppet 4.

## Authors

* Guto Carvalho (gutocarvalho@gmail.com)
* Miguel Di Ciurcio Filho (miguel.filho@gmail.com)

## Contributors

* Adriano Vieira
* Lauro Silveira
* Taciano Tres

## Compatibility

This project was tested using CentOS 7 and Puppet 4.

## Requirements

* Virtualbox >= 4
* Vagrant >= 1.8
  * plugin vagrant-hostsupdater (host records on host)
  * plugin vagrant-hosts (host records on guests)
  * plugin vagrant-proxyconf
* Box puppetlabs/centos-7.2-64-puppet

You must have at least 3 GB of free RAM to run PCP smoothly.

## Setup

    vagrant plugin install vagrant-hosts
    vagrant plugin install vagrant-hostsupdater
    vagrant box add puppetlabs/centos-7.2-64-puppet
    git clone https://github.com/puppet-br/pcp.git
    cd pcp
    vagrant up

### Proxy Setup

If you want to use the proxy plugin

1. Installation

  ```
  vagrant plugin install vagrant-proxyconf
  ```

2. Configuration

```
HTTP_PROXY="http://proxy:3128"
HTTPS_PROXY=HTTP_PROXY
```

## Environment

There are three VMs in the vagrant environment

* puppetserver.hacklab, 192.168.250.20
* puppetdb.hacklab, 192.168.250.25
* puppetmq.hacklab, 192.168.250.30

### environment::puppetserver

puppetserver, puppetdb-termini and puppet agent.

### environment::puppetdb

puppetdb, postgresql, puppet agent and puppet explorer.

### environment::puppetmq

activemq and puppet agent.

## Structure

This project uses the pcp-controlrepo repository as source to install the
production environment using r10k.

    https://github.com/puppet-br/pcp-controlrepo

## Mcollective

You can test the mcollective through the puppetmq.hacklab vm

    vagrant ssh puppetmq.hacklab
    sudo -i
    mco find

## PuppetExplorer

Access the puppet explorer dashboard throught this address

    https://puppetdb.hacklab

You need to accept the certificate.

Run the agent on all VMs to create more reports.
