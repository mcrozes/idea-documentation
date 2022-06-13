---
layout: editorial
---

# What is IDEA?

Integrated Design Engineering on AWS (**IDEA**) is a solution that helps customers more easily deploy and operate a multiuser environment for computationally intensive workflows. The solution features a large selection of compute resources; fast network backbone; unlimited storage; and budget and cost management directly integrated within AWS. The solution also deploys a user interface (UI) and automation tools that allows you to create your own queues, scheduler resources, Amazon Machine Images (AMIs), software, and libraries. This solution is designed to provide a production ready reference implementation to be a starting point for deploying an AWS environment to run scale-out workloads, allowing you to focus on running simulations designed to solve complex computational problems.\


## How to get started

Refer to this guide if you do not have any prior experience with IDEA. We will show you how to install your first cluster and interact with it:

{% content-ref url="https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/M6r7H9BSuy4gJL3V4I0K/" %}
[Install IDEA](https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/M6r7H9BSuy4gJL3V4I0K/)
{% endcontent-ref %}

IDEA is a framework on which you can deploy multiple independent modules based on your own requirements. Select the module below to learn more.&#x20;

{% hint style="info" %}
* You can add module(s) on an existing IDEA clusters
* Web Interface & Directory Services are mandatory and will be deployed on any new IDEA environment
{% endhint %}

{% content-ref url="https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/QthiamUzKn8KJLl0hYBf/" %}
[Virtual Desktop Interface](https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/QthiamUzKn8KJLl0hYBf/)
{% endcontent-ref %}

{% content-ref url="https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/LGamNPuOYtjAP3GFfRJO/" %}
[Scale-Out Workloads (HPC Simulations)](https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/LGamNPuOYtjAP3GFfRJO/)
{% endcontent-ref %}

{% content-ref url="https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/2fYhGrE1N98BfOc1T8n3/" %}
[Directory Services (LDAP or Active Directory)](https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/2fYhGrE1N98BfOc1T8n3/)
{% endcontent-ref %}

&#x20;Refer to the Support page in case you need any sort of assistance

