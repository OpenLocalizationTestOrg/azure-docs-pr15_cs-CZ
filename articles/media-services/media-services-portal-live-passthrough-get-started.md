<properties 
    pageTitle="Jak provést živou streamování s místním kodéry pomocí portálu Azure | Microsoft Azure" 
    description="Tento kurz vás provede jednotlivými kroky vytvoření kanálu, který je nakonfigurovaný pro předávací oznámení o doručení." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Jak provádět live streamování s místním kodéry pomocí portálu Azure

> [AZURE.SELECTOR]
- [Portál]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [ZBÝVAJÍCÍ]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Tento kurz vás provede jednotlivými kroky pomocí portálu Azure k vytvoření **kanálu** nakonfigurovaný pro předávací oznámení o doručení. 

##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto jsou potřebná k dokončení kurzu:

- Účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Účet Media Services. Vytvoření účtu Media Services, zjistěte, [jak vytvořit účet služby média](media-services-portal-create-account.md).
- Webové kamery. Například [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).

Důrazně doporučujeme zkontrolovat v následujících článcích:

- [Azure Media Services RTMP podporují a Live kodéry](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Základní informace o Live vodě pomocí Azure Media Services](media-services-manage-channels-overview.md)
- [Live streamování s místním kodéry vytvořené bitrate více datových proudů](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Běžné situace živou streamování

Následující kroky popisují úkoly týkající se vytváření běžné živou streamování aplikace, které používají kanály, které jsou nakonfigurovány pro předávací doručení. Tento kurz ukazuje, jak vytvářet a spravovat předávací kanálu a živou události.

1. Videokameru, tak připojte k počítači. Spuštění a nakonfigurujte živou encoder místní, který výstupy více bitrate RTMP nebo fragmentován MP4 toku. Další informace najdete v tématu [Podpora RTMP Azure Media Services a Live kodéry](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Tento krok mohou provést také po vytvoření kanálu.

1. Vytvoření a spuštění předávací kanálu.
1. Načtení kanálu jedí adresu URL. 

    Adresa URL ingest se používá živou kodérem odešlete kanálu proudu.
1. Získat adresu URL kanálu náhled. 

    Pomocí této adresy URL pro ověření, že vaše kanálu správně dostává informace o živou proudu.

3. Vytvoření živou událostí a programů. 

    Při použití portálu Azure, vytvoření živou události vytvoří také aktivum. 
      
    >[AZURE.NOTE]Je nutné mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze kterého chcete proudů.
1. Jakmile budete připraveni spustit přenos a archivace začněte událostí a programů.
2. Volitelně můžete živou encoder signál zahájíte reklamu. Inzerce je vložena do výstupu proudu.
1. Ukončení události a programů kdykoli budete chtít přestat datových proudů a archivace události.
1. Odstranění události nebo program (a volitelně odstraňte majetku).     

>[AZURE.IMPORTANT] Seznamte s [živou streamování s místním kodéry, které vytváření bitrate více datových proudů](media-services-live-streaming-with-onprem-encoders.md) pročíst koncepty a důležité informace související s živou streamování s místním kodéry a předávací kanály.

##<a name="to-view-notifications-and-errors"></a>Chcete-li zobrazit oznámení a chyby

Pokud chcete zobrazit oznámení a chyby vytvořené pomocí portálu Azure, klikněte na ikonu oznámení.

![Oznámení](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Konfigurace streamování koncové body 

Služba Media poskytuje dynamické balení, který umožňuje poskytovat více bitrate MP4s v následujících formátech streamování: MPEG ČÁRKU, HLS, hladký přenos nebo běhu, aniž byste museli k opětovnému vytvoření balíčku do těchto streamování formáty. Dynamické balení potřebujete jenom k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services vytvoří a slouží odpovídající odpověď založená na žádosti o z klienta.

Umožní využít výhod dynamické balení potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete doručení obsahu.  

Vytvořit a změnit počet datových proudů rezervovaná jednotky, postupujte takto:

1. Přihlaste se na [portál Azure](https://portal.azure.com/).
1. V okně **Nastavení** klikněte na **datových proudů koncové body**. 

2. Klikněte na výchozí streamování koncového bodu. 

    Zobrazí se okno **Výchozí STREAMOVÁNÍ podrobnosti o koncovém bodu** .

3. Chcete-li zadat počet jednotek, datových proudů, posuňte jezdec **datových proudů jednotky** .

    ![Přenos jednotky](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení nové jednotky může trvat až 20 minut.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Vytvoření a spuštění předávací kanály a události

Kanál je přidružené k události nebo programy, které vám umožní určit, publikování a úložiště segmentů v živé proudu. Kanály spravovat události. 
    
Můžete zadat počet hodin, chcete-li zachovat zaznamenané obsah do programu nastavením délky **Okno Archiv** . Tato hodnota můžete nastavit z minimální 5 minut maximálně 25 hodin. Délka okno archiv také určuje, že maximální množství času klientů můžete požádat o zpět v čase od aktuální pozice live. Události mohlo by umožnit spuštění přes zadaný časový úsek, ale obsah, který zpozdit délka okno nepřetržitě ignorován. Tato hodnota tato vlastnost také určuje, jak dlouho rozrůstat manifesty klienta.

Každou událost je přidružený aktivum. Publikovat na událost, musíte vytvořit OnDemand locator pro související materiály. Máte tento locator umožňuje vytváření datových proudů adresy URL, která můžete poskytnout klienty.

Kanálu podporuje až tři souběžně pracovního události, abyste mohli vytvářet několika archivy stejné příchozí proudu. Můžete publikovat a archivace různé části události podle potřeby. Požadovanou firmy je například archivace 6 hodin program, ale vysílání pouze posledních 10 minut. K tomu potřebujete k vytvoření dvou souběžně spuštěné programy. Jeden program je nastavena na archivovat 6 hodin události, ale není publikován program. Jiném programu je nastavena pro archivaci 10 minut a tento program je publikován.

Neměli znovu použít existující živou události. Místo toho vytvoření a spuštění novou událost pro každou událost.

Spusťte událost, když budete chtít spustit přenos a archivace. Ukončení programu kdykoli budete chtít přestat datových proudů a archivace události. 

Odstraňte archivované obsah, zastavte a odstranění události a odstraňte související materiály. Aktivum nelze odstranit, pokud je využívána události; nejdříve odstranit událost. 

I po ukončení a odstranění události, by uživatelé moct můžete vysílat datovými proudy archivované obsahu jako videa na službu, pro, dokud neodstraníte majetku.

Pokud chcete zachovat archivované obsah, ale není k dispozici pro přenos, odstraňte streamování locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>Používat portál k vytvoření kanálu 

Tato část popisuje možnost **Vytvořit** použít k vytvoření předávací kanálu.

Podrobné informace o předávací kanály najdete v článku [Live streamování s místním kodéry, které vytváření bitrate více datových proudů](media-services-live-streaming-with-onprem-encoders.md).

1. [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
2. V okně **Nastavení** klikněte na **Live streamování**. 

    ![Začínáme](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Zobrazí se okno **živou streamování** .

3. Klikněte na **Vytvořit** vytvoříte předávací kanálu s RTMP jedí protokol.

    Zobrazí se okno **Vytvořit nový kanál** .
4. Zadejte název nového kanálu a klikněte na **vytvořit**. 

    Vytvoří předávací kanálu s RTMP jedí protokol.

##<a name="create-events"></a>Vytvoření události

1. Vyberte kanál, ke kterému chcete přidat událost.
2. Tlačítko **Live události** .

![Události](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Získání jedí adresy URL

Po vytvoření kanálu můžete získat jedí adresy URL, které budou poskytovat živou encoder. Encoder používá tyto adresy URL zadat živou proudu.

![Vytvoření](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Podívejte se na události

Podívejte se na událost kliknutím na **sledovat** na portálu Azure nebo zkopírujte adresu URL streamování a pomocí přehrávače podle svého výběru. 
 
![Vytvoření](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Živou události získat automaticky převedou na obsah na vyžádání při ukončení.

##<a name="clean-up"></a>Vyčistit

Podrobné informace o předávací kanály najdete v článku [Live streamování s místním kodéry, které vytváření bitrate více datových proudů](media-services-live-streaming-with-onprem-encoders.md).

- Pouze v případě, že zastavily všechny události/aplikace na kanál, můžete ji ukončit kanálu.  Po ukončení kanálu není vzniknou všechny poplatky. Pokud nepotřebujete ho znovu spustit, bude mít stejný jedí adresy URL, nebudete muset změnit konfiguraci vašeho encoder.
- Kanálu lze odstranit pouze v případě, že byly odstraněny všechny živou události na kanál.

##<a name="view-archived-content"></a>Zobrazení archivované obsahu

I po ukončení a odstranění události, by uživatelé moct můžete vysílat datovými proudy archivované obsahu jako videa na službu, pro, dokud neodstraníte majetku. Aktivum nelze odstranit, pokud je využívána události; nejdříve odstranit událost. 

Ke správě svém majetku, vyberte **Nastavení** a klikněte na **prostředky**.

![Prostředky](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
