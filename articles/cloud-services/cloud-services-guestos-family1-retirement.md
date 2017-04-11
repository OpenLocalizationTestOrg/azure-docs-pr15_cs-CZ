<properties
   pageTitle="Všimněte si 1 vyřazování webů hosta operačních systémů | Microsoft Azure"
   description="Informace o kdy došlo k vyřazování webů Azure hosta OS řady 1 a jak lze zjistit, pokud se vás týká, obsahuje"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Oznámení hosta OS řady 1 vyřazování webů

Vyřazování webů s operačním systémem řady 1 byl nejdřív ohlásí na 1 června 2013.

**2 z 2014** Azure hostovaného operačního systému (s operačním systémem hosta) řady úředně důchodu 1.x, který je založený na operačního systému Windows Server 2008. Všechny pokusy o nasazení nové služby nebo upgrade existující služby pomocí řady 1 se nepovede s chybovou zprávou informující o, aby byla už nepoužívá hostem OS řady 1.

**3 listopadu 2014** Rozšířená podpora pro hosta OS řady 1 a plně ukončení. Bude mít vliv na všechny stránky služby pořád řady 1. Společnost Microsoft může přestat tyto služby kdykoli. Není nijak zaručené, které služby, zůstanou spustit, pokud jste ručně pořídit sami.

Pokud máte další otázky, navštivte [Fóra pro Cloud Services](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) nebo [Kontaktujte podporu Azure](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Se vás týká?

Cloudovým službám se týká, pokud platí některá z následujících akcí:

1. Máte hodnotu "osFamily ="1"výslovně zadaný v souboru ServiceConfiguration.cscfg cloudové službě.
2. Nemáte hodnotu osFamily výslovně zadaný v souboru ServiceConfiguration.cscfg cloudové službě. V současné době systému používá v tomto případě výchozí hodnotu "1".
3. Azure klasické portál uvede hodnota řady hostovaný operační systém jako "Systému Windows Server 2008".

Najít, které služby cloudu běží které operačních systémů, můžete spustit skript pod v prostředí PowerShell Azure, když musíte [nastavit Azure PowerShell](../powershell-install-configure.md) nejdřív. Další informace o skript najdete v tématu [Azure hosta OS řady 1 ukončit z životnost: červen 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Cloudovým službám bude ovlivněny vyřazování webů s operačním systémem řady 1, pokud sloupci osFamily výstupu skriptu je prázdná nebo obsahuje hodnotu "1".

## <a name="recommendations-if-you-are-affected"></a>Pokud se vás týká, doporučení

Doporučujeme, abyste že na jeden z podporovaných OS rodiny hosta nejdřív migrujete role cloudové služby:

**Hostovaného OS řady 4.x** – Windows serveru 2012 R2 *(doporučeno)*

1. Ujistěte se, že aplikace používá SDK 2.1 nebo vyšší s .NET framework 4.5 4.0 a 4.5.1.
2. Nastavit atribut osFamily "4" v souboru ServiceConfiguration.cscfg a přeinstalujte cloudové služby.


**Hostovaného OS řady 3.x** – Windows serveru 2012

1. Ujistěte se, že aplikace používá SDK 1,8 nebo novější se .NET framework 4.5 nebo 4.0.
2. Nastavit atribut osFamily "3" v souboru ServiceConfiguration.cscfg a přeinstalujte cloudové služby.


**Hostovaného OS řady 2.x** – Windows Server 2008 R2

1. Ujistěte se, že aplikace používá SDK 1.3 a vyšší s .NET framework 3.5 nebo 4.0.
2. Nastavit atribut osFamily "2" v souboru ServiceConfiguration.cscfg a přeinstalujte cloudové služby.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Rozšířená podpora pro hosta OS řady 1 ukončí 3 listopadu 2014
Cloudovým službám na Host operačních systémů 1 už nejsou podporované. Migrujte vypnout řady 1 co nejdříve Chcete-li předejít přerušení služby.  

## <a name="next-steps"></a>Další kroky
Prohlédněte si nejnovější [vydání s operačním systémem Host](cloud-services-guestos-update-matrix.md).
