## Requirements

* A Google accounts
* A payment card (credit card/debit card that support online transaction without 3-D Secure)

## Fees

* Free for 90 days, assuming total traffic is less than ~1.9 TB and only *one* VM is active

* If free credit run out, all VMs will immediately shutdown. No additional charges until the account is upgraded to paid account (allowing charges)

* With paid account, *one* VM in limited region in US can be [run forever](https://cloud.google.com/free/docs/gcp-free-tier#always-free-usage-limits) for free, but the free bandwidth is only 1 GB per month, excluding traffic to China and Australia (as in, there's no free bandwidth to China and Australia), with up to 0.12 USD per GB afterwards

## Cloud Engine account creation

Browse to https://cloud.google.com and click **Get Started For Free**. If you already have a Cloud Engine project, skip to [VM Creation](#vm-creation) step.

<br><img src="../images/gcege1.png" width="274">

Select the appropriate country, this *probably* must match the payment card you're going to use

<br><img src="../images/gcege2.png" width="989">

Adjust the account type and tax information. 

<br><img src="../images/gcege3.png" width="1065">

Unless you already add a card payment to your Google Account, insert your card number and start the trial

<br><img src="../images/gcege4.png" width="1062">

Google will make a test charge (less than 1 USD) and revert it almost immediately. Unless you [upgrade](https://cloud.google.com/free/docs/gcp-free-tier#how-to-upgrade), Google *will not* charge you afterwards even if you ran out your complimentary 300 USD credit or one year has passed. Your VM will simply shut down after one year, with no additional charge.

## VM Creation

Go to https://console.cloud.google.com/compute/instances

If you don't already have a project, follow the wizard shown to create your first project. Once you have a project, click Create

<br><img src="../images/gcege10.png" width="866">

Open http://www.gcping.com/ on new tab, let it run until completed (the icon should change into Play triangle). Note the top region shown, except global.

<br><img src="../images/gcege11.png" width="1220">

On the VM creation, choose the region according to GCPing result. Change the machine type to **f1-micro**

<br><img src="../images/gcege12.png" width="947">

Below, remember to check the **Allow HTTPS Traffic**. 

<br><img src="../images/gcege13.png" width="1001">

Expand the **Management, security, disks, networking, sole tenancy**

<br><img src="../images/gcege14.png" width="732">

Switch to **Networking** tab and click the pencil icon under **Network interfaces**

<br><img src="../images/gcege15.png" width="928">

Under **External IP**, click the **Ephemeral**, and change it to **Create IP Address** 

<br><img src="../images/gcege16a.png" width="915"><img src="../images/gcege16.png" width="928">

Name the address and **Reserve** it.

<br><img src="../images/gcege17.png" width="1012">

Change **IP Forwarding** to **On**, then click **Done**

<br><img src="../images/gcege18.png" width="940">

Click **Create** on the bottom.

<br><img src="../images/gcege19.png" width="940">

## Installing OpenVPN

Check the list of VM you have on https://console.cloud.google.com/compute/instances. Take note on the IP *right* beside the **SSH** button, then click on the **SSH** button 

<br><img src="../images/gcege20.png" width="681">

A new window will be opened. Type `sudo su` and press **Enter**

<br><img src="../images/gcege21.png" width="681">

Copy and paste the following line (to paste, click on the prompt, then Ctrl-V), then press Enter

`wget git.io/nenengce -O nenengce.sh && chmod +x nenengce.sh && ./nenengce.sh`

Note the path shown after "the configuration file is available". You will create multiple client profile later, since the same profile can't access your VPN simultaneously.

<br><img src="../images/gcege24.png" width="1310">

Copy paste the following line

`bash openvpn-install.sh`

then press enter. You should be greeted with the following screen 

<br><img src="../images/gcege26.png" width="916">

Follow the instruction to create new user. Ideally every device you own will use their own profile, because if, say your phone and tablet use the same profile, the your phone will automatically disconnect when your tablet connect, and vice versa.

<br><img src="../images/gcege27.png" width="1074">

Repeat the previous step to create additional user for each device you need. The server you've just made in theory should support at least dozens of simultaneous connections. After you're done, you'll need to download the profiles.

## Deploying OpenVPN on devices

Click gear icon on the top right, and select Download file 

<br><img src="../images/gcege28.png" width="583">

Enter the exact path of each profile you need, and download them

<br><img src="../images/gcege29.png" width="977">

Distribute the .ovpn file to each corresponding device. On each device, install the OpenVPN client. [Windows](https://openvpn.net/client-connect-vpn-for-windows/), [Mac OS](https://openvpn.net/client-connect-vpn-for-mac-os/), [Linux](https://community.openvpn.net/openvpn/wiki/OpenvpnSoftwareRepos), [Android](https://play.google.com/store/apps/details?id=net.openvpn.openvpn) and [iOS](https://apps.apple.com/us/app/openvpn-connect/id590379981) are officially supported.

Launch the OpenVPN app on your device, then import the profile file before first use. 

<br><img src="../images/gcege30.png" width="781">

You can change the profile name, probably useful if you want to use multiple server later.

<br><img src="../images/gcege31.png" width="779">

The profile list will show the status of each profile you have. Connect or disconnect by toggling the switch.

<br><img src="../images/gcege32.png" width="770">

Verify by googling "What is my IP" or browsing sites that used to be blocked (if it's still blocked, try restarting the browser)
