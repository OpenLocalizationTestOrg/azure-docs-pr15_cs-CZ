<properties
   pageTitle="Správa sady záznamů DNS a záznamů pomocí portálu Azure | Microsoft Azure"
   description="Správa sady záznamů DNS a záznamy v Azure DNS vaší domény na Azure DNS hostingu. Všechny příkazy Powershellu pro operace sady záznamů a záznamů."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>Spravovat záznamy DNS a sady záznamů pomocí prostředí PowerShell



> [AZURE.SELECTOR]
- [Portál Azure](dns-operations-recordsets-portal.md)
- [Azure rozhraní příkazového řádku](dns-operations-recordsets-cli.md)
- [Prostředí PowerShell](dns-operations-recordsets.md)



Tento článek popisuje, jak spravovat sady záznamů a záznamy zóny DNS pomocí prostředí Windows PowerShell.

Je důležité pochopit rozdíl mezi sady záznamů DNS a jednotlivých záznamů DNS. Sada záznamů je v kolekci záznamů v zóně, které mají stejný název a jsou stejného typu. Další informace najdete v tématu [Vytvoření DNS záznamu sady a záznamů pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).

Ke správě sady záznamů a záznamů, musíte nejnovější verzi rutiny prostředí PowerShell Azure správce prostředků. Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Další informace o práci s Powershellu najdete v článku [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Vytvořit novou sadu záznam a záznamu

Vytvoření záznamu můžete nastavit pomocí prostředí PowerShell najdete v tématu [Vytvoření DNS záznamu sady a záznamů pomocí prostředí PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Získání sada záznamů

Pokud chcete načíst existující sada záznamů, použijte `Get-AzureRmDnsRecordSet`. Určení vyžadovaného že záznam nastavení relativní název, typ záznamu a zóny.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Stejně jako u `New-AzureRmDnsRecordSet`, název záznamu musí být relativní název, což znamená, musíte vyloučit název zóny.

Je možné zadat zónu pomocí zóny název a název skupiny prostředků nebo pomocí objektu pásma:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Vrátí místní objekt, který představuje sada záznamů, který byl vytvořený v Azure DNS.

## <a name="list-record-sets"></a>Seznam sady záznamů

Můžete taky použít`Get-AzureRmDnsRecordSet` do seznamu sady záznamů o vynecháme-li *– název* a/nebo *– RecordType* parametry.

### <a name="to-list-all-record-sets"></a>Chcete-li zobrazit všechny sady záznamů

V tomto příkladu vrací všechny sady záznamů, bez ohledu na jméno nebo typ záznamu:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Do seznamu sady záznamů daného typu záznamu

V tomto příkladu vrací všechny sady záznamů, které odpovídají typu daného záznamu. V tomto případě tedy sady záznamů vrátila je "Na" záznamů:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Zóny lze nastavit pomocí buď *– Název_zóny* a *– ResourceGroupName* parametry (znázorněná) nebo zadáním objektu pásma:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Přidání záznamu do sady záznamů

Přidání záznamů do sady záznamů pomocí `Add-AzureRmDnsRecordConfig` rutiny. Toto je operaci offline. Dojde ke změně jen místní objekt, který představuje sady záznamů.

Parametry pro přidávání záznamů do sady záznamů lišit v závislosti na typu sady záznamů. Třeba při použití sady záznamů typu "A", můžete zadat pouze záznamy s parametrem *-Adresa_ipv4*.

Další záznamy lze přidat do každého záznamu nastavit voláním Další `Add-AzureRmDnsRecordConfig`. Přidání až 20 záznamů do libovolné sadě záznamů. Však sady záznamů typu "CNAME" může obsahovat maximálně jeden záznam a sada záznamů nesmí obsahovat dva identickými záznamy. Prázdný sady záznamů (s nulovou záznamů) se dají vytvářet, ale na názvové servery Azure DNS nezobrazují.

Po sady záznamů obsahuje požadovanou sadu záznamů, je potřeba potvrdit pomocí `Set-AzureRmDnsRecordSet` rutiny. Po sada záznamů potvrzené, nahradíte tím to stávající sady záznamů v Azure DNS.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Chcete-li vytvořit záznam a nastavit jeden záznam

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Pořadí operací vytvořit záznam může být také *kanálu*, což znamená, že předejte objekt sada záznamů pomocí kanálu, spíše než předáním jako parametr. Příklad:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Příklady další typ záznamu

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Upravit existující sady záznamů

Postup úpravy existující sada záznamů jsou podobné kroky, kterými při vytváření záznamů. Pořadí operací vypadá takto:

1.  Načtení existujícího záznamu můžete nastavit pomocí `Get-AzureRmDnsRecordSet`.

2.  Úprava sady záznamů podle přidávání záznamů, odebrání záznamů, změně záznamu parametry nebo změně záznamu nastavte time to live (TTL). Toto je operaci offline. Dojde ke změně jen místní objekt, který představuje sady záznamů.

3.  Uložte provedené změny pomocí `Set-AzureRmDnsRecordSet` rutiny. To nahradí existující sady záznamů v Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Aktualizujte záznam v existující sada záznamů

V tomto příkladu Změníme IP adresu existující "" záznamu:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

`Set-AzureRmDnsRecordSet` Rutina používá etag kontroly zajistit, že nejsou přepíšou souběžné změny. Použití *-Přepsat* příznak potlačí tyto kontroly. Další informace najdete v tématu [o etags a značky](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Úprava záznamu SOA

Nelze přidat nebo odebrat záznamy z automaticky vytvoří SOA sady záznamů ve vrcholu zóna (název = "@"). Však můžete změnit libovolné parametrů záznamu SOA (s výjimkou "host (hostitel)") a záznam nastavte hodnotu TTL.

Následující příklad ukazuje, jak změnit vlastnost *e-mailu* SOA záznamu:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Chcete-li změnit záznamy názvového serveru na vrcholu zóny

Nelze přidat, odebrat nebo změnit záznamy v automaticky vytvoří NS sady záznamů ve vrcholu zóny (název = "@"). Pouze změny, které je povolen je upravit sady záznamů TTL.

Následující příklad ukazuje, jak změna TTL vlastnosti sady záznamů názvového serveru:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Přidání záznamů do existující sada záznamů

V tomto příkladu jsme dva další záznamy MX přidáte do sady existujícího záznamu:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Odebrání záznamu z existující sady záznamů

Záznamy je možné odebrat ze záznamu můžete nastavit pomocí `Remove-AzureRmDnsRecordConfig`. Záznam, který má být odebrán musí být přesné shody se stávajícího záznamu přes všechny parametry. Změny musí potvrzeného pomocí `Set-AzureRmDnsRecordSet`.

Odebrání posledním záznamu v sadě záznamů neodstraní sady záznamů. Další informace naleznete v tématu [Odstranění sady záznamů](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Pořadí operací chcete nějaký záznam odebrat ze sady záznamů lze také přesměrovat, což znamená, že předejte objekt sada záznamů pomocí kanálu, spíše než předáním jako parametr. Příklad:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Odebrání záznamu AAAA v sadě záznamů

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Odebrání záznamu CNAME sada záznamů

Sada záznamů CNAME může obsahovat maximálně jeden záznam, odebrání tohoto záznamu ponechá prázdné sadě záznamů.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Odebrání záznamu MX sada záznamů

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Záznam názvového serveru odebrání sada záznamů

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Odebrání sada záznamů SRV záznamu

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Odebrání záznamu TXT v sadě záznamů

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Odstranění sada záznamů

Sady záznamů můžete odstranit pomocí `Remove-AzureRmDnsRecordSet` rutiny. Nelze odstranit nebo SOA a sady záznamů názvového serveru na vrcholu zóna (název = "@") vytvořené automaticky při vytvoření zóny. Budou odstraněny automaticky po odstranění zóny.

Použijte jeden z těchto tří způsobů Pokud chcete odebrat sadu záznamů:

### <a name="specify-all-the-parameters-by-name"></a>Určení všechny parametry podle názvu

Volitelné *– vyšší* přepnout mohou sloužit k potlačit okně pro potvrzení.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Určit sady záznamů tak, že název a typ a zónu objektem

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Určení sady záznamů podle objektu

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Při zadávání záznamu můžete nastavit pomocí objektu, umožňuje etag kontroly zajistit, aby se neodstraní souběžné změny. Volitelné *-Přepsat* příznak potlačí tyto kontroly. Další informace najdete v článku [Etags a značky](dns-getstarted-create-dnszone.md#tagetag) .

Sada záznamů objekt lze také přesměrovat místo předávány jako parametr:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Další kroky

Další informace o Azure DNS najdete v tématu [Přehled Azure DNS](dns-overview.md). Informace o automatizaci DNS najdete v tématu [vytváření DNS zones a záznam nastaví pomocí .NET SDK](dns-sdk.md).

Další informace o zpětném DNS záznamy najdete v tématu [Správa obráceném záznamy DNS pro služby pomocí Powershellu](dns-reverse-dns-record-operations-ps.md).
