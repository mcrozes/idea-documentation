# FAQ

## Cluster Management

### How do I safe-list a new IP to access my IDEA environment

<details>

<summary>Answer</summary>

To safelist a new IP, navigate to VPC > Managed Prefix List and add your new entry into the Prefix List created by IDEA.

Alternatively, you can run the following `idea-admin.sh` command:

```
./idea-admin.sh utils cluster-prefix-list add-entry
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION> \
  --cidr x.x.x.x/x \ 
  --description '<DESCRIPTION>'
```

</details>

### I never received the welcome email after installing IDEA, how can I create an admin user?

<details>

<summary>Answer</summary>

Run the following commands to create a new admin user via IDEA APIs

<pre class="language-bash"><code class="lang-bash">IDEA_ADMIN_USER="username"
IDEA_ADMIN_USER_PASSWORD="password"
IDEA_USER_EMAIL_ADDRESS="email_address"
IDEA_CLUSTER_NAME="idea-xxx"
IDEA_DEPLOYMENT_REGION="region where you deployed IDEA"

# Retrieve Client ID
CLIENT_ID_ARN=$(./idea-admin.sh config show \
--query "cluster-manager.client_id" \
--cluster-name $IDEA_CLUSTER_NAME \
--aws-region $IDEA_DEPLOYMENT_REGION \
--format raw)

CLIENT_ID=$(aws secretsmanager get-secret-value --secret-id $CLIENT_ID_ARN --query "SecretString" --output text --region $IDEA_DEPLOYMENT_REGION)# Retrieve Client Secret

# Retrieve Client secret
CLIENT_SECRET_ARN=$(./idea-admin.sh config show \
--query "cluster-manager.client_secret" \
--cluster-name $IDEA_CLUSTER_NAME \
--aws-region $IDEA_DEPLOYMENT_REGION \
--format raw)

CLIENT_SECRET=$(aws secretsmanager get-secret-value --secret-id $CLIENT_SECRET_ARN --query "SecretString" --output text --region $IDEA_DEPLOYMENT_REGION)

# Retrieve Cognito URL
COGNITO_USER_POOL=$(./idea-admin.sh config show \
--query "identity-provider.cognito.domain_url" \
--cluster-name $IDEA_CLUSTER_NAME \
--aws-region $IDEA_DEPLOYMENT_REGION \
--format raw)

# Retrieve ALB endpoint
IDEA_ALB=$(./idea-admin.sh config show \
--query "cluster.load_balancers.external_alb.load_balancer_dns_name" \
--cluster-name $IDEA_CLUSTER_NAME \
--aws-region $IDEA_DEPLOYMENT_REGION \
--format raw)

# Generate Authorization Header (remove -w 0 if using Mac)
AUTHORIZATION_HEADER=$(echo -n $CLIENT_ID:$CLIENT_SECRET | base64 -w 0)
<strong>
</strong><strong># Request Bearer
</strong>curl --silent --insecure --location --request POST "$COGNITO_USER_POOL/oauth2/token" \
--header "Authorization: Basic $AUTHORIZATION_HEADER" \
--header "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "scope=cluster-manager/read cluster-manager/write" > .bearer

# Bearer output is stored as text file in order to use -r. File is removed shortly after
BEARER=$(cat .bearer | jq -r ".access_token")

rm -rf .bearer
<strong>
</strong><strong># Create Admin User
</strong>curl --silent --insecure --location --request POST "https://$IDEA_ALB/cluster-manager/api/v1" \
--header "Authorization: Bearer $BEARER" \
--header "Content-Type: application/json" \
--data-raw '{
"header": {
"namespace": "Accounts.CreateUser"
},
"payload": {
"user": {
"username": "'$IDEA_ADMIN_USER'",
"password": "'$IDEA_ADMIN_USER_PASSWORD'",
"email": "'$IDEA_USER_EMAIL_ADDRESS'",
"additional_groups": ["managers-cluster-group", "administrators-cluster-group]
},
"email_verified": true
}
}'
</code></pre>

</details>

### How do I patch/update an IDEA module

<details>

<summary>Answer</summary>

See [patch-idea-module-idea-admin.sh-patch.md](../first-time-users/cluster-operations/update-idea-cluster/patch-idea-module-idea-admin.sh-patch.md "mention")

</details>

### How do I uninstall IDEA?

<details>

<summary>Answer</summary>

See [uninstall-idea.md](../first-time-users/cluster-operations/uninstall-idea.md "mention")

</details>

### **How to customize the logo/title or subtitle of my IDEA environment**

<details>

<summary><strong>Answer</strong></summary>

The logo, title and subtitle of the Web Portal can be customized using configurations.

![](https://confluence.amazon.com/download/attachments/108564578/Screen%20Shot%202022-07-11%20at%207.49.14%20AM.png?version=2\&modificationDate=1657551271000\&api=v2)

### Defaults <a href="#customizelogo-titleandsubtitle-defaults" id="customizelogo-titleandsubtitle-defaults"></a>

* title - Integrated Digital Engineering on AWS (IDEA)
* logo - IDEA Default Logo
* subtitle - \<cluster-name> (\<aws-region>)

### Customization <a href="#customizelogo-titleandsubtitle-customization" id="customizelogo-titleandsubtitle-customization"></a>

#### Logo <a href="#customizelogo-titleandsubtitle-logo" id="customizelogo-titleandsubtitle-logo"></a>

Logo can be customized by uploading appropriate logo file to the cluster's S3 Bucket. Copy the S3 object key and run the below command:

```bash
./idea-admin.sh config \
set Key=cluster-manager.web_portal.logo,Type=string,Value=assets/logo.png \
--cluster-name <CLUSTER_NAME> \
--aws-region <REGION>
```

#### Title <a href="#customizelogo-titleandsubtitle-title" id="customizelogo-titleandsubtitle-title"></a>

Title can be customized by running the below command:

```bash
./idea-admin.sh config \ 
  set "Key=cluster-manager.web_portal.title,Type=string,Value=My Company" \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION>
```

#### Subtitle <a href="#customizelogo-titleandsubtitle-subtitle" id="customizelogo-titleandsubtitle-subtitle"></a>

Subtitle can be customized by running the below command:

```bash
./idea-admin.sh config \
  set "Key=cluster-manager.web_portal.subtitle,Type=string,Value=R&D Cluster" \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION>


```

</details>



## Logs

### Where are the application logs stored?

<details>

<summary>Answer</summary>

IDEA modules such as cluster-manager, virtual-desktop-controller and scheduler run a python based application server.

The application server logs are available under: **/opt/idea/app/logs**

All logs will be available in **application.log**. In rare occasions, few logs may be available under **stdout.log**.

Logging can configured per application server using IDEA Cluster Configuration. Below is the logging configuration for cluster-manager:

```
./idea-admin.sh config show \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION> \
  --query "cluster-manager.logging.*"
+-----------------------------------------------+--------------------+---------+
| Key                                           | Value              | Version |
+-----------------------------------------------+--------------------+---------+
| cluster-manager.logging.default_log_file_name | application.log    |    1    |
| cluster-manager.logging.logs_directory        | /opt/idea/app/logs |    1    |
| cluster-manager.logging.profile               | production         |    1    |
+-----------------------------------------------+--------------------+---------+
```

</details>

### Where are the Node Bootstrap logs stored (e.g: module not started correctly)

<details>

<summary>Answer</summary>

Log in to the EC2 machine and check the logs under /root/boostrap/logs.&#x20;

All infrastructure nodes such as directoryservice (openldap-server), scheduler, bastion-host, virtual-desktop controller use a standard directory structure during bootstrap.

</details>

