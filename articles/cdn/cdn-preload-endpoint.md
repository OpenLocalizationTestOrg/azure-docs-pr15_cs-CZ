<properties
    pageTitle="Předem načíst prostředky na koncový bod Azure CDN | Microsoft Azure"
    description="Naučte se předem načíst režim cached obsahu na CDN koncového bodu."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Předem načíst prostředky na koncový bod Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Ve výchozím nastavení prostředky nejdřív mezipaměti, jako jsou požadovány. To znamená, že první žádost z jednotlivých oblastech může trvat déle vzhledem k tomu, že nebudete mít serverech obsah cached a bude potřeba žádost zdrojový server předat dál. Předem načítání obsahu zabráněno tento první latence přístupů.

Kromě poskytuje lepší zákazníka, předem načítání režim cached prostředky můžete taky zatížení sítě zdrojového serveru.

> [AZURE.NOTE] Předem načítání prostředky je užitečné pro velké události nebo obsah, který zpřístupní současně velkého počtu uživatelů, jako je nová verze film nebo aktualizace softwaru.

Tento kurz vás provede předem načítání režim cached obsahu ve všech uzlech Azure CDN okraje.

## <a name="walkthrough"></a>Návod

1. Na [Portálu Azure](https://portal.azure.com)procházením profilu CDN obsahující koncový bod, který chcete předem nahrát.  Otevře se zásuvné profilu.

2. Klikněte na tlačítko koncového bodu v seznamu.  Otevře se zásuvné koncového bodu.

3. Z koncového bodu zásuvné CDN klikněte na tlačítko Načíst.

    ![Koncový bod zásuvné CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Otevře se zásuvné načíst.

    ![Zásuvné zatížení CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Zadejte úplnou cestu každého majetku chcete nahrát (například `/pictures/kitten.png`) v textovém poli **cestu** .

    > [AZURE.TIP] Další **cestu** textová pole se zobrazí po zadejte text, který umožňuje sestavit seznam více prostředky.  Prostředky můžete odstranit ze seznamu kliknutím na tlačítko se třemi tečkami (...).
    >
    > Cesty musí být relativní adresu URL, která vyhovuje následující [regulárních výrazů](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Každý materiálů musí mít vlastní cesty.  Není žádná funkce zástupných znaků pro předem načtení prostředky.

    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Klikněte na tlačítko **načíst** .

    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Existuje omezení požadavků 10 zatížení za minutu za CDN profilu.

## <a name="see-also"></a>Viz taky
- [Odstranění koncového bodu Azure CDN](cdn-purge-endpoint.md)
- [Azure CDN REST API odkaz – vymazání nebo předem načtení koncový bod](https://msdn.microsoft.com/library/mt634451.aspx)
