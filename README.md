## pfSense packer vagrant box
pfSense OVA file creation VirtualBox with Packer

## Download ISO manually
As packer is not able to download gzipped ISO files directly, you have to download
it manually and extract it. Then customize the pfsense-network.json file so packer could
find the ISO file locally.

https://www.pfsense.org/download/

## Create vagrant box
To build images, simply run.

```
git clone https://github.com/pwasiewi/packer-pfSense
cd packer-ubuntu
export VAGRANT_CLOUD_TOKEN=the token string taken from Vagrant https://app.vagrantup.com/settings/tokens
#It uploads the box version based on version number from template.json e.g. "version": "1.8"
#change 42n4 to your vagrant account name!!
packer build -only=virtualbox-iso pfsense-2.4.2-network.json
```

All packages can be taken from http://pkg.freebsd.org/freebsd:11:x86:64/latest/All/

Pfsense is based on freebsd 11.1

## Run Vagrant box (you can omit previous points and use my 42n4/pfsense box)

```
wget https://raw.githubusercontent.com/pwasiewi/packer-pfSense/master/Vagrantfile.2routers
ln -sfn Vagrantfile.2routers Vagrantfile
#pull box from https://app.vagrantup.com
vagrant init 42n4/pfsense
#NIC config in Vagrantfile, change to your settings: I have created boxes in 192.168.0.0/24 network
vagrant destroy -f; vagrant up
```

## Test ssh
```
ssh -vvv -C -A -X vagrant@127.0.0.1 -p2222

```


## Packer Debug

```shell
# Debug for bash etc..
export PACKER_LOG="DEBUG"

# unDebug for bash etc..
export PACKER_LOG=""
```


## config.xml
---

* CPU: `2` Core
* Memory: `2048` MB
* DISK Size: `8192` GB
* ssh enable (LAN Network)
* Time Zone: `Europe/Warsaw`
* Add package
  * sudo
  * virtualbox-ose-additions-noX11 with all dependencies


### OVA information
---

|             |                                              |
|:-           |:-                                            |
| Product     | pfSense                                      |
| Product URL | https://www.pfsense.org                      |
| Version     | 2.4.2                                        |
| Vendor      | @pwasiewi (@FoxBoxsnet fork)                 |
| vendor URL  | https://github.com/pwasiewi                  |
| Repository  | https://github.com/pwasiewi/packer-pfSense   |


### NIC
---

| json    | NET  | NIC | MODE    |
|:-       |:-    |:-   |:-       |
| network | WAN  | em1 | bridged |
| network | LAN  | em2 | intnet  |
| network |      | em3 | intnet  |
| network | NAT  | em0 | nat     |


### adduser
---
|           |         |
|:-         |:-       |
| username  | vagrant |
| password  | vagrant |
| groupname | admins  |

LICENCE APACHE
@taken from https://github.com/FoxBoxsnet/packer-pfSense and changed a little
