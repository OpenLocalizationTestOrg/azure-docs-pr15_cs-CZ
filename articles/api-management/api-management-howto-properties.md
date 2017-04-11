<properties 
    pageTitle="Použití vlastností v zásady správy API Azure" 
    description="Naučte se používat vlastnosti do zásad správy rozhraní API Azure." 
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


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Použití vlastností v zásady správy rozhraní API Azure

Zásady správy rozhraní API jsou výkonné schopnosti, pomocí kterých vydavatele, kterého chcete změnit chování rozhraní API prostřednictvím konfigurace systému. Zásady jsou kolekce příkazy, které jsou proveden sebou na žádost nebo odpovědi rozhraní API. Zásady příkazy můžete vytvořen pomocí literál typu textové hodnoty, zásady výrazy a vlastnosti. 

Jednotlivé instance služby správy rozhraní API obsahuje sadu vlastností klíč/dvojice, které jsou globální instanci služby. Tyto vlastnosti lze použít ke správě konstantní řetězcových hodnot ve všech konfigurace rozhraní API a zásady. Každá vlastnost má následující atributy.


| Atribut | Typ            | Popis                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Jméno      | řetězec          | Název vlastnosti. Může obsahovat jenom písmena, číslice, období, přerušované čáry a podtržítka. |
| Hodnota     | řetězec          | Hodnota vlastnosti. Nemusí být prázdné nebo obsahovat pouze mezery.                           |
| Tajná    | Logická hodnota         | Určuje, zda hodnota je tajná a by měl být šifrovaná.                                |
| Značky      | pole řetězce | Volitelné značky, je-li můžete použít k filtrování seznamu vlastností.                               |

Na portálu Publisheru na kartě **Vlastnosti** jsou nakonfigurovány vlastnosti. V následujícím příkladu jsou nakonfigurované tři vlastnosti.

![Vlastnosti][api-management-properties]

