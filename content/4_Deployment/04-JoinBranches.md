---
title: Lab 4 - Join Branch FGTs to FMG
linkTitle: Lab 4 - Join Branch FGTs
weight: 20
---

## Join Branch FGTs to FMG

You will now log into the Branch1 and Branch2 FGT GUIs and point them to the FMG.  FMG will use the Serial Number associated with the model devices for authorization.  We will not  discover the Branch FGTs from FMG like we did the HUB FGTs in Lab 1.
<!-- MSG-COMMENT - Update FNDN Screenshot -->
Let’s get these Branch FGTs into FMG and push their configs.  Connect to Branch1 via HTTPS.
![Branch FNDN](branchfndn.png?width=550px)

&nbsp;

 - Username – admin
 - Password – Leave Blank
 - Click ‘Login’

![Join2](br1admin.png?width=450px)

{{% notice warning %}}
DO NOT CREATE A PASSWORD!!!!!  Click 'Later' when prompted to Change Your Password. This will break FMG's ability to onboard and push the config.

![Ignore1](donotchangepassword.png?width=750px)
{{% /notice %}}





&nbsp;



**Open a Fortigate CLI Window**

![FGT CLI](fgtcli.png?width=950px)

Paste the following commands into the CLI:
{{% notice note %}}
You will need the FortiManager Serial Number, it can be found on the FortiManager Dashboard page, copy that number and replace **\<FMG SERIAL NUMBER\>** with the actual Serial Number.


![FMG Serial Number](fmgserialnumber.png?width=750px)
{{% /notice %}}

```
config system central-management
    set type fortimanager
    set serial-number <FMG SERIAL NUMBER>
    set fmg 198.18.250.1
end

```
![Branch1 CLI Join](fgtclijoin.png?width=750px)

&nbsp;

Quickly transition back over to your FMG browser.  Your Branch1 device should now be in the Auto-link process.  

- Click the Notifications button in the top menu to see the auto-link status. 
![Join6](br1autolink.png?width=950px)

&nbsp;

Your Device Manager should look like this when the Branch1 auto-link process completes.  Notice the green checkmarks next to the Policy Package and Provisioning Templates.
![Join7](br1provisioned.png?width=950px)

&nbsp;

Jump back over to your Branch1 browser.  Re-login and validate it received all its configuration.  Check the configurations for Interfaces, VPNs, BGP, SD-WAN, Policy, etc.  Your VPN tunnels should be up.  BGP neighbors should be up, and you should have the HUB BGP routes, SDWAN SLAs should be passing.  

{{% notice note %}}
The supplementary Branch CLI Template changed the password to fortinet.
Use admin/fortinet to login after the configuration has been pushed.
{{% /notice %}}

**Repeat the above steps to Join Branch2 to FMG.**

Your Device Manager should look like this after Branch2 has been joined to FMG:
![Join8](fmgfinal.png?width=950px)

&nbsp;

**If you rember in our System Template, we setup logging to FortiAnalyzer.  Let's log in to FAZ and check for Branch1 and Branch2 logs.**
<!-- MSG-COMMENT - UPDATE FNDN SCREENSHOOT LATER -->
Log into FAZ again:
![Join8](fazconnect.png?width=450px)

Login to FAZ
 - admin/`Fortinet1234!`
![Join8](fazlogin.png?width=450px)

As you can see in FAZ Device Manager, all 4 of your FortiGates are now logging Real-Time logs.  Feel free to click around and see the logs.

![Join8](fazgoodlogging.png?width=950px)




&nbsp;

# This completes Lab4 and the FMG POC Prep Workshop!  Thank you!