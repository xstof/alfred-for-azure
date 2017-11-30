# Alfred workflows for Microsoft Azure

This repository contains [Alfred](https://www.alfredapp.com) workflows for working with the Microsoft Azure cloud platform.

![Gif Animation - Alfred for Azure](./images/alfred-for-azure.gif)

## Installation

- Install the Azure CLI
- Login using `az login` 
- Import the workflow into Alfred by opening the `.workflow` file within `/workflow`

## Usage

- Launch Alfred and type `azg` (which is shorthand for "Azure Resource Group").
- Choose your resource group (note that only resource groups from your current active subscription in the `az` command line are shown)
- Choose the action to run within that resource group

### Opening a Resource Group in the Azure Portal

My favorite Alfred shortcut: quickly lookup your Resource Group and hit "open in portal".  No more hunting around: your browser will show up and all your resources will be shown without detours. 

### Stop Virtual Machines

The "Stop vm's" action deallocates all virtual machines in the chosen resource group.  Deallocation of VM's prevent their compute time from being charged for.  It does so by essentially running the following command line:

~~~sh
az vm list -g ${rgname} -o tsv --query="[].name" | xargs -L1 /usr/local/bin/az vm deallocate -g ${rgname} --no-wait -n
~~~

### Start Virtual Machines

The "Start vm's" action starts all virtual machines in the chosen resource group.  It does so by means of the following command line:

~~~sh
az vm list -g ${rgname} -o tsv --query="[].name" | xargs -L1 /usr/local/bin/az vm start -g ${rgname} --no-wait -n
~~~

### Restart Virtual Machines

Handy in case you'd like to do a quick reboot of the virtual machines in your resource group.  They'll be rebooted like this:

~~~sh
az vm list -g ${rgname} -o tsv --query="[].name" | xargs -L1 /usr/local/bin/az vm restart -g ${rgname} --no-wait -n
~~~

### Waiting for a Deployment to finish

For long running deployments it can come handy to have Alfred warn you when a deployment finished.  It will do so by giving you a notification.  Just pick your resource group and the name of the deployment to watch.

### Deleting a Resource Group

Use with care.  Deletes the Resource Group which has been chosen.  Will _NOT_ come and ask for permission nor forgiveness.  If deemed to dangerous, just remove it from the workflow within Alfred settings.