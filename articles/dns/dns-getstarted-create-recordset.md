<properties
   pageTitle="Vytvoření sady záznamů a záznamy zóny DNS pomocí prostředí PowerShell | Microsoft Azure"
   description="Jak vytvořit záznamy hostitele DNS Azure. Nastavení záznamu nastaví a záznamů pomocí prostředí PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Vytvoření sady záznamů DNS a záznamů pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Portál Azure](dns-getstarted-create-recordset-portal.md)
- [Prostředí PowerShell](dns-getstarted-create-recordset.md)
- [Azure rozhraní příkazového řádku](dns-getstarted-create-recordset-cli.md)

Tento článek vás provede proces vytváření záznamů a sady záznamů pomocí prostředí Windows PowerShell. Po vytvoření zóny DNS přidejte záznamy DNS pro vaši doménu. K tomuto účelu Nejdřív musíte znát záznamy DNS a sady záznamů.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Zkontrolujte, jestli máte nejnovější verzi prostředí PowerShell

Ověřte, zda jste nainstalovali nejnovější verzi rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

## <a name="create-a-record-set-and-record"></a>Vytvoření sada záznamů a záznam

Tato část popisuje jak vytvořit záznam a sadě záznamů.


### <a name="1-connect-to-your-subscription"></a>1. připojit se ke svému předplatnému

Spusťte konzolu prostředí PowerShell a připojit se ke svému účtu. K připojení použijte v následujícím příkladu:

    Login-AzureRmAccount

Zaškrtněte políčko předplatná pro účet.

    Get-AzureRmSubscription

Určete předplatné, ke které chcete použít.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Další informace o práci s Powershellu najdete v článku [použití Windows pomocí Správce prostředků](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. vytvořit sadu záznamů

Vytvoření sady záznamů pomocí `New-AzureRmDnsRecordSet` rutiny. Při vytváření sada záznamů, musíte k určení sady záznam název, zóně, time to live (TTL) a typ záznamu.

K vytvoření záznamu nastavit v vrcholu zónu (v tomto případě "contoso.com"), použijte název záznamu "@", včetně uvozovek. Toto je běžné názvů DNS.

Následující příklad vytvoří záznam sada se relativní název "www" v zóně DNS "contoso.com". Plně kvalifikovaný název sady záznamů, musí být "www.contoso.com". "A"; je typ záznamu a TTL je 60 sekund. Po dokončení tohoto kroku budete mít sada záznamů prázdné "www", přiřazená proměnné *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Pokud záznam nastavit už existuje

Pokud záznam nastavit už existuje, příkaz se nezdaří, pokud *-Přepsat* použít přepínač. *-Přepsat* možnost výzvy k potvrzení, které můžete potlačit pomocí aktivační události *– vyšší* přepnout.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


V tomto příkladu je zadat zónu pomocí zóny název a název skupiny prostředků. Můžete taky můžete použít objekt zóny jako vrácenou `Get-AzureRmDnsZone` nebo `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Vrátí místní objekt, který představuje sada záznamů, který byl vytvořený v Azure DNS.

### <a name="3-add-a-record"></a>3. přidat záznam

Použití sady záznamů nově vytvořený "www", budete muset přidat do něj záznamy. Můžete přidat IPv4 záznamy *o* záznam "www" můžete nastavit pomocí v následujícím příkladu. V tomto příkladu je založena na proměnné *$rs* nastavené v předchozím kroku.

Přidání záznamů do záznamu můžete nastavit pomocí `Add-AzureRmDnsRecordConfig` je operaci offline. Pouze lokální proměnné *$rs* se aktualizuje.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. potvrďte změny

Potvrďte změny provedené v sadě záznamů. Použití `Set-AzureRmDnsRecordSet` nahrajete změny sady Azure DNS záznamů.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. načíst sady záznamů

Můžete vyhledat záznam z Azure DNS můžete nastavit pomocí `Get-AzureRmDnsRecordSet` jak je vidět v následujícím příkladu.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Můžete také nástroje nslookup nebo další nástroje DNS k vytvoření dotazu nové sadě záznamů.

Pokud nebyly dosud delegované domény, kterou chcete názvové servery Azure DNS, musíte explicitně zadejte jméno, serveru a adresa zóny.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Vytvoření sady záznamů každého typu se jeden záznam


Následující příklady ukazují, jak vytvořit záznam každý typ záznamu. Každý záznam sada obsahuje jeden záznam.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Další kroky

[Jak spravovat zóny DNS pomocí prostředí PowerShell](dns-operations-dnszones.md)

[Spravovat záznamy DNS a sady záznamů pomocí prostředí PowerShell](dns-operations-recordsets.md)

[Automatizace Azure operace s .NET SDK](dns-sdk.md)
