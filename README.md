The IPTraffic module for serving VPN customers
==============================================

Features:

* RADIUS authentication
* Realtime traffic calculation using [Netflow](http://en.wikipedia.org/wiki/Netflow) v5 as a traffic source
* Flexible tariffs (subnet rules, time rules)
* Using PostgreSQL database for the storing users, tariff plans, RADIUS attributes and sessions
* It's easy to add your own backends

Configuration
-------------

The following modules should be added to the netspire.conf file:

    {mod_iptraffic, [{tariffs, "tariffs.conf"}, {session_timeout, 60}, {delay_stop, 5}, {disconnect_on_shutdown, yes}]}

The default value of the **session_timeout** option is 60 seconds and may be ommited.

The ***delay_stop*** option is used to delay stopping of the session to receive all data from netflow sensor after the session closing (After receiving Accounting-Stop packet).
The default value of the ***delay_stop*** option is 5 seconds and and may be ommited.

The ***disconnect_on_shutdown*** option is used to specify is need to disconnect clients from NAS in case of application shutdown.
The default value of the ***disconnect_on_shutdown*** option is yes and may be ommited.

You MUST set **Acct-Interim-Interval** RADIUS attribute for client. This attribute is required to prolong session and it's value MUST be significantly less than **session_timeout**.
Note that if Netspire does not receiving interim updates from NAS via RADIUS, sessions will be marked as *expired* and closed, regardless of real state on NAS.

Be aware about **Acct-Interim-Interval** radius attribute limitation for Linux pppd. It should be no less then 60 seconds.

Also you need to load SQL schema objects from schema.sql file to the already created database.
