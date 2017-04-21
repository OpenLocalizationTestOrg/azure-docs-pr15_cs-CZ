##<a name="tcp-settings-for-azure-vms"></a>Nastavení protokolu TCP Azure VMs

Azure VMs komunikovat veřejné Internet pomocí [překladu síťových adres] [ nat] (překládání adres). Zařízení překladu síťových adres přiřadit veřejnou IP adresu a port Azure OM, povolení, OM stanovit socket pro komunikaci s jiných zařízeních. Pokud paketů ukončíte prochází této socket po určité době zařízení ukončuje mapování a socket je zadarmo pro použití v jiných VMs.

Toto je běžné chování překladu síťových adres, které může způsobovat problémy s komunikace na základě TCP aplikace, které očekávají socket být zachován za časový limit. Existují dvě nečinnosti nastavení vzít v úvahu, je vhodná pro relace ve stavu *vytvořili připojení* :

- **příchozí** prostřednictvím [Služba Vyrovnávání zatížení Azure][azure-lb-timeout]. Tento časový limit ve výchozím nastavení 4 minut a můžete upravit až 30 minut.
- **odchozí** pomocí [SNAT] [ snat] (zdroj překladu síťových adres). Tento časový limit je nastavený na 4 minut a nelze upravit.

Abyste měli jistotu, že nejsou ztratilo připojení za časový limit, nezapomeňte aplikace sleduje relace aktivní, nebo můžete nakonfigurovat podkladového operačního systému k tomu nevyzve. Nastavení pro použití se liší pro systémy Linux a Windows, jak je ukázáno v následujícím příkladu.

[Linux][linux], změňte proměnných jádra dole.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Pro [systém Windows][windows], změňte hodnoty registru.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Nastavení výše uvedených zajistit zůstat aktivní paketu odeslání po 2 minuty (120 sekund) nečinnosti a odeslaný každých 30 sekund. A pokud 8 tyto pakety nezdaří, je nezobrazí relace.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md