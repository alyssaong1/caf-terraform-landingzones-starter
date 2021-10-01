# Enterprise-Scale Landing Zones using Cloud Adoption Framework with Terraform

Imagine that you are a platform team looking to adopt Azure for your organization, and you would like a set of prescriptive guidance and templates to set up your organization's infrastructure so that it is on a path to sustainable scale from day 1. The purpose of this repository is to provide you with a starting point when composing your Azure landing zones with Cloud Adoption Framework for Terraform. It provides sample files, folder structure, and advice on how to get started creating an Infrastructure as Code DevOps environment.

In this guide, you will setup and deploy your first enterprise-scale landing zones. 

## Foundational Reading 

Before starting, we recommend that you first read through the following to get a foundational understanding of the Cloud Adoption Framework and the Enterprise-Scale Landing Zones:

- [Introduction to core CAF concepts and terminology]() 
- [Overview of Enterprise-Scale](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/) - design principals and architecture of the enterprise scale landing zones. 

### Repositories used

Landing zones for Terraform are composed of multiple open-source components and projects. We will mainly be using 2 repositories when setting up the landing zones. Our approach is to separate the configuration repository and the logic repository:

* **Configuration repository (you are here!)**: this template is an example of configuration repository for CAF landing zones, containing definition of the configuration for your different environments. In real world, this is often separate repositories, but to simplify things, we created a repo with examples containing various environments.
* **Logic repository**: the [Azure CAF landing zone repository](https://github.com/azure/caf-terraform-landingzones)

There are other open source repositories for core Terraform module logic and tooling like Rover as well, but in this starter, you will only need the above 2. 

TO CHECK:
- Landingzones-starter would change its name eventually.
- Need something better to describe caf-terraform-landingzones


## Deploying your Landing Zones

### Prerequisites and Setup

Please go through [this guide](./2-Setup.md) for detailed instructions on how to setup your dev environment. 

Once you have completed the setup, select the option most suited to your scenario to deploy your landing zones:

### Option 1: Multi Subscription Mode
You are part of a platform or SRE team looking to automate and scale out the creation of Azure subscriptions and resources for your internal teams. Note that you will need Account Owner role on the Enterprise Agreement enrollment to run the multi subscription setup.

Go to [Multi-Subscription Mode Setup](./3-MultiSubscription.md)

### Option 2: Standalone Subscription Mode
You don't have an EA and you just want to try out deploying landing zones on a single subscription that you own.

Go to [Standalone Subscription Mode Setup](./4-SingleSubscription.md)

