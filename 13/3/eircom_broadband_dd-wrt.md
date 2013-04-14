Title: eircom broadband & dd-wrt
Date: 2013-03-18 21:22
Tags: internet, linux
Category:
Slug: eircom-broadband-dd-wrt
Author: Finbarr O'Callaghan
Summary: setting up dd-wrt on eircom broadband


connecting to the [wan][1]

<p align="center"><img src="/static/images/wan_settings.png" alt="wan settings"/></p>

    :::text
    username: eircom@eircom.net
    password: broadband1
    vpi/vci: 8/35
    payload type: bridged
<br />

and since port forwarding didn't work by default, drop the following in the
command shell and [Save Firewall][2] 

    :::text
     iptables -t nat -A POSTROUTING -j MASQUERADE 
<br />

<p align="center"><img src="/static/images/firewall_settings.png"
alt="firewall settings"/></p>



[1]: http://support.eircom.net/SRVS/CGI-BIN/WEBCGI.EXE/&/?St=290&E=0000000000047147396&K=3908&Sxi=12&Problem=obj(6372)&DCSext.clickedFrom=bb;home;topqueries

[2]: http://www.dd-wrt.com/phpBB2/viewtopic.php?t=150437&postdays=0&postorder=asc&start=0



