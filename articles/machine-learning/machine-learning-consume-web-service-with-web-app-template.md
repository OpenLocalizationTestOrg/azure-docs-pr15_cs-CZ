<properties
    pageTitle="Používání webové služby strojového výukové pomocí šablony webové aplikace | Microsoft Azure"
    description="Pomocí šablony webové aplikace v Azure Marketplace využívat prediktivní webové služby Azure počítač přečíst."
    keywords="Webová služba operationalization rozhraní REST API pro počítač výuka"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Používání webové služby Azure počítače učení pomocí šablony webové aplikace

>[AZURE.NOTE] Toto téma popisuje technik pro klasické webové služby. 

Jednou jste vyvinuté prediktivní modelu a nasazené jako Azure webové služby pomocí počítače výukové Studio nebo pomocí nástrojů, jako jsou R nebo Python, se dostanete operationalized modelu pomocí rozhraní REST API.

Existuje několik způsobů, jak používat rozhraní REST API a přístup k webové službě. Aplikace můžete napsat například v jazyce C# R a Python pomocí kódu vzorku za vás přihlášení vygenerované při nasazení (dostupné na stránce Nápověda rozhraní API v řídicím panelu webové služby ve počítače výukové studiu) webové služby. Nebo můžete použít ukázkový sešit Microsoft Excel vytvoří (také k dispozici v řídicím panelu webové služby v Studio).

