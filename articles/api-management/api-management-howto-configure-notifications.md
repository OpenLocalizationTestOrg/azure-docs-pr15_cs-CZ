<properties 
    pageTitle="Postup při konfiguraci oznámení a e-mailové šablony v části Správa rozhraní API Azure" 
    description="Zjistěte, jak konfigurovat oznámení a e-mailové šablony v části Správa rozhraní API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Postup při konfiguraci oznámení a e-mailové šablony v části Správa rozhraní API Azure

Rozhraní API Správa poskytuje možnost konfigurace oznámení na určitých událostí a nakonfigurovat e-mailové šablony, které se používají komunikovat s správci a vývojáři instance rozhraní API správy. V tomto tématu se dozvíte, jak nakonfigurovat oznámení k dispozici událostí a poskytuje přehled konfigurace šablon e-mailu používá pro tyto události.

## <a name="publisher-notifications"> </a>Konfigurace oznámení na Publisheru

Konfigurace upozornění, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

Na levé straně zobrazit dostupné oznámení v nabídce **Rozhraní API Správa** klikněte na **oznámení** .

![Oznámení aplikace Publisher][api-management-publisher-notifications]

Následující seznam události je možné konfigurovat pro oznámení.

-   **Požadavky na odběr (vyžadování schválení)** - zadané e-mailu příjemce a uživatelé dostanou e-mailová oznámení o požadavcích předplatné pro rozhraní API produkty vyžadování schválení.
-   **Nové předplatné** - zadané e-mailu příjemce a uživatelé, dostanou e-mailová oznámení o nové předplatná s klíčem product rozhraní API.
-   **Galerie požadavků aplikací** - zadané e-mailu příjemce a uživatelé dostanou e-mailová oznámení po odeslání nové aplikace do Galerie aplikací.
-   **SKRYTÁ** - zadané e-mailu příjemce a uživatelé, dostanou e-mailu skrytá kopie všechny e-maily odesílané pro vývojáře.
-   **Nový případ nebo komentář** – zadané e-mailu příjemce a uživatelé dostanou e-mailová oznámení při nový případ nebo komentáře odeslané na portálu pro vývojáře.
-   **Uzavření účtu zprávy** – zadané e-mailu příjemce a uživatelé dostanou e-mailová oznámení, když máte zavřený účet.
-   **Limit kvóty předplatné Approaching** – následující e-mailu příjemce a uživatelé dostanou e-mailová oznámení při použití předplatné získá zavřít kvóta využití prostředků.

U každé události můžete určit pomocí textového pole e-mailové adresy příjemců e-mailu nebo vyberte uživatele ze seznamu.

E-mailové adresy, aby vás upozornil, zadejte je do textového pole e-mailovou adresu. Pokud máte víc e-mailové adresy oddělte je čárkami.

![Příjemci oznámení][api-management-email-addresses]

Chcete-li určit uživatele, aby vás upozornil, klikněte na tlačítko **Přidat příjemce**, zaškrtněte políčko vedle uživatele, které chcete být odesláno oznámení a klepněte na tlačítko **OK**.

>Všimněte si, že pouze správci se zobrazí v seznamu.

Po konfiguraci příjemci oznámení, klikněte na **Uložit** , aby příjemce aktualizované oznámení.

>Jestliže přejdete na kartu **Oznámení Publisheru** portálu publisher upozorní neuložené změny.

## <a name="email-templates"> </a>Konfigurace e-mailové šablony

Rozhraní API Správa poskytuje e-mailové šablony pro e-mailové zprávy, které jsou odeslané při správě a pomocí služby. Následující e-mailové šablony jsou k dispozici.

-   Odeslání aplikace Galerie schválené
-   Karta Vývojář farewell písmeno
-   Karta Vývojář kvótu blíží oznámení
-   Pozvat uživatele
-   Nový komentář přidán k problému
-   Nový případ pro přijetí
-   Nové předplatné aktivovali
-   Potvrzení o obnovení předplatného
-   Odmítne požadavek na odběr
-   Přijaté žádosti předplatného

Tyto šablony můžete upravit podle potřeby.

Můžete zobrazit a nakonfigurovat e-mailové šablony pro správu rozhraní API instanci, v nabídce **Rozhraní API správy** na levé straně klikněte na **upozornění** a klikněte na kartu **E-mailové šablony** .

![E-mailové šablony][api-management-email-templates]

Pokud chcete zobrazit nebo upravit šablonu, vyberte ji z rozevíracího seznamu **šablon** .

![Seznam šablon e-mailu][api-management-email-templates-list]

Každá šablona e-mailu má předmět ve formátu prostého textu a definici textu ve formátu HTML. Všechny požadované položky můžete přizpůsobit podle potřeby.

![Editor šablon e-mailu][api-management-email-template]

Seznam **Parametry** obsahuje seznam parametrů, která vloženým do textu nebo předmětu budou nahradit hodnotu určené při odesílání e-mailu. Vložit parametr, umístěte kurzor na místo, kam chcete parametr přejdete a klikněte na šipku vlevo od název parametru.

Klikněte na **Náhled** nebo **Odeslat test** najdete v článku jak e-mailu bude vypadat nebo odeslání e-mailu test.

>Všimněte si, že parametry nejsou nahradit skutečnými hodnotami při zobrazení náhledu nebo odesláním testu.
>
>Uložení změn do e-mailové šablony, klikněte na **Uložit**a zrušit změny klepněte na tlačítko **Storno**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance