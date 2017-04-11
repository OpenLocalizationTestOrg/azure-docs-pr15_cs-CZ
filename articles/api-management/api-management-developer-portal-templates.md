<properties 
    pageTitle="Jak přizpůsobit portálu pro vývojáře Správa rozhraní API Azure pomocí šablon | Microsoft Azure" 
    description="Zjistěte, jak přizpůsobit portálu pro vývojáře Správa API Azure pomocí šablony." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Jak přizpůsobit portálu pro vývojáře Správa rozhraní API Azure pomocí šablony

Azure rozhraní API Správa poskytuje několik přizpůsobení funkcí, které správcům umožňují [přizpůsobit vzhled a chování portálu pro vývojáře](api-management-customize-portal.md)i přizpůsobit obsah stránky portálu vývojář pomocí sady šablon, které konfigurace obsahu samotné stránky. Použití [DotLiquid](http://dotliquidmarkup.org/) syntaxe a zadané sady prostředky lokalizované řetězce, ikony a ovládacích prvků stránky, máte velkou flexibilitu ke konfiguraci obsahu stránek podle potřeby tyto šablony.

## <a name="developer-portal-templates-overview"></a>Přehled portálu šablon vývojář

Karta Vývojář v portálu šablony spravuje v portálu pro vývojáře správcům instanci služby správy rozhraní API. Správa šablon vývojář, přejděte na vaše rozhraní API správy instance služby na portálu klasické Azure a klikněte na tlačítko **Procházet**.

![Karta Vývojář v portálu][api-management-browse]

Pokud jste už v portálu Publisheru, máte přístup k portálu pro vývojáře po kliknutí na **portál pro vývojáře**.

![Karta Vývojář v portálu nabídky][api-management-developer-portal-menu]

Pro přístup k portálu šablony vývojář, klikněte na ikonu přizpůsobit na levé straně zobrazení nabídky Úpravy a klikněte na možnost **šablony**.

![Karta Vývojář v portálu šablony][api-management-customize-menu]

Seznam šablon nabízí několik kategorie šablon překrývajícím různé stránky v portálu pro vývojáře. Každá šablona se liší, ale kroky upravovat a publikovat změny zůstávají stejné. Chcete-li upravit šablonu, klikněte na název šablony.

![Karta Vývojář v portálu šablony][api-management-templates-menu]

Klepnutím na šablonu přejdete na stránku portálu vývojář, který lze přizpůsobit tak, že této šabloně. V tomto příkladu **seznamu produktů** se zobrazí šablony. Šablona **seznamu produktů** určuje oblast obrazovky označen červené obdélník. 

![Šablony seznamu produktů][api-management-developer-portal-templates-overview]

Některé šablony jako šablony **Uživatelského profilu** přizpůsobit různé části stejné stránce. 

![Šablony profilu uživatele][api-management-user-profile-templates]

Editor pro každý šablona portál vývojář má dvě části zobrazený ve spodní části stránky. Levé straně zobrazuje podokna úprav šablony a pravé straně datového modelu doplňku pro šablonu. 

Úpravy podokno šablony obsahuje kód, který určuje vzhled a chování na stránku v portálu pro vývojáře. Značky v šabloně používají syntaxi [DotLiquid](http://dotliquidmarkup.org/) . Jeden Oblíbené editor pro DotLiquid je [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Změny provedené šabloně během úprav zobrazené v reálném čase v prohlížeči, ale nejsou viditelné pro vaši zákazníci do můžete [Uložit](#to-save-a-template) a [Publikovat](#to-publish-a-template) šablony.

![Šablona značky][api-management-template]

Podokno **šablony dat** obsahuje vodítko do datového modelu pro osob, které jsou dostupné pro použití v určité šablony. Tato příručka poskytuje zobrazením dynamických dat, které jsou aktuálně zobrazeny v portálu pro vývojáře. Po kliknutí na obdélník v pravém horním rohu podokna **data šablony** můžete rozbalit podokna šablony.

![Šablony datového modelu][api-management-template-data]

V předchozím příkladu jsou dva produkty v portálu pro vývojáře byly načtená z kontingenčního seznamu data zobrazená v podokně **data šablony** , jak je vidět v následujícím příkladu.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Revize v šabloně **seznam produktů** zpracuje data, která chcete zadejte požadované výstupní pomocí iterace kolekci produktů můžete zobrazit informace a odkaz na každý jednotlivým produktům. Poznámka `<search-control>` a `<page-control>` prvky na značku. Tyto ovládat zobrazení hledání a stránkování ovládací prvky na stránce. `ProductsStrings|PageTitleProducts`odkaz na lokalizované řetězec obsahující `h2` text záhlaví stránky. Seznam řetězec zdrojů, stránek a ikony umožňující použít v portálu šablony vývojář najdete v tématu [Správa API referenční informace pro vývojáře portálu šablony](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Pokud chcete uložit šablonu

Uložení šablony, klikněte na Uložit v editoru šablony.

![Uložení šablony][api-management-save-template]

Uložené změny nejsou živou v portálu pro vývojáře až po publikování.

## <a name="to-publish-a-template"></a>Chcete-li publikovat šablony

Můžete publikovat uložené šablony, buď jednotlivě nebo všechny najednou. Publikování jednotlivých šablony, klikněte na Publikovat v editoru šablony.

![Publikování šablony][api-management-publish-template]

Kliknutím na tlačítko **Ano** potvrďte a vyberte šablonu aktivní na portálu pro vývojáře.

![Potvrďte publikování][api-management-publish-template-confirm]

V seznamu šablony publikovat všechny verze aktuálně nepublikované šablony, klikněte na **Publikovat** . Nepublikované šablon jsou označené hvězdičkou za název šablony. V tomto příkladu je publikován šablony **seznamu produktů** a **Product** .

![Publikování šablony][api-management-publish-templates]

Klikněte na **publikovat přizpůsobení** potvrďte.

![Potvrďte publikování][api-management-publish-customizations]

Jsou efektivní okamžitě v portálu pro vývojáře nově publikované šablony.

## <a name="to-revert-a-template-to-the-previous-version"></a>Pokud se chcete vrátit šablonou pro předchozí verze

Vrátit šablonou pro předchozí publikované verze, klikněte na obnovit v editoru šablony.

![Vrátit šablony][api-management-revert-template]

Klikněte na **Ano** potvrďte.

![Potvrďte][api-management-revert-template-confirm]

Po dokončení operace vrácení změn je živou v portálu pro vývojáře dříve publikované verze šablony.

## <a name="to-restore-a-template-to-the-default-version"></a>Chcete-li obnovit na výchozí verzi šablony

Obnovení výchozí verze šablony je proces se dvěma kroky. Nejdřív musíte obnovit šablony a potom musí být publikován obnovených verzích.

Obnovení jednoho šablony na výchozí verzi, klikněte na obnovit v editoru šablony.

![Vrátit šablony][api-management-reset-template]

Klikněte na **Ano** potvrďte.

![Potvrďte][api-management-reset-template-confirm]

Všechny šablony jejich výchozí verze, klepnutím na tlačítko **Obnovit výchozí šablony** seznamů šablony.

![Obnovení šablony][api-management-restore-templates]

Obnovená musí pak publikovat šablony jednotlivě nebo v celém dokumentu pomocí kroků uvedených v tématu [publikování šablony](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Referenční informace pro vývojáře portálu šablony

Referenční informace pro vývojáře portálu šablony, řetězec zdroje, ikony a ovládacích prvků stránky najdete v článku [Správa rozhraní API vývojář portálu šablony](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Podívejte se na Videoukázku přehledu

Podívejte se na následující video a zjistěte, jak přidat diskusní vývěsku a hodnocení rozhraní API a operace stránky v portálu pro vývojáře pomocí šablony.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







