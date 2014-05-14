# Learn BOSH with a bosh-lite demo

This project includes an easy-to-run command that does everything to build [bosh-lite](https://github.com/cloudfoundry/bosh-lite) on your local machine, and deploy an example system. The "killer app" system for BOSH to deploy [Cloud Foundry](http://www.cloudfoundry.com/).

Sections:

* [FAQ](#faq)
* [Quick Start](#quick-start)
* [Requirements](#requirements)
* [Concepts](#concepts), while you wait
* [Slow Start](#slow-start), step-by-step
* [Quick Shutdown](#quick-shutdown)

## FAQ

**What is BOSH?** An open-source platform for deploying and managing systems on your favorite infrastructures: vSphere, OpenStack and AWS.

**Can I try it out on my laptop without AWS/OpenStack/vSphere?** Yes. For dev/test/demo of BOSH, we have [bosh-lite](https://github.com/cloudfoundry/bosh-lite) - a version of BOSH that runs on your local machine that manages Linux Containers, rather than virtual machines on your favorite infrastructure.

**Can I use bosh-lite on my Mac?** Yes, bosh-lite is run within a Linux virtual machine via Vagrant/Virtualbox; and not directly run within your OS X machine.

**Can I use bosh-lite on my Windows machine?** Ah, no, not afaik. Even though Vagrant/Virtualbox can run on Windows; it is the current BOSH CLI that may not work on a Windows machine.

**What is this project?** The purpose of this project is to give you a Quick Start to running bosh-lite on your local machine, and then introducing you to the concepts of BOSH. Everything you can do on bosh-lite, every BOSH release you can deploy, can then be used on your favourite infrastructure. This project will deploy [cf-release](https://github.com/cloudfoundry/cf-release) and [gemfire-bosh-release](https://github.com/jcherng-pivotal/gemfire-bosh-release) to the bosh-lite. In addition, [gemfire service broker](https://github.com/jcherng-pivotal/cloudfoundry-brokers) and a gemfire service will be created. Finally, two applications binded with the gemfire service will be deployed to the Cloud Foundry running on your local bosh-lite.

**Explain it again. What is BOSH and what is bosh-lite?** bosh-lite runs within Vagrant and when it provisions "vms" (aka virtual machines/servers/machines) it is actually provisioning Linux containers (via the Warden project, similar to the Docker project). BOSH is the production version and is configured to provision "vms" on a single target infrastructure.

Each VM is bound to the infrastructure's networking. Each VM can have an optional "persistent disk" attached (for example, an EBS volume on AWS). Each VM then runs one or more processes. All the processes across all the VMs compose together as a "system". Systems are used to run your company.

BOSH deploys these systems (called "deployments"), and can upgrade them, scale them, and heal them.

bosh-lite performs all the same functions except entirely within your laptop, and without the concept of "persistent disks".

**What infrastructures are currently supported by BOSH?** BOSH can currently control AWS (EC2 and VPC networking), OpenStack (Nova & Neutron networking), and vSphere. There are also variations of BOSH for: vCloud, Google Compute, Cloud Stack and Tier 3/Century Link. There is major work going on in the BOSH project to make it much easier to describe support for different infrastructures.

**Who is using BOSH?** Currently, BOSH is primarily being used to deploy and run small, medium and large private Cloud Foundry deployments; as well as to run Pivotal's public Cloud Foundry: [run.pivotal.io](https://run.pivotal.io).

**What can I deploy with BOSH?** You can deploy any system, assuming your system is a collection processes to be run on virtual machines. Either you write new BOSH releases for your own software/systems; or you use pre-existing BOSH releases from the community. Try Googling for "boshrelease" or visit [https://github.com/cloudfoundry-community](https://github.com/cloudfoundry-community)

There are a number of open source BOSH release projects for running open source systems ([redis](https://github.com/cloudfoundry-community/redis-boshrelease), [etcd](https://github.com/cloudfoundry-community/etcd-boshrelease), [jenkins](https://github.com/cloudfoundry-community/jenkins-boshrelease), [gitlab](https://github.com/drnic/gitlab-boshrelease))

## Quick Start

During this script, approximately 2.5G of content is downloaded (Vagrant box, Cloud Foundry source & dependencies, Warden image/stemcell). Also, the first time you run the installer (it can be re-run over and over to upgrade and rebuild), it will compile the source packages of Cloud Foundry one time. If you re-run the script below, these assets will not be downloaded again.

1. Clone a copy of bosh-lite-cf-gf-demo:
  ```
  cd ~/workspace
  git clone https://github.com/jcherng-pivotal/bosh-lite-cf-gf-demo
  ```

2. Run the bosh-lite-cf-gf-demo script:
  ```
  ./bosh-lite-cf-gf-demo/binscripts/bosh-lite-cf-gf-demo
  ```

You may you want to [read through the scripts](https://github.com/jcherng-pivotal/bosh-lite-cf-gf-demo/tree/master/binscripts) first.

If the script ever fails, re-run it and it will skip over any steps that were already successfully.

## Requirements

For bosh-lite to boot up, it has the following requirements:

* Vagrant/Virtualbox 1.4+ - for booting the bosh-lite virtual machine on your laptop
* Ruby 1.9.3 - for the BOSH CLI

The installer script uses the following to get some assets:

* wget
* git

To deploy Cloud Foundry and then talk to Cloud Foundry as an administrator/user:

* spiff - used to construct a large YAML file used to deploy BOSH releases
* cf - Cloud Foundry's own CLI for users and admins

The installer script also uses the following to compile sample applications:

* maven

The is project has dependencies on third party licensed binary files. You will have to download the following files seperately:

* [jdk7 - Linux 64 tgz version](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)
   - Rename the tgz file to jdk-7-linux-x64.tar.gz and place it under [bosh-lite-cf-gf-demo/blobs/java](https://github.com/jcherng-pivotal/bosh-lite-cf-gf-demo/tree/master/blobs/java)
* [Pivotal GemFire](https://network.gopivotal.com/products/pivotal-gemfire)
   - Rename the zip file to Pivotal_GemFire_7.zip and place it under [bosh-lite-cf-gf-demo/blobs/gemfire](https://github.com/jcherng-pivotal/bosh-lite-cf-gf-demo/tree/master/blobs/gemfire)

The installer script will test for the existence of these requirements. It's currently not clever enough to check for versions, so you will be prompted to confirm you have the right versions.

## Concepts

The Quick Start will take a while. Or a long time if you have slow internet, downloading the 2.5G of bits and bobs.

Let's learn something about BOSH.

### Servers, VMs and jobs

Concept number one is the Server. Also known as virtual machines, VMs, or `vms`.

## Slow Start



## Quick Shutdown & Cleanup

```
cd ~/bosh-lite-tutorial/bosh-lite
vagrant destroy
```

If you want to boot bosh-lite back up, and re-deploy Cloud Foundry, then run the `curl | bash` script above.

If you never want to boot bosh-lite ever again, then delete the root folder of the tutorial:

```
rm -rf ~/bosh-lite-tutorial
```
