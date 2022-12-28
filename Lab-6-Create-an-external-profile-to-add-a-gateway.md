In this lab we will create an external profile associated to the context _public_ . After adding this context, we will add a gateway to send and receive calls to the external world. If you are behind a NAT router testing your PBX in a Virtual Machine, discover your external address. Just use google and search for "whatsmyip". Use your external IP in ext-sip-ip and ext-rtp-ip

Step 1: Add the profile external to the sip_profiles

cd /etc/freeswitch/sip_profiles

Use your favorite text editor such as nano or vi to edit the file external.xml. Replace <internal_ip> by your internal IP address and <externa_ip> by the external address discovered using google. 

`<profile name="external">`\
        `<aliases></aliases>`\
        `<gateways></gateways>`\
        `<domains></domains>`\
        `<settings>`\
                `<param name="sip-ip" value="<internal_ip>"/>`\
                `<param name="rtp-ip" value="<internal_ip>"/>`\
                `<param name="ext-sip-ip" value="<external_ip>"/>`\
                `<param name="ext-rtp-ip" value="<external_ip>"/>`\
                `<param name="sip-port" value="5080"/>`\
                `<param name="context" value="public"/>`\
                `<param name="extension" value="1000"/>`\
        `</settings>`\
`</profile>`

Pay attention to the parameters context and sip-port, now public and 5080 respectively. 

Step 2: Add the gateway to send external calls

In the file external.xml between <gateways></gateways> add the following lines. use as the username and fromuser a random number between 1010 to 1060. It is good if each student get a different account in the fake provider.  

 `<gateway name="sip.flagonc.com">`\
`                        <param name="username" value="1045"/>`\
                        `<param name="password" value="supersecret"/>`\
                        `<param name="from-domain" value="sip.flagonc.com"/>`\
                        `<param name="from-user" value="1045"/>`\
                        `<param name="sip-port" value="5600"/>`\
                        `<param name="proxy" value="sip.flagonc.com:5600"/>`\
                        `<param name="register" value="true"/>`\
                        `<param name="ping" value="25"/>`\
                        `<param name="context" value="public"/>`\
`</gateway>`

Step 3: Reload and verify

Use the following command to reload the profile

`sofia profile external restart reloadxml`

To verify, please use:

`fs>sofia status`

<sub>`freeswitch@fs> sofia status`\
                     `Name          Type                                       Data      State`\
`=================================================================================================`\
                 `external       profile            sip:mod_sofia@192.168.0.71:5080      RUNNING (0)`\
`external::sip.flagonc.com       gateway              sip:1020@sip.flagonc.com:5600      REGED`\
             `192.168.0.71         alias                                   internal      ALIASED`\
                 `internal       profile            sip:mod_sofia@192.168.0.71:5060      RUNNING (0)`\
`=================================================================================================`</sub>

Step 4: Dial 91234567 from one of your softphones. Does the call complete?

Step 5: (OPTIONAL) If you prefer to have one gateway per file, add the following lines to the external.xml and create an external subdirectory to add gateways file by file. It is just another way to do the same thing. This is the way the example files coming with freeswitch are organized. 

` <gateways>`\
    `<X-PRE-PROCESS cmd="include" data="external/*.xml"/>`\
 `</gateways>`

Create a subdirectory external

`mkdir external`

In the subdirectory external create:

edit a file with the name flagonc.xml

`<include>`\
 `<gateway name="sip.flagonc.com">`\
`                        <param name="username" value="1045"/>`\
                        `<param name="password" value="supersecret"/>`\
                        `<param name="from-domain" value="sip.flagonc.com"/>`\
                        `<param name="from-user" value="1045"/>`\
                        `<param name="sip-port" value="5600"/>`\
                        `<param name="proxy" value="sip.flagonc.com:5600"/>`\
                        `<param name="extension" value="1000"/>`\
                        `<param name="register" value="true"/>`\
                        `<param name="ping" value="25"/>`\
                        `<param name="context" value="public"/>`\
`</gateway>`\
`</include>`


