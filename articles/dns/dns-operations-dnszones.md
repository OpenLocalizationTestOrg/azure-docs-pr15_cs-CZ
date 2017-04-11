<properties
   pageTitle="Správa zóny DNS pomocí prostředí PowerShell | Microsoft Azure"
   description="Můžete spravovat pomocí prostředí Powershell Azure zóny DNS. Jak se dají aktualizovat, odstranit nebo vytvořit zóny DNS na Azure DNS"
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

# <a name="how-to-manage-dns-zones-using-powershell"></a>Jak spravovat zóny DNS pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure rozhraní příkazového řádku](dns-operations-dnszones-cli.md)
- [Prostředí PowerShell](dns-operations-dnszones.md)



Tento článek vám ukáže, jak spravovat DNS zone pomocí prostředí PowerShell. Abyste mohli použít tento postup, musíte nainstalovat nejnovější verzi rutiny prostředí PowerShell správce prostředků Azure (1.0 nebo novější). Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.


## <a name="create-a-new-dns-zone"></a>Vytvoření nové zóny DNS

Vytvoření zóny DNS najdete v tématu [vytvoření zóny DNS pomocí prostředí PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Získání zóny DNS

Zóny DNS načíst, použít `Get-AzureRmDnsZone` rutiny. Tuto operaci vrátí objekt zóny DNS odpovídající do existující oblasti v Azure DNS. Objekt obsahuje data o zónu (jako je třeba počet sady záznamů), ale neobsahuje sami sady záznamů.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Zóny DNS seznamu

Vynecháním název zóny z `Get-AzureRmDnsZone`, můžete vytvořit výčet všechny zóny ve skupině zdroje. Tuto operaci vrátí matici zóny objektů.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Aktualizace zóny DNS

Zdroj zóny DNS můžete měnit pomocí `Set-AzureRmDnsZone`. To neaktualizuje některý ze sady záznamů DNS v oblasti (přečtěte si, [jak spravovat službu DNS záznamy](dns-operations-recordsets.md)). Používá se pouze aktualizovat vlastnosti zóny zdroje samotné. Toto je aktuálně omezené Azure zdroje správci "značky" zóny zdroje. Další informace najdete v článku [Etags a značky](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Použijte jeden z těchto věcí dva způsoby, jak aktualizovat DNS zónu:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Určení zónu pomocí zóny prostředků a název skupiny

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Určení zónu pomocí $zone objektu

Určení zónu pomocí $zone objektu z `Get-AzureRmDnsZone`. Při použití `Set-AzureRmDnsZone` s objektem $zone Etag kontroly se použijí k zajištění souběžné změny nejsou přepsat. Můžete použít volitelné *-Přepsat* přepnout potlačí tyto kontroly. Další informace najdete v článku [Etags a značky](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Odstranění zóny DNS

Zóny DNS můžete odstranit pomocí rutiny AzureRmDnsZone odebrat.

Před odstraněním zóny DNS v Azure DNS, musíte odstranit všechny sady záznamů, s výjimkou názvového serveru a SOA záznamy na kořenové úrovni zóny, které byly vytvořeny automaticky při vytvoření zóny.

Použijte jeden z následujících dvou způsobů odebrat zóny DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Určení zóně pomocí zóny název a název skupiny prostředků

Tuto operaci má volitelný *– vyšší* přepínač, který potlačí zobrazení výzvy k potvrzení, kterou chcete odebrat zóny DNS.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Určení zónu pomocí $zone objektu

Určení zónu pomocí $zone objektu z `Get-AzureRmDnsZone`. Tuto operaci má volitelný *– vyšší* přepínač, který potlačí zobrazení výzvy k potvrzení, kterou chcete odebrat zóny DNS. Stejně jako u `Set-AzureRmDnsZone`, zadání zóně pomocí objektu $zone umožňuje Etag kontroly zajistit souběžné změny se neodstraní. <BR>
Volitelné *-Přepsat* příznak potlačí tyto kontroly. Další informace najdete v článku [Etags a značky](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Objekt zóny lze také přesměrovat místo předávány jako parametr:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Další kroky

Po vytvoření zóny DNS vytvořte [sady záznamů a záznamů](dns-getstarted-create-recordset.md) spustíte Překlad jmen pro vaši doménu Internet.