Nemovitostí s hodnotou může obsahovat literálů a [zásady výrazů](https://msdn.microsoft.com/library/azure/dn910913.aspx). Následující tabulka ukazuje předchozí vlastnosti tři ukázkové a jejich atributy. Hodnota `ExpressionProperty` je zásad výraz, který vrátí hodnotu typu string obsahující aktuální datum a čas. Vlastnost `ContosoHeaderValue` označen jako tajné, tak, aby jeho hodnotu nezobrazuje.

| Jméno               | Hodnota                      | Tajná | Značky    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | NEPRAVDA  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | PRAVDA   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | NEPRAVDA  |         |

## <a name="to-use-a-property"></a>Chcete-li použít vlastnost

Použít vlastnost v zásadu, vlastnost Název vložte do dvojitých dvojice složených závorek jako `{{ContosoHeader}}`, jak je ukázáno v následujícím příkladu.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

V tomto příkladu `ContosoHeader` slouží jako název záhlaví `set-header` zásady, a `ContosoHeaderValue` se použije jako hodnota záhlaví. Při vyhodnocování tuto zásadu během žádost nebo odpověď na bránu pro správu rozhraní API `{{ContosoHeader}}` a `{{ContosoHeaderValue}}` se nahradí hodnotami příslušných vlastnost.

Vlastnosti mohou sloužit jako úplný atribut nebo hodnoty elementů uvedeno v předchozím příkladu, ale můžete také měly být vložen do nebo společně s částí Doslovný text výrazu, jak je vidět v následujícím příkladu:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Vlastnosti mohou také obsahovat zásad výrazů. V následujícím příkladu `ExpressionProperty` se používá.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Při vyhodnocování tuto zásadu `{{ExpressionProperty}}` nahrazuje s hodnotou: `@(DateTime.Now.ToString())`. Protože hodnota je výraz zásady, parametrem expression Vyhodnocená každá její položka a zásady vytvářejí s jeho spuštění.

Tento mimo můžete otestovat v portálu pro vývojáře voláním operaci, kterou má zásady s vlastnostmi v rozsahu. V následujícím příkladu se nazývá operace s dvěma předchozího příkladu `set-header` zásady s vlastnostmi. Všimněte si, že odpověď obsahuje dva vlastní záhlaví, které byly konfigurují zásady s vlastnostmi.

![Karta Vývojář v portálu][api-management-send-results]

Když se podíváte na [sledování inspektor API](api-management-howto-api-inspector.md) pro hovory, který obsahuje dva předchozí ukázka zásad s vlastnostmi, uvidíte dva `set-header` zásady s vlastnostmi vložené a vyhodnocení výrazu zásad pro vlastnost obsahující výraz zásady.

![Rozhraní API inspektor sledování][api-management-api-inspector-trace]

Všimněte si, že během nemovitostí s hodnotou může obsahovat zásad výrazů, nemovitostí s hodnotou nesmí obsahovat další vlastnosti. Pokud text obsahující odkaz na vlastnost se používá k hodnotou nemovitosti, jako například `Property value text {{MyProperty}}`, tento odkaz vlastnost nebude nahrazen a bude součástí hodnotu.

## <a name="to-create-a-property"></a>Vytvořit vlastnost

Pokud chcete vytvořit vlastnost, klikněte na **Přidat vlastnost** na kartě **Vlastnosti** .

![Přidat vlastnosti][api-management-properties-add-property-menu]

**Název** - **hodnota** jsou požadované hodnoty. -Li tuto vlastnost hodnotu tajná, zaškrtněte políčko **Toto je tajná** . Zadejte volitelné značek Nápověda k uspořádání vlastnosti a klikněte na **Uložit**.

![Přidat vlastnosti][api-management-properties-add-property]

Uložení nové vlastnosti textové pole **Hledat vlastnost** je vyplněn název nové vlastnosti a se zobrazí nová vlastnost. Pokud chcete zobrazit všechny vlastnosti, zrušte **vlastností vyhledávacího** textového pole a stiskněte klávesu enter.

![Vlastnosti][api-management-properties-property-saved]

Informace o vytváření vlastnost pomocí rozhraní REST API najdete v tématu [Vytvoření vlastnost pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Chcete-li upravit vlastnosti

Upravit vlastnost, klikněte na **Upravit** vedle vlastnost na Upravit.

![Úprava vlastností][api-management-properties-edit]

Proveďte požadované změny a klikněte na **Uložit**. Pokud změníte název vlastnosti, se automaticky aktualizují zásad, které odkazují na tuto vlastnost použít nový název.

![Úprava vlastností][api-management-properties-edit-property]

Informace o úpravách vlastnost pomocí rozhraní REST API naleznete v tématu [Úprava vlastnost pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Odstranit vlastnost

Odstranit vlastnost, klikněte na tlačítko **Odstranit** vedle vlastnost na odstranit.

![Odstranit vlastnost][api-management-properties-delete]

Klikněte na **Ano, odstranění** potvrďte.

![Potvrzení odstranění][api-management-delete-confirm]

>[AZURE.IMPORTANT] Pokud je vlastnost odkazuje zásad, nebude možné a vymažete ho úspěšně, dokud neodeberete vlastnost ze všech zásad, které používá.

Informace o odstranění vlastnost pomocí rozhraní REST API najdete v tématu [odstranění vlastnosti pomocí rozhraní REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Hledání a filtr vlastností

Karta **Vlastnosti** obsahuje hledání a filtrování funkce, které vám pomůžou se správou vlastnosti. V seznamu vlastností vyfiltrovat název vlastnosti, zadejte hledaný termín do textového pole **Vlastnosti vyhledávání** . Pokud chcete zobrazit všechny vlastnosti, zrušte **vlastností vyhledávacího** textového pole a stiskněte klávesu enter.

![Hledání][api-management-properties-search]

Pokud chcete filtrovat podle značky hodnoty v seznamu vlastností, zadejte do textového pole **Filtrovat podle značky** jednu nebo více značek. Pokud chcete zobrazit všechny vlastnosti, zrušte zaškrtnutí políčka textové pole **Filtrovat podle klíčových slov** a stiskněte klávesu enter.

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a>Další kroky

-   Další informace o práci s zásady
    -   [Zásady správy rozhraní API](api-management-howto-policies.md)
    -   [Odkaz na zásad](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Zásady výrazů](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Podívejte se na Videoukázku přehledu

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

