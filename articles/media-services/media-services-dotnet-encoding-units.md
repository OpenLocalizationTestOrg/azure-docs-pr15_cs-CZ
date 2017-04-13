<properties 
    pageTitle="Jak přidat kódování jednotky" 
    description="Zjistěte, jak přidat kódování jednotek s .NET"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Jak zobrazit kódování s .NET SDK

> [AZURE.SELECTOR]
- [Portál](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Základní informace

>[AZURE.IMPORTANT] Ujistěte se, chcete-li zobrazit v tématu [Přehled](media-services-scale-media-processing-overview.md) získat další informace o měřítka médií zpracování tématu.
 
Pokud chcete změnit typ rezervovaná jednotky a počet jejích kódování rezervovaná jednotky pomocí .NET SDK, postupujte takto:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Otevření požadavek podpory můžete

Ve výchozím nastavení všech účtů Media Services můžete přizpůsobit až 25 kódování a 5 na vyžádání, datových proudů rezervovaná jednotky. Vyšší mezní hodnoty můžete požádat o otevřením požadavek podpory můžete.

###<a name="open-a-support-ticket"></a>Otevřete požadavek podpory můžete

Chcete-li otevřít technické podpory lístek takto:

1. Klikněte na, [využijte možnost získání podpory](https://manage.windowsazure.com/?getsupport=true). Pokud nejste přihlášeni, se zobrazí výzva k zadání přihlašovacích údajů.

1. Vyberte předplatné.

1. Ve skupinovém rámečku Typ podporu vyberte "Technické".

1. Klikněte na "Vytvořit lístek".

1. Vyberte "Azure Media Services" v seznamu produktů prezentovat na další stránce.

1. Výběr typu"problém", která je vhodné pro váš problém.

1. Klikněte na pokračovat.

1. Postupujte podle pokynů na další stránce a potom zadejte údaje o problém.

1. Klikněte na Odeslat otevřete lístek.



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
