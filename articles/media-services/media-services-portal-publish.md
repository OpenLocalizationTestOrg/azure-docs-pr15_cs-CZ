<properties
    pageTitle="  Publikování obsahu pomocí portálu Azure | Microsoft Azure"
    description="Tento kurz vás provede jednotlivými kroky publikování obsahu pomocí portálu Azure."
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

# <a name="publish-content-with-the-azure-portal"></a>Publikování obsahu pomocí portálu Azure

> [AZURE.SELECTOR]
- [Portál](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [ZBÝVAJÍCÍ](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Základní informace

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

Pokud chcete, aby vaše uživatele adresu URL, která mohou sloužit k můžete vysílat datovými proudy nebo stáhnout obsah, musíte nejdřív "publikovat" váš materiálů vytvořením locator. Locator poskytnutí přístupu k souborům obsažené v majetek. Media Services podporuje dva typy Locator: 

- Přenos Locator (OnDemandOrigin), používá adaptivní datového proudu (například toku MPEG POMLČKU, HLS, a přitom zajistit hladký přenos). Vytvoření datových proudů locator vaší materiálů musí obsahovat souboru .ism. 
- Postupné (přidružení zabezpečení) Locator používá pro doručování videa přes postupné stahování.


Streamování adresa URL je v tomto formátu a můžete ho přehrát hladký přenos prostředky.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Vytvářet HLS streamování adresy URL, připojení (formát = m3u8 aapl) na adresu URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Vytvoření pomlčka MPEG streamování adresy URL, připojení (formát = mpd čas csf) na adresu URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Adresa URL přidružení zabezpečení má následující formát.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Další informace najdete v tématu [Přehled doručování obsahu](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Pokud jste použili k vytvoření Locator před březnem 2015 portálu, byly vytvořeny Locator s datum vypršení platnosti dva rok.  

Aktualizovat data vypršení platnosti na locator, použijte [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Všimněte si, že když aktualizujete datum vypršení platnosti přidružení zabezpečení URL adresu URL změní.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Používat portál publikování aktivum

Používat portál publikování aktivum, postupujte takto:

1. [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
1. Vyberte **Nastavení** > **prostředky**.
1. Vyberte majetek, který chcete publikovat.
1. Klikněte na tlačítko **Publikovat** .
1. Vyberte typ locator.
2. Stisknutím klávesy **Přidat**.

    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adresa URL bude přidán do seznamu **Publikované adresy URL**.

## <a name="play-content-from-the-portal"></a>Přehrávání obsahu z portálu

Portál Azure poskytuje obsahu player, které můžete použít k testování videa.

Klikněte na požadované video a potom klikněte na tlačítko **Přehrát** .

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

Některé aspekty, které platí:

- Zkontrolujte, jestli že byla publikovaná video.
- Tento **Přehrávač** se přehrává výchozí streamování koncového bodu. Pokud chcete přehrát z jiné než výchozí streamování koncového bodu, klikněte na zkopírujte adresu URL a používat jiné player. Například [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Streamování koncový bod, odkud jsou streamování musí běžet.  
- Ke streamování z datových proudů koncového bodu, bychom přidat nejméně jednu streamování jednotku. Další informace najdete v tématu [Toto](media-services-portal-scale-streaming-endpoints.md) .   

##<a name="next-steps"></a>Další kroky

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


