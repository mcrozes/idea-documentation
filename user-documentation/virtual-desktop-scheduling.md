# Virtual desktop scheduling



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
