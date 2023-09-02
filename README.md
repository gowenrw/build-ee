# build-ee ansible playbook and artifacts

A quick and dirty ansible playbook and artifacts to build a local execution environment (for ansible-navigator to consume) and optionally push that ee to a container registry (for AWX/AAP to consume).

This was written to be run from ansible core using ```ansible-playbook``` and not navigator since running this inside a navigator execution environment can be problematic and I didn't want to deal with that.

Note that building your own execution enviornment is optional in case you wish to tweak the contents.
If you just need an EE that contains the collections listed here you could just pull my latest EE build.

I have published this build to docker hub which can be pulled with this podman command:
```
podman pull docker.io/altbier/cloud-creator-awx-ee:latest
```

## Requirements

This code has been refactored to use ansible-builder version 3+ and is not backwards compatable with previous versions due to the configuration file version changes.

## Configuring

These are the configuration files
* [config-ee.yml](./config-ee.yml)
  * This is the main config file that contains all the container settings
  * The galaxy section has a list of ansible collections to install in the ee
  * The python section has a list of Python modules to install in the ee
  * The system section has a list of system level packages to install in the ee
* [build-vars.yml](./build-vars.yml)
  * This is the variable file for the build-ee playbook
  * There are several settings that can be tweaked here including the ee_name
  * The version here is used as a tag when pushing to a registry

## Running

Once the configuration files are set simple execute the build-ee playbook
```
ansible-playbook build-ee.yml
```

Note that I am using ansible core's ```ansible-playbook``` to run this playbook.
This is because we want the execution of this build to stay in the local environment.
If we were to use ansible-navigator it's use of an execution environment would cause issues.

## Environment

This has been tested on CentOS Stream 9 and RHEL 9 systems with the following installed:
* ansible-core >= 2.15.0
* ansible-builder >= 3.0.0
* podman >= 4.6.0
