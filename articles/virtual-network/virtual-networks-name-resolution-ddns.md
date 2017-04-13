<properties
   pageTitle="Použití dynamického DNS k registraci názvy hostitelů"
   description="Tahle stránka umožňuje podrobnosti o tom, jak nastavit dynamické DNS zaregistrujte názvy hostitelů v serverů DNS."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Pomocí dynamického DNS zaregistrujte názvy hostitelů v serveru DNS

[Azure poskytuje překlad](virtual-networks-name-resolution-for-vms-and-role-instances.md) virtuálních počítačích (VMs) a rolí instance. Pokud vaše překlad potřebuje překračují poskytovanou Azure, je zadat serverů DNS. To vám dává přizpůsobit řešení DNS tak, aby odpovídal vašim požadavkům. Potřebujete Příklad přístupu k prostředkům místní prostřednictvím řadiče domény Active Directory.

Když jsou hostovány vaše vlastní servery DNS jako Azure VMs můžete dál hostname dotazy na stejné vnet na Azure řešení názvy hostitelů. Pokud nechcete použít tento postup, můžete zaregistrovat názvy hostitelů OM v serveru DNS pomocí dynamického DNS.  Azure nemá možnost (například přihlašovací údaje) přímo vytvoření záznamů v místní servery DNS, často je třeba alternativní uspořádání. Tady jsou některé běžné scénáře s alternativy.

## <a name="windows-clients"></a>Klienti systému Windows

Nejsou doméně klienty Windows pokusy o nezabezpečenou aktualizace dynamické DNS (DDNS) při spuštění nebo pokud se změní IP adresa jeho. Název DNS je název hostitele plus primární přípony DNS. Azure ponechá primární přípony DNS prázdná, ale je možné nastavit v OM prostřednictvím [uživatelského rozhraní](https://technet.microsoft.com/library/cc794784.aspx) nebo [pomocí automatizaci](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Doméně klienty Windows zaregistrovat jejich IP adres u řadiče domény pomocí zabezpečené dynamické DNS. Tento proces připojení k doméně nastaví primární přípony DNS v klientském počítači a vytváří a udržuje vztah důvěryhodnosti.

## <a name="linux-clients"></a>Linux klientů

Klienti Linux obvykle nechcete zaregistrovat se serverem DNS při spuštění, budou se předpokládá, že DHCP server znamená. Servery Azure společnosti nemají možnost nebo přihlašovací údaje k registraci záznamů v serveru DNS.  Nástroj s názvem *nsupdate*, která je součástí balíček vazbu, slouží k odesílání aktualizací dynamické DNS. Protože standardizovány protokol dynamické DNS můžete *nsupdate* , i když nejste vazbu na serveru DNS.

Můžete použít zavěšení, které jsou k dispozici tak, že klienty vytvářet a spravovat položce hostname na serveru DNS. Během cyklu DHCP spustí klienta skripty */etc/dhcp/dhclient-exit-hooks.d/*. Mohou sloužit k registraci nové adresy IP pomocí *nsupdate*. Příklad:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

Můžete také příkaz *nsupdate* provádět zabezpečené aktualizace dynamické DNS. Pokud používáte svázat DNS server, pár veřejný privátní klíč je například [vygenerovalo](http://linux.yyz.us/nsupdate/).  Je server DNS s veřejnou část klávesu [nakonfigurovaný](http://linux.yyz.us/dns/ddns-server.html) tak, aby ho můžete ověřit podpis do žádosti. Pomocí možnosti *-k* musí poskytovat že dvojici klíč *nsupdate* v pořadí pro dynamické DNS aktualizovat požadavek určený k podpisu.

Pokud používáte Windows DNS server, můžete použít ověřování protokolem Kerberos s parametrem *– g* v *nsupdate* (není k dispozici ve verzi Windows *nsupdate*). K tomuto účelu použijte *kinit* načíst přihlašovacích údajů (například z [keytab soubor](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Potom *nsupdate g* vyzvedne přihlašovacích údajů z mezipaměti.

V případě potřeby můžete přiřadit vaše VMs příponu hledání DNS. Přípona DNS jsou zadány v souboru */etc/resolv.conf* . Většina Linux distros automatickou správu obsahu tohoto souboru, takže obvykle nelze upravit ho. Můžete však přepsat přípony Příkazem klienty *Nahradit* . Postup, v */etc/dhcp/dhclient.conf*přidáte:

        supersede domain-name <required-dns-suffix>;

