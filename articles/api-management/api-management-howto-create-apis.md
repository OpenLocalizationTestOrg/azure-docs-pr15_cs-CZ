<properties 
    pageTitle="Jak vytvořit rozhraní API správy rozhraní API Azure" 
    description="Informace o vytváření a konfigurace API správy API Azure." 
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

# <a name="how-to-create-apis-in-azure-api-management"></a>Jak vytvořit rozhraní API správy rozhraní API Azure

Rozhraní API správy rozhraní API představuje sady operacích, které můžete uplatnit v klientských aplikacích. Nové rozhraní API vytvořené v aplikaci publisher portálu a pak se přidají požadované operace. Po přidání operace rozhraní API se přidá na produkt a lze publikovat. Po publikování rozhraní API můžete předplatné a používají vývojáři.

Tato příručka zobrazuje cílem prvního kroku v procesu: vytvoření a konfigurace nového rozhraní API správy rozhraní API. Další informace o přidávání operace a publikování produkt najdete v článku [Přidání operace rozhraní API][] a [jak vytvářet a publikovat produktu][].

## <a name="create-new-api"> </a>Vytvoření nového rozhraní API

Rozhraní API vytváří a nakonfigurovaný na portálu aplikace publisher. Přístup k portálu Publisheru, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě.

![Publisher portal][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

V nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** a potom klikněte na **Přidat rozhraní API**.

![Vytvoření rozhraní API][api-management-create-api]

Pomocí okna **Přidat nové rozhraní API** pro nastavení nového rozhraní API.

![Přidání nového rozhraní API][api-management-add-new-api]

Následující pole se používají pro nastavení nového rozhraní API.

-   **Rozhraní API webových název** obsahuje jedinečný a popisný název rozhraní API. Zobrazí se v portály pro vývojáře a aplikace publisher.
-   **Adresa URL webové služby** odkazuje služby protokolu HTTP provádění rozhraní API. Rozhraní API správy přepošle požadavky na tuto adresu.
-   **Webová adresa URL rozhraní API přípona** připojen k základní adresu URL pro službu pro správu rozhraní API. Základ adresa URL je společná pro všechny rozhraní API hostitelem instanci služby správy rozhraní API. Správa API odlišuje API podle jejich přípony a přípony proto musí být jedinečné pro každý API pro dané aplikace publisher.
-   **Webová adresa URL rozhraní API schéma** určuje protokoly, které lze použít pro přístup k rozhraní API. Ve výchozím nastavení není zadán HTTPs.
-   K volitelnému zadání toto nové rozhraní API pro produkt, klikněte na **produkty (volitelné)** rozevíracího seznamu a zvolte produktu. Tento krok může opakuje tisknutím rozhraní API přidáte několik produktů.

Jakmile se požadované hodnoty jsou nakonfigurovány, klikněte na **Uložit**. Po vytvoření nového rozhraní API souhrnnou stránku pro rozhraní API se zobrazí na portálu aplikace publisher.

![Rozhraní API souhrn][api-management-api-summary]

## <a name="configure-api-settings"> </a>Nastavení konfigurace rozhraní API

Na kartě **Nastavení** ověření a úpravy konfigurace rozhraní API. **Rozhraní API webových jméno**, **adresu URL webové služby**a **rozhraní API webová adresa URL přípony** původně nastaveny při rozhraní API se vytvoří a kterou lze zde. **Popis** obsahuje volitelný popis a **adresu URL webové rozhraní API schéma** určuje protokoly, které lze použít pro přístup k rozhraní API.

![Nastavení rozhraní API][api-management-api-settings]

Konfigurace brány ověřování pro službu back-end provádění rozhraní API, vyberte kartu **zabezpečení** . Konfigurace ověřování **HTTP základní** nebo **certifikátů** mohou sloužit **pomocí přihlašovacích údajů** rozevíracího seznamu. Použít základní ověřování HTTP, zadejte požadované údaje. Informace o použití ověření certifikátů klienta zjistěte, [jak zajistit back-end služby pomocí ověřování certifikátu klienta v části Správa rozhraní API Azure][].

Na kartu **zabezpečení** lze také konfigurace **ověření uživatele** pomocí OAuth 2.0. Další informace najdete v článku [jak povolit vývojář účty pomocí OAuth 2.0 správy rozhraní API Azure][].

![Nastavení základní ověřování][api-management-api-settings-credentials]

Klepněte na tlačítko **Uložit** uložte provedené změny provedené na nastavení rozhraní API.

## <a name="next-steps"> </a>Další kroky

Po vytvoření rozhraní API a nastavení konfigurovaná další kroky mají přidat operace API, přidání rozhraní API k produktu a publikovat tak, aby byla k dispozici pro vývojáře. Další informace naleznete v následujících článcích.

-   [Jak přidat operace rozhraní API][]
-   [Jak vytvořit a publikovat k produktu][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Jak přidat operace rozhraní API]: api-management-howto-add-operations.md
[Jak vytvořit a publikovat k produktu]: api-management-howto-add-products.md

[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Jak zabezpečené ověřování certifikátů správy rozhraní API Azure pomocí klienta služby back-end]: api-management-howto-mutual-certificates.md
[Jak povolit vývojář účty pomocí OAuth 2.0 správy rozhraní API Azure]: api-management-howto-oauth2.md