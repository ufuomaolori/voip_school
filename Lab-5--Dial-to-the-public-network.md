In this lab we are going to discover how to dial external numbers

Step 1: Add the following lines to the default.xml file between <context></context>

   `<!-- Dial to pstn starting with 9-->`\
   `<extension name="pstn">`\
        `<condition field="destination_number" expression="^9([1-9]\d*)">`\
                `<action application="bridge" data="sofia/gateway/sip.flagonc.com/"/>`\
        `</condition>`\
    `</extension>`\

Step 2: Now dial 91234567 and check if you can hear the message from the external gateway ("you have dialed to gateway , the digits you have dialed are.....). The external gateway plays this recording to help you understand what have been dialed to the pstn.