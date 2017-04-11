<properties
   pageTitle="Začínáme se službou Azure DNS | Microsoft Azure"
   description="Naučte se vytvářet zóny DNS pro službu Azure DNS. Toto je krok za krokem získat vaše první zóny DNS vytvořili pro spuštění hostingu DNS domény pomocí prostředí PowerShell."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-a-dns-zone-using-powershell"></a>Vytvoření DNS zónu pomocí prostředí Powershell

> [AZURE.SELECTOR]
- [Azure portálu](dns-getstarted-create-dnszone-portal.md)
- [Prostředí PowerShell](dns-getstarted-create-dnszone.md)
- [Azure rozhraní příkazového řádku](dns-getstarted-create-dnszone-cli.md)

Tento článek vás provede jednotlivými kroky k vytvoření zóny DNS pomocí prostředí PowerShell. Můžete taky vytvořit pomocí rozhraní příkazového řádku nebo portálu Azure zóny DNS.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Informace o Etags a značky

### <a name="etags"></a>Etags

Předpokládejme, že dva lidé nebo dva procesy pokusíte upravit záznam DNS ve stejnou dobu. Které z nich wins? A nápoje ví, že budou jste právě přepíšou změny vytvořenou někým jiným?

Azure DNS používá Etags ke správě souběžné změny stejný zdroj bezpečné. Etag přidružen má každý zdroj DNS (zóny nebo sada záznamů). Kdykoli načte zdroj jeho Etag také načíst. Při aktualizaci zdroje, máte možnost předávat zpět Etag tak Azure DNS můžete ověřte, že Etag shody serveru. Protože každou aktualizaci zdroji výsledkem Etag obnovována, Etag neshodu označuje, že došlo ke změně souběžné. Etags také při vytváření nového prostředku zajistit, že zdroje dosud neexistuje.

Ve výchozím nastavení používá Azure DNS PowerShell Etags blokovat souběžné změny do zón a záznam sady. Volitelné *-Přepsat* mohou sloužit k potlačit Etag kontroly, v tomto případě souběžné změny, které se uskutečnily přepíše přepínač.

Na úrovni Azure DNS REST API jsou určeny Etags pomocí protokolu HTTP záhlaví.  V následující tabulce je uveden jejich chování:

|Záhlaví|Chování|
|------|--------|
|Žádná|UMÍSTĚNÍ povede vždy (žádné kontroly Etag)|
|Když POZVYHLEDAT<etag>|Vložit pouze úspěšná, pokud existuje zdroje a jsou vraceny Etag|
|Když POZVYHLEDAT *     | Vložit pouze úspěšná, pokud existuje zdroje|
|Když žádné časově POZVYHLEDAT * |  Vložit pouze úspěšná, pokud není k dispozici zdroje|

### <a name="tags"></a>Značky

Značky se liší od Etags. Značky jsou seznam párů název hodnota a používají Azure správcem na popisek materiály pro účely seskupení a fakturace. Další informace o značky najdete v článku [použití značek k uspořádání Azure zdroje](../resource-group-using-tags.md).

Azure prostředí PowerShell DNS podporuje značky na zón a sady záznamů zadána pomocí možností `-Tag` parametr.


## <a name="before-you-begin"></a>Než začnete

Zkontrolujte, jestli máte následující položky před zahájením konfiguraci.

- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [Web MSDN pro odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

- Musíte nainstalovat nejnovější verzi rutiny prostředí PowerShell správce prostředků Azure (1.0 nebo novější). Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

## <a name="step-1---sign-in"></a>Krok 1: přihlášení

Spusťte konzolu prostředí PowerShell a připojit se ke svému účtu. Další informace najdete v tématu [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

K připojení použijte v následujícím příkladu:

    Login-AzureRmAccount

Zaškrtněte políčko předplatná pro účet.

    Get-AzureRmSubscription

Určete předplatné, ke které chcete použít.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Krok 2 – Vytvoření skupiny prostředků

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Se používá jako výchozí umístění pro zdroje v dané skupině zdroje. Však vzhledem k tomu všechny zdroje DNS globální, ne místní, výběr umístění skupiny zdrojů nemá žádný vliv na Azure DNS.

Pokud používáte existující skupiny zdrojů, můžete tento krok přeskočit.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Krok 3: registrace

Služba Azure DNS spravuje poskytovatel Microsoft.Network zdroje. Předplatného Azure je potřeba registrován na používání poskytovatele tohoto zdroje, než budete moct použít Azure DNS. Toto je jednorázové operace pro každého předplatného.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Krok 4 – vytvoření zóny DNS

Zóny DNS vytvořený s využitím `New-AzureRmDnsZone` rutiny. Existuje příklady pod pro vytváření zóny DNS pomocí hesla nebo bez značky. Další informace o značek naleznete v části [značky](#tags) v tomto článku.

>[AZURE.NOTE] Do pole Server Azure DNS zóny jmen stanovit bez ukončení **"."**. Třeba jako "**contoso.com**" nekoupili "**contoso.com.**".

### <a name="to-create-a-dns-zone"></a>Vytvoření zóny DNS

Následující příklad vytvoří zóny DNS s názvem *contoso.com* ve skupině zdroje s názvem *MyResourceGroup*. Umožňuje vytvořit zónu DNS zaokrouhlením hodnoty pro vlastní příkladu.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Vytvoření zóny DNS se značkami

Následující příklad ukazuje, jak vytvořit zóny DNS s dvěma značky *projektu = ukázku* a *Obálka = test*. Umožňuje vytvořit zónu DNS zaokrouhlením hodnoty pro vlastní příkladu.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Zobrazení záznamů

Vytvoření zóny DNS vytváří také následující záznamy DNS:

- Záznam *Start autorita* (SOA). Toto je prezentovat u původního příspěvku každé zóny DNS.

- Záznamy autoritativní názvového serveru (NS). Tyto zobrazení, které názvové servery jsou hostingu zóny. Azure DNS pomocí fondu názvové servery a tak jiné názvové servery může přidělovat jiné oblasti v Azure DNS. V tématu Další informace [delegáta domain Azure DNS](dns-domain-delegation.md) .

Chcete-li zobrazit tyto záznamy, použijte `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Záznam nastaví na kořenové (nebo *vrcholu*) použití zóny DNS **@** jako záznam nastavte název.


## <a name="test"></a>Test

Pomocí nástroje DNS například nslookup, Vykopat nebo [rutiny prostředí PowerShell vyřešit Název_dns](https://technet.microsoft.com/library/jj590781.aspx)můžete otestovat zóny DNS.

Pokud nebyly dosud delegované vaši doménu používat nové zóny Azure DNS, musíte se směrování dotazu DNS přímo na názvové servery zóny. Názvové servery zóny jsou uvedeny v záznamy názvového serveru, jak je uvedeno tak, že `Get-AzureRmDnsRecordSet` nad. Ujistěte se, náhradní správné hodnoty zóny do příkazu dole.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Další kroky

Po vytvoření zóny DNS vytvořte [sady záznamů a záznamů](dns-getstarted-create-recordset.md) spustíte Překlad jmen pro vaši doménu Internet.

