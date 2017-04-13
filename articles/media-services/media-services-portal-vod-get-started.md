<properties
    pageTitle=" Začínáme s doručení obsahu jako služba Azure portálu | Microsoft Azure"
    description="Tento kurz vás provede jednotlivými kroky implementace služby základní doručování obsahu poskytují (VoD) pomocí aplikace Azure Media Services (AMS) pomocí portálu Azure."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Začínáme s doručení obsahu na vyžádání pomocí portálu Azure

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Tento kurz vás provede jednotlivými kroky implementace služby základní doručování obsahu poskytují (VoD) pomocí aplikace Azure Media Services (AMS) pomocí portálu Azure.

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

Tento kurz zahrnuje následující úkoly:

1.  Vytvořte účet Azure Media Services.
2.  Nakonfigurujte streamování koncový bod.
1.  Nahrajte soubor videa.
1.  Kódování zdrojového souboru do sadu adaptivní bitrate MP4 souborů.
1.  Publikujte majetek a získat datových proudů a postupné adresy URL pro stahování.  
1.  Přehrávání obsahu.


## <a name="create-an-azure-media-services-account"></a>Vytvořte účet Azure Media Services

Postup v této části ukazují, jak vytvořit účet AMS.

1. Přihlaste se na [portál Azure](https://portal.azure.com/).
2. Klikněte na **+ Nový** > **Web + Mobile** > **Media Services**.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Vytvoření** účtu služby médií zadejte požadované hodnoty.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Do pole **Název účtu**zadejte název nového účtu AMS. Název účtu Media Services je všechna malá písmena číslic nebo písmen bez mezer a 3 až 24 znaků.
    2. V předplatného vyberte mezi různých Azure předplatné, které máte přístup.
    
    2. **Pole Skupina zdroje**vyberte zdroj nové nebo existující.  Skupina zdroje je kolekce zdroje, které mají životního cyklu, oprávnění a zásady. Další informace [v tomto poli](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Do pole **umístění**vyberte zeměpisná oblast slouží k uložení záznamy média a metadat pro váš účet Media Services. Tato oblast slouží k obrázku a vysílání datových proudů. Do pole rozevíracího seznamu se zobrazí jenom k dispozici oblasti Media Services. 
    
    3. **Úložiště účtu**vyberte účet úložiště k poskytování úložiště objektů blob obsahu ze svého účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisná oblast účtem Media Services nebo můžete vytvořit účet úložiště. Vytvoření nového účtu úložiště ve stejné oblasti. Pravidla pro názvy účtů úložiště jsou stejné jako u účtů Media Services.

        Další informace o ukládání [tady](storage-introduction.md).

    4. Vyberte **Připnout do řídicího panelu** zobrazíte průběh nasazení účtu.
    
7. Klikněte na **vytvořit** v dolní části formuláře.

    Po úspěšném vytvoření účtu stav se změní na **systém**. 

    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Správa účtu AMS (například nahrát videa, kódovat prostředky, sledovat pokrok projektu) pomocí okna **Nastavení** .

## <a name="manage-keys"></a>Správa klíče

Potřebujete název účtu a primární klíče informace programově přístup k účtu Media Services.

1. Na portálu Azure vyberte svůj účet. 

    **Nastavení** okno na pravé straně. 

2. V okně **Nastavení** vyberte **klíče**. 

    **Správa klíčů** windows se zobrazí název účtu a zobrazí se primární a sekundární klíče. 
3. Stiskněte tlačítko Kopírovat a zkopírujte tyto hodnoty.
    
    ![Mediální služby klíče](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Konfigurace streamování koncové body

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly videa přes adaptivní bitrate streamování klienty. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

Služba Media poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 zakódovaný obsah v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) těsně za běhu, aniž byste museli ukládat předem sbalené verze každého z nich streamování formáty.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sadu adaptivní bitrate MP4 souborů (kódování kroky jsou prokázáno dál v tomto kurzu).  
- Vytvořte alespoň jeden streamování jednotku pro *streamování koncový bod* ze které budete chtít doručení obsahu. Postupem zobrazení, jak lze změnit počet datových proudů jednotky.

Dynamické balení, stačí k ukládání a zaplatit soubory ve formátu jediný úložiště a Media Services vytvoří a slouží odpovídající odpověď založená na žádosti o z klienta.

Vytvořit a změnit počet datových proudů rezervovaná jednotky, postupujte takto:


1. V okně **Nastavení** klikněte na **datových proudů koncové body**. 

2. Klikněte na výchozí streamování koncového bodu. 

    Zobrazí se okno **Výchozí STREAMOVÁNÍ podrobnosti o koncovém bodu** .

3. Chcete-li zadat počet jednotek, datových proudů, posuňte jezdec **datových proudů jednotky** .

    ![Přenos jednotky](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení nové jednotky může trvat až 20 minut.

## <a name="upload-files"></a>Nahrávání souborů

Proudu videí pomocí počítač Azure Media Services, budete muset nahrát videa zdroje, kódovat do více bitrates a publikovat výsledek. Cílem prvního kroku je uvedené v této části. 

1. V okně **Nastavení** klikněte na **prostředky**.

    ![Nahrávání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Klikněte na tlačítko **Odeslat** .

    Zobrazí se okno **Nahrát video materiálů** .

    >[AZURE.NOTE] Neexistuje žádná omezení velikosti souboru.
    
4. Přejděte do požadované video v počítači, vyberte ji a stiskněte tlačítko OK.  

    Nahrávání spustí a zobrazí se průběh v části název souboru.  

Po dokončení nahrávání se zobrazí nový majetek uvedené v okně **prostředky** . 

## <a name="encode-assets"></a>Kódování prostředky

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly adaptivní bitrate streamování klienty. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze). Příprava adaptivní bitrate streamování videa, budete muset kódovat zdroje videa do více bitrate souborů. Měli byste použít **Media Encoder standardní** encoder kódování videa.  

Media Services také poskytuje dynamické balení, který umožňuje poskytovat více bitrate MP4s v následujících formátech streamování: MPEG ČÁRKU, HLS, hladký přenos nebo běhu, aniž byste museli k opětovnému vytvoření balíčku do těchto streamování formáty. Dynamické balení, stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services vytvoří a slouží odpovídající odpověď založená na žádosti o z klienta.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování zdrojový soubor do sadu souborů, více bitrate MP4 (kódování kroky jsou v této části prokázáno později).
- Potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [Konfigurace streamování koncové body](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### <a name="to-use-the-portal-to-encode"></a>Používat portál kódování

Tato část popisuje kroky, kterými můžete kódování obsahu s Media Encoder standardní.

1.  V okně **Nastavení** zvolte **prostředky**.  
2.  V okně **prostředky** vyberte majetek, který byste chtěli kódování.
3.  Stiskněte tlačítko **kódovat** .
4.  V okně **kódovat aktivum** vyberte "Media Encoder standardní" procesoru a předvolby. Například, pokud znáte vstupní videa má rozlišení 1920 × 1080 pixelů, pak můžete použít "H264 více Bitrate 1080p" přednastavené. Další informace o přednastavené, najdete v [tomto](https://msdn.microsoft.com/library/azure/mt269960.aspx) článku – je důležité vybrat přednastavené, která je nejvhodnější pro zadání videa. Pokud máte nízkém rozlišení (640 x 360) a pak neměli používat jako výchozí "H264 více Bitrate 1080 p" přednastavené.
    
    Pro snadnější správu máte možnost upravit název výstup materiálů a název projektu.
        
    ![Kódování prostředky](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Stisknutím klávesy **vytvořit**.

### <a name="monitor-encoding-job-progress"></a>Sledování kódování postupu projektu

Pokud chcete sledovat průběh kódování úlohy, klikněte na **Nastavení** (v horní části stránky) a potom vyberte **úlohy**.

![Úlohy](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Publikování obsahu

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

>[AZURE.NOTE] Pokud jste použili k vytvoření Locator před březnem 2015 portálu, Locator s datum vypršení platnosti dva roky vytvořená.  

Aktualizovat data vypršení platnosti na locator, použijte [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Když aktualizujete datum vypršení platnosti přidružení zabezpečení locator změní adresu URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Používat portál publikování aktivum

Používat portál publikování aktivum, postupujte takto:

1. Vyberte **Nastavení** > **prostředky**.
1. Vyberte majetek, který chcete publikovat.
1. Klikněte na tlačítko **Publikovat** .
1. Vyberte typ locator.
2. Stisknutím klávesy **Přidat**.

    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adresa URL se přidají do seznamu **Publikované adresy URL**.

## <a name="play-content-from-the-portal"></a>Přehrávání obsahu z portálu

Portál Azure poskytuje obsahu player, které můžete použít k testování videa.

Klikněte na požadované video a potom klikněte na tlačítko **Přehrát** .

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

Některé aspekty, které platí:

- Zkontrolujte, jestli že byla publikovaná video.
- Tento **Přehrávač** se přehrává výchozí streamování koncového bodu. Pokud chcete přehrát z jiné než výchozí streamování koncového bodu, klikněte na zkopírujte adresu URL a používat jiné player. Například [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##<a name="next-steps"></a>Další kroky

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


