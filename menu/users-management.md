# Users management

To manage IDEA users, navigate to the **"Cluster Management**" section on the left sidebar of IDEA menu and click "**Users**"

<figure><img src="../.gitbook/assets/Screen Shot 2022-10-23 at 11.36.57 AM.png" alt=""><figcaption><p>Users portal</p></figcaption></figure>

### Create a new user

1. Click "**Create User**"
2. Specify the username
3. Specify the email address
4. Select whether or not the email is verified.&#x20;
   1. **NO** (default & recommended): A temporary password will be send to the email and user will need to change it at first login
   2. **YES**: You will be asked to manually set up a password for the user
5. Choose the permissions applicable for the user. To learn more about the different permissions, refer to [groups-management.md](groups-management.md "mention")
6. Choose the login shell

If "**Email is Verified**" is not checked during account creation, the user will receive a welcome email message with a temporary password. User will be required to change this password after first successful login.

<figure><img src="../.gitbook/assets/Screen Shot 2022-10-23 at 11.42.59 AM.png" alt=""><figcaption><p>Example of invitation email</p></figcaption></figure>

### Understanding Confirmation Status

Confirmed: User can log in to IDEA

Force Change Password: User will be required to change his/her password after the next successful login

### Enable a user

* Select a user with Status set to "Disabled"
* Click "**Actions**" then "**Enable User**"

### Disable a user

* Select a user with Status set to "Enabled"
* Click "**Actions**" then "**Disable User**"

### Give user Cluster Admin privileges

* Select a standard user
* Click "**Actions**" then "**Set as Admin user**"

### Remove Cluster Admin privileges for a user

* Select a user with admin privileges
* Click "**Actions**" then "**Remove as Admin user**"

### Add user to group

* Select the user
* Click "**Actions**" then "**Add User to Group**"
*   Choose the group from the list

    <figure><img src="../.gitbook/assets/Screen Shot 2022-10-23 at 12.34.45 PM.png" alt=""><figcaption><p>Example of group selection</p></figcaption></figure>

### Remove user from group

* Select the user
* Click "**Actions**" then "**Remove User From Group**"
* Select the group(s) you want your user to be removed from

### Reset password for a user

* Select the user
* Click "**Actions**" then "**Reset Password**"

{% hint style="info" %}
You cannot reset the password of a user if his/her confirmation status is not "Confirmed"
{% endhint %}