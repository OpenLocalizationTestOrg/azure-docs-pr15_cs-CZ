<properties
    pageTitle="Ochrana API se správou Azure rozhraní API | Microsoft Azure"
    description="Zjistěte, jak chránit vaše rozhraní API s kvót a omezení zásady (omezení rychlosti)."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Chránit vaše API s sazba omezení správou API Azure

Tato příručka, se dozvíte, jak je snadné přidat ochranu backendovou rozhraní API nakonfigurováním sazba kvóty a zásady službou Azure rozhraní API Management.

V tomto kurzu vytvoříte "Bezplatnou zkušební verzi" rozhraní API produktu, který umožňuje vývojářům volat až 10 za minutu a maximální hodnota je 200 hovorů za týden vaše rozhraní API pomocí [Limit volání sazbu vztaženou na předplatné](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a zásady [kvóta využití sadu jedno předplatné](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . Potom publikovat rozhraní API a otestujte zásad limit sazba.

Pokročilejší omezení scénáře použití zásad [sazba limit tak, že klíč](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [kvóta tak, že klíč](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) najdete v článku [omezení službou Azure rozhraní API Management Upřesnit požadavků](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>k vytvoření produktu

V tomto kroku vytvoříte bezplatnou zkušební verzi produktu, který nevyžaduje schválení předplatného.

>[AZURE.NOTE] Pokud už máte produkt nakonfigurované a chcete použít pro účely tohoto návodu, můžete přejít na [Konfigurovat volání sazba kvóty a zásady][] a postupujte podle kurzu odtud pomocí produkt místo bezplatnou zkušební verzi produktu.

Začněte tím, klikněte na **Spravovat** v Azure Classic službě rozhraní API správy. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [spravovat svůj první rozhraní API správy rozhraní API Azure][] .

Klikněte na **produkty** v nabídce **Rozhraní API Management** na levé straně otevřete stránku **produkty** .

![Přidání produktu][api-management-add-product]

Klikněte na **Přidat produktu** k zobrazení dialogového okna **Přidat nového produktu** .

![Přidání nového produktu][api-management-new-product-window]

Do pole **název** zadejte **Bezplatnou zkušební verzi**.

Do pole **Popis** zadejte následující text:  **předplatitele budou moct spouštět 10 hovory/minutu maximální hodnota je 200 hovory/týdnu, po jejímž uplynutí přístup odepřen.**

Produkty v části Správa API může být chráněny nebo otevřete. Chráněný produkty musíte mít předplatné před jejich použitím. Otevřít produkty můžete použít bez přihlášení k odběru. Ujistěte se, že **vyžaduje předplatné** je zaškrtnuto vytvořit chráněné produktu, který vyžaduje předplatné. Toto je ve výchozím nastavení.

Pokud budete potřebovat správce můžete zkontrolovat a přijmout nebo odmítnout předplatné pokusy o tento produkt, vyberte **vyžadovat schválení předplatného**. Pokud není zaškrtnuté políčko, pokusy předplatné bude schválené automatického. V tomto příkladu předplatná automaticky schvalují, tak, aby se vybrat pole.

Umožňuje účty vývojář k odběru tisknutím dělený součinem jejich nové zaškrtněte políčko **Povolit víc předplatných souběžné** . Tento kurz není využít víc předplatných souběžné, takže ponechejte zrušené zaškrtnutí políčka.

Po zadání všech hodnot klikněte na **Uložit** vytvořte produktu.

![Product přidali][api-management-product-added]

Ve výchozím nastavení jsou viditelné pro uživatele ze skupiny **Administrators** nových produktů. Nyní klikněte na tlačítko Přidat skupinu **vývojáři** . Klikněte na **Bezplatnou zkušební verzi**a potom klikněte na kartu **zobrazení** .

>V části Správa rozhraní API skupiny slouží ke správě viditelnost produktů pro vývojáře. Produkty udělovat viditelnost skupiny a vývojáři můžou zobrazení a odběr produkty, které jsou viditelné skupiny, do kterých patří. Další informace najdete v tématu [jak vytváření a používání skupin správy rozhraní API Azure][].

![Přidání vývojáři skupiny][api-management-add-developers-group]

Zaškrtněte políčko **vývojáři** a potom klikněte na **Uložit**.

## <a name="add-api"> </a>Přidáte rozhraní API dělený součinem jejich

V tomto kroku kurzu přidáme rozhraní API ozvěn tím dělený součinem jejich nové bezplatnou zkušební verzi.

>Jednotlivé instance služby správy rozhraní API je předkonfigurovaná pomocí rozhraní API ozvěn tím, které mohou sloužit k vyzkoušet a další informace o správě rozhraní API. Další informace najdete v tématu [Správa první API správy rozhraní API Azure][].

V nabídce **Rozhraní API správy** na levé straně klikněte na **produkty** a klikněte na **Bezplatnou zkušební verzi** pro nastavení produktu.

![Konfigurace produktů][api-management-configure-product]

Klikněte na **Přidat rozhraní API produkt poprvé**.

![Přidání API k produktu][api-management-add-api]

Vyberte **Ozvěnu API**a potom klikněte na **Uložit**.

![Přidání rozhraní API ozvěnu][api-management-add-echo-api]

## <a name="policies"> </a>Ke konfiguraci volání sazba kvóty a zásad

Limity rychlosti a kvót nakonfigurovány v editoru zásad. Nabídce klepněte na **zásady** **Správy rozhraní API** na levé straně. V seznamu **produktů** klikněte na **Bezplatnou zkušební verzi**.

![Zásady produktu][api-management-product-policy]

Klikněte na **Přidat zásadu** import šablony zásady a začít vytvářet sazbu kvóty a zásady.

![Přidání zásad][api-management-add-policy]

Pokud chcete vložit zásad, umístěte kurzor do zobrazení **příchozí** nebo **odchozí** část šablonu zásad. Sazba kvóty a zásady jsou příchozí zásady, takže umístěte kurzor na místo v prvku příchozí.

![Editor zásad][api-management-policy-editor-inbound]

Dva zásad, které jsme přidáváte v tomto kurzu jsou zásady [Limit volání sazbu vztaženou na předplatné](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavte kvóta využití prostředků jedno předplatné](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Zásady příkazy][api-management-limit-policies]

Po kurzor je umístěn v elementu **příchozí** zásad, klikněte na šipku vedle **Limit volání sazbu vztaženou na předplatné** vložit jeho šablonou.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Limit volání sazbu vztaženou na předplatného** můžete použít na úrovni produktu a lze také na úrovni jednotlivých operace název a rozhraní API. V tomto kurzu se používají pouze úroveň produktu zásady, tak zabránění **api** a **operace** prvky elementu **limit rychlosti** , takže jenom prvek zůstane vnější **limit rychlosti** , jak je vidět v následujícím příkladu.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

V bezplatnou zkušební verzi produktu sazba maximální povolenou hovoru je 10 hovory za minutu, takže, zadejte **10** jako hodnotu atributu **volání** a **60** pro atribut **prodloužení** .

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Konfigurace zásad **nastavit kvóta využití prostředků jedno předplatné** , umístěte kurzor těsně pod nově přidaný **limit rychlosti** element element **příchozí** a potom klikněte na šipku vlevo od **nastavit kvóta využití prostředků jedno předplatné**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Protože této zásady také má být na úrovni produktu, odstraňte název prvků **rozhraní api** a **operace** jak je vidět v následujícím příkladu.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kvóty můžou být založená na počtu hovorů za intervalu nebo šířku pásma. V tomto kurzu budeme nejsou omezení na základě šířky pásma, tak odstranění atribut **šířky pásma** .

    <quota calls="number" renewal-period="seconds">
    </quota>

V bezplatnou zkušební verzi produktu kvóty je 200 hovory za týden. Jako hodnotu atributu **volání** zadat **200** a zadejte **604800** jako hodnotu atributu **období obnovení** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Intervalech zásad jsou uvedeny v sekundách. Chcete-li vypočítat interval za týden, můžete spočítá počet dnů (7) vynásobte hodnotou počet hodin za den (24) počet minut v hodina (60) tak, že počet sekund, než jednu minutu (60): 7 *24* 60 * 60 = 604800.

Po dokončení konfigurace zásad by měly odpovídat v následujícím příkladu.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Po nakonfigurování požadované zásady klikněte na **Uložit**.

![Uložení zásad][api-management-policy-save]

## <a name="publish-product"></a> Publikovat produktu

Teď, když přidají rozhraní API a jsou nakonfigurované zásady, produktu musí být publikován tak, aby ji může používat vývojáři. V nabídce **Rozhraní API správy** na levé straně klikněte na **produkty** a klikněte na **Bezplatnou zkušební verzi** pro nastavení produktu.

![Konfigurace produktů][api-management-configure-product]

Klikněte na **Publikovat**a potom klikněte na **Ano, publikujte** potvrďte.

![Publikování produktu][api-management-publish-product]

## <a name="subscribe-account"> </a>Chcete přihlásit účet vývojář dělený součinem jejich

Teď, když je publikován produktu, je k dispozici pro předplatné a používají vývojáři.

>Správci správy rozhraní API instance jsou automaticky předplatné pro každý produkt. V tomto kurzu kroku jsme bude přihlášení k odběru jednoho z účtů vývojář není správcem dělený součinem jejich bezplatnou zkušební verzi. Pokud vývojář část roli správce, pak můžete sledovat spolu s Tento krok, i když jste již přihlášeni.

**Uživatelé** v nabídce klikněte na **Správa rozhraní API** na levé straně a potom klikněte na název účtu vývojář. V tomto příkladu jsme se pomocí účtu **Chvojková Gragg** vývojář.

![Konfigurace vývojář][api-management-configure-developer]

Klikněte na **Přidat předplatného**.

![Přidání předplatného][api-management-add-subscription-menu]

Vyberte **Bezplatnou zkušební verzi**a potom klikněte na **Přihlásit se k odběru**.

![Přidání předplatného][api-management-add-subscription]

>[AZURE.NOTE] V tomto kurzu nejsou povoleny víc předplatných souběžné bezplatnou zkušební verzi produktu. Kdyby, by se vás název předplatného, jak je vidět v následujícím příkladu.

![Přidání předplatného][api-management-add-subscription-multiple]

Po kliknutí na tlačítko **Přihlásit se k odběru**, produkt v seznamu se zobrazí **předplatného** pro uživatele.

![Předplatné přidali][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Volání operaci a otestovat limit sazba

Teď nakonfigurované a publikované bezplatnou zkušební verzi produktu můžete volat některé operace jsme testování zásad limit sazba.
Přejděte na portálu pro vývojáře kliknutím na **portál pro vývojáře** v nabídce pravého horního.

![Karta Vývojář v portálu][api-management-developer-portal-menu]

Klikněte na **rozhraní API** v nabídce horní a klikněte na **Rozhraní API ozvěnu**.

![Karta Vývojář v portálu][api-management-developer-portal-api-menu]

Klikněte na **Získat zdroje**a potom klikněte na **vyzkoušet**.

![Otevřené konzoly][api-management-open-console]

Zachovat výchozí hodnoty parametrů a pak vyberte kód předplatného pro produkt bezplatnou zkušební verzi.

![Klíč předplatného][api-management-select-key]

>[AZURE.NOTE] Pokud máte víc předplatných, je potřeba vybrat klíč pro **Bezplatnou zkušební verzi**, jinak zásad, které jste nakonfigurovali v předchozích krocích neprojeví.

Klepněte na tlačítko **Odeslat**a potom zobrazit odpověď. Poznámka: **Stav odpovědi** **200 OK**.

![Výsledky operace][api-management-http-get-results]

Klikněte na **Odeslat** sazbou větší než limit zásad sazba 10 hovorů za minutu. Po překročení omezení zásad sazba bude vrácena stav odpovědi **429 příliš mnoho požadavků** .

![Výsledky operace][api-management-http-get-429]

**Obsah odpověď** označuje zbývající interval před opakování proběhne úspěšně.

Je-li sazba limit zásada 10 hovorů za minutu v podstatě, další hovory se nepovede až 60 sekund uplynuly od prvního 10 úspěšné volání dělený součinem jejich před překročení limitu sazba. V tomto příkladu je interval zbývající 54 sekund.

## <a name="next-steps"> </a>Další kroky

-   Podívejte se na ukázku nastavení omezení sazba a kvót ukazuje následující video.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Spravovat svůj první rozhraní API správy rozhraní API Azure]: api-management-get-started.md
[Jak vytvořit a používat skupiny správy rozhraní API Azure]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Konfigurace volání sazba kvóty a zásad]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
