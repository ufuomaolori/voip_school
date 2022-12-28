To transfer a call you will need three phones or receive a call from pstn and transfer to a second phone. I will demonstrate this in both ways, with an incoming call and with three phones. 

We are going to use *1 to transfer. The transfer assisted by FreeSwitch, no need to have a transfer button in the phone.

Step 1: Create an extra phone (1002) you already know how to do it. You may use csipsimple in your android phone or zoiper in an iPhone as your third phone. 

Step 2: Bind the meta application to the *1

    <!-- Dial local extensions -->
    <extension name="Local_Extension">
      <condition field="destination_number" expression="^(10[01][0-9])$">
         <action application="bind_meta_app" data="1 b s execute_extension::dx XML features"/>
        <action application="bridge" data="user/@${domain_name}"/>
      </condition>
    </extension>

Step 3: Include the transfer feature in your dialplan, use the default.xml, no need to use features.xml

      <!-- Features -->
      <context name="features">
          <!-- In call Transfer for phones without a transfer button -->
          <extension name="dx">
              <condition field="destination_number" expression="^dx$">
	          <action application="answer"/>
	          <action application="read" data="11 11 'tone_stream://%(10000,0,350,440)' digits 5000 #"/>
	          <action application="execute_extension" data="is_transfer XML features"/>
              </condition>
          </extension>

          <extension name="is_transfer">
              <condition field="destination_number" expression="^is_transfer$"/>
              <condition field="${digits}" expression="^(\d+)$">
	          <action application="transfer" data="-bleg ${digits} XML default"/>
	          <anti-action application="eval" data="cancel transfer"/>
              </condition>
          </extension>

      </context>

Step 4: Reload xml

`fs_cli:>reloadxml`

Step 5: Test the transfer. 

Call from 1000 to 1001 and transfer using *11002. 

Check if this works. 
