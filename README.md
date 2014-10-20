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
```

Add the previsous code in the *nmap-service-probes* file (exemple: /usr/share/nmap/nmap-service-probes)

Data Access Messaging Protocol *service probe*
------

[Data Access Messaging Protocol](http://community.actian.com/w/files/1/17/DAMP_Spec.pdf) is used by Ingres Data Access server (iigcd).

In the nmap-services file, replace
```bash
unknown 21071/udp       0.000654
```
by
```bash
ingres-dam      21071/tcp       0.000654 #Data Access Messaging Protocol used by Ingres Data Access Server (iigcd)
```


In the *nmap-service-probes* file, add this source code in order to detect the DAM protocol:
```bash
##############################NEXT PROBE##############################
#Detect the Data Access Messaging Protocol (DAM) used by Ingres Data Access Server (iigcd)
#"2300" --length of the following array +2
#"4a43544c" --Transport packet ID
#"4352" --Connection Request (ascii ‘CR’)
#"010102" --DAM-TL Protocol Level (lvl 2)
#"02010f" --Packet Size
#"0604444d4d4c" --Message Layer Protocol ID ('DMML')
#"030d" --Length of the following Session mask
#"010107" -- DAM-ML protocol level (lvl 7)
#"0308daafb0479210e2e5" --Session mask

Probe TCP dam-connection
q|\x23\x00\x4a\x43\x54\x4c\x43\x52\x01\x01\x02\x02\x01\x0f\x06\x04\x44\x4d\x4d\x4c\x03\x0d\x01\x01\x07\x03\x08\xda\xaf\xb0\x47\x92\x10\xe2\xe5|
ports 21071, 21064


# (..) --packet length
# "4a43544c4343" and "444d544c4343" --Only 2 possibility for the Transport
Packet ID
# "0604444d4d4c" --Currently, only a single message layer protocol is
defined (444d4d4c)
match dam-connection
m/^(..)(\x4a\x43\x54\x4c\x43\x43|\x44\x4d\x54\x4c\x43\x43)(.*)\x06\x04\x44\x4d\x4d\x4c/s
p/ingres-dam/
```
