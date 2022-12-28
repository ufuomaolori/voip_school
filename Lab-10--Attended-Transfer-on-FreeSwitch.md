Now we are going to do the same as the last lab, but with attended call transfer. Again you will need three phones or receive a call from pstn and transfer to a second phone. I will demonstrate this in both ways, with an incoming call and with three phones.

We are going to use *2 for attended transfer. The transfer is assisted by FreeSwitch, no need to have a transfer button in the phone.

Step 1: Bind the meta application to the *2

    <!-- Dial local extensions -->
    <extension name="Local_Extension">
        <condition field="destination_number" expression="^(10[01][0-9])$">
            <action application="bind_meta_app" data="1 b s execute_extension::dx XML features"/>
            <action application="bind_meta_app" data="2 b s execute_extension::att_xfer XML features"/>
            <action application="bridge" data="user/@${domain_name}"/>
        </condition>
    </extension>

Step 2: Include the attended transfer feature in your dialplan, use the default.xml, no need to use features.xml

    <extension name="att_xfer">
        <condition field="destination_number" expression="^att_xfer$">
            <action application="read" data="3 4 'tone_stream://%(10000,0,350,440)' digits 30000 #"/>
            <action application="set" data="origination_cancel_key=#"/>
            <action application="att_xfer" data="user/${digits}@$${domain}"/>
         </condition>
    </extension>

Step 3: Reload xml

fs_cli:>reloadxml

Step 4: Test the transfer.

Call from 1000 to 1001 and transfer using *11002.

Check if this works.

