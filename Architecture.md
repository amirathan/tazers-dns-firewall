# How Do I Internet? #

In order to understand how Tazers fits into an existing DNS architecture, it helps to understand how forward lookups work (i.e. "host" or "A" resource record resolution).
![http://janik.us/fwd_dns.png](http://janik.us/fwd_dns.png)


---


# Traditional/Typical DNS Blacklisting #

There are issues with the method commonly used to blacklist domains.  For instance, when a spoofed IP resolution is returned to the user, where does it point to?  Is there even a system listening at the blackhole IP?  If there is a system listening at that IP, is the inbound traffic organized or even mapped back to the original query?  Logging layer 3 and 4 connections to the blackhole system is often useless if you can't associate the inbound connections to their respective DNS queries that led them to the blackhole.
![http://janik.us/traditional_blacklist.png](http://janik.us/traditional_blacklist.png)


---


# Tazers DNS Blacklisting #

The Tazers architecture moves actual resolution to an "out-of-band" DNS server, and uses stub zones on the upstream enterprise DNS servers to point blacklisted domains to the out-of-band DNS server for resolution.  Moving the zones to a DNS server outside the typical resolution path is beneficial for several reasons:
  * Stub zones are dynamic - Whereas conditional forwarding requires manual administration.
  * Separation of duties - Your security team can administer zones from a single out-of-band server, instead of requiring accounts on each upstream enterprise DNS server.
  * Traffic and zones can be more organized - Only the zones you're concerned with are hosted on the system.
  * You have full control of zone resolution regarding those domains you choose to blacklist.
By creating multiple Ethernet aliases on the out-of-band server, you can create multiple BIND views that each listen to their own interface.  This is useful for separating different types of blacklisted domains (e.g., "malware" and "advertisements") to their own "honeypot".  This allows you to manage subsequent Layer 3 connections based on the interface IP a domain is blacklisted to point to.

![http://janik.us/tazers_blacklist.png](http://janik.us/tazers_blacklist.png)