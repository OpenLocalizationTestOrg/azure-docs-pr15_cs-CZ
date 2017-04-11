<properties 
    pageTitle="Jak vytvořit a publikovat produkt správy rozhraní API Azure" 
    description="Naučte se vytvářet a publikovat produkty správy rozhraní API Azure." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Jak vytvořit a publikovat produkt správy rozhraní API Azure

V části Správa rozhraní API Azure obsahuje produkt jeden nebo více rozhraní API i kvóta využití prostředků a podmínky použití. Po publikování produkt vývojáře odběr dělený součinem jejich a začněte používat rozhraní API produktu. Téma obsahuje pokyny pro vytvoření produktu, přidání rozhraní API a publikováním pro vývojáře.

## <a name="create-product"> </a>Vytvoření produktu

Operace se přidá a nakonfigurované tak, aby rozhraní API v portálu aplikace publisher. Přístup k portálu Publisheru, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

Klikněte na **produkty** v nabídce na levé straně otevřete stránku **produkty** a klikněte na **Přidat produkt**.

![Produkty][api-management-products]

![Nového produktu][api-management-add-new-product]

Do pole **název** a popis produktu do pole **Popis** zadejte popisný název produktu.

Produkty v části Správa rozhraní API může být **otevřený** nebo **chráněného**. Chráněný produkty musíte mít předplatné před jejich použitím při otevření produkty můžete použít bez přihlášení k odběru. Zaškrtněte **vyžaduje předplatné** pro vytvoření chráněné produktu, který vyžaduje předplatné. Toto je ve výchozím nastavení.

Zaškrtněte **vyžaduje schválení předplatného** , pokud chcete správce můžete zkontrolovat a přijmout nebo odmítnout předplatné pokusy o produktu. Pokud je zaškrtnuté políčko, budou předplatné pokusy automatické schválení. Podrobnosti o předplatných najdete v článku [zobrazení předplatitele k produktu][].

Povolit účty vývojář k odběru tisknutím dělený součinem jejich, zaškrtněte políčko **Povolit víc předplatných** . Pokud není zaškrtnuté toto políčko, každý účet vývojář odběr jen jednou dělený součinem jejich.

![Neomezený víc předplatných][api-management-unlimited-multiple-subscriptions]

Chcete-li omezit počet víc souběžné předplatných, zaškrtněte políčko **omezit počet souběžné předplatná pro** a zadejte limit předplatného. V následujícím příkladu jsou čtyři každý účet vývojář souběžné předplatná.

![Čtyři víc předplatných][api-management-four-multiple-subscriptions]

Po konfiguraci všech nových možností produktu klikněte na **Uložit** a vytvořte nového produktu.

![Produkty][api-management-products-page]

>Ve výchozím nastavení nových produktů se nepublikované a jsou viditelné jenom pro skupiny **Administrators** .

Konfigurace produktem, klepněte na název tohoto produktu na kartě **produkty** .

## <a name="add-apis"> </a>Přidat rozhraní API pro produkt

Stránka **produkty** obsahuje čtyři odkazy pro konfiguraci: **Souhrn**, **Nastavení**, **viditelnost**a **účastníků**. Karta **Souhrn** je místo, kam můžete přidat rozhraní API a publikovat nebo zrušit publikování produktu.

![Souhrn][api-management-new-product-summary]

Před publikováním produkt potřebujete přidat jeden nebo více rozhraní API. Klikněte na možnost **Přidat rozhraní API produkt poprvé**.

![Přidání rozhraní API][api-management-add-apis-to-product]

Vyberte požadované rozhraní API a klikněte na **Uložit**.

## <a name="add-description"> </a>Přidejte informace popisující k produktu

Karta **Nastavení** umožňuje poskytovat podrobné informace o produktu, například její účel, umožňuje přístup k rozhraní API a dalšími informacemi. Obsah je určený jenom pro vývojáře, které budou volání rozhraní API a můžete se měly zapisovat ve formátu prostého textu nebo kód HTML.

![Nastavení produktu][api-management-product-settings]

Zaškrtněte **vyžaduje předplatné** pro vytvoření chráněné produktu, který vyžaduje předplatné pro použití, nebo zrušte zaškrtnutí políčka vytvořit otevřít produkt, kterou můžete volat bez přihlášení k odběru.

Pokud chcete ručně všechny produktu předplatné žádostí o schválení, vyberte možnost **vyžadovat schválení předplatného** . Ve výchozím nastavení jsou všechna předplatná s klíčem product automaticky udělena.

Pokud chcete povolit účty vývojář k odběru tisknutím dělený součinem jejich, zaškrtněte políčko **Povolit víc předplatných** a volitelně určete omezení. Pokud není zaškrtnuté toto políčko, každý účet vývojář odběr jen jednou dělený součinem jejich.

Volitelně zadejte do pole **podmínky použití** popisující podmínky použití produktu, které předplatitele musíte přijmout k použití produktu.

## <a name="publish-product"> </a>Publikovat k produktu

Před rozhraní API v produktu lze volat, musí být publikováno produktu. Na kartě **souhrnné informace** k produktu klikněte na **Publikovat**a potom klikněte na **Ano, publikujte** potvrďte. Soukromá dříve publikované produktu, klikněte na **Zrušit publikování**.

![Publikování produktu][api-management-publish-product]

## <a name="make-visible"> </a>Aktivujte produkt pro vývojáře

Karta **viditelnost** umožňuje vybrat, jaké role budou moct najdete v článku produktu na portálu pro vývojáře a odběr dělený součinem jejich.

![Přehled produktu][api-management-product-visiblity]

Zapnutí nebo vypnutí viditelnost produkt pro vývojáře ve skupině, zaškrtněte nebo zrušte zaškrtnutí políčka vedle skupiny a klikněte na **Uložit**.

>Další informace najdete v článku [jak vytvářet a používat skupiny pro správu účtů vývojář správy rozhraní API Azure][].

## <a name="view-subscribers"> </a>Zobrazit účastníky k produktu

Na kartě **Účastníci** jsou uvedena vývojáři, kteří odběru jste se přihlásili dělený součinem jejich. Kliknutím na název vývojáře můžete zobrazit podrobnosti a nastavení pro každý vývojář. V tomto příkladu žádné vývojáři ještě k odběru služeb produktu.

![Vývojáři][api-management-developer-list]

## <a name="next-steps"> </a>Další kroky

Jakmile se přidají požadované rozhraní API a produkt publikován, můžete vývojáři odběr dělený součinem jejich a začněte volat rozhraní API. Kurz, který ukazuje tyto položky stejně jako konfiguraci rozšířeného produktu najdete v článku [jak vytvořit a nakonfigurovat nastavení rozšířeného produktu v části Správa rozhraní API Azure][].

Další informace o práci s produkty viz následující video.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Zobrazit účastníky k produktu]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Jak vytvořit a používat skupiny pro správu účtů vývojář správy rozhraní API Azure]: api-management-howto-create-groups.md
[Jak vytvořit a nakonfigurovat nastavení rozšířeného produktu v části Správa rozhraní API Azure]: api-management-howto-product-with-rules.md 