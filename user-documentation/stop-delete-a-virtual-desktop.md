# Stop/Delete a virtual desktop

To access the Virtual Desktop lifecycle section, click "**Actions**" button associated to the virtual desktop:

<figure><img src="../.gitbook/assets/Screen Shot 2022-10-25 at 2.00.17 PM.png" alt=""><figcaption><p>Control your virtual desktop lifecycle</p></figcaption></figure>

### Stop

Click "**Action**" then "**Virtual Desktop State**"  and finally "**Stop**" to stop your current virtual desktop session. Stopped session will not suffer any data loss and you can restart a stopped session at any moment.

### Terminate

Click "**Action**" then "**Virtual Desktop State**"  and finally "**Terminate**" to permanently terminate a virtual desktop  session.&#x20;

{% hint style="warning" %}
Terminating a session may cause data loss if you are using ephemeral storage, so make sure to have uploaded all your data back to IDEA filesystem first.
{% endhint %}

### Hibernate

{% hint style="info" %}
When you hibernate an instance, your desktop state is saved in memory. When you restart it, all your applications will automatically resume. On the other hand, stopping a virtual desktop is the same as powering off your laptop. Please not all EC2 instances support hibernation.
{% endhint %}

Click "**Action**" then "**Virtual Desktop State**"  and finally "**Hibernate**" to hibernate your current virtual desktop session. Hibernated session will not suffer any data loss and you can restart the session at any moment.&#x20;

##
