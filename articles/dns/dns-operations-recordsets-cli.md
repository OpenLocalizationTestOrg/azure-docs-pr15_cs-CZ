<properties
   pageTitle="Správa sady záznamů DNS a záznamy DNS Azure pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Správa sady záznamů DNS a záznamy v Azure DNS vaší domény na Azure DNS hostingu. Všechny příkazy rozhraní příkazového řádku pro operace sady záznamů a záznamů."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Spravovat záznamy DNS a sady záznamů pomocí rozhraní příkazového řádku


> [AZURE.SELECTOR]
- [Portál Azure](dns-operations-recordsets-portal.md)
- [Azure rozhraní příkazového řádku](dns-operations-recordsets-cli.md)
- [Prostředí PowerShell](dns-operations-recordsets.md)


Tento článek popisuje, jak spravovat sady záznamů a záznamy zóny DNS pomocí různé platformy Azure rozhraní příkazového řádku (rozhraní příkazového řádku).

Je důležité pochopit rozdíl mezi sady záznamů DNS a jednotlivých záznamů DNS. Sada záznamů je sada záznamů v zóně, které mají stejný název a jsou stejného typu. Další informace najdete v tématu [Principy sady záznamů a záznamy](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Konfigurace rozhraní příkazového řádku Azure různé platformy

Je to Azure DNS služby Azure jen správce prostředků. Rozhraní API Správa služby Azure nemuselo mít. Zkontrolujte, že je režim správce prostředků pomocí nakonfigurované Azure rozhraní příkazového řádku `azure config mode arm` příkaz.

Pokud se zobrazí **Chyba: "dns" není příkaz azure**, je velmi pravděpodobné, protože používáte Azure rozhraní příkazového řádku v režimu Správa služby Azure, ne v režimu správce prostředků.

## <a name="create-a-new-record-set-and-record"></a>Vytvoření nové sady záznamů a záznam

Pokud chcete vytvořit nový záznam v portálu Azure nastavit, najdete v článku [Vytvoření sady záznamů a záznamy](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Načtení sada záznamů

Pokud chcete načíst existující sada záznamů, použijte `azure network dns record-set show`. Zadat skupinu prostředků, názvu zóny záznam nastavení relativní název a typ záznamu. Pomocí níže uvedeném příkladu, nahradit hodnoty vlastní.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Seznam sady záznamů

Vypíše všechny záznamy zóny DNS pomocí `azure network dns record-set list` příkaz. Budete muset zadat název zdroje a název zóny.

### <a name="to-list-all-record-sets"></a>Chcete-li zobrazit všechny sady záznamů

V tomto příkladu vrací všechny sady záznamů, bez ohledu na jméno nebo typ záznamu:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Do seznamu sady záznamů daného typu

V tomto příkladu vrací všechny sady záznamů, které odpovídají dané typ záznamu (v tomto případě záznamy "A"):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Přidání záznamu do sady záznamů

Přidání záznamů do sady záznamů pomocí `azure network dns record-set add-record`příkaz. Parametry pro přidávání záznamů do sady záznamů lišit v závislosti na typu záznamu, který je nastaven. Třeba když použijete sadu záznamu typu "A", můžete zadat pouze záznamy s parametrem `-a <IPv4 address>`.

Vytvořit sadu záznamů, můžete `azure network dns record-set create`příkaz. Zadat skupinu prostředků, názvu zóny záznamu nastavit relativní název, typ záznamu a čas live (TTL). Pokud `--ttl` není definované parametr, čtyři výchozí hodnota (v sekundách).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Po vytvoření "Na" sady záznamů přidejte adresu IPv4 pomocí `azure network dns record-set add-record`příkaz.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Následující příklady ukazují, jak vytvořit záznam každý typ záznamu. Každý záznam sada obsahuje jeden záznam.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Aktualizujte záznam v sadě záznamů

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Přidání jiné IP adresy (1.2.3.4) do existující "" záznamu nastavit ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Pokud chcete odebrat existující hodnotu ze sady záznamů
Použití `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Odebrání záznamu z sada záznamů

Záznamy je možné odebrat ze záznamu můžete nastavit pomocí `azure network dns record-set delete-record`. Záznam, který má být odebrán musí být přesné shody se stávajícího záznamu přes všechny parametry.

Odebrání posledním záznamu v sadě záznamů neodstraní sady záznamů. Další informace naleznete v části tohoto článku s názvem [Odstranit sady záznamů](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Odebrání záznamu AAAA v sadě záznamů

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Odebrání záznamu CNAME sada záznamů

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Odebrání záznamu MX sada záznamů

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Záznam názvového serveru odebrání sada záznamů

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Odebrání záznamu PTR v sadě záznamů
V tomto případě "my-arpa-server zone.com" představuje zónu ARPA představující rozsah IP.  Každý PTR sady záznamů v této zóně odpovídá IP adresu tohoto rozsahu IP.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Odebrání sada záznamů SRV záznamu

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Odebrání záznamu TXT v sadě záznamů

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Odstranění sada záznamů

Sady záznamů můžete odstranit pomocí `Remove-AzureRmDnsRecordSet` rutiny. Nelze odstranit nebo SOA a sady záznamů názvového serveru na vrcholu zóna (název = "@") vytvořené automaticky při vytvoření zóny. Budou odstraněny automaticky po odstranění zóny.

V následujícím příkladu "" záznamu nastavit "test d" odeberou z zóny DNS "contoso.com":

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Volitelné *– q* přepnout mohou sloužit k potlačit okně pro potvrzení.


## <a name="next-steps"></a>Další kroky

Další informace o Azure DNS najdete v tématu [Přehled Azure DNS](dns-overview.md). Informace o automatizaci DNS najdete v tématu [vytváření DNS zones a záznam nastaví pomocí .NET SDK](dns-sdk.md).

Pokud chcete pracovat s obráceném DNS záznamy, najdete v tématu [Správa obráceném záznamy DNS pro služby pomocí rozhraní příkazového řádku Azure](dns-reverse-dns-record-operations-cli.md).
