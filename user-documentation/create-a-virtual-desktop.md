# How to create a virtual desktop

{% hint style="info" %}
To create a new IDEA Virtual Desktop, you must have installed the Virtual Desktop module. You can install the module on an existing IDEA cluster.
{% endhint %}

## Create your Windows or Linux desktop

To access the Virtual Desktop section, click "**Virtual Desktop**" on the left sidebar:

![Access the Virtual Desktop section easily by clicking 'Virtual Desktops" on the left sidebar](<../.gitbook/assets/Screen Shot 2022-06-06 at 3.43.31 PM.png>)

To launch your virtual desktop click <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 6.42.23 PM.png" alt="" data-size="line">button. You will be prompted with a new modal asking you a couple of questions:

* A name for your desktop
* The operating system you want to use from:
  * **Linux**
    * Amazon Linux 2
    * CentOS 7
    * Red Hat Enterprise Linux 7
  * **Windows**:
    * Windows Server 2019
* The software stack to provision. A software stack is an EC2 AMI with pre-installed and pre-configured applications defined by your cluster administrator. Follow this link (TOBEADDED) to learn how to create custom software stack for your team.
* Size of the main EBS partition
* Type of EC2 instance to provision. You will be able to change this value later on without having to re-create a new desktop if needed
* Optional: Advanced option such as enforcing a subnet ID

![Form to complete before creating your own Windows or Linux desktop](<../.gitbook/assets/Screen Shot 2022-06-06 at 3.47.13 PM.png>)

Click<img src="../.gitbook/assets/Screen Shot 2022-06-06 at 6.43.02 PM.png" alt="" data-size="line">button to launch your virtual desktop creation. You will instantly see a new card with your desktop information. Your virtual desktop will be ready within 10-15 minutes. Startup time is based on the image selected, the operating system as well as the instance type. Windows tends to boot faster as some components such as the scheduler or EFS are not installed.

![](<../.gitbook/assets/Screen Shot 2022-06-06 at 3.55.34 PM.png>)

Wait a couple of minutes until your desktop is ready.

{% hint style="success" %}
IDEA automatically detects GPU instances and install the relevant drivers (NVIDIA GRID, NVIDIA Tesla, AMD) automatically
{% endhint %}

## How to access your Windows or Linux desktop

Once your virtual desktop is up and running, you can click the card and connect it either via web or DCV client.&#x20;

<details>

<summary>Easiest: Access your desktop from within your web browser</summary>

Click<img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.08.13 PM.png" alt="" data-size="line">button or click the thumbnail to access your Windows or Linux desktop directly via your browser.

</details>

<details>

<summary>Best Performance: Use DCV Client </summary>

Click <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.07.45 PM.png" alt="" data-size="line"> button to download your `.dcv` file. To open this file, you will need to have the DCV Client installed on your system. Click the <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.09.14 PM.png" alt="" data-size="line"> icon to access to the download link and installation instructions.

</details>

![Example of Linux virtual desktop up and running](<../.gitbook/assets/Screen Shot 2022-06-06 at 4.03.27 PM.png>)

## How to change the schedule of your Windows or Linux desktop

By default, your virtual desktop come with no schedule, which means your desktop will stay up until you stop/terminate it, and will stay stopped until you turn it back on. You can change this behavior by configuring your own scheduling, and IDEA will ensure your desktop will automatically start/stop based on your own requirements.

{% hint style="info" %}
Virtual Desktop will only be stopped if idle (e.g: no active session connected and CPU usage below 15% for at least 15 minutes). This is meant to prevent accidental stop and ensure you won't have to worry if you have a simulation running on your desktop overnight but have configured auto-stop after 8PM
{% endhint %}

You can at any moment review whether or not you have a schedule configured on your desktop by checking the top bar of your session (note: schedule are unique to each desktop)![](<../.gitbook/assets/Screen Shot 2022-06-06 at 4.21.15 PM.png>)

To create/edit a schedule, click <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.22.16 PM.png" alt="" data-size="line">then <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.22.48 PM.png" alt="" data-size="line">This will open a new modal where you will be able to choose the schedule for any given day:

![](<../.gitbook/assets/Screen Shot 2022-06-06 at 4.23.42 PM.png>)

Simply click the dropdown menu to chose your schedule for that day using the different presets below:

| Mode            | Running Desktop                        | Stopped Desktop                                |
| --------------- | -------------------------------------- | ---------------------------------------------- |
| No Schedule     | Stay running until you stop/terminate  | Stay stopped until you manually restart it     |
| Stop All Day    | Will be stopped if idle after 00H      | Will stay stopped                              |
| Started All Day | Will stay running                      | Will be automatically started after 00H        |
| Working Hours   | Will be started at 9 AM                | Will be stopped if idle after 5 PM             |
| Custom Schedule | Will be started based on your own time | Will be stopped if idle based on your own time |

{% hint style="info" %}
Schedule is re-evaluated every 30 minutes
{% endhint %}

## How to change the hardware of your Windows or Linux Desktop&#x20;

First, you must stop your current desktop before being able to upgrade/downgrade the instance type associated to your desktop. To do so, click <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.22.16 PM.png" alt="" data-size="line"> then <img src="../.gitbook/assets/image (4).png" alt="" data-size="line">and <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 6.06.26 PM.png" alt="" data-size="line">

{% hint style="info" %}
Stopping a desktop will not cause any data loss.
{% endhint %}

Wait until the state of your desktop is<img src="../.gitbook/assets/Screen Shot 2022-06-06 at 6.07.46 PM.png" alt="" data-size="line">. Click <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.22.16 PM.png" alt="" data-size="line"> then  <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 6.08.36 PM.png" alt="" data-size="line">and choose your new instance.

{% hint style="info" %}
You can verify what instance type is used by your virtual desktop by checking the desktop setting bar:&#x20;

&#x20;![](<../.gitbook/assets/Screen Shot 2022-06-06 at 6.10.06 PM.png>)
{% endhint %}

Once your instance is changed, restart your desktop by clicking <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 4.22.16 PM.png" alt="" data-size="line">then<img src="../.gitbook/assets/image (5).png" alt="" data-size="line">and  <img src="../.gitbook/assets/Screen Shot 2022-06-06 at 6.11.27 PM.png" alt="" data-size="line">.

## Stop/Terminate/Hibernate your Windows or Linux Desktop

### Stop

Click <img src="../.gitbook/assets/image (6).png" alt="" data-size="line">then  <img src="../.gitbook/assets/Screen Shot 2022-06-07 at 3.26.23 PM.png" alt="" data-size="line">and finally <img src="../.gitbook/assets/image (3).png" alt="" data-size="line">to stop your current virtual desktop session. Stopped session will not suffer any data loss and you can restart a stopped session at any moment.

### Terminate

Click <img src="../.gitbook/assets/image (6).png" alt="" data-size="line">then  <img src="../.gitbook/assets/Screen Shot 2022-06-07 at 3.26.23 PM.png" alt="" data-size="line">and finally <img src="../.gitbook/assets/image.png" alt="" data-size="line">to permanently terminate a virtual desktop  session. Terminating a session may cause data loss if you are using ephemeral storage, so make sure to have uploaded all your data back to IDEA filesystem first.

### Hibernate

{% hint style="info" %}
When you hibernate an instance, your desktop state is saved in memory. When you restart it, all your applications will automatically resume. On the other hand, stopping a virtual desktop is the same as powering off your laptop. Note not all EC2 instances support hibernation.
{% endhint %}

Click <img src="../.gitbook/assets/image (6).png" alt="" data-size="line">then  <img src="../.gitbook/assets/Screen Shot 2022-06-07 at 3.26.23 PM.png" alt="" data-size="line">and finally "Hibernate" to hibernate your current virtual desktop session. Hibernated session will not suffer any data loss and you can restart the session at any moment.&#x20;

## Retrieve Session Information

Click <img src="../.gitbook/assets/image (2).png" alt="" data-size="line">then <img src="../.gitbook/assets/Screen Shot 2022-06-07 at 3.46.07 PM.png" alt="" data-size="line"> to retrieve your session information such as instance type, subnet id, operating system etc ...

![](<../.gitbook/assets/Screen Shot 2022-06-07 at 3.46.51 PM.png>)
