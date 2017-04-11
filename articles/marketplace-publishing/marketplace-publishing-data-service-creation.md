<properties
   pageTitle="Návod, jak vytvořit datové služby pro Marketplace | Microsoft Azure"
   description="Podrobné pokyny, jak vytvářet, certifikace a nasazení datové služby pro nákup na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Datové služby publikování průvodce pro Azure Marketplace

>[AZURE.IMPORTANT] **V současné době jsme už rychlého připojení všechny nové vydavatelé datové služby. Nové dataservices nebude získat schváleno seznam.** Pokud máte s aplikací business SaaS chcete publikovat na AppSource můžete najít další informace [v tomto poli](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo vývojář služby rádi byste publikovat na webu Azure Marketplace můžete najít další informace [v tomto poli](https://azure.microsoft.com/marketplace/programs/certified/).

Po dokončení kroku 1, [Vytvoření účtu a registrace](marketplace-publishing-accounts-creation-registration.md), jsme celým můžete [Obecné netechnických](marketplace-publishing-pre-requisites.md) a [Technické požadavky](marketplace-publishing-data-service-creation-prerequisites.md) na nabídku datové služby na Azure Marketplace. Teď můžeme vás provede jednotlivými kroky pro vytvoření nabídky k datové služby na [Portál publikování] [ link-pubportal] pro Azure Marketplace.

## <a name="1---login-to-the-publishing-portal"></a>1. přihlášení na publikování portálu.

Přejděte na [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**První přihlášení čas na portál publikování použijte stejný účet, u kterého byla zaregistroval vaší společnosti prodávající profilu v Centru pro vývojáře.**  (Později můžete přidat jakékoli zaměstnanec vaší společnosti jako co správce na portálu publikování).

Klikněte na dlaždici **publikovat datové služby** to při prvním přihlášení na portál publikování.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. v na navigační nabídku na levé straně zvolte **Datové služby** .

  ![Kreslení](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. vytvořit nové datové služby

Zadejte název pro vaší nové datové služby nabízet a klikněte na +"na" na pravé straně.

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. revize podnabídky v části nově vytvořený služby dat na navigační nabídku.

Klikněte na kartu **návodu** a zkontrolujte všechny potřebné kroky potřebné k publikování správně službu dat na webu Azure Marketplace.

> [AZURE.TIP] Vždy můžete klepnout na odkazy na stránce "Návodu" nebo pomocí karty na podnabídky datové služby nabídku na levé straně.

## <a name="5---create-a-new-plan"></a>5. Vytvořte nový plán.

### <a name="offers-plans-transactions"></a>Nabízí, plány, transakce.

Každou nabídku může obsahovat více plány, ale musí mít aspoň jedna (1) plán. Když koncoví uživatelé přihlášení k odběru vaši nabídku přihlášení odběru z jednoho plánu nabídku. Každý plán definuje, jak koncoví uživatelé budou moct používat službu.

Aktuálně Azure Marketplace podpora pouze měsíční předplatné transakce založené modelu datových služeb, tedy koncovým uživatelům zaplatí měsíční poplatek podle cena konkrétní plán, který budou, nemáte předplatné a budou moct používat každý měsíc počet transakce podle plánu.

Každou transakci obvykle rozumí počet záznamů, které vrátí datové služby na základě dotazu odeslal službě. Výchozí hodnota je 100. Počet transakce do všechny dotazy, bude číslo záznamů vydělena číslem 100 a zaokrouhlené nahoru na nejbližší celé číslo.

Je služba Azure Marketplace vrstvy odpovědností sledování (metr) počet transakce využívané obou dotazů.

> [AZURE.IMPORTANT] Koncových uživatelů, kterým limitu transakce během určitého měsíce budou blokovány nadále používat službu až do konce jejich měsíční předplatné obrázku.

> Plán nebo jiné plány můžete (ale ne musí) zahrnout neomezený počet transakce.

### <a name="create-a-plan"></a>Vytvoření plánu.
1. Klikněte na **"a"** vedle "Přidat nový plán".

2. Vyberte si jednu z možností: **Neomezeno** nebo **omezené** použití pro tento plán.  Pokud omezené zadejte počet transakce plánu vám umožní využívat za měsíc.

    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Portál publikování také navrhne "Plán identifikátor", který slouží ke komunikaci s koncoví uživatelé název plánu v uživatelském rozhraní a budou službou Tržiště taky slouží k identifikaci plánu. Pokud chcete, můžete změnit "Plánování identifikátor".

    > [AZURE.NOTE] "Plánování identifikátor" musí být jedinečný v rámci každé nabídky. Jak mnoho identifikátory použité v identifikátor publikování plánu portál bude zablokován po první publikování výrobní a nebude možné změnit tento identifikátor.

3. Klikněte na přijmout svého výběru.

4. Potom se objeví výzva několik další otázky týkající se plánu nově vytvořený.

    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Otázka|Význam|
|----|----|
|**Tento plán je zadarmo a k dispozici celém světě?**|Můžete vytvořit úplně uvolnění z bezplatné plán. Pokud je pouze plán k této nabídce – znamená to, že publikujete "Bezplatné nabízejí" v tržiště. Pokud je pouze pro jeden (z málo) plán, it vám nabídne možnost nabízet koncoví uživatelé zobrazíte další informace o službě s relativně malým počtem poštovních transakce za měsíc.  Pokud je odpověď "Ano", zobrazí se výzva žádné další otázky.|

> [AZURE.NOTE] Koncoví uživatelé vždy upgradovat na placené plány.

|Otázka|Význam|
|----|----|
|**Je dostupná bezplatná zkušební verze?**|Můžete vybrat mezi "Bez zkušební verze" vůbec nebo předat možnost použít plánu "Jeden měsíc". Vydavatelé třeba tato možnost slouží k poskytnutí koncoví uživatelé možnost pochopit výhody nabídky zdarma pro jeden měsíc.|

> [AZURE.IMPORTANT] Koncoví uživatelé uvidí jenom pro zakoupení bezplatnou zkušební verzi, pokud jste povolili platební nástroj například platební kartou se smlouvou enterprise.

> Po uplynutí tohoto měsíce z bezplatnou zkušební verzi Azure Marketplace začnou nabíjení zákazníci cenu datu předplatné, pokud zákazník, které iniciuje zrušení předplatného. Koncoví uživatelé budou poskytovány žádné zvláštní oznámení.

|Otázka|Význam|
|----|----|
|**Tento plán vyžaduje propagační kód koupit?**| Vydavatelé mají možnost omezit přístup k jejich služby plány jednotného zasílání zpráv zadáním speciální kód, s názvem "A Promocode" konkrétní zákazníkům. Pouze koncových uživatelů, kterým bude mít tento Promocode budou moct přihlášení k odběru plánu. Pokud se rozhodnete "Bez", pak souhlasíte, že všichni z oblasti, kde nabídky je k dispozici (viz [Marketplace marketingového obsahu Průvodce](marketplace-publishing-push-to-staging.md) podrobné informace) budou moct přihlášení k odběru tohoto plánu. Zobrazí se výzva žádné další otázky.|
|**Také skrýt tento plán od kohokoli, kdo nemá platné propagační kód?**|Pokud odpověď na předchozí otázce je "Ano" vydavatele má možnost úplně odebrat tohoto plánu zobrazoval v uživatelském rozhraní tržiště. Znamená to, neuvidíte zákazníci tento plán na stránce podrobnosti nabídky. Koncoví uživatelé, kteří obdrží promocode koupit, budou moct pomocí tohoto promocode odběr.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. Vytvořte svůj Marketplace marketingového obsahu
Informace o poskytnutí informací podle **Marketing, ceny, podporu a kategorií** karty prosím navštivte web [Marketplace marketingového obsahu příručka](marketplace-publishing-push-to-staging.md) , která je společná pro všechny artefakty publikovaných v galerii Azure Marketplace.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. vaší nabídky se připojte ke službě (na základě SQL Azure nebo webové služby na základě).

V nabídce klikněte na **Datové služby** podsložek.

V horní části stránky budete vyzváni k poskytnutí nabídka **Namespace**.  

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.png)

Pod otázkou definovat jak vydavatele bude vystavení nově vytvořený nabídka z Azure Marketplace. (Podrobnosti najdete v článku [Data služby technické základní Guide](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Publikování databáze na základě služby**

Klikněte na **databáze**. Na následující stránce zobrazí:

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.3.png)

Vytvoření mapování CSDL pro datovou sadu založené na SQL Azure DB:

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.4.png)

A pak pro každou tabulku

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.6.png)

Pokud webová služba

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Přečtěte si [mapování stávající webové služby k protokolu OData pomocí CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) podrobné pokyny a příklady pro vytváření CSDL webové služby.

## <a name="next-steps"></a>Další kroky
Teď, když jste vytvořili datové služby nabídky, zkontrolujte, zda postupujte podle pokynů [Průvodce marketingového obsahu Marketplace](marketplace-publishing-push-to-staging.md) před Přechod vpřed k [testování datové služby v pracovní](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Viz taky
- [Začínáme: Jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
- Pokud vás zajímají Principy celkové proces mapování OData a účel, v tomto článku [Data Service OData mapování](marketplace-publishing-data-service-creation-odata-mapping.md) kontroloval definice, struktury a pokyny.
- Pokud vás zajímají výukové Principy konkrétní uzly a jejich parametry v tomto článku [Datových služeb OData mapování uzlů](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definice a vysvětlivky, příklady a použít případu kontext.
- Pokud vás zajímají revizí příkladů naleznete v tomto článku jsou [Data Service OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete v článku ukázkový kód a interpretaci syntaxe kódu a kontext.


[link-pubportal]:https://publish.windowsazure.com
