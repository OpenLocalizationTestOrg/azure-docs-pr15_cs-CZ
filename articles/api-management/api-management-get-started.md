<properties
    pageTitle="Spravovat svůj první rozhraní API správy rozhraní API Azure | Microsoft Azure"
    description="Naučte se vytvořit API, přidejte operace a začít používat správu API."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Spravovat svůj první rozhraní API správy rozhraní API Azure

## <a name="overview"> </a>Základní informace

Tato příručka, uvidíte, jak můžete rychle začít pracovat při používání rozhraní API Správa Azure a první rozhraní API hovor.

## <a name="concepts"> </a>Co je Správa rozhraní API Azure?

Správa rozhraní API Azure umožňuje prohlédněte všechny back-end a spuštění plnohodnotný rozhraní API aplikace sady založeny na něm.

Obvyklé scénáře, patří:

* **Mobilní infrastruktury zabezpečení** uzavírání aplikace access s klávesami rozhraní API zabránění DOS útoky pomocí omezení nebo použití zásad pokročilým zabezpečením jako JWT tokenu ověření.
* **Povolení nezávislí výrobci softwaru partnera ekosystémů** nabízející rychlého připojení rychlé partnera prostřednictvím portálu pro vývojáře a vytváření rozhraní API fasády chcete oddělit od interním implementace, které nejsou zralé pro spotřebu partnera.
* **Aplikaci vnitřní rozhraní API** nabízející centralizované umístění pro organizaci komunikovat o dostupnosti a posledním změnám rozhraní API, uzavírání podle účty organizace, access všechny podle zabezpečené kanálu mezi brány rozhraní API a back-end.


Systém se skládá z těchto složek:

* **Rozhraní API brány** je koncový bod, který:
  * Rozhraní API spojí a přesměrovává na vaše back-end je možné zadat.
  * Ověří rozhraní API klíče JWT tokenů, certifikáty a další údaje.
  * Vynutí využití kvóty a ohodnocení omezení.
  * Transformace API rychlé úpravy bez úpravy kódu.
  * Uloží do back-end odpovědi na místo, kam nastavení.
  * Protokoly volání metadat pro účely analýzy.

* **Portál Publisheru** je rozhraní pro správu, pokud nastavíte rozhraní API aplikace. Použití:
    * Definování a importovat API schéma.
    * Balíček rozhraní API do produktů.
    * Nastavení zásad jako kvóty nebo transformace rozhraní API.
    * Dozvědět se víc z analýzy.
    * Správa uživatelů.

* **Karta Vývojář v portálu** slouží jako stavu hlavní web pro vývojáře, kde můžete:
    * Přečtěte následující dokumentaci pro rozhraní API pro čtení.
    * Vyzkoušejte si rozhraní API prostřednictvím interaktivních konzoly.
    * Vytvoření účtu a přihlášení k odběru získat rozhraní API klíče.
    * Access analýzy na jejich použití.


## <a name="create-service-instance"> </a>Vytvořit instanci rozhraní API správy

>[AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Pokud nemáte účet, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure][].

Cílem prvního kroku spíš práce s rozhraní API správy je pro vytvoření instance služby. Přihlaste se k [Portálu klasické Azure][] a klikněte na **Nová** **Aplikace služeb**, **Správa rozhraní API**, **vytvořit**.

![Nové instance rozhraní API správy][api-management-create-instance-menu]

**Adresy URL**zadejte název jedinečný dílčí domény pro adresu URL služby.

Vyberte požadovanou **oblast** a **předplatného** pro instanci služby. Po provedení požadované hodnoty, klikněte na tlačítko **Další** .

![Nová služba Správa rozhraní API][api-management-create-instance-step1]

Zadejte **Contoso Ltd.** **Název organizace**a zadejte e-mailovou adresu v poli **E-Mail správce** .

>[AZURE.NOTE] Tento e-mailovou adresu se používá pro upozornění systému správy API. Další informace najdete v tématu [jak nakonfigurovat oznámení a e-mailové šablony v části Správa rozhraní API Azure][].

![Nová služba Správa rozhraní API][api-management-create-instance-step2]

Instancí služby správy rozhraní API nabízí tři úrovně: vývojář, Standard a Premium. Ve výchozím nastavení nové instance služby správy rozhraní API vzniká ve vrstvě vývojář. Vyberte vrstvy standardní nebo Premium, zaškrtněte políčko **Upřesňující nastavení** a vyberte požadovanou úroveň na následující obrazovce.

