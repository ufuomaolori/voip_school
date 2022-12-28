In this lab you will install FreeSwitch using packages. 

Step 1 - Now (14/04/2022) FreeSwitch requires a personal access token to download the repository

Authentication required

SignalWire Personal Access Tokens (PAT)s are required to access FreeSWITCH install packages.
HOWTO Create a SignalWire Personal Access Token
https://freeswitch.org/confluence/display/FREESWITCH/HOWTO+Create+a+SignalWire+Personal+Access+Token

Step 2 - Set up the source list of packages

```
TOKEN=YOURSIGNALWIRETOKEN
 
apt-get update && apt-get install -y gnupg2 wget lsb-release
 
wget --http-user=signalwire --http-password=$TOKEN -O /usr/share/keyrings/signalwire-freeswitch-repo.gpg https://freeswitch.signalwire.com/repo/deb/debian-release/signalwire-freeswitch-repo.gpg
 
echo "machine freeswitch.signalwire.com login signalwire password $TOKEN" > /etc/apt/auth.conf
echo "deb [signed-by=/usr/share/keyrings/signalwire-freeswitch-repo.gpg] https://freeswitch.signalwire.com/repo/deb/debian-release/ `lsb_release -sc` main" > /etc/apt/sources.list.d/freeswitch.list
echo "deb-src [signed-by=/usr/share/keyrings/signalwire-freeswitch-repo.gpg] https://freeswitch.signalwire.com/repo/deb/debian-release/ `lsb_release -sc` main" >> /etc/apt/sources.list.d/freeswitch.list
 
# you may want to populate /etc/freeswitch at this point.
# if /etc/freeswitch does not exist, the standard vanilla configuration is deployed
apt-get update && apt-get install -y freeswitch-meta-all
```

Step 3 - Check the installation using fs_cli

```
fs_cli
```

To exit the console use /quit

Step 4 - To start, stop and restart use:

```
service freeswitch stop
service freeswitch start
service freeswitch restart
```