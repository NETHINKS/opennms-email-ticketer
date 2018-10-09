# opennms-email-ticketer

This is a small Add-On for OpenNMS that forwards OpenNMS alarms by e-mail on demand (by clicking on a button in the OpenNMS WebUI). This feature could be used to to create tickets in some custom ticket systems using their mail interface.

**At the moment this is only a basic implementation, that needs to be extended.**

## functionality

This Add-On is realized as Drools rules and uses some parts of the OpenNMS trouble ticket integration. The trouble ticket integration provides additional buttons (create ticket/ update ticket / close ticket) on the alarm details page in the OpenNMS WebUI. If one of the buttons is pressed, a new Event will be created (e.g. uei.opennms.org/troubleTicket/create). These events contains the alarmId as a parameter.

The created Drools rules process the uei.opennms.org/troubleTicket/create events and create E-Mails, which could be sent to a ticket system. The mailserver configuration in the OpenNMS javamail-configuration.properties file will be used.


## setup

* Copy the content of the drools-engine.d subdirectory to `<OpenNMS-dir>/etc/drools-engine.d`
* Enable the OpenNMS Correlator in `<OpenNMS-dir>/etc/service-configuration.xml`
* configure your mailserver in `<OpenNMS-dir>/etc/javamail-configuration.properties`
* set the target address for e-mails in `<OpenNMS-dir>/etc/drools-engine.d/ticketer/drools-engine.xml`
* set `opennms.alarmTroubleTicketEnabled = true` in opennms.properties
* restart OpenNMS

