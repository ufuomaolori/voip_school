In this lab you will learn how to restrict external destinations by class, voip, local, ld, int

In the directory we have created a variable called toll_allow. This variable is present in the dialplan in the form ${toll_allow}. Using a regular expression and nesting condition expressions we are going to restrict calls per user. 

Step 1: Add domestic, ld, international to user 1000

Step 2: Add only domestic to the user 1001

Step 3: Add the following rules to the dialplan replacing the current rule for international

`<extension name="domestic">`\
  `<condition regex="all">`\
    `<regex field="toll_allow" expression="domestic"/>`\
    `<regex field="destination_number" expression="^9([2-9][0-9]{6})$"/>`\
    `<action application="bridge" data="sofia/gateway/sip.flagonc.com/$1"/>`\
  `</condition>`\
`</extension>`

`<extension name="long_distance">`\
  `<condition regex="all">`\
    `<regex field="toll_allow" expression="ld"/>`\
    `<regex field="destination_number" expression="^9(1[2-9][0-9]{2}[2-9][0-9]{6})$"/>`\
    `<action application="bridge" data="sofia/gateway/sip.flagonc.com/$1"/>`\
  `</condition>`\
`</extension>`

`<extension name="international">`\
  `<condition regex="all">`\
    `<regex field="toll_allow" expression="international"/>`\
    `<regex field="destination_number" expression="^9(011\d*)"/>`\
    `<action application="bridge" data="sofia/gateway/sip.flagonc.com/$1"/>`\
  `</condition>`\
`</extension>`

Step 4: Try to dial an international number 011554823456789 from user 1000

Did the call worked?

Step 5 Try now with the user 1001

Why the calls did not work?
