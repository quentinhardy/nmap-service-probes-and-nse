nmap-service-probes-and-nse
===========================

This GIT contains some *nse* scripts and *services probes* not in nmap for the moment.

Tibco RDV *service probe*
------

[Tibco RDV product](http://www.tibco.com/products/automation/enterprise-messaging/rendezvous/default.jsp)

*Service Probe* to detect the Tibco RDV service:

```bash
##############################NEXT PROBE##############################
Probe TCP tibco-rdv q|\x00\x00\x00\x00\x00\x00\x00\x04\x00\x00\x00\x00|
ports 7500

match tibco-rdv

m|\x00\x00\x00\x00\x00\x00\x00\x04\x00\x00\x00\x00\x00\x00\x00\x02\x00\x00\x00\x02\x00\x00\x00\x00\x00\x00\x00\x01\x00\x00\x00\x00\x04\x00\x00\x00\x04\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00|s
p/unknown/
```bash

Add the previsous code in the *nmap-service-probes* file (exemple: /usr/share/nmap/nmap-service-probes)
