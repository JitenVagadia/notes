## DNS Terminologies
* domain registrar (route53, go daddy)
* dns records (aaaa,cname)
* zone file - has dns records
* name server - resolve queries
* browser(maybe os) asks local dns server for ip, that server does:
  * go to root dns server (ICANN) for tld (.com) ns lookup
  * then go to tld dns server (ICANN.IANA) for sld (example.com) ns lookup
  * then go to sld dns server (godaddy,route53) for ip

## Route 53
* HA (only one with 100% availability SLA), scalable, managed, authoritative(mean you can change records)
* also a domain registrar
* records
  * how to route traffic
  * contains: domain name, record type, ip, routing policy, ttl
  * record type ns -> ns is for a hosted zone
* hosted zones, can be public/private
* cname
  * points to a host name
* alias
  * points to an aws resource
  * extension to dns
  * always of type a/aaaa with value being a hostname instead of ip
  * no ttl
  * an extension only for aws managed services (not ec2)
* health check -> automated dns failover
  * route 53 health checkers are outside vpc, can't connect to private ips
    * one way is to create cloud watch alarm on top and do health check on that
* routing policies
  * simple -  single value/ multi value (random picked by client), no health check association
  * weighted - different weights fro different ip/hosts, has health check association
  * latency - routed to nearest, has health check association
  * geolocation - region based, specify location, and a default value, has health check association
  * geoproximity - based on both location of user and resources, resources have a bias value (more-> more traffic)
    * need route53 traffic flow to use this
  * ip based  - based on client ip, gets routed to whichever cidr block they belong
* domain registrar (buy domain name) vs dns service (manage your records)
  * can buy from godaddy and use route 53 to manage (give go daddy name server ip of route53)