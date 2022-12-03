# Standard Installation

{% hint style="warning" %}
Review [pre-requisites.md](pre-requisites.md "mention") section first
{% endhint %}

## Install IDEA from scratch

{% tabs %}
{% tab title="Linux/Mac" %}
**Option1: Automatic installation**&#x20;

Copy/Paste this command on your terminal to launch the installation

```
curl https://raw.githubusercontent.com/awslabs/integrated-digital-engineering-on-aws/main/idea-admin.sh -o ~/idea-admin.sh \
  && /bin/bash ~/idea-admin.sh quick-setup
```

**Option2: Download the installer and execute it manually**

As an alternative, you can download `idea-admin.sh` via this [https://raw.githubusercontent.com/awslabs/integrated-digital-engineering-on-aws/main/idea-admin.sh](https://raw.githubusercontent.com/awslabs/integrated-digital-engineering-on-aws/main/idea-admin.sh) and execute it on your Linux or Mac environment via `/bin/bash idea-admin.sh quick-setup`&#x20;
{% endtab %}

{% tab title="Windows" %}
Installation of IDEA in Windows is managed by Powershell. Download and execute the following script on your local Windows workstation: [https://raw.githubusercontent.com/awslabs/integrated-digital-engineering-on-aws/main/idea-admin.ps1  ](https://raw.githubusercontent.com/awslabs/integrated-digital-engineering-on-aws/main/idea-admin.ps1)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
./idea-admin.sh utility offers many features. Refer to [idea-cli-utility.md](../../developer-portal/idea-cli-utility.md "mention") for more details.
{% endhint %}

Follow the installation wizard to install IDEA. During the installation, you will be prompted for various AWS parameters. `quick-setup` proceed to a brand new deployment, if you are looking to re-use existing AWS resources running on your AWS environment (VPC, subnets ..), refer to [Broken link](broken-reference "mention").

{% hint style="warning" %}
Run `export IDEA_ADMIN_AWS_CREDENTIAL_PROVIDER=Ec2InstanceMetadata` prior to `quick-setup` if you are planning to authenticate to AWS using IAM role and not IAM user.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-13 at 10.32.32 AM.png" alt=""><figcaption><p>Installation of IDEA is managed by a wizard.</p></figcaption></figure>

Installation will take anywhere between 30 to 80 minutes depending the module(s) you are planning to install. Connection strings will be displayed at the end of the installation

```
checking endpoint status for cluster: idea-test, url: https://idea-test-external-alb-1837136939.us-east-2.elb.amazonaws.com ...
+------------------------------+---------------------------------------------------------------------------------------------------+---------+
| Module                       | Endpoint                                                                                          | Status  |
+------------------------------+---------------------------------------------------------------------------------------------------+---------+
| Cluster Manager              | https://idea-test-external-alb-1837136939.us-east-2.elb.amazonaws.com/cluster-manager             | SUCCESS |
| eVDI                         | https://idea-test-external-alb-1837136939.us-east-2.elb.amazonaws.com/vdc                         | SUCCESS |
| Scale-Out Computing          | https://idea-test-external-alb-1837136939.us-east-2.elb.amazonaws.com/scheduler                   | SUCCESS |
| OpenSearch Service Dashboard | https://idea-test-external-alb-1837136939.us-east-2.elb.amazonaws.com/_dashboards/                | SUCCESS |
+------------------------------+---------------------------------------------------------------------------------------------------+---------+
```

## Install IDEA using existing AWS resources

As an alternative, you can install IDEA using existing resources running on your AWS environment such as VPC, Subnets, Filesystems, OpenSearch clusters and more.&#x20;

To enable this mode, use `--existing-resources` as shown below:

```bash
./idea-admin.sh quick-setup --existing-resources
```

IDEA installer will guide you through the entire process during Step 3

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-23 at 11.30.02 AM.png" alt=""><figcaption><p>Select the resources you want to re-use via the installer CLI interface</p></figcaption></figure>

## Change default installation parameters

IDEA is 100% customizable and because of that, we cannot provide all the options via a single wizard. Follow these steps if you want to change the default parameters for any modules.

1 - Proceed to the regular Installation via `idea-admin.sh quick-setup`

2 - Pause when the installer prompt you with **"Are you sure you want to update cluster settings db with above configuration from local file system?"**

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-15 at 2.05.19 PM.png" alt=""><figcaption><p>Pause your installation and open a new terminal to edit the configuration</p></figcaption></figure>

At this point, open a new terminal and navigate to `` ~/.idea/clusters/<YOUR_CLUSTER_NAME>/<YOUR_REGION>/` ``&#x20;

You should see `` `values.yml` `` and a `` `config` `` folder. `config` contains all settings which are module specific

```bash
ls ~/.idea/clusters/idea-test/us-east-2/config/
analytics		directoryservice	metrics
bastion-host		global-settings		scheduler
cluster			idea.yml		shared-storage
cluster-manager		identity-provider	vdc
```

3 - Update your parameter(s)

In this example, we will show you how to update the default parameter named "vdc.dcv\_session.allowed\_instance\_family\_types" (this parameter control what type of EC2 instance can be provisioned as virtual desktops by the end users).

The screen below (generated during `idea-admin.sh quick-setup` ) reports virtual desktops can only use t3 or g4dn instance type.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-15 at 1.49.09 PM.png" alt=""><figcaption><p>Default parameters showing only 2 instance types being allowed for virtual desktops</p></figcaption></figure>

To update this parameter, edit  \``` ~/.idea/clusters/<YOUR_CLUSTER_NAME>/<YOUR_REGION>/config/vdc/settings.yml` `` and update the relevant key.

![](<../../.gitbook/assets/Screen Shot 2022-11-15 at 1.56.33 PM.png>)

4 - Reload the installer configuration

&#x20;Go back to your IDEA installer and choose "Reload Changes"

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-15 at 1.57.28 PM.png" alt=""><figcaption><p>Reload the configuration after making your changes</p></figcaption></figure>

5 - Validate the installer will now use your new parameter(s)

Reloading configuration should take less than 5 seconds. Once done, validate your parameter(s) is/are have been successfully updated. The screen below confirm "vdc.dcv\_session.allowed\_instance\_family\_types" has been correctly updated with my changes.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-15 at 1.58.23 PM.png" alt=""><figcaption><p>IDEA will now configure the environment with your changes</p></figcaption></figure>

6 - Continue the installation

You can now move forward with the installer by choosing "Yes".

{% hint style="info" %}
IDEA parameters always use the same syntax: module.section.key.

Example:&#x20;

If you want to update **scheduler.ec2.enable\_detailed\_monitoring,** you will have to edit `` \~/.idea/clusters/\<YOUR\_CLUSTER\_NAME>/\<YOUR\_REGION>/config/**scheduler**/settings.yml and find `enable_detailed_monitoring` key within the `ec2` section.
{% endhint %}
