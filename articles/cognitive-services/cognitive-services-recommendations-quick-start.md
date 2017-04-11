<properties
    pageTitle="Úvodní příručka: počítače výukové doporučení API | Microsoft Azure"
    description="Azure počítače výukové doporučení – úvodní příručka"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Úvodní příručka pro kognitivní rozhraní API služeb doporučení

Tento dokument popisuje, jak na integrovaný služby nebo aplikace pro použití rozhraní [API doporučení](http://go.microsoft.com/fwlink/?LinkId=759710).
Můžete najít podrobnější informace o rozhraní API doporučení a jiných služeb, kognitivní [zde](http://go.microsoft.com/fwlink/?LinkId=759709). V této příručce můžete také zjistit [Doporučení rozhraní API odkaz](http://go.microsoft.com/fwlink/?LinkId=759348) po ruce.


<a name="Overview"></a>
## <a name="general-overview"></a>Obecné základní informace

Tento dokument je podrobného průvodce. Naším cílem je pro vás provede jednotlivými kroky potřebné školení modelu a tak, aby ukazovaly na zdroje, které vám umožní používat modelem ze svého pracovního prostředí.

Tento cvičení bude trvat asi 30 minut.

Použití [Rozhraní API doporučení](http://go.microsoft.com/fwlink/?LinkId=759710), je potřeba provést následující kroky:

1. Vytvoření modelu – modelu je kontejner použití dat, katalogu dat a doporučení modelu.
1. Import katalogu dat – katalog obsahuje metadata informace o položkách.
1. Údaje o využití import - použití zásad správy informací můžete odeslat v jedním ze způsobů 2 (nebo obojí):
  -  Uložením souboru, který obsahuje data o využití.
  -  Odeslání dat pořízení událostí.
  Obvykle nahrát soubor použití za účelem vytvoření datového modelu pro počáteční doporučení (zavádění) a použijte ji dokud systému shromažďuje dost dat pomocí formátu pořízení data.
1. Vytvoření modelu doporučení: to je asynchronní operace, ve kterém systému doporučení zabírá všechna data použití a vytvoří model doporučení. Tato operace může trvat několik minut nebo i několik hodin v závislosti na velikosti data a konfigurace parametrů Tvůrce dotazů. Při aktivaci sestavení, dostanete ID tvůrce dotazů. Ho použijte k prohlédnutí po ukončení procesu sestavení předtím, než začnete používat doporučení.
1. Používání doporučení - Get doporučení pro určité položky nebo seznam položek.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Pusťme se do práce.

Začněte vytvářet modelu doporučení. Pak budete provedeme vás jak používat výsledky generované modelem power doporučení na obchodování webu.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Úkol 1 - podpisový pro rozhraní API doporučení ####

V tomto úkolu se přihlásit ke službě doporučení API a vytvoření modelu doporučení.

1. Přejděte na **Portál Azure** na [http://portal.azure.com/](http://portal.azure.com/) a přihlaste se pomocí účtu Azure.

1.  Klepněte na **+ Nový**.

1. Vyberte požadovanou možnost **měřítka** .

1. Vyberte produkt **Kognitivní rozhraní API služeb** .
Tento produkt budete moci začít předplatné pro libovolnou z těchto služeb kognitivní rozhraní API (nominální, analýzy textu, počítače zrakem atd.). Doporučujeme dnes se zaměřuje na rozhraní API doporučení.

1. Na cílovou stránku kognitivní rozhraní API služeb zadejte **název účtu** u předplatného doporučení. (Pro instace: "MyRecommendations"). Tento název neměli mít všechny mezery.

1. Na **Typ rozhraní API**vyberte **doporučení**.

1. U **vrstvy ceny**můžete vybrat plán. Můžete vybrat **Free** osy na 10 000 transakce/měsíc. Toto je bezplatná plán, takže je užitečné při spuštění zkušební verze systému. Jakmile přejděte na výrobní doporučujeme zvažte hlasitost žádost a měnit typu plánu. (Poznámka: máte najednou jenom jedno předplatné bezplatné osy)

1. **Pole Skupina zdroje**vyberte nebo vytvořte nový účet, pokud už nemáte.

1. Můžete změnit další prvky v dialogovém okně vytvořit. Jsme by měl zmínit, že poskytovatel zdroje dnes je podporována pouze z USA datacentrech.

1. Jakmile dokončíte všechny zůstává, klikněte na **vytvořit**.

1. Počkejte několik minut zdroj, který má být nasazené.
Po jejím nasazení, můžete přejít **klávesy** v části **Nastavení** zásuvné kde se dozvíte primárních a sekundárních klíč, který používá rozhraní API.  Zkopírujte primární klíč, musíte ho při vytváření prvního modelu.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Úkol 2 – nebyla přenést data? ####

Rozhraní API doporučení dozvíte z katalogu a své transakce za účelem zajištění doporučení dobré produktu. To znamená, že budete muset kanálu správné údaje o svých produktů (jsme volání tento soubor **katalogu** ) a sadu transakce dostatečně velký k tomu, aby mohla najít zajímavé vzorků spotřeby (název tohoto **použití**).

1. Obvykle byste měli dotaz databázové transakce k těchto informací.
Jen v případě, že nemáte je užitečný, uvádíme ukázkových dat pro vás na základě dat transakce Microsoft Store.

 Stažení dat ze [tady](http://aka.ms/RecoSampleData). Kopírování a rozbalit MsStoreData.Zip do jiné složky na místním počítači.

 > **Poznámka:** Ukázkový kód, který bude stáhnout a spustit v 3 úkolu obsahuje vzorová data už vložena do – tak, aby tento úkol je nepovinný krok.  Který říká, tento úkol vám umožní stáhnout běžného sady dat a povolit porozumět vstupní hodnoty do rozhraní API doporučení lepší.

1.  Teď Podívejme se na soubor katalogu. Přejděte do umístění, které jste zkopírovali data.
 Otevřete soubor katalogu v **poznámkovém bloku**.

 Zjistíte, že je soubor katalogu poměrně jednoduché. Má následující formát`<itemid>,<item name>,<product category>`

 >  Office AAA-04294, Online DwnLd E34 OfficeLangPack 2013 32/64 <br>
 > Office AAA-04303, Online DwnLd a OfficeLangPack 2013 32/64  <br>
 > C9F 00168, KRUSELL Kiruna překlopit titulní pro telefony Nokia Lumia 635 - velbloudů, Příslušenství

 Jsme by měl zmínit, že soubor katalogu může být mnohem lepší, například přidat metadata o produktech (jsme volání tyto *Funkce položky*). V části [Formát katalogu](http://go.microsoft.com/fwlink/?LinkID=760716) v rozhraní API odkaz podrobné informace byste měli vidět na formát katalogu.

1. Pojďme stejně postupujte i s daty použití. Zjistíte, zda je datum použití formátu `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015/08/04T11:02:52, nákup 0003BFFDD4C2148C 297833400, 2015/08/04T11:02:50, nákup 0003BFFDD4C2118D 297833300, 2015/08/04T11:02:40, nákup 00030000D16C4237 297833300, 2015/08/04T11:02:37, nákup 0003BFFDD4C20B63 297833400, 2015/08/04T11:02:12, nákup 00037FFEC8567FB8 297833400, 2015/08/04T11:02:04, nákup

Všimněte si, že první tři prvky povinná. Typ události je nepovinný krok. Můžete se podívat na [používání formát](http://go.microsoft.com/fwlink/?LinkID=760712) Další informace o tomto tématu.

 > **Množství zpracovávaných dat je potřeba?**
 <p>
Dobře skutečně záleží na využití samotné. Systém učí, když uživatelé si různých položek. Pro některé sestavení jako FBT je důležité vědět položek, které jsou zakoupené ve stejné transakce. (Název tohoto *spolu výskytů*). Je dobré pravidlo mít většiny položek stačí účastnit 20 transakce zabere nejméně, takže pokud jste měli 10 000 položek ve vašem katalogu, doporučujeme, že máte alespoň 20 výskyty určeným počtem transakce nebo asi 200 000 transakce.
Ještě jednou jde pravidlo. Je třeba experimentovat s daty.
</p>

<a name="Ex1Task3"></a>
####Úkol 3: vytvoření modelu doporučení####

Teď máte nastavený účet a máte dat, vytvoření první modelu.

V tomto úkolu použijete ukázková aplikace k vytvoření prvního modelu.

1. Nejprve byste měli znát [Doporučení rozhraní API odkaz](http://go.microsoft.com/fwlink/?LinkId=759348).

1. Stáhněte [Ukázkový aplikace](http://go.microsoft.com/fwlink/?LinkID=759344) do místní složky.

1. Otevřete ve Visual Studiu **RecommendationsSample.sln** řešení nachází ve složce **C#** .

1. Otevřete soubor **SampleApp.cs** . Poznámka: aby následující kroky v souboru:
 + Vytvoření modelu
 + Přidání souboru katalogu
 + Přidání souboru použití
 + Aktivace sestavení modelu
 + Získání doporučení založené na pár položek
<p></p>

1. Nahraďte hodnotu pro pole **AccountKey** klávesu od úkolu 1.

1. Pokaždé procházet všemi kroky řešení, kde uvidíte získá vytvoření modelu.

1. Zkuste vyměnit katalogu a použití soubory, které jste právě stáhli vytvořit nový model Microsoft Store nebo knihy doporučení. Je třeba změnit název modelu a položky, pro které požadujete doporučení.

1. Po vytvoření modelu poznamenejte si **ID modelu** je bude potřeba při požadování doporučení v provozním prostředí.

>  Přečtěte si další informace o vytváření typů a hodnocení kvality buildy [tady](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Vložení modelu výrobní! ###

Teď můžete pochopit, jak vytvořit model a používání doporučení, dalším krokem je k umístění ve výrobním na vašem webu mobilní aplikace nebo integrovat do vašeho systému CRM nebo ERP.
Samozřejmě každý z těchto by být různé. Protože jako webové služby výzva API doporučení, je třeba moct snadno volání v některé z těchto různých prostředích.

**Důležité:**  Pokud chcete zobrazovat doporučení z veřejné klienta (například vaše produktovému), měli vytvořit proxy serveru, který poskytuje doporučení. To je důležité, abyste kód rozhraní API, nebude vystaven na externí (potenciálně nedůvěryhodných) entity.

Tady je několik dalších rad umístění, kde můžete použít doporučení:

### <a name="product-page"></a>Stránka produktů

**Doporučení položky**
<p>Pokud model byl školení na data nákupu, umožní zákazníka a *objevovat produkty, které by mohly zajímat lidem, které jste zakoupili Zdrojová položka*.</p>
<p>Pokud model byl školení na klikněte na data, vám umožní zákazníka a *objevovat produkty, které by mohly navštívili osobami, které jste navštívili položky zdroje*. Tento typ modelu může vrátit podobné položky.</p>

**Často koupili společně doporučení**
<p>Často koupili společně Tvůrce dotazů A může školení, abyste se mohli sad položek mívají společně s tuto položku zakoupit.</p>

### <a name="check-out-page"></a>Podívejte se na stránku

**Doporučení položky**
<p>Doporučení, která modelu může trvat jako zadávání seznam položek. Tak můžete předávat na vstupu získat doporučení předat všechny položky v koši.
V tomto případě modelu poskytne doporučení, které jsou předmětem zájmu uvedeny všechny položky v koši.
</p>

### <a name="landing-page"></a>Úvodní stránka

**Doporučení uživatele**
<p>
Doporučení možných modelu jako zadávání id uživatele.  Historie transakce tak, že daný uživatel bude použit poskytovat uživatele zadaného přizpůsobených doporučení.
</p>

Podívejte se na [Získání položku doporučení si přečtěte následující dokumentaci](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Co je další krok?
Gratulujeme, pokud jste provedli je to úplně! Informace o tom, že další můžete navštívit úplný [Doporučení rozhraní API odkaz](http://go.microsoft.com/fwlink/?LinkId=759348) Pokud máte nějaké otázky, neváhejte a kontaktujte nás vmlapi@microsoft.com
