# PCP
## **P**uppet **C**ommunity **P**latform

#### Table of contents

1. [Overview](#overview)
2. [Technologies](#technologies)
3. [Authors](#authors)
4. [Contributors](#contribuidores)
5. [Compatibility](#compatibility)
6. [Requirements](#requirements)
7. [Setup](#setup)
8. [VMs](#vms)
9. [Control](#control)
10. [Mcollective](#mcollective)
11. [PuppetExplorer](#puppetexplorer)
12. [Proxy](#proxy)

## 1. Overview

The PCP project aims to offer a complete Puppet virtual environment for testing and also development of Puppet modules.

This project provides a easy way to install and integrate the main puppet tools.

## 2. Technologies

* Puppet Server 2.4.0
* Puppet Agent 1.5.0
* Mcollective 2.8.8
* PuppetDB 4.1.0
* PostgreSQL 9.5.3
* Puppet Explorer 2.0.0
* ActiveMQ 5.13.2

Everything will be installed and configured using Puppet 4.

## 3. Authors

* Guto Carvalho (gutocarvalho@gmail.com)
* Miguel Di Ciurcio Filho (miguel.filho@gmail.com)

## 4. Contributors

* Adriano Vieira
* Lauro Silveira
* Taciano Tres

## 5. Compatibility

This project was tested using CentOS 7 and Puppet 4.

## 6. Requirements

* Virtualbox >= 4
* Vagrant >= 1.8
  * plugin vagrant-hostsupdater (host records on host)
  * plugin vagrant-hosts (host records on guests)
* Box puppetlabs/centos-7.2-64-puppet

You must have at least 3 GB of free RAM to run PCP smoothly.

## 7. Setup

## 7.1 Pre-reqs

```
vagrant plugin install vagrant-hosts
vagrant plugin install vagrant-hostsupdater
vagrant box add puppetlabs/centos-7.2-64-puppet
git clone https://github.com/puppet-br/pcp.git
cd pcp
```

### 7.2. Installations

We have two setup options, monolitic or split.

#### 7.2.1 monolitic

If you want the monolitic installation:

    cd pcp
    cd vms/monolitic
    vagrant up

#### 7.2.2 split

If you want the split installation:

    cd pcp
    cd vms/split
    vagrant up

## 8. VMs

## 8.1 Monolitic installation

There is one VM in the vagrantfile of the monolitic installation

* puppetserver.hacklab, 192.168.250.35
  * puppetserver, puppetdb-termini and puppet agent
  * puppetdb, postgresql, puppet agent and puppet explorer
  * activemq and puppet agent

Everything will be installed together.

## 8.2 Split installation

There are three VMs in the vagrantfile of the split installation

* puppetserver.hacklab, 192.168.250.20
  * puppetserver, puppetdb-termini and puppet agent.
* puppetdb.hacklab, 192.168.250.25
  * puppetdb, postgresql, puppet agent and puppet explorer.
* puppetmq.hacklab, 192.168.250.30
  * activemq and puppet agent.

## 10. Control

This project uses the pcp-controlrepo repository as source to install the
production environment using r10k.

    https://github.com/puppet-br/pcp-controlrepo

It's just an example, you can fork, modify or use your own control repo.

## 11. Mcollective

### 11.1 Monolitic

To test mcollective use the commands below

    vagrant ssh puppet-pcpm.hacklab
    sudo -i
    mco find

### 11.2 Split

To test mcollective use the commands below

    vagrant ssh puppetmq.hacklab
    sudo -i
    mco find

## PuppetExplorer

### 11.1 Monolitic

To access the puppet explorer dashboard use the url below

    https://puppet-pcpm.hacklab

You need to accept the certificate.

Run the agent to create more reports.

### 11.1 Split

To access the puppet explorer dashboard use the url below

    https://puppetdb.hacklab

You need to accept the certificate.

Run the agent on all VMs to create more reports.

## 12. Proxy

To use the vagrant proxy plugin follow the instructions

### 12.1. Installation

  ```
  vagrant plugin install vagrant-proxyconf
  ```

### 12.2 Configuration

To configure all possible software on all Vagrant VMs, add the following to $HOME/.vagrant.d/Vagrantfile (or to a project specific Vagrantfile):

```
Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = "http://192.168.0.2:3128/"
    config.proxy.https    = "http://192.168.0.2:3128/"
    config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
  end
  # ... other stuff
end
```

More info at https://github.com/tmatilai/vagrant-proxyconf
