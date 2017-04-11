<properties
   pageTitle="Vytvoření DNS zónu pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Naučte se vytvářet zóny DNS pro službu Azure DNS podrobné spustíte, který je hostitelem vašeho DNS domény pomocí rozhraní příkazového řádku"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Vytvoření Azure DNS zónu pomocí rozhraní příkazového řádku


> [AZURE.SELECTOR]
- [Azure portálu](dns-getstarted-create-dnszone-portal.md)
- [Prostředí PowerShell](dns-getstarted-create-dnszone.md)
- [Azure rozhraní příkazového řádku](dns-getstarted-create-dnszone-cli.md)


Tento článek vás provede jednotlivými kroky vytvoření zóny DNS pomocí rozhraní příkazového řádku. Můžete taky vytvořit pomocí prostředí PowerShell nebo portálu Azure zóny DNS.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Než začnete

Tyto pokyny pomocí Microsoft Azure rozhraní příkazového řádku. Je potřeba provést aktualizaci na nejnovější Azure rozhraní příkazového řádku (0.9.8 nebo novější) používat příkazy Azure DNS. Typ `azure -v` zjistit, jakou verzi Azure rozhraní příkazového řádku je nainstalovaný v počítači.

## <a name="step-1---set-up-azure-cli"></a>Krok 1: nastavení rozhraní příkazového řádku Azure

### <a name="1-install-azure-cli"></a>1. instalace Azure rozhraní příkazového řádku

Můžete nainstalovat Azure rozhraní příkazového řádku pro Windows, Linux nebo MAC. Podle těchto kroků se musí dokončit před můžete spravovat DNS Azure pomocí rozhraní příkazového řádku Azure. Další informace najdete na [instalace Azure rozhraní příkazového řádku](../xplat-cli-install.md). Příkazy DNS vyžadují rozhraní příkazového řádku Azure verze 0.9.8 nebo novější.

Všechny příkazy poskytovatele sítě v rozhraní příkazového řádku může být dokončíte tento příkaz:

    azure network

### <a name="2-switch-cli-mode"></a>2. režim rozhraní příkazového řádku přepnout

Azure DNS používá správce prostředků Azure. Zkontrolujte, že přepnete rozhraní příkazového řádku režim ARM příkazy.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. přihlásit ke svému účtu Azure

Zobrazí se výzva k ověření pomocí svých přihlašovacích údajů. Mějte na paměti, že můžete použít jenom INVESTOVAT účty.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Vyberte předplatné

Zvolte, které předplatné Azure používat.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. vytvořit skupinu zdrojů

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Se používá jako výchozí umístění pro zdroje v dané skupině zdroje. Však vzhledem k tomu všechny zdroje DNS globální, ne místní, výběr umístění skupiny zdrojů nemá žádný vliv na Azure DNS.

Pokud používáte existující skupiny zdrojů, můžete tento krok přeskočit.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. register

Služba Azure DNS spravuje poskytovatel Microsoft.Network zdroje. Předplatného Azure je potřeba registrován na používání poskytovatele tohoto zdroje, než budete moct použít Azure DNS. Toto je jednorázové operace pro každého předplatného.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Krok 2 – Vytvoření zóny DNS

Zóny DNS se vytvoří pomocí `azure network dns zone create` příkaz. Volitelně můžete vytvořit zóny DNS spolu s značky. Značky jsou seznam párů název hodnota a používají Azure správcem na popisek materiály pro účely seskupení a fakturace. Další informace o značky najdete v článku [použití značek k uspořádání Azure zdroje](../resource-group-using-tags.md).

Do pole Server Azure DNS zóny jmen stanovit bez ukončení **"."**. Třeba jako "**contoso.com**" nekoupili "**contoso.com.**".


### <a name="to-create-a-dns-zone"></a>Vytvoření zóny DNS

Následující příklad vytvoří zóny DNS s názvem *contoso.com* ve skupině zdroje s názvem *MyResourceGroup*.

V příkladu slouží k vytvoření DNS zónu, nahrazení hodnoty pro vlastní.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Vytvoření DNS pásma a značky.

Azure rozhraní příkazového řádku DNS podporuje značky zóny DNS rozlišit pomocí volitelné *-značku* parametr. Následující příklad ukazuje, jak vytvořit zóny DNS s dvěma značky, projektu = ukázku a obálka = test.

Umožňuje vytvořit zóny DNS a značky, nahrazení hodnoty pro vlastní následujícím příkladu.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Zobrazení záznamů

Vytvoření zóny DNS vytváří také následující záznamy DNS:

- Záznam "Spustit z SOA" (Start). Toto je prezentovat u původního příspěvku každé zóny DNS.

- Záznamy autoritativní názvového serveru (NS). Tyto zobrazení, které názvové servery jsou hostingu zóny. Azure DNS pomocí fondu názvové servery a tak jiné názvové servery můžete přidělovat jiné oblasti v Azure DNS. Další informace najdete v tématu [delegáta domain Azure DNS](dns-domain-delegation.md) .

Chcete-li zobrazit tyto záznamy, použijte `azure network dns-record-set show`.<BR>
*Použití: dns sítě sada záznamů zobrazit < skupina zdroje >< název dns zone > <name><type>*


V následujícím příkladu po spuštění příkazu s zdrojů skupina *myresourcegroup*, záznam nastavte název *"@"* (pro záznam kořenové) a zadejte *SOA*, přinese výstupu takto:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
K zobrazení vytvořená pomocí zónu záznamy názvového serveru, použijte tento příkaz:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Záznam nastaví na kořenové (nebo *vrcholu*) použití zóny DNS **@** jako záznam nastavte název.

## <a name="test"></a>Test

Pomocí nástroje DNS například nslookup VYKOPAT, můžete otestovat DNS zónu nebo `Resolve-DnsName` rutiny prostředí PowerShell.

Pokud nebyly dosud delegované vaši doménu používat nové zóny Azure DNS, musíte přímé dotaz DNS přímo na názvové servery zóny. Názvové servery zóny jsou uvedeny v záznamy názvového serveru jako výše uvedené tak, že "Zobrazit sada záznamů dns azure síť". Ujistěte se, náhradní správné hodnoty zóny v příkazu dole.

Tento příklad používá VYKOPAT k vytvoření dotazu contoso.com doménu používat názvové servery přiřazeny zóny DNS. Dotaz obsahuje tak, aby ukazovaly názvový server, pro kterou jsme použili * @ * a název zóny pomocí VYKOPAT.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Další kroky

Po vytvoření zóny DNS vytvořte [sady záznamů a záznamů](dns-getstarted-create-recordset-cli.md) spustíte Překlad jmen pro vaši doménu Internet.
