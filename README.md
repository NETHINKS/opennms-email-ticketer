# opennms-email-ticketer

This is a small Add-On for OpenNMS that forwards OpenNMS alarms by e-mail on demand (by clicking on a button in the OpenNMS WebUI). This feature could be used to to create tickets in some custom ticket systems using their mail interface.


## functionality

This Add-On is realized as Drools ruleset and uses some parts of the OpenNMS trouble ticket integration. The trouble ticket integration provides additional buttons (create ticket / update ticket / close ticket) on the alarm details page in the OpenNMS WebUI. If one of the buttons is pressed, a new Event will be created (e.g. uei.opennms.org/troubleTicket/create). These events contains the alarmId as a parameter.

The created Drools ruleset process uei.opennms.org/troubleTicket/create events and create e-mails, which could be sent to a ticket system. The mailserver configuration in the OpenNMS javamail-configuration.properties file will be used.

Receiver, subject and message text of the mails are fully configurable. Receiver and subject are configured in `drools-engine.d/ticketer/ticketer-configuration.properties`, the message text can be defined in a text file (e.g. mailbody.tpl). Template variables can be used in the definitions.


## setup

* Copy the content of the drools-engine.d subdirectory to `<OpenNMS-dir>/etc/drools-engine.d`
* Enable the OpenNMS Correlator in `<OpenNMS-dir>/etc/service-configuration.xml`
* configure your mailserver in `<OpenNMS-dir>/etc/javamail-configuration.properties`
* configure the ticketer in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/ticketer-configuration.properties`
* update the path to the ticketer configuration file in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/drools-engine.xml`
* set `opennms.alarmTroubleTicketEnabled = true` in opennms.properties
* restart OpenNMS

## configuration

The tickter can be configured in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/ticketer-configuration.properties`. Changes on that file will be directly applied without restarting OpenNMS.

The following options can be set:

| setting         | description |
|-----------------|-------------|
|receiver         | receiver mail address |
|subject          | mail subject. Template variables can be used (see section below) |
|message.template | path to a file, which contains the message of the mail to be sent. Template variables can be used (see section below) |
|user._username_.mail | By default, the ticketer uses the sender address defined in javamail-configuration.properties. This behavior can be overwritten by defining an user._username_.mail setting (where _username_ is the login name of the OpenNMS user, which created the ticket). With that setting, an new sender adress can be defined depending on the OpenNMS user. | 


### template variables

Within the definitions of subject and message, the follwing template variables can be used and will be replaced by the current values of the given alarm:

| variable | description |
|----------|-------------|
|{{alarm\_id}}            | ID of the alarm |
|{{alarm\_user}}          | OpenNMS user, who created the ticket |
|{{alarm\_uei}}           | UEI of the alarm |
|{{alarm\_sev}}           | severity of the alarm severity |
|{{alarm\_logmsg}}        | logmessage of the alarm |
|{{node\_label}}          | nodelabel, if a node is assigned with an alarm, otherwise empty string | 
|{{node\_asset\_\<key\>}} | asset field of a node, if it is assigned with an alarm, otherwise empty string (e.g. {{node\_asset\_city}}) |


## supported OpenNMS versions
This version is developed and tested with OpenNMS Meridian 2017.