>[AZURE.NOTE] Vývojář osy je určený pro vývoj, testování a kde dostupnost se netýká pilotní rozhraní API programy. V Standard a Premium úrovní můžete změnit počet rezervovaná jednotku zpracovávání více přenosy. Úrovně Standard a Premium poskytují služby správy rozhraní API s největší zpracování výkon. Můžete tento kurz pomocí libovolné osy. Další informace o rozhraní API Správa vrstev najdete v článku [Správa rozhraní API ceny][].

Zaškrtněte políčko pro vytvoření instance služby.

![Nová služba Správa API][api-management-instance-created]

Po vytvoření instance služby dalším krokem je jak vytvářet nebo importovat rozhraní API.

## <a name="create-api"> </a>Import rozhraní API

Rozhraní API se skládá ze sady operacích, které lze zavolat a od klientské aplikaci. Rozhraní API operace jsou servery proxy stávající webové služby.

Rozhraní API se dají vytvářet (a přidáním operace) ručně, nebo můžete importovat. V tomto kurzu budeme importovat rozhraní API pro ukázkové kalkulačky webové služby poskytované společností Microsoft a hostitelem Azure.

>[AZURE.NOTE] Vytvoření rozhraní API a ruční přidání operace, najdete v článku [jak vytvářet API](api-management-howto-create-apis.md) a [jak si přidat operace rozhraní API](api-management-howto-add-operations.md).

Rozhraní API se konfigurovat z portálu vydavatele prostřednictvím portálu klasické Azure. Přístup na portál Publisheru, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě.

![Portál aplikace Publisher][api-management-management-console]

K importu kalkulačky API, v nabídce **API správy** na levé straně klikněte na **API** a klikněte na **Import API**.

![Tlačítko pro import API][api-management-import-api]

Proveďte následující kroky pro nastavení kalkulačky rozhraní API:

1. Klikněte na **Z adresy URL**, zadejte do textového pole **Adresa URL dokumentu specifikace** **http://calcapi.cloudapp.net/calcapi.json** a klikněte na přepínač **Swagger** .
2. Do textového pole **přípona rozhraní API webových adres URL** zadejte **výpočty** .
3. Klikněte do pole **produkty (nepovinné)** a zvolte **Starter**.
4. Klepnutím na tlačítko **Uložit** import rozhraní API.

![Přidání nového rozhraní API][api-management-import-new-api]

