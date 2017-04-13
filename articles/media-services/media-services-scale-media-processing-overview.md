<properties
    pageTitle="Měřítko médií zpracování přehled | Microsoft Azure"
    description="V tomto tématu se dozvíte měřítka zpracování médium s Azure Media Services."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Změna měřítka přehled zpracování médií

Tahle stránka umožňuje uvidíte, jak a proč zobrazit zpracování média. 

## <a name="overview"></a>Základní informace

Účet Media Services je přidružené k rezervovaná typu jednotku, která určuje rychlosti, ke kterému jsou zpracovány médií zpracování úkolů. Můžete vybrat mezi následujícími typy rezervovaná jednotky: **S1**, **S2**nebo **S3**. Například stejné kódování dlouhodobě spuštěná úloha rychlejší při použití **S2** vyhrazená jednotka typ porovnat s typem **S1** . Další informace najdete v tématu [Vyhrazené typy jednotku](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Kromě určující typ rezervovaná jednotky, můžete určit zřízení účtu s rezervovaná jednotky. Počet jednotek, zřizování rezervovaná zjistí počet médií úkoly, které budou v něm zpracovány souběžně daného účtu. Například pokud váš účet má pět rezervovaná jednotky, pak pět médií úkoly se probíhá souběžně tak dlouho, co je potřeba úkoly ke zpracování. Zbývající úkoly počká ve frontě a bude získat přiděleno pro zpracování postupně po dokončení pracovního úkolu. Pokud účet nemá žádné rezervovaná jednotky zřízení, pak úkoly bude vybráno postupně. V tomto případě prodleva mezi jeden úkol dokončení a další jedna počáteční závisí na dostupnosti zdrojů v systému.

## <a name="choosing-between-different-reserved-unit-types"></a>Volba mezi typy různé rezervovaná jednotek

V následující tabulce pomáhá při nastavení rozhodnutí při výběru mezi různé kódování rychlosti. Ho taky obsahuje několik srovnávacích případy a adresy URL přidružení zabezpečení, které můžete použít ke stažení videa na kterých můžete otestovat vlastní:

Scénáře|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Zamýšlené použití případu| Jednoduché bitrate kódování. <br/>Soubory na ss nebo nižší rozlišení, není čas citlivé, nízké náklady.|Jeden bitrate více bitrate kódování a.<br/>Obvyklé použití pro kódování ss a HD. |Jeden bitrate více bitrate kódování a.<br/>Úplné HD 4K rozlišení videa a. Čas citlivé, rychlejší vyřízení kódování. 
Srovnávací|[Vstupního souboru: 5 minut dlouhé 640x360p na 29,97 snímků za sekundu](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Kódování jednoho bitrate souboru MP4, ve stejné rozlišení trvá asi 11 minut.|[Vstupní soubor: 5 minut dlouhé 1280x720p na 29,97 snímků za sekundu](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Kódování s "H264 jednoho Bitrate 720p" přednastavené přijímá přibližně 5 minut.<br/><br/>Kódování s "H264 více Bitrate 720p" přednastavení trvá asi 11.5 minut.|[Vstupního souboru: 5 minut dlouhé 1920x1080p na 29,97 snímků za sekundu](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Kódování s "H264 jednoho Bitrate 1080p" přednastavené trvá asi 2.7 minut.<br/><br/>Kódování s "H264 více Bitrate 1080p" přednastavení trvá asi 5.7 minut.

##<a name="considerations"></a>Co byste měli zvážit

>[AZURE.IMPORTANT] Zkontrolujte důležité údaje související se popsaná v této části.  

- Rezervovaná jednotky práce paralelním prováděním všechny zpracování média, včetně indexování úlohy pomocí Azure Media indexování.  Však na rozdíl od kódování indexovací úlohy není získat zpracovány rychleji s rychleji rezervovaná jednotky.

- Pokud pomocí fondu sdílené, to znamená, bez rezervovaná jednotky, potom úkolů kódovat mají stejné výkonu s S1 RUs. Existuje však žádné horní mez čas úkolů můžete stráví ve frontě stav a kdykoli, nejvýše jeden úkol je běžet.

- Následující datacentrech nenabízejí typ jednotky **S2** vyhrazená: Brazílie jih Indie západní, Indie střední a Jižní Indie.

- Následující datacentrech nenabízejí typ jednotky **S3** vyhrazená: Brazílie jih Indie Západní Indie centrální.

- Největší počet jednotek určeným pro 24 hodin se používá pro výpočet nákladů.


##<a name="quotas-and-limitations"></a>Kvót a omezení

Informace o kvótách a omezení a jak můžete otevřít požadavek podpory můžete najdete v článku [kvóty](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Další krok

Dosažení měřítka úkol zpracování médií s některým z těchto technologie: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portál](media-services-portal-scale-media-processing.md)
- [ZBÝVAJÍCÍ](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
