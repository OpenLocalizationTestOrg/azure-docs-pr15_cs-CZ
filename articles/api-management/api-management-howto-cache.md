<properties
    pageTitle="Přidat do mezipaměti pro zvýšení výkonu správy rozhraní API Azure | Microsoft Azure"
    description="Zjistěte, jak zlepšit latence, využití šířky pásma a webové služby načtěte doplněk pro volání správy rozhraní API služeb."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Přidat do mezipaměti pro zvýšení výkonu správy rozhraní API Azure

Operace v části Správa rozhraní API je možné konfigurovat pro ukládání do mezipaměti odpověď. Ukládání do mezipaměti odpověď může výrazně snížit latence rozhraní API, šířku pásma a webové služby načítání dat, která není často měnit.

Tato příručka, uvidíte, jak přidat odpověď ukládání do mezipaměti vašeho rozhraní API a konfigurace zásad pro operace rozhraní API ozvěnu vzorku. Potom můžete zavolat operaci z portálu pro vývojáře pro ověření ukládání do mezipaměti v akci.

>[AZURE.NOTE] Další informace o ukládání položek klíčem pomocí výrazů zásad najdete v článku [vlastní ukládání do mezipaměti správy rozhraní API Azure](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před postupovat podle kroků v této příručce, musíte mít instanci služby správy rozhraní API s rozhraním API a nakonfigurovali produktu. Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

## <a name="configure-caching"> </a>Konfigurace operace pro ukládání do mezipaměti

V tomto kroku byste zkontrolovat nastavení mezipaměti operaci **Získat zdroje (v mezipaměti)** vzorku rozhraní API ozvěnu.

>[AZURE.NOTE] Jednotlivé instance služby správy rozhraní API je předkonfigurované pomocí rozhraní API ozvěn tím, které mohou sloužit k vyzkoušet a další informace o správě rozhraní API. Další informace najdete v tématu [začít používat správu rozhraní API Azure][].

Začněte tím, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

V nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** a klikněte na **Rozhraní API ozvěnu**.

![Rozhraní API ozvěnu][api-management-echo-api]

Klikněte na kartu **operace** a pak klikněte na **Získat zdroje (v mezipaměti)** operace ze seznamu **operace** .

![Rozhraní API ozvěnu operace][api-management-echo-api-operations]

Klikněte na kartu **mezipaměť** zobrazíte nastavení mezipaměti pro tuto operaci.

![Ukládání do mezipaměti kartu][api-management-caching-tab]

Povolit ukládání do mezipaměti pro operaci, zaškrtněte políčko **Povolit** . V tomto příkladu je povoleno ukládání do mezipaměti.

Každá odpověď operace je nastaven na základě hodnot v polích **měnit podle parametrů řetězce dotazu** a **měnit podle záhlaví** . Pokud chcete do mezipaměti víc odpovědí na základě parametrů řetězce dotazu nebo záhlaví, nakonfigurujete je v těchto dvou polí.

**Doba trvání** Určuje intervalu vypršení platnosti odpovědi uložené v mezipaměti. V tomto příkladu interval je **3600** sekund, který odpovídá hodinu.

V tomto příkladu pomocí konfigurace ukládání do mezipaměti, první požadavek operaci **Získat zdroje (v mezipaměti)** vrátí odpověď z back-end služby. Tato odpověď bude do mezipaměti, nastaven zadaném záhlaví a parametrů řetězce dotazu. Následující volání operace s odpovídající parametrů, bude mít odpověď uložená v mezipaměti vrácen, dokud interval doba trvání mezipaměti vypršela.

## <a name="caching-policies"> </a>Zkontrolujte mezipaměti zásady

V tomto kroku zkontrolujte nastavení mezipaměti pro operaci **Získat zdroje (v mezipaměti)** vzorku rozhraní API ozvěnu.

Při ukládání do mezipaměti nakonfigurováno pro operaci na kartě **ukládání do mezipaměti** , ukládání do mezipaměti zásady se přidají na operaci. Tyto zásady můžete zobrazit a upravit v editoru zásad.

Klikněte na tlačítko **zásadám** **Správy rozhraní API** nabídku na levé straně a vyberte **rozhraní API ozvěnu / stažení zdroje (v mezipaměti)** v rozevíracím seznamu **operace** .

![Operace oboru zásad][api-management-operation-dropdown]

Zásady pro tuto operaci se zobrazí v editoru zásad.

![Editor zásad správy rozhraní API][api-management-policy-editor]

Definice zásad pro tuto operaci obsahuje zásad, které definují konfigurace ukládání do mezipaměti, které byly posouzeny pomocí **mezipaměť** karty v předchozím kroku.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Změny provedené v mezipaměti zásady v editoru zásad se projeví na kartě **mezipaměť** operace a naopak.

## <a name="test-operation"> </a>Hovorů operaci a otestujte ukládání do mezipaměti

Ukládání do mezipaměti v akci zobrazíte jsme operace zavolat z portálu pro vývojáře. Klikněte na **portál pro vývojáře** v nabídce horní doprava.

![Karta Vývojář v portálu][api-management-developer-portal-menu]

Klikněte na **rozhraní API** v nabídce začátek a vyberte **Rozhraní API ozvěnu**.

![Rozhraní API ozvěnu][api-management-apis-echo-api]

>Pokud máte jenom jednu rozhraní API nakonfigurované nebo viditelné ke svému účtu, kliknutím na rozhraní API přejdete přímo na operace pro tuto rozhraní API.

Vyberte operaci **Získat zdroje (v mezipaměti)** a klikněte na **Otevřít konzoly**.

![Otevřené konzoly][api-management-open-console]

Konzole umožňuje vyvolat operace přímo na portálu pro vývojáře.

![Konzoly][api-management-console]

Zachovat výchozích hodnot pro **parametr1** a **param2**.

Vyberte požadovanou klíč z rozevíracího seznamu **předplatné klíč** . Pokud váš účet obsahuje jenom jedno předplatné, již být vybrána.

Zadejte **sampleheader:value1** v textovém poli **požádat o záhlaví** .

Klikněte na **HTTP Get** a poznamenejte si hlavičky odpovědi.

Zadejte **sampleheader:value2** v textovém poli **požádat o záhlaví** a klikněte na **HTTP Get**.

Všimněte si, že hodnota **sampleheader** pořád **hodnota1** v odpovědi. Vyzkoušejte několik různých hodnotách a Všimněte si, že je vrácena režim cached odpověď od prvního hovoru.

Zadejte do pole **param2** **25** a klikněte na **HTTP Get**.

Všimněte si, že hodnota **sampleheader** v odpovědi teď **hodnota2**. Protože výsledky operace související v řetězci dotazu, nebyla vrátí předchozí odpověď uložená v mezipaměti.

## <a name="next-steps"> </a>Další kroky

-   Další informace o ukládání do mezipaměti zásad najdete v tématu [ukládání do mezipaměti zásady][] [odkaz zásad správy rozhraní API][].
-   Další informace o ukládání položek klíčem pomocí výrazů zásad najdete v článku [vlastní ukládání do mezipaměti správy rozhraní API Azure](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Začít používat správu rozhraní API Azure]: api-management-get-started.md

[Odkaz na zásad správy rozhraní API]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Ukládání do mezipaměti zásady]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
