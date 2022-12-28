In this lab, we well delete the default dial plan and start from scratch using the simplest example possible. How to dial between two extensions. 

Step 1: Move all the example files to the tmp directory. We are not going to learn from the examples. To really learn we will have to start from the most basic example. 

`cd /etc/freeswitch/dialplan`\
`mkdir /tmp/dialplan`\
`mv * /tmp/dialplan`

Step 2: Let's create a file default.xml with the default context and instructions to dial between extensions

`<?xml version="1.0" encoding="utf-8"?>`\
`<include>`\
  `<context name="default">`\
    `<extension name="Local_Extension">`\
      `<condition field="destination_number" expression="^(10[01][0-9])$">`\
        `<action application="bridge" data="user/$1@${domain_name}"/>`\
      `</condition>`\
    `</extension>`\
  `</context>`\
`</include>`

Step 3: Dial between the extensions 1000 and 1001 and vice versa

This is the simplest example of a dialplan, but very helpful to understand the basics


