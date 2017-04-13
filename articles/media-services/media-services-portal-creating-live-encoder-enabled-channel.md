<properties 
    pageTitle="Jak provést živou streamování vytvoření bitrate více datových proudů pomocí portálu Azure pomocí služby Azure Media Services | Microsoft Azure" 
    description="Tento kurz vás provede jednotlivými kroky vytvoření kanálu, který přijme živou toku jednoduchým bitrate a kódování více bitrate proudu pomocí portálu Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
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


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Jak provést živou streamování vytvoření bitrate více datových proudů pomocí portálu Azure pomocí služby Azure Media Services

> [AZURE.SELECTOR]
- [Portál](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [ROZHRANÍ REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Tento kurz vás provede kroky pro vytvoření **kanálu** , který přijme živou toku jednoduchým bitrate a kódování více bitrate proudu.

>[AZURE.NOTE]Další informace týkající se kanály, kteří mají povolenou živou kódování najdete v článku [Live streamování pomocí Azure Media Services k vytváření bitrate více datových proudů](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Běžné situace živou streamování

Následují obecné kroky při vytváření běžné živou streamování aplikací.

>[AZURE.NOTE] Je v současné době, maximální doporučené trvání událost nastavit jako živou 8 hodin. Pokud potřebujete k spuštění kanálu po delší dobu, obraťte se na amslived na Microsoft.com.

1. Videokameru, tak připojte k počítači. Spuštění a konfigurace živou encoder místní, které můžete vytvořit jednu bitrate toku v některém z následujících protokolů: RTMP hladký přenos nebo RTP (MPEG-TS). Další informace najdete v tématu [Podpora RTMP Azure Media Services a Live kodéry](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Tento krok mohou provést také po vytvoření kanálu.

1. Vytvoření a spuštění kanálu. 

1. Načtení kanálu jedí adresu URL. 

    Adresa URL ingest se používá živou kodérem odešlete kanálu proudu.
1. Získat adresu URL kanálu náhled. 

    Pomocí této adresy URL pro ověření, že vaše kanálu správně dostává informace o živou proudu.

3. Vytvoření události/programu (, který bude tvořit aktivum). 
1. Publikujte požadovanou událost (vytvoří OnDemand locator pro související materiály).  

    Je nutné mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze kterého chcete proudů.
1. Spusťte událost, když budete chtít spustit přenos a archivace.
2. Volitelně můžete živou encoder signál zahájíte reklamu. Inzerce je vložena do výstupu proudu.
1. Ukončení události kdykoli budete chtít zastavení datových proudů a archivace události.
1. Odstranění události (a volitelně odstraňte majetku).   

##<a name="in-this-tutorial"></a>V tomto kurzu

V tomto kurzu se používá portálu Azure provádět následující úkoly: 

2.  Konfigurace streamování koncové body.
3.  Vytvoření kanálu, který je povoleno provádět živou kódování.
1.  Získání adresy URL jedí k poskytnutí s dynamickými encoder. Pomocí této adresy URL bude živou encoder jedí proudu do kanálu. .
1.  Vytvoření události/aplikace (a aktivum)
1.  Publikovat materiálů a získat streamování adresy URL  
1.  Přehrávání obsahu 
2.  Vyčištění

##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto jsou potřebná k dokončení kurzu.

- Pro dokončení tohoto kurzu, třeba účet Azure. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
- Účet Media Services. Vytvoření účtu Media Services, najdete v tématu [Vytvoření účet](media-services-portal-create-account.md).
- Webkamerou a kódovací, který můžete poslat jednoho bitrate live proudu.

##<a name="configure-streaming-endpoints"></a>Konfigurace streamování koncové body 

Služba Media poskytuje dynamické balení, který umožňuje poskytovat více bitrate MP4s v následujících formátech streamování: MPEG ČÁRKU, HLS, hladký přenos nebo běhu, aniž by bylo nutné znovu balíček do těchto streamování formáty. Dynamické balení potřebujete jenom k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta.

Umožní využít výhod dynamické balení potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete doručení obsahu.  

Vytvořit a změnit počet datových proudů rezervovaná jednotky, postupujte takto:

1. Přihlaste se na [portál Azure](https://portal.azure.com/) a vyberte svůj účet AMS.
1. V okně **Nastavení** klikněte na **datových proudů koncové body**. 

2. Klikněte na výchozí streamování koncového bodu. 

    Zobrazí se okno **Výchozí STREAMOVÁNÍ podrobnosti o koncovém bodu** .

3. Chcete-li zadat počet jednotek, datových proudů, posuňte jezdec **datových proudů jednotky** .

    ![Přenos jednotky](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení nové jednotky může trvat až 20 minut.

##<a name="create-a-channel"></a>Vytvoření kanálu

1. [Azure portál](https://portal.azure.com/)vyberte Media Services a potom klikněte na název účtu Media Services.
2. Vyberte **Live streamování**.
3. Vyberte možnost **vytvořit vlastní**. Tato možnost umožňuje vytvořit kanálu, aktivované řešení živou kódování.

    ![Vytvoření kanálu](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Klikněte na možnost **Nastavení**.
    
    1.  Zvolte typ **Live kódování** kanálu. Tento typ Určuje, že chcete vytvořit kanál, který aktivované řešení živou kódování. To znamená, že příchozí jednoho bitrate toku poslané na kanál a kódování do více bitrate toku pomocí zadaného živou encoder nastavení. Další informace najdete v tématu [Live streamování pomocí Azure Media Services k vytváření bitrate více datových proudů](media-services-manage-live-encoder-enabled-channels.md). Klikněte na OK.
    2. Zadejte název kanálu.
    3. Klikněte na OK v dolní části obrazovky.
    
5. Vyberte kartu **Ingest** .

    1. Na této stránce můžete vybrat streamování protokol. Pro typ **Live kódování** kanálu platné protokol možnosti jsou:
        
        - Jednoduché bitrate Fragmented MP4 (hladký přenos)
        - Jeden bitrate RTMP
        - RTP (MPEG-TS): MPEG-2 Transport toku přes RTP.
        
        Podrobné vysvětlení jednotlivých Protocol (protokol) najdete v článku [Live datových proudů pomocí Azure Media Services k vytváření bitrate více datových proudů](media-services-manage-live-encoder-enabled-channels.md).
    
        Nemůžete změnit možnosti protokolu při kanálu nebo jeho přidružený události/programy spuštěné. Pokud nastavíte různé protokoly, měli byste vytvořit samostatné kanály pro každý streamování protokol.  

    2. Omezení IP můžete použít v ingest. 
    
        Můžete definovat IP adresy, kteří jsou oprávněni jedí video tohoto kanálu. Povolené IP adresy můžete zadat jako jednu IP adresu (například "10.0.0.1"), rozsahu IP pomocí IP adresy a masky podsítě CIDR (například "10.0.0.1/22") nebo rozsahu IP pomocí IP adresy a masky tečkovaným desetinné podsítě (například "10.0.0.1(255.255.252.0)').

        Pokud jsou zadány žádné IP adresy a neexistuje definice pravidla budou povoleny žádná IP adresa. Pro všechny IP adresu povolit vytvořit pravidlo a nastavte 0.0.0.0/0.

6. Na kartě **Náhled** platí omezení IP na Náhled.
7. Na kartě **kódování** nastavte předvolby kódování. 

    V současné době systému pouze předvolba můžete vybrat je **výchozí 720 p**. Chcete-li zadat vlastní přednastavení, otevřete lístek podpory společnosti Microsoft. Zadejte název předvolby vytvoří. 

>[AZURE.NOTE] V současné době start kanál může trvat až 30 minut. Obnovení kanál může trvat až 5 minut.

Po vytvoření kanálu se klikněte na kanál a klikněte na **Nastavení** , kde si můžete pustit vaše konfigurace kanály. 

Další informace najdete v tématu [Live streamování pomocí Azure Media Services k vytváření bitrate více datových proudů](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Získání jedí adresy URL

Po vytvoření kanálu můžete získat jedí adresy URL, které budou poskytovat živou encoder. Encoder používá tyto adresy URL zadat živou proudu.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Vytváření a správa události

###<a name="overview"></a>Základní informace

Kanál je přidružené k události nebo programy, které vám umožní určit, publikování a úložiště segmentů v živé proudu. Kanály spravovat události/programy. Vztah kanálu a Program je velmi podobné tradiční médií Pokud kanálu konstantní toku obsahu a program má obor vymezený na některé časových událost na tento kanál.

Můžete zadat počet hodin, chcete-li zachovat zaznamenané obsahu pro událost nastavením délky **Okno Archiv** . Tato hodnota můžete nastavit z minimální 5 minut maximálně 25 hodin. Délka okno archiv také určuje, že maximální množství času klientů můžete požádat o zpět v čase od aktuální pozice live. Události mohlo by umožnit spuštění přes zadaný časový úsek, ale obsah, který zpozdit délka okno nepřetržitě ignorován. Tato hodnota tato vlastnost také určuje, jak dlouho rozrůstat manifesty klienta.

Každou událost je přidružený aktivum. OnDemand locator pro přidružené materiálů musí vytvořit pro publikování události. Máte tento locator vám umožní vytváření datových proudů adresy URL, která můžete poskytnout klienty.

Kanálu podporuje až tři souběžně pracovního události, abyste mohli vytvářet několika archivy stejné příchozí proudu. Můžete publikovat a archivace různé části události podle potřeby. Požadovanou firmy je například archivace 6 hodin událost, ale vysílání pouze posledních 10 minut. K tomu potřebujete k vytvoření dvou souběžně systém událostí. Jednu událost je nastavený na archivovat 6 hodin události, ale není publikován program. Jiné události je nastavený na archivovat 10 minut a tento program je publikován.

Neměli znovu použít existující programy pro nové zvláštní události. Místo toho vytvoření a spuštění nové aplikace pro každou událost.

Otevření události/aplikace Jakmile budete připraveni spustit přenos a archivace. Ukončení události kdykoli budete chtít přestat streamování a archivace události. 

Odstraňte archivované obsah, zastavte a odstranění události a odstraňte související materiály. Aktivum nelze odstranit, pokud je využívána události. nejdříve odstranit událost. 

I po ukončení a odstranění události, by uživatelé moct můžete vysílat datovými proudy archivované obsahu jako videa na službu, pro, dokud neodstraníte majetku.

Pokud chcete zachovat archivované obsah, ale není k dispozici pro přenos, odstraňte streamování locator.

###<a name="createstartstop-events"></a>Vytvoření, spuštění nebo zastavení události

Až budete mít toku, které do kanálu začnete vytvořením materiálů, programů a datových proudů Locator streamování události. To bude archivovat proudu a jeho Příprava uživatelům, pomocí datových proudů koncového bodu. 

Existují dva způsoby spuštění událostí: 

1. Na stránce **kanálu** stisknutím **Live události** přidáte novou událost.

    Určení: název události, název majetku, okno archiv a možnost šifrování.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Pokud vlevo **Publikovat Tato událost živou** zaškrtnuté políčko získat událost publikování adresy URL vytvořena.
    
    **Zahájení**, můžete stisknout pokaždé, když budete chtít vysílat události.

    Jakmile začnete události, můžete stisknout **kukátko** spustit přehrávání obsahu.

2. Můžete taky můžete použít zástupce a na tlačítko **Přejít Live** na stránce **kanálu** . Tím vytvoříte výchozí materiálů, programů a datových proudů Locator.

    Událost je uvedená **výchozí** a okno archiv nastavena na 8 hodin.

Můžete se podívat na událost publikované na stránce **Live události** . 

Pokud kliknete na tlačítko **Vypnout Air**přestane všechny živou události. 


##<a name="watch-the-event"></a>Podívejte se na události

Podívejte se na událost kliknutím na **sledovat** na portálu Azure nebo zkopírujte adresu URL streamování a pomocí přehrávače podle svého výběru. 
 
![Vytvoření](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Živou události automaticky převede událostí na obsahu na vyžádání při ukončení.

##<a name="clean-up"></a>Vyčistit

Pokud se dělají streamování události a chcete vyčistit zdroje zřízení dříve, postupujte následujícím způsobem.

- Vypnout předání proudu z encoder.
- Vypnout kanál. Po ukončení kanálu nepřejímá všechny poplatky. Pokud nepotřebujete ho znovu spustit, bude mít stejný jedí adresy URL, nebudete muset změnit konfiguraci vašeho encoder.
- Můžete zastavit koncový bod streamování, pokud chcete nadále poskytovat archivu živou události datového proudu na vyžádání. Pokud je v přestal stavu kanál, nepřejímá všechny poplatky.
  
##<a name="view-archived-content"></a>Zobrazení archivované obsahu

I po ukončení a odstranění události, by uživatelé moct můžete vysílat datovými proudy archivované obsahu jako videa na službu, pro, dokud neodstraníte majetku. Aktivum nelze odstranit, pokud je využívána události; nejdříve odstranit událost. 

Ke správě svém majetku, vyberte **Nastavení** a klikněte na **prostředky**.

![Prostředky](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Co byste měli zvážit

- Je v současné době, maximální doporučené trvání událost nastavit jako živou 8 hodin. Pokud potřebujete k spuštění kanálu po delší dobu, obraťte se na amslived na Microsoft.com.
- Je nutné mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze kterého chcete proudů.


##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
