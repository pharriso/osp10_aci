# Red Hat OSP10 Integration with ACI 2.2(2e)

These templates can be used to deploy OSP10 with ACI in unified mode. This deploys a very basic overcloud to just prove ACI integration with RHOSP10.

## Pre-Reqs

Install undercloud. Standard install with nothing exciting.

Obtain OpenStack Director support files and ACI Plugin tar file from Cisco website - https://software.cisco.com/portal/pub/download/portal/select.html?&mdfid=285968390&softwareid=286304714

## Install ACI TripleO patch

Install Cisco tripleo patch rpm on undercloud - We tested with aci-tripleo-patch-10.0-7 

NOTE! There is a problem with the yaml files provided by the rpm (fixed upstream) which means that the ACI config is not re-applied when the
stack is updated. Copy apic_puppet.yaml from the git repo to /opt/aci-tripleo-patch/puppet/ to fix this.

## Modify Overcloud images

Unpack the contents of the Cisco ACI plugin tar file into /home/stack/aci_patch

We also need to download later versions of boost as the versions provided by the Cisco tar file cause dependency issues:

yum install boost-chrono --downloadonly --downloaddir=/tmp/boost

Remove boost* packages from /opt/aci-tripleo-patch and copy in boost packages from /tmp/boost

There is a small python program which we can run that modifies the overcloud images - /opt/aci-tripleo-patch/bin/apply_tripleo_aci_patch.py

The script expects to find our default overcloud images in /home/stack/images

The process modifies the images and then uploads them into glance for us in the undercloud.

## Installation

Copy template files into /home/stack/templates and modify as required. 

Run deployment as per deploy.sh file

## Post Install

Within ACI you should now have a VMM Domain. Using these example templates you should see a VMM called OSP10 (derived from ACIApicSystemId).

From the controller you can validate the creation of the VMM domain:

aimctl manager vmm-domain-find

+-----------+--------+
| type      | name   |
|-----------+--------|
| OpenStack | OSP10  |
+-----------+--------+
