<properties
    pageTitle=" Měřítko streamování koncové body pomocí portálu Azure | Microsoft Azure"
    description="Tento kurz vás provede jednotlivými kroky nastavení velikosti datových proudů koncové body pomocí portálu Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Změna velikosti datových proudů koncové body pomocí portálu Azure

##<a name="overview"></a>Základní informace

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly videa přes adaptivní bitrate streamování klienty. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

Služba Media poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 zakódovaný obsah v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) těsně za běhu, aniž byste museli ukládat předem sbalené verze každého z nich streamování formáty.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sadu adaptivní bitrate MP4 souborů (kódování kroky jsou prokázáno dál v tomto kurzu).  
- Vytvořte alespoň jeden streamování jednotku pro *streamování koncový bod* ze které budete chtít doručení obsahu. Postupem zobrazení, jak lze změnit počet datových proudů jednotky.

Dynamické balení potřebujete jenom k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta.

Kromě toho můžete určit kapacitu streamování koncového bodu služby pro zpracování potřeb rostoucí šířky pásma úpravou streamování jednotky. Doporučujeme přidělit jednu nebo více jednotek měřítko pro aplikace v pracovním prostředí. Streamování jednotky poskytnout kapacity i vyhrazené výstupní, který se dá zakoupit úsecích 200 MB / a další funkce které funkce, která obsahuje: [dynamické balení](media-services-dynamic-packaging-overview.md), CDN integraci a Upřesnit konfiguraci. Další informace najdete v tématu [Správa datových proudů koncové body pomocí portálu Azure](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Změna velikosti datových proudů koncové body

Vytvořit a změnit počet datových proudů rezervovaná jednotky, postupujte takto:

1. [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
2. V okně **Nastavení** vyberte **koncové body datových proudů**.
3. Klikněte na streamování koncový bod, který chcete změnit měřítko. 
4. Přesuňte jezdec určit počet datových proudů jednotky
 
![Přenos koncový bod](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Platí následující omezení:

- Přidělení nových streamování jednotek může trvat zhruba 20 minut dokončete. 
- Přechod z libovolné kladné hodnoty datových proudů jednotky zpět na žádný, v současné době můžete zakázat okamžitou streamování až jednu hodinu.
- Největší počet jednotek určeným pro 24 hodin se používá pro výpočet nákladů. Informace o cenách podrobnosti najdete v tématu [Media Services ceny podrobnosti](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Další kroky

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


