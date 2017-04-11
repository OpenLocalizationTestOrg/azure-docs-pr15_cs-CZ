<properties 
    pageTitle="Rozhraní API správy klíčů koncepty" 
    description="Informace o rozhraní API, produktů, role, skupiny a jiných základních koncepcí rozhraní API správy." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Návod k importu definici rozhraní API s operacemi správy rozhraní API Azure

V části Správa rozhraní API vytvořením nového rozhraní API a operace přidat ručně nebo rozhraní API lze spolu s operace v jednom kroku.

Rozhraní API a jejich operace lze z následujících formátů.

-   WADL
-   Swagger

Tato příručka, jak vytvořit nového rozhraní API a a jeho operace v jednom kroku importu. Informace o ruční vytvoření rozhraní API a přidání operace najdete v článku [jak vytvářet rozhraní API][] a [jak si přidat operace rozhraní API][].

## <a name="import-api"> </a>Import rozhraní API

Rozhraní API vytváří a nakonfigurovaný na portálu aplikace publisher. Přístup k portálu Publisheru, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

![Portál aplikace Publisher][api-management-management-console]

V nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** a klikněte na **import rozhraní API**.

![Rozhraní API importu][api-management-import-apis]

Okno **Importovat API** má tři karty, které odpovídají tři způsoby, jak poskytnout specifikace rozhraní API.

-   **Ze schránky** umožňuje vložte specifikace rozhraní API do určené textové pole.
-   **Ze souboru** můžete procházet a vyberte soubor, který obsahuje specifikaci rozhraní API.
-   **Z adresy URL** umožňuje zadat adresu URL specifikace pro rozhraní API.

![Import rozhraní API formát][api-management-import-api-clipboard]

Po zadání specifikace rozhraní API, použijte přepínačů na pravé straně označíte specifikace formátu. Podporované jsou tyto formáty.

-   WADL
-   Swagger

Potom zadejte **přípona rozhraní API webových adres URL**. To je připojen k základní adresu URL služby správy rozhraní API. Základ adresa URL je společná pro všechny rozhraní API hostovaných na každý výskyt služby správy rozhraní API. Rozhraní API správy odlišuje rozhraní API podle jejich přípony a přípony proto musí být jedinečné pro každý rozhraní API v konkrétní instanci služby správy rozhraní API.

Po zadání všech hodnot klikněte na **Uložit** vytvořte rozhraní API a související operace. 

>[AZURE.NOTE] Kurz importu základní kalkulačky rozhraní API ve formátu Swagger v tématu [Správa první API správy rozhraní API Azure](api-management-get-started.md).

## <a name="export-api"></a> Exportovat rozhraní API

Kromě importu nového rozhraní API, můžete exportovat definice vaše rozhraní API z portálu Microsoft publisher. K tomu, klikněte na **Exportovat rozhraní API** v systému **Souhrn** **rozhraní API**.

![Rozhraní API pro export][api-management-export-api]

Rozhraní API je možné vyexportovat pomocí WADL nebo Swagger. Vyberte požadovaný formát, klikněte na tlačítko **Uložit**a zvolte umístění, do kterého chcete soubor uložit.

![Rozhraní API formát pro export][api-management-export-api-format]

## <a name="next-steps"> </a>Další kroky

Po vytvoření rozhraní API a operace importu, můžete zkontrolovat a nakonfigurovat další nastavení, přidání rozhraní API k produktu a publikovat tak, aby byla k dispozici pro vývojáře. Další informace najdete v tématu následující vodítka.

-   [Postup při konfiguraci nastavení rozhraní API][]
-   [Jak vytvořit a publikovat k produktu][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance

[Jak přidat operace rozhraní API]: api-management-howto-add-operations.md
[Jak vytvořit a publikovat k produktu]: api-management-howto-add-products.md
[Jak vytvořit rozhraní API]: api-management-howto-create-apis.md
[Postup při konfiguraci nastavení rozhraní API]: api-management-howto-create-apis.md#configure-api-settings
