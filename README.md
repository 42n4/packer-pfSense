# pfSense packer OVA
pfSense OVA file creation VirtualBox with Packer

# Download ISO manually
As packer is not able to download gzipped ISO files directly, you have to download
it manually and extract it. Then customize the pfsense-network.json file so packer could
find the ISO file locally.

https://www.pfsense.org/download/

# Create OVA

```
packer build -only=virtualbox-iso pfsense-2.4.2-network.json
#all packages can be taken from http://pkg.freebsd.org/freebsd:11:x86:64/latest/All/
#pfsense is based on freebsd 11.1
#upload box to https://app.vagrantup.com
vagrant up
```

## Pakcer Debug

```shell
# Debug for bash etc..
export PACKER_LOG="DEBUG"

# Debug for fish shell
set -x PACKER_LOG DEBUG


# unDebug for bash etc..
export PACKER_LOG=""

# unDebug for fish shell
set -x PACKER_LOG

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

|||
|:-|:-|
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

|:-         |:-       |
| username  | vagrant |
| password  | vagrant |
| groupname | admins  |

LICENCE APACHE
@taken from https://github.com/FoxBoxsnet/packer-pfSense and changed a little
