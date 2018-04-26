# Red Hat OSP10 Integration with ACI 2.2(2e)

These templates can be used to deploy OSP10 with ACI in unified mode

## Pre-Reqs

Install undercloud. Standard install with nothing exciting

--- Install ACI TripleO patch ---


Install Cisco tripleo patch rpm on undercloud - We tested with aci-tripleo-patch-10.0-7 

NOTE! There is a problem with the yaml files provided by the rpm (fixed upstream) which means that the ACI config is not re-applied when the
stack is updated. Copy apic_puppet.yaml from the git repo to /opt/aci-tripleo-patch/puppet/ to fix this.

--- Modify Overcloud images ---

Download the plugin tar file from Cisco as well and unpack the contents into /home/stack/aci_patch

* Need to document what I did to ensure agent-ovs was installed in overcloud images *

There is a small python program which modifies the overcloud images - /opt/aci-tripleo-patch/bin/apply_tripleo_aci_patch.py

The script expects to find our default overcloud images in /home/stack/images

The process modifies the images and then uploads them into glance for us in the undercloud.

## Installation

Copy template files into /home/stack/templates

Run deployment as per deploy.sh file
