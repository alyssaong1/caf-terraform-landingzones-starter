# Deploying Enterprise-Scale Landing Zones in Multi Subscription Mode

TODO: Find the page that describes what is deployed from L0-4

- Level 0 - launchpad, state for the different teams' landing zones
- Level 1 - policies, eslz
- Level 2 - core networking layer (virtual wan and virtual hubs) shared by teams
- level 3 - subscriptions and networking components (like virtual network) for projects and teams in the organization
- level 4 - infrastructure at the application layer, usually managed by individual team SREs 

We will first deploy the shared platform components from level 0 to 2. Once the platform has been deployed, the next scenario is where you will provision the Azure Subscriptions/Landing Zones for the individual teams and/or projects in the organization in level 3. We call this the Azure Vending Machine layer, as it is a process that takes in the configuration values for the project/team and generates the infrastructure as code to be deployed. 

In most cases, the organization's platform team would manage levels 0-3. Individual teams would make their own resource customizations and additions on level 4, based on their project requirements. 

## Creating your first Azure Subscription manually

Assuming an empty Azure AD tenant​, you will need to create an Azure subscription manually first to support the subscription delegation to the service principals. You will need Account Owner role on the Enterprise Agreement enrollment. Skip this step if you already have an Azure Subscription in your tenant. 

Go to the [Azure Portal](portal.azure.com) and login as the EA account owner. Create an Azure subscription from the portal.

![CreateSubscription](./img/create-subscription-portal.PNG)

Name the subscription and keep the EA owner as the subscription owner.

![SubscriptionOwner](./img/subscription-owner.PNG)

Wait for the subscription creation.

![SubscriptionCreationSuccess](./img/create-subscription-success.PNG)


## Use Rover to authenticate with Azure

You must be authenticated with Azure first, run the following command in the terminal and follow the instructions. You will be prompted to login to your Azure account via the browser.

```bash
# Find tenant id or primary domain with the following instructions: https://docs.microsoft.com/en-us/partner-center/find-ids-and-domain-names
rover login -t <tenant id or primary domain> 
```

Rover will echo back the subscription selected by default for your environment. This should be your launchpad subscription (the one you just created). If this is not the right subscription, modify it using the following command:

```bash
az account list # Get a full list of subscriptions you own, to find the right subscription ID
az account set --subscription <subscription_GUID>
```


## Generate and Deploy the Platform Layers (Level 0-2)

You can find all the infrastructure configuration values for the platform in [the following folder](/orgs/contoso/multi-sub/platform). You will first need to replace some of the configuration with your own values. Navigate to the [demo.caf.platform.yaml](/orgs/contoso/multi-sub/platform/demo.caf.platform.yaml) file. 

For instance, replace the following block in the yaml file with your own details:
```yaml
primary_subscription_details:
      subscription_id: set_subscription_guid
      subscription_name: set_subscription_name
      tenant_id: set_tenant_id
```

Then, run the following commands to generate the CAF configurations for the platform landing zones:

```bash
cd /tf/caf/starter/templates/platform

rover ignite \
  --playbook /tf/caf/starter/templates/platform/ansible.yaml \
  -e base_templates_folder=/tf/caf/starter/templates \
  -e config_folder=/tf/caf/orgs/contoso/multi-sub/platform \
  -e config_folder_asvm=/tf/caf/orgs/contoso/multi-sub/asvm \
  -e scenario=demo \
  -e boostrap_launchpad=false \
  -e deploy_subscriptions=false

```

TO CHECK:
- Is there documentation for rover ignite command?

Running the above `rover ignite` command uses the values in your configuration yaml files to generate the CAF IaC for the platform landing zones, by populating the templates with your specified configuration values. You can find the platform templates at [starter/templates/platform](/starter/templates/platform).

**Why separate out the configuration values?**
- Reason 1
- Reason 2

Once the command has run, your generated platform configuration files will be found [here](/configuration/contoso/demo/multi-sub/platform). You can execute the steps in the [level 0/launchpad readme](/configuration/contoso/demo/multi-sub/platform/level0/launchpad/readme.md) as the starting point to begin deploying the CAF launchpad.

### Updating the platform configurations

Should you need to make modifications to the platform configurations in future, you can ... and then re-run the `rover ignite` command to regenerate the CAF config files. 

## Next Step
Once you have deployed all the platform components, head over to [this doc](../asvm/README.md) to learn how to deploy the ASVM levels (3 and 4). 


### Generating the Azure Subscription Vending Machine (ASVM) configurations

```bash
cd /tf/caf/starter/templates/asvm

rover ignite \
  --playbook /tf/caf/starter/templates/platform/ansible.yaml \
  -e base_templates_folder=/tf/caf/starter/templates \
  -e config_folder=/tf/caf/orgs/contoso/multi-sub/platform \
  -e config_folder_asvm=/tf/caf/orgs/contoso/multi-sub/asvm \
  -e scenario=demo \
  -e boostrap_launchpad=false \
  -e deploy_subscriptions=false
```
