In this lab you will create the Internal SIP profile in the port 5060. This will be important to register your phones. 

Step 1: Go to the fs directory

`cd /etc/freeswitch`

Step 2: Move the profiles provided by the default configuration to /tmp (We will start from scratch)

`mv /etc/freeswitch/sip_profiles /tmp

Step 3: Use your favorite editor (vi, nano...) to create the file internal.xml

Add the following content to internal.xml

`<profile name="internal">`\
        `<aliases></aliases>`\
        `<gateways></gateways>`\
        `<domains>`\
                `<domain name="all" alias="true" parse="false"/>`\
        `</domains>`\
        `<settings>`\
                `<param name="sip-ip" value="192.168.0.71"/>`\
                `<param name="rtp-ip" value="192.168.0.71"/>`\
                `<param name="sip-port" value="5060"/>`\
                `<param name="context" value="default"/>`\
                `<param name="auth-calls" value="true"/>`\
        `</settings>`\
`</profile>`\

Change the parameters sip-ip and rtp-ip to match your current IP address. 

Usually, FreeSwitch is able to get the address directly from the OS and set it to $${local_ip_v4}. However to avoid any problems in the detection we will assign a static IP. 

Step 3: Reload the sip_profile

Access the freeswitch console

`fs_cli`

Reload the sip internal profile 

`fs_cli>sofia profile internal restart reloadxml`

Step 3: Create two users in the directory

Let's start from scratch. Go to the default directory and move all files to tmp

Create a backup
`cd /etc/freeswitch/directory/default`\
`mkdir /tmp/default`\
`mv * /tmp/default`

Create your users using your favourite text editor

`vi 1000.xml`

`<include>`\
  `<user id="1000">`\
    `<params>`\
      `<param name="password" value="#pleasechange#"/>`\
      `<param name="vm-password" value="1000"/>`\
    `</params>`\
    `<variables>`\
      `<variable name="toll_allow" value="domestic,international,local"/>`\
      `<variable name="accountcode" value="1000"/>`\
      `<variable name="user_context" value="default"/>`\
      `<variable name="effective_caller_id_name" value="Extension 1000"/>`\
      `<variable name="effective_caller_id_number" value="1000"/>`\
      `<variable name="outbound_caller_id_name" value="$${outbound_caller_name}"/>`\
      `<variable name="outbound_caller_id_number" value="$${outbound_caller_id}"/>`\
      `<variable name="callgroup" value="techsupport"/>`\
    `</variables>`\
  `</user>`\
`</include>`

`vi 1001.xml`

`<include>`\
  `<user id="1001">`\
    `<params>`\
      `<param name="password" value="#pleasechange#"/>`\
      `<param name="vm-password" value="1001"/>`\
    `</params>`\
    `<variables>`\
      `<variable name="toll_allow" value="domestic,international,local"/>`\
      `<variable name="accountcode" value="1001"/>`\
      `<variable name="user_context" value="default"/>`\
      `<variable name="effective_caller_id_name" value="Extension 1001"/>`\
      `<variable name="effective_caller_id_number" value="1001"/>`\
      `<variable name="outbound_caller_id_name" value="$${outbound_caller_name}"/>`\
      `<variable name="outbound_caller_id_number" value="$${outbound_caller_id}"/>`\
      `<variable name="callgroup" value="techsupport"/>`\
    `</variables>`\
  `</user>`\
`</include>`

Step 4: Restart freeswitch 

`service freeswitch restart`

Step 5: Use Blink and Zoiper and register to FreeSwitch as shown in the video

Step 6: Check registrations

Get in the CLI

`fs_cli`

Show registrations

`show registrations`

To exit CLI use 

`/quit`

Step 6: You can make calls using the default dialplan

