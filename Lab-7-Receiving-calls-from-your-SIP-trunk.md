In this lab you will learn how to receive external calls. To receive calls we will have to create a context called public. To simplify we will create a new file called public.xml. 

Step 1: Go to the dialplan directory and use your favorite editor to edit the file public.xml

`cd /etc/freeswitch/dialplan`\
`vi public.xml`

Step 2: Add the following content to the file 

`<include>`\
 `<context name="public">`\
   `<extension name="public_extensions">`\
      `<condition field="destination_number" expression="^(10[01][0-9])$">`\
        `<action application="transfer" data=" XML default"/>`\
      `</condition>`\
   `</extension>`\
 `</context>`\
`</include>`\

Step 3: Reload xml 

`fs_cli>reloadxml`

Step 4: Use your browser to go to http://sip.flagonc.com. You wil get in a click2dial application. Use 1000 as the Name and 1045 as the extension. The system will originate a call to your the registered PBX and connect to a funny recording (monkeys). 



