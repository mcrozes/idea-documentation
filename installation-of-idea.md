# Installation of IDEA

## 1-Click installer via Docker

### Pre-Requisite

You must have Docker and AWSCLIv2 installed. Please refer to the links below if you need to install them on your system:

* [ ] Install Docker from the [official Docker website](https://docs.docker.com/get-docker/)
* [ ] Install AWSCLIv2 from the [official AWS website](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* [ ] Ensure your IAM user has the IDEAInstaller policy associated (see policy link)

{% hint style="info" %}
**Docker License:** Please ensure you or your organization adheres to the [Docker Subscription Service Agreement](https://www.docker.com/legal/docker-subscription-service-agreement/). Otherwise you must proceed to a manual installation.
{% endhint %}

If needed, add the following policy to your IAM user. This policy is needed to download the IDEA container from [AWS Elastic Container Registry (ECR)](https://aws.amazon.com/ecr/)

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ecr-public:GetAuthorizationToken",
                "sts:GetServiceBearerToken"
            ],
            "Resource": "*"
        }
    ]
}
```

[Refer to this guide](broken-reference) if you do not already have your AWS IAM user.

### Launch the installer

#### Linux/Mac

```bash
curl -s https://githuburltobeadded | /bin/bash 
```

#### Windows

```
powershell to be added soon
```

IDEA installer will automatically start and validate your environment.&#x20;

{% hint style="info" %}
**Optional**: Add `-p <profile_name>` if you are not using the default profile on your AWSCLIv2.&#x20;

For example append`"-p ideainstaller"` to the command if you are using the IAM user created on the previous page:

`curl -s https://blabla | /bin/bash -p ideainstaller`
{% endhint %}

{% hint style="warning" %}
**Troubleshooting**: [Visit the support page](https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/9OtbmuZYu8NBMrxD7tw5/) in case you need any assistance with this script. Most likely errors will be related to missing binaries (e.g: Docker/AWSCLIv2 are not installed or detected) or inability to download the IDEA container from AWS ECR (refer to the IAM policy mentioned above)
{% endhint %}

![IDEA installer will validate your environment, pull the container from ECR and launch it](<.gitbook/assets/Screen Shot 2022-05-31 at 3.19.40 PM (1).png>)

{% hint style="success" %}
The installer is interactive and you can use the arrows key to choose from the options provided. Options are automatically detected based on your own AWS environment
{% endhint %}

![Simply use the arrow keys to select your choice. Options are automatically detected based on your environment](<.gitbook/assets/Screen Shot 2022-06-01 at 9.50.17 PM.png>)

#### Step 1: AWS Credentials and Region

| AWS Profile   | Select the IAM profile you want to use. Make sure you select an IAM user with enough privileges (see Pre-Requisite above) |
| ------------- | ------------------------------------------------------------------------------------------------------------------------- |
| AWS Partition | Select the AWS partition.  AWS Standard is recommended unless you have a business need to use GovCloud/China              |
| AWS Region    | Select the AWS region you want to use for your IDEA cluster.                                                              |

#### Step2: Cluster Settings

| Cluster Name                | The name of your IDEA cluster                                                                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Administrator Email Address | Email address of the first IDEA user (with admin privileges). Login information will be sent to this email address for first login.                           |
| VPC Cidr Block              | VPC CIDR to create                                                                                                                                            |
| SSH Keypair                 | SSH key to connect to the clusters as root                                                                                                                    |
| Cluster Access              | IP address or Prefix List from where you are planning to access IDEA. This can be changed later                                                               |
| ALB Access                  | Choose whether you want to have your IDEA entrypoint deployed in a public or private subnet                                                                   |
| ACM Certificate             | Choose if you want to import an existing SSL certificate for your HTTPS endpoint or create a self-signed. Domain and HTTPS certificates can be changed later  |
| Directory Provider          | Choose whether you want to use OpenLDAP or AWS Managed AD as directory service                                                                                |

{% hint style="info" %}
Make sure to use a valid email address, otherwise you will never receive the login information to access your IDEA cluster
{% endhint %}

### Step 3: Application Settings

| Base OS                                        | Choose the type of OS to provision by default. This can be changed later                                                         |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| (HPC) Scheduler Instance Type                  | Select the EC2 instance type to provision for the scheduler host. This can be changed later                                      |
| (HPC) Scheduler Volume Size                    | Select the amount of storage to provision on the scheduler host                                                                  |
| (VDI) Virtual Desktop Controller Instance Type | Select the EC2 instance type to provision for the VDI controller. This can be changed later                                      |
| (VDI) Virtual Desktop Controller Volume Size   | Select the amount of storage to provision on the VDI controller host                                                             |
| (VDI) Session Life                             | Select the default time for which the DCV credential token will be valid. Customer will have to re-generate the token if expired |

{% hint style="info" %}
Some options might not be available based on your deployment. For example you won't see the Virtual Desktops option if you decide to only deploy the HPC module
{% endhint %}

IDEA installer will generate a .yaml file with all your installation parameters.

![](<.gitbook/assets/Screen Shot 2022-06-03 at 4.00.36 PM.png>)

You can reference this file if you want to skip all the installer wizard via the `--config / -c` parameter:

```bash
idea_installer.sh --config /root/.idea/clusters/idea-test/us-east-1/user_config.yml
```

IDEA installer will automatically validate your configuration and ensure everything is setup correctly on your AWS environment before proceeding to the installation

![IDEA will validate the AWS environment and your settings.](<.gitbook/assets/Screen Shot 2022-06-03 at 4.05.43 PM.png>)

### Deployment & Installation

Deployment and installation is automated and will take about 1 hour. You can access all the deployment logs in the console.

![](<.gitbook/assets/Screen Shot 2022-06-03 at 4.08.49 PM.png>)
