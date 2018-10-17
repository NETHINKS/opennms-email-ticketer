# opennms-email-ticketer

This is a small Add-On for OpenNMS that forwards OpenNMS alarms by e-mail on demand (by clicking on a button in the OpenNMS WebUI). This feature could be used to to create tickets in some custom ticket systems using their mail interface.


## functionality

This Add-On is realized as Drools ruleset and uses some parts of the OpenNMS trouble ticket integration. The trouble ticket integration provides additional buttons (create ticket/ update ticket / close ticket) on the alarm details page in the OpenNMS WebUI. If one of the buttons is pressed, a new Event will be created (e.g. uei.opennms.org/troubleTicket/create). These events contains the alarmId as a parameter.

The created Drools ruleset process uei.opennms.org/troubleTicket/create events and create e-mails, which could be sent to a ticket system. The mailserver configuration in the OpenNMS javamail-configuration.properties file will be used.

Receiver, subject and message text of the mails are fully configurable. Receiver and subject are configured in `drools-engine.d/ticketer/drools-engine.xml`, the message text can be defined in a text file (e.g. mailbody.tpl).

Within the definitions of subject and message, the follwing template variables can be used and will be replaced by the current values of the given alarm:

* {{node\_label}}
* {{alarm\_id}}
* {{alarm\_user}} (OpenNMS user, who created the ticket)
* {{alarm\_uei}}
* {{alarm\_sev}} (alarm severity)
* {{alarm\_logmsg}}

## setup

* Copy the content of the drools-engine.d subdirectory to `<OpenNMS-dir>/etc/drools-engine.d`
* Enable the OpenNMS Correlator in `<OpenNMS-dir>/etc/service-configuration.xml`
* configure your mailserver in `<OpenNMS-dir>/etc/javamail-configuration.properties`
* set the target address for e-mails in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/drools-engine.xml`
* set the subject for e-mails in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/drools-engine.xml`
* set the path to the mail body template file in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/drools-engine.xml`
* define the message text for e-mails in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/mailbody.tpl`
* set `opennms.alarmTroubleTicketEnabled = true` in opennms.properties
* restart OpenNMS

## supported OpenNMS versions
This version is developed and tested with OpenNMS Meridian 2017.