Ale nejrychlejší a nejsnadnější způsob pro přístup k webové služby prostřednictvím webové šablony aplikací dostupné v [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure počítače výukové šablony webových aplikací

Webové šablony aplikací dostupné v Azure Marketplace můžete vytvořit vlastní webové aplikace, které zná vstupních dat a očekávané výsledky webové služby. Je třeba udělat stačí udělit přístup webové aplikace pro webové služby a data a šablona bude zbývající.

Dvě šablony jsou k dispozici:

- [Šablona aplikace webové služby Azure ML žádosti a odpovědi](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Šablona aplikace pro Web Azure ML dávku spuštění služby](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Každá šablona vytvoří ukázkové ASP.NET aplikace pomocí pomocí rozhraní API URI a klíč pro webové služby a nasadí jako webu Azure. Šablona odpovědi na žádosti o služby prostředku vytvoří do webových aplikací, kterou chcete odeslat jeden řádek dat webová služba získat výsledkem je jedna hodnota. Šablona dávku spuštění služby (BES) vytvoří do webových aplikací, kterou chcete poslat počtu řádků dat za účelem získání výsledkem je více hodnot.

Žádné kódování je nutné použít tyto šablony. Jenom poskytnout URI rozhraní API a klíče a šabloně vytvoří aplikaci.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Použití šablony odpovědi na žádosti o službu (záznamy)

Po nasazení webová služba můžete podle následujících kroků můžete záznamy web app šablony, jak je znázorněno na následujícím obrázku.

![Proces použití šablony web záznamů][image1]

1. Ve počítače výukové Studiu otevřete kartu **Webovým službám** a pak otevřete webové služby, kterou chcete mít přístup. Zkopírujte klávesu uvedený v **klíči rozhraní API** a uložte jej.

    ![Rozhraní API klíč][image3]

2. Otevřete stránku nápovědy rozhraní API **Žádostí a odpovědí** . V horní části stránky, v části **požádat o**zkopírujte hodnotu **Požádat o URI** a uložte jej. Tato hodnota bude vypadat takhle:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Žádost o URI][image4]

3. Přejděte na [portál Azure](https://portal.azure.com) **přihlášení**, klikněte na **Nový**, vyhledejte a vyberte **Azure ML odpovědi na žádosti o služby v prohlížeči**a potom klikněte na **vytvořit**. 

    - Pojmenujte webovou aplikaci jedinečný. Adresa URL webové aplikace bude tento název následovaný `.azurewebsites.net.` například`http://carprediction.azurewebsites.net.`

    - Vyberte Azure předplatné a služby za kterých je spuštěný webové služby.

    - Klikněte na **vytvořit**.

    ![Vytvoření webové aplikace][image5]

4. Po Azure nasazení web appu, klikněte na adresu **URL** na webové stránce nastavení aplikace v Azure nebo zadejte adresu URL ve webovém prohlížeči. Například`http://carprediction.azurewebsites.net.`

5. Při prvním spuštění web appu se zobrazí výzvu k **Rozhraní API Post URL** a **Rozhraní API klíče**.
Zadejte hodnoty, které jste si předtím uložili:
    - **Identifikátor URI požadavku** na stránce nápovědy rozhraní API pro **rozhraní API Post URL**
    - **Rozhraní API klíč** z řídicího panelu webové služby pro **Rozhraní API klíče**.

    Klikněte na **Odeslat**.

    ![Zadejte příspěvek URI a rozhraní API klíč][image6]

6. Web appu zobrazí její **Konfiguraci webové aplikace** stránku s aktuální nastavení webové služby. Tady se můžete měnit nastavení používaná web appu.

    > [AZURE.NOTE] Nastavení pouze změna pro tento web app. Nezmění výchozí nastavení webové služby. Třeba při změně sem zadejte **Popis** nezmění popis zobrazené na řídicím panelu webové služby ve počítače výukové studiu.

    Až budete hotovi, klikněte na **Uložit změny**a pak klikněte na **Přejít na domovskou stránku**.

7. Na domovské stránce, kterou můžete zadat hodnoty odešlete webové služby klikněte na tlačítko **Odeslat**, a vrátí výsledek.

Pokud chcete se vrátit na stránce **Konfigurace** přejděte `setting.aspx` stránky ve web appu. Příklad: `http://carprediction.azurewebsites.net/setting.aspx.` zobrazí se výzva k zadání klávesu rozhraní API znovu – budete potřebovat, který chcete přejít na stránku a aktualizovat nastavení.

Můžete zastavit, restartujte nebo odstranit web app v portálu Azure jako jakékoli jiné v prohlížeči. Dokud je spuštěný můžete vyhledat domácí webovou adresu a zadejte nové hodnoty.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Použití šablony dávku spuštění služby (BES)

Šablona BES webové aplikace můžete použít stejným způsobem jako šablonu záznamy s tím rozdílem, že v prohlížeči, která se vytvoří vám umožní odeslat více řádků dat a přijímání výsledkem je více hodnot.

Výsledky z webové služby spuštění dávky jsou uloženy v kontejneru Azure úložiště; vstupní hodnoty můžou pocházet z úložiště Azure nebo místní soubor.
Ano musíte mít kontejneru Azure úložiště pro uložení výsledky vrácené web appu a budete muset Příprava zadávání dat.

![Proces použití BES webové šablony][image2]

1. Použijte stejný postup při vytváření aplikace BES web jako šablonu vyberete záznamy, s výjimkou:
    - Získáte **Požádat o URI** z stránce Nápověda ke **Spuštění dávky** rozhraní API pro webové služby.
    - Přejděte do [Aplikace šablona Web spuštění služby Azure ML dávky](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) otevření BES šablony na webu Azure Marketplace a klikněte na **Vytvořit Web App**.

2. Místo, kam chcete výsledky uložené, zadejte informace kontejneru cíl na domovské stránce web app. Taky určete, kde v prohlížeči získat vstupní hodnoty, v místní soubor nebo kontejneru Azure úložiště.
Klikněte na **Odeslat**.

    ![Ukládání informací][image7]

Web appu se zobrazí stránka se stavem.
Po dokončení projektu budete mít k dispozici umístění výsledky v úložišti objektů blob Azure. Máte taky možnost stahování výsledky místního souboru.

## <a name="for-more-information"></a>Další informace

Další informace...

- vytvoříte počítače výukové experiment s Studio výukové počítače, najdete v článku [vytvoření první experiment v Azure počítače výukové Studio](machine-learning-create-experiment.md)

- nasazení jste se naučili počítače experimentovat jako webové služby najdete v tématu [nasazení webové služby Azure počítače výuky](machine-learning-publish-a-machine-learning-web-service.md)

- Další způsoby, jak pro přístup k vaší webové služby, najdete v článku [jak používat webové služby Azure počítače výukové](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