{% content-ref url="https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/9OtbmuZYu8NBMrxD7tw5/" %}
[Support](https://app.gitbook.com/o/ewXgnQpSEObr0Vh0WSOj/s/9OtbmuZYu8NBMrxD7tw5/)
{% endcontent-ref %}

## Key Features <a href="#easy-installation" id="easy-installation"></a>

### Easy installation[¶](https://awslabs.github.io/scale-out-computing-on-aws/#easy-installation) <a href="#easy-installation" id="easy-installation"></a>

[Installation of your Scale-Out Computing on AWS cluster](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/install-soca-cluster/) is fully automated and managed by CloudFormation

{% hint style="info" %}
Did you know?

* You can have multiple Scale-Out Computing on AWS clusters on the same AWS account
* Scale-Out Computing on AWS comes with a list of unique tags, making resource tracking easy for AWS Administrators
{% endhint %}

### Access your cluster in 1 click[¶](https://awslabs.github.io/scale-out-computing-on-aws/#access-your-cluster-in-1-click) <a href="#access-your-cluster-in-1-click" id="access-your-cluster-in-1-click"></a>

You can [access your Scale-Out Computing on AWS cluster](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/access-soca-cluster/) either using DCV (Desktop Cloud Visualization)[1](https://awslabs.github.io/scale-out-computing-on-aws/#fn:1) or through SSH.

### Simple Job Submission[¶](https://awslabs.github.io/scale-out-computing-on-aws/#simple-job-submission) <a href="#simple-job-submission" id="simple-job-submission"></a>

Scale-Out Computing on AWS [supports a list of parameters designed to simplify your job submission on AWS](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/integration-ec2-job-parameters/). Advanced users can either manually choose compute/storage/network configuration for their job or simply ignore these parameters and let Scale-Out Computing on AWS picks the most optimal hardware (defined by the HPC administrator)

```bash
# Advanced Configuration
user@host$ qsub -l instance_type=c5n.18xlarge \
    -l instance_ami=ami-123abcde
    -l nodes=2 
    -l scratch_size=300 
    -l efa_support=true
    -l spot_price=1.55 myscript.sh

# Basic Configuration
user@host$ qsub myscript.sh
```

{% hint style="info" %}
* [Check our Web-Based utility to generate you submission command](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/job-configuration-generator/)
* [Refer to this page for tutorial and examples](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/launch-your-first-job/)
* [Refer to this page to list all supported parameters](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/integration-ec2-job-parameters/)
* Jobs can also be submitted [via HTTP API](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/control-hpc-job-with-http-web-rest-api/) or [via web interface](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/submit-hpc-jobs-web-based-interface/)
{% endhint %}

### OS agnostic and support for custom AMI[¶](https://awslabs.github.io/scale-out-computing-on-aws/#os-agnostic-and-support-for-custom-ami) <a href="#os-agnostic-and-support-for-custom-ami" id="os-agnostic-and-support-for-custom-ami"></a>

Customers can integrate their Centos7/Rhel7/AmazonLinux2 AMI automatically by simply using `-l instance_ami=<ami_id>` at job submission. There is no limitation in term of AMI numbers (you can have 10 jobs running simultaneously using 10 different AMIs). SOCA supports heterogeneous environment, so you can have concurrent jobs running different operating system on the same cluster.

{% hint style="warning" %}
**AMI using OS different than the scheduler**

In case your AMI is different than your scheduler host, you can specify the OS manually to ensure packages will be installed based on the node distribution.

In this example, we assume your Scale-Out Computing on AWS deployment was done using AmazonLinux2, but you want to submit a job on your personal RHEL7 AMI

```bash
user@host$ qsub -l instance_ami=<ami_id> \
                -l base_os=rhel7 myscript.sh
```
{% endhint %}

### Web User Interface[¶](https://awslabs.github.io/scale-out-computing-on-aws/#web-user-interface) <a href="#web-user-interface" id="web-user-interface"></a>

Scale-Out Computing on AWS includes a simple web ui designed to simplify user interactions such as:

* [Start/Stop DCV sessions in 1 click](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/access-soca-cluster/#graphical-access-using-dcv)
* [Download private key in both PEM or PPK format](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/access-soca-cluster/#ssh-access)
* [Check the queue and job status in real-time](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/manage-ldap-users/)
* [Add/Remove LDAP users](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/manage-ldap-users/)
* [Access the analytic dashboard](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/my-activity/)
* [Access your filesystem](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/my-files/)
* [Understand why your jobs are stuck in the queue](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/my-job-queue/#understand-why-your-job-cannot-start)
* [Create Application profiles and let your users submit job directly via the web interface](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/submit-hpc-jobs-web-based-interface/)

### HTTP Rest API[¶](https://awslabs.github.io/scale-out-computing-on-aws/#http-rest-api) <a href="#http-rest-api" id="http-rest-api"></a>

Users can submit/retrieve/delete jobs [remotely via an HTTP REST API](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/control-hpc-job-with-http-web-rest-api/)

### Budgets and Cost Management[¶](https://awslabs.github.io/scale-out-computing-on-aws/#budgets-and-cost-management) <a href="#budgets-and-cost-management" id="budgets-and-cost-management"></a>

You can [review your HPC costs](https://awslabs.github.io/scale-out-computing-on-aws/budget/review-hpc-costs/) filtered by user/team/project/queue very easily using AWS Cost Explorer.

Scale-Out Computing on AWS also supports AWS Budget and [let you create budgets](https://awslabs.github.io/scale-out-computing-on-aws/budget/set-up-budget-project/) assigned to user/team/project or queue. To prevent over-spend, Scale-Out Computing on AWS includes hooks to restrict job submission when customer-defined budget has expired.

Lastly, Scale-Out Computing on AWS let you create queue ACLs or instance restriction at a queue level. [Refer to this link for all best practices in order to control your HPC cost on AWS and prevent overspend](https://awslabs.github.io/scale-out-computing-on-aws/budget/prevent-overspend-hpc-cost-on-aws-soca/).

### Detailed Cluster Analytics[¶](https://awslabs.github.io/scale-out-computing-on-aws/#detailed-cluster-analytics) <a href="#detailed-cluster-analytics" id="detailed-cluster-analytics"></a>

Scale-Out Computing on AWS [includes OpenSearch (formerly Elasticsearch) and automatically ingest job and hosts data](https://awslabs.github.io/scale-out-computing-on-aws/analytics/monitor-cluster-activity/) in real-time for accurate visualization of your cluster activity.

Don't know where to start?

Scale-Out Computing on AWS [includes dashboard examples](https://awslabs.github.io/scale-out-computing-on-aws/analytics/build-kibana-dashboards/) if you are not familiar with OpenSearch (formerly Elasticsearch) or Kibana.

### 100% Customizable[¶](https://awslabs.github.io/scale-out-computing-on-aws/#100-customizable) <a href="#100-customizable" id="100-customizable"></a>

Scale-Out Computing on AWS is built entirely on top of AWS and can be customized by users as needed. Most of the logic is based of CloudFormation templates, shell scripts and python code. More importantly, the entire Scale-Out Computing on AWS codebase is open-source and [available on Github](https://github.com/awslabs/scale-out-computing-on-aws).

### Persistent and Unlimited Storage[¶](https://awslabs.github.io/scale-out-computing-on-aws/#persistent-and-unlimited-storage) <a href="#persistent-and-unlimited-storage" id="persistent-and-unlimited-storage"></a>

Scale-Out Computing on AWS includes two unlimited EFS storage (/apps and /data). Customers also have the ability to deploy high-speed SSD EBS disks or FSx for Lustre as scratch location on their compute nodes. [Refer to this page to learn more about the various storage options](https://awslabs.github.io/scale-out-computing-on-aws/storage/backend-storage-options/) offered by Scale-Out Computing on AWS

### Centralized user-management[¶](https://awslabs.github.io/scale-out-computing-on-aws/#centralized-user-management) <a href="#centralized-user-management" id="centralized-user-management"></a>

Customers [can create unlimited LDAP users and groups](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/manage-ldap-users/). By default Scale-Out Computing on AWS includes a default LDAP account provisioned during installation as well as a "Sudoers" LDAP group which manage SUDO permission on the cluster.

### Automatic backup[¶](https://awslabs.github.io/scale-out-computing-on-aws/#automatic-backup) <a href="#automatic-backup" id="automatic-backup"></a>

Scale-Out Computing on AWS [automatically backup your data](https://awslabs.github.io/scale-out-computing-on-aws/security/backup-restore-your-cluster/) with no additional effort required on your side.

### Support for network licenses[¶](https://awslabs.github.io/scale-out-computing-on-aws/#support-for-network-licenses) <a href="#support-for-network-licenses" id="support-for-network-licenses"></a>

Scale-Out Computing on AWS [includes a FlexLM-enabled script which calculate the number of licenses](https://awslabs.github.io/scale-out-computing-on-aws/tutorials/job-licenses-flexlm) for a given features and only start the job/provision the capacity when enough licenses are available.

### Automatic Errors Handling[¶](https://awslabs.github.io/scale-out-computing-on-aws/#automatic-errors-handling) <a href="#automatic-errors-handling" id="automatic-errors-handling"></a>

Scale-Out Computing on AWS performs various dry run checks before provisioning the capacity. However, it may happen than AWS can't fullfill all requests (eg: need 5 instances but only 3 can be provisioned due to capacity shortage within a placement group). In this case, Scale-Out Computing on AWS will try to provision the capacity for 30 minutes. After 30 minutes, and if the capacity is still not available, Scale-Out Computing on AWS will automatically reset the request and try to provision capacity in a different availability zone. [To simplify troubleshooting, all these errors are reported on the web interface](https://awslabs.github.io/scale-out-computing-on-aws/web-interface/my-job-queue/#understand-why-your-job-cannot-start)

### Custom fair-share[¶](https://awslabs.github.io/scale-out-computing-on-aws/#custom-fair-share) <a href="#custom-fair-share" id="custom-fair-share"></a>

Each user is given a score which vary based on:

* Number of job in the queue
* Time each job is queued
* Priority of each job
* Type of instance

Job that belong to the user with the highest score will start next. Fair Share is is configured at the queue level (so you can have one queue using FIFO and another one Fair Share)

### And more ...[¶](https://awslabs.github.io/scale-out-computing-on-aws/#and-more) <a href="#and-more" id="and-more"></a>

Refer to the various sections (tutorial/security/analytics ...) to learn more about this solution
