# Couchbase Server Install Script

This folder contains a script for installing Couchbase server and its dependencies. Use this script along with the
[run-couchbase-server script](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/modules/run-couchbase-server) 
to create a Couchbase [Amazon Machine Image 
(AMI)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) that can be deployed in 
[AWS](https://aws.amazon.com/) across an Auto Scaling Group using the [couchbase-cluster 
module](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/modules/couchbase-cluster).

This script has been tested on the following operating systems:

* Ubuntu 16.04
* Amazon Linux

There is a good chance it will work on other flavors of Debian, CentOS, and RHEL as well.



## Quick start

To install Couchbase, use `git` to clone this repository at a specific tag (see the [releases 
page](https://github.com/gruntwork-io/terraform-aws-couchbase/releases) for all available tags) and run the 
`install-couchbase-server` script:

```
git clone --branch <VERSION> https://github.com/gruntwork-io/terraform-aws-couchbase.git
terraform-aws-couchbase/modules/install-couchbase-server/install-couchbase-server
```

The `install-couchbase-server` script will install Couchbase, its dependencies, and the [run-couchbase-server 
script](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/modules/run-couchbase-server).
You can execute the `run-couchbase-server` script when the server is booting to start Couchbase and configure it to 
automatically join other nodes to form a cluster.

We recommend running the `install-couchbase-server` script as part of a [Packer](https://www.packer.io/) template to 
create a Couchbase [Amazon Machine Image (AMI)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) (see the 
[couchbase-ami example](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/examples/couchbase-ami) for 
fully-working sample code). You can then deploy the AMI across an Auto Scaling Group using the [couchbase-cluster 
module](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/modules/couchbase-cluster) (see the 
[examples folder](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/examples) for fully-working 
sample code).




## Command line Arguments

Run `install-couchbase-server --help` to see all available arguments.

```
Usage: install-couchbase-server [options]

This script can be used to install Couchbase Server and its dependencies. This script has been tested with Ubuntu 16.04 and Amazon Linux.

Options:

  --version		The version of Couchbase to install. Default: 5.1.0.
  --checksum		The SHA-256 checksum of the Couchbase package. Required if --version is specified. You can get it from the downloads page of the Couchbase website.
  --swapiness		The OS swapiness setting to use. Couchbase recommends setting this to 0. Default: 0.

Example:

  install-couchbase-server --version 5.1.0 --checksum 4d6a1f159577f283f6f980f6ab9161630eb2d8fd228429029de004b1be46ad76
```



## How it works

The `install-couchbase-server` script does the following:

1. [Install Couchbase binaries and scripts](#install-couchbase-binaries-and-scripts)
1. [Update swap settings](#update-swap-settings)
1. [Disable transparent huge pages](#disable-transparent-huge-pages)


### Install Couchbase binaries and scripts

Install the following:

* `Couchbase`: Install Couchbase using the appropriate [Linux 
  installer](https://developer.couchbase.com/documentation/server/5.1/install/install-linux.html). 
* `run-couchbase-server`: Copy the [run-couchbase-server 
  script](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/modules/run-couchbase-server) into 
  `/opt/couchbase/bin`. 


### Update swap settings

Set the "swapiness" setting on your OS to 0. See [Swap Space and Kernel 
Swappiness](https://developer.couchbase.com/documentation/server/current/install/install-swap-space.html) for details.


## Disable transparent huge pages

Disable transparent huge pages on your OS. See [Disabling Transparent Huge Pages 
(THP)](https://developer.couchbase.com/documentation/server/current/install/thp-disable.html) for details.



## Why use Git to install this code?

We needed an easy way to install these scripts that satisfied a number of requirements, including working on a variety 
of operating systems and supported versioning. Our current solution is to use `git`, but this may change in the future.
See [Package Managers](https://github.com/gruntwork-io/terraform-aws-couchbase/tree/master/_docs/package-managers.md) 
for a full discussion of the requirements, trade-offs, and why we picked `git`.