>[AZURE.NOTE] **Rozhraní API správy** aktuálně podporuje 1.2 a 2.0 verzi dokumentu Swagger pro import. Ujistěte se, že, i když [specifikace Swagger 2.0](http://swagger.io/specification) prohlašuje, že `host`, `basePath`, a `schemes` vlastnosti jsou volitelné, dokumentu Swagger 2.0 **musí** obsahovat tyto vlastnosti; v opačném případě se nebudou importovány. 

Po importu rozhraní API souhrnnou stránku pro rozhraní API se zobrazí na portálu aplikace publisher.

![Rozhraní API souhrn][api-management-imported-api-summary]

V části rozhraní API obsahuje několik karty. Základní metriky a informace o rozhraní API, zobrazí na kartě **souhrnné informace** . Karta [Nastavení](api-management-howto-create-apis.md#configure-api-settings) slouží k prohlížení a úpravy konfigurace rozhraní API. Karta [operace](api-management-howto-add-operations.md) slouží ke správě rozhraní API operace. Konfigurace brány ověřování pro back-end serveru pomocí [základní ověřování vzájemné ověření](api-management-howto-mutual-certificates.md)nebo certifikátu a konfigurace [uživatele se tak mohli ověřovat pomocí OAuth 2.0](api-management-howto-oauth2.md)lze na kartu **zabezpečení** .  Karta **problémy** slouží k zobrazení problémů vykázaného vývojáři, kteří používají vaše rozhraní API. Kartě **Products** slouží ke konfiguraci produkty, které obsahují toto rozhraní API.

Ve výchozím nastavení součástí pokaždé rozhraní API správy dva ukázkové produkty:

-   **Starter**
-   **Neomezený**

V tomto kurzu jsme přidali základní API kalkulačky dělený součinem jejich Starter při importu rozhraní API.

Před uskutečněním volání rozhraní API, vývojáři musíte nejdřív přihlásit k odběru produktu, který umožňuje přístup k němu. Vývojáři můžou přihlásit se k odběru produkty v portálu pro vývojáře nebo správci můžou přihlášení k odběru vývojáři odpovídaly produktům na portálu aplikace publisher. Jste správce vzhledem k tomu, že jste vytvořili instanci systému správy rozhraní API v předchozích krocích v kurzu, takže jste již přihlášeni každý produkt ve výchozím nastavení.

## <a name="call-operation"> </a>Zavolat operaci z portálu pro vývojáře

Operace můžete volat přímo na portálu pro vývojáře, která poskytuje pohodlný způsob, jak prohlédněte si a otestujte operace rozhraní API. V tomto kurzu kroku zavolá operace základní kalkulačky API **Přidat dvě celá čísla** . V nabídce v horní klikněte na **portál pro vývojáře** napravo od portálu aplikace publisher.

![Karta Vývojář v portálu][api-management-developer-portal-menu]

Klikněte na **API** z nabídky nejvyšší a klepněte na položku **Základní Kalkulačka** zobrazíte dostupných operací.

![Karta Vývojář v portálu][api-management-developer-portal-calc-api]

Poznámka: Ukázka popisy a parametry importovaných spolu s rozhraní API a operací, poskytnutí si přečtěte následující dokumentaci pro vývojáře, která budou používat tuto operaci. Tyto popisy můžete také přidat při operace jsou ručně.

Zavolat **přidat dvěma celých čísel v nástroji** operace, klikněte na **vyzkoušet**.

![Vyzkoušejte si to][api-management-developer-portal-calc-api-console]

Zadání některé hodnoty parametrů nebo ponechejte výchozí hodnoty a pak klikněte na **Odeslat**.

![Nastavit informace HTTP Get][api-management-invoke-get]

Až vyvolání operaci portálu pro vývojáře zobrazí **Stav odpovědi**, **hlavičky odpovědi**a veškerý **obsah odpověď**.

![Odpověď][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Analýzu

Pokud chcete analýzu pro základní kalkulačky, přepněte zpět na portálu Publisheru tak, že vyberete **Spravovat** v nabídce v horní napravo od portálu pro vývojáře.

![Správa][api-management-manage-menu]

Výchozí zobrazení portálu aplikace publisher je **řídicího panelu**, který nabízí přehled správy rozhraní API instance.

![Řídicí panel][api-management-dashboard]

Nastavte ukazatel myši grafu **Základní Kalkulačka** zobrazíte konkrétní metriky pro použití rozhraní API pro daného období.

>[AZURE.NOTE] Pokud nevidíte všechny řádky v grafu, přepněte se zpátky do portálu pro vývojáře některé volat do rozhraní API, chvíli počkejte a pak se vrátit do řídicího panelu.

Klikněte na **Zobrazit podrobnosti** zobrazit souhrnnou stránku pro rozhraní API, včetně zvětšená verze zobrazeného metriky.

![Technologie pro analýzu][api-management-mouse-over]

![Souhrn][api-management-api-summary-metrics]

Podrobné metriky a sestav, **analýzy** v nabídce klikněte na **Správa rozhraní API** na levé straně.

![Základní informace][api-management-analytics-overview]

V části **analýzy** obsahuje následující čtyři karty:

-   **Na první pohled** poskytuje celkový použití a stav metriky i horní vývojáři, hlavní produkty, horní rozhraní API a horním operace.
-   **Použití** poskytuje podrobné pohled na volání rozhraní API a šířky pásma, včetně zeměpisné zastoupení.
-   **Stav** se zaměřuje na stavů, mezipaměti úspěšnosti doby odezvy a rozhraní API a poskytování služeb doby odezvy.
-   **Aktivity** obsahuje sestavy, které rozbalíte na konkrétní aktivitu tak, že vývojář, produktů, rozhraní API a operace.

## <a name="next-steps"> </a>Další kroky

- Zjistěte, jak se [chránit API s sazba omezení](api-management-howto-product-with-rules.md).

[Bezplatnou zkušební verzi Azure]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Postup při konfiguraci oznámení a e-mailové šablony v části Správa rozhraní API Azure]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Rozhraní API správy ceny]: http://azure.microsoft.com/pricing/details/api-management/

[Azure klasické portálu]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
