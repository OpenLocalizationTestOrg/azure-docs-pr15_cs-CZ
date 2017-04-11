<properties
   pageTitle="Rychlý úvod Azure AD grafu rozhraní API | Microsoft Aure"
   description="Azure Active Directory grafu rozhraní API poskytuje programový přístup k Azure AD pomocí rozhraní REST API OData koncové body. Aplikace můžete provádět rozhraní API graf vytvořit, číst, aktualizovat a odstranění (CRUD) operací na adresář data a objekty."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Rychlý úvod Azure AD grafu rozhraní API

Rozhraní API grafu Azure Active Directory (AD) poskytuje programový přístup k Azure AD pomocí rozhraní REST API OData koncové body. Aplikace můžete provádět rozhraní API graf vytvořit, číst, aktualizovat a odstranění (CRUD) operací na adresář data a objekty. Pomocí rozhraní API grafu můžete například vytvořit nového uživatele, zobrazit nebo aktualizovat vlastnosti uživatele, změnit heslo uživatele, zkontrolovat členství ve skupinách na základě rolí přístup pomocí protokolu, zakázání nebo odstranění uživatele. Další informace o funkcích rozhraní API graf a scénáře aplikací najdete v článku [rozhraní API Azure AD graf](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) a [rozhraní API požadavcích Azure AD graf](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure AD grafu rozhraní API funkce je také k dispozici prostřednictvím [Aplikace Microsoft Graph](https://graph.microsoft.io/)jednotné rozhraní API, který obsahuje rozhraní API z jiných služeb společnosti Microsoft, jako je Outlook, Onedrivu, OneNote, plánování a Office Graphu, získat přístup prostřednictvím jeden koncový bod a pomocí jednoho přístupový token.

## <a name="how-to-construct-a-graph-api-url"></a>Jak vytvořit adresu URL rozhraní API grafu

V grafu API pro přístup k datům adresář a objekty (to znamená, zdroje nebo entity), podle kterých chcete provádět operace CRUD můžete adresy URL podle protokolu otevřít Data (OData). Adresy URL používané v grafu API tvořeny čtyři hlavních částí: service root, identifikátor klienta, cestu zdroje a možnosti řetězce dotazu: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Prohlédněte příklad následující adresu URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Service Root**: V Azure AD graf rozhraní API aplikace, vždycky je kořen služby https://graph.windows.net.
- **Identifikátor URI klienta**: to může být (ověřený registrovaný) název domény, v příkladu výše contoso.com. Lze ji ID objektu klienta nebo "myorganiztion" nebo "Já" alias. Další informace najdete v tématu [adresování entity a operace v rozhraní API grafu).](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)
- **Cesta zdroje**: Tato část adresy URL identifikuje příslušný zdroj k serveru (uživatelů, skupin, určitého uživatele, nebo určitou skupinou atd.) Ve výše uvedeném příkladu je nejvyšší úrovně "skupiny" na adresu, kterou nastavení zdroje. Můžete taky řešit jenom konkrétní entity, například "uživatelé / {objektu}" nebo "uživatelé/userPrincipalName".
- **Parametry dotazu**:? odděluje části zdroje cestu s oddílem parametry dotazu. V všechny žádosti o v grafu API není potřeba parametr dotazu "verze rozhraní api". Rozhraní API graf taky podporuje následující možnosti dotazu OData: **$filter** **$orderby**, **$, rozbalte položku**, **$top**a **$format**. Nejsou podporovány následující možnosti dotazu aktuálně: **$count** **$inlinecount**a **$skip**. Další informace najdete v tématu [podporované dotazy, filtrů a stránkování možnosti v rozhraní API Azure AD graf](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graf rozhraní API verze

Určete verzi pro žádost o grafu API v dotazu parametr "verze rozhraní api". U verze 1.5 a novější použijte hodnotu číselném verze; verze rozhraní API = 1,6. Pro starší verze použijete datum řetězcem, který vyhovuje do formátu RRRR-MM-DD; například verze rozhraní api = 2013-11-08. Funkce použijte řetězec "beta"; například rozhraní api verze = beta. Další informace o rozdílech mezi verzemi rozhraní API grafu najdete v článku [Správa verzí rozhraní API Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graf rozhraní API metadat

Vrátit soubor metadat rozhraní API grafu, přidejte segmentu "$metadata" za identifikátor klienta v příkladu adresy URL, vrátí následující adresu URL metadat pro firmu ukázku používané Průzkumníkem grafu: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Zadejte tuto adresu URL v adresním řádku prohlížeče zobrazíte metadata. Dokument metadat CSDL vrácené popisuje entity a komplexní, jejich vlastnosti a funkce a akce zveřejněné příslušným verzi rozhraní API grafu požadujete. Vynechání parametru verze rozhraní api vrátí metadat pro nejnovější verzi.

## <a name="common-queries"></a>Běžné dotazy

[Běžné dotazy rozhraní API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) jsou uvedeny běžné dotazy, které se dá používat se zobrazí Azure AD se v grafu, včetně dotazy, které můžete použít k přístupu k prostředkům nejvyšší úrovně v adresáři vašeho a k provádění operací ve vašem adresáři.

Například `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` vrátí firemních informací pro adresáře contoso.com.

Nebo `https://graph.windows.net/contoso.com/users?api-version=1.6` zobrazí seznam všech objektů uživatele v adresáři contoso.com.

## <a name="using-the-graph-explorer"></a>Pomocí Průzkumníka grafu

Průzkumník grafu pro rozhraní API Azure AD grafu můžete použít k vytvoření dotazu dat adresáře při vytváření aplikace.

> [AZURE.IMPORTANT] Průzkumník grafu nepodporuje psaní nebo odstranění dat z adresáře. Pouze můžete provádět operace čtení v adresáři Azure AD pomocí Průzkumníka grafu.

Výstupu by se zobrazí, pokud jste byli přejděte do aplikace Explorer grafu, zaškrtněte políčko použít ukázku společnosti a zadejte `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` pro zobrazení všech uživatelů v adresáři ukázku:

![Azure AD grafu rozhraní API aplikace explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Načtení Explorer grafu**: K načtení nástroje, přejděte na [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Klikněte na položku **Společnosti Ukázka použití** spustit Průzkumníka grafu data z klienta vzorku. Přihlašovací údaje pro použití společnosti ukázku nepotřebujete. Můžete taky můžete klikněte na tlačítko **přihlásit** a přihlaste se pomocí svých přihlašovacích údajů účet Azure AD spustit Průzkumníka grafu vašeho klienta. Pokud spustíte grafu Explorer vlastního klienta, vy nebo váš správce muset souhlas při přihlášení. Pokud máte předplatné Office 365, máte automaticky Azure AD klienta. Přihlašovací údaje, které používáte pro přihlášení k Office 365 jsou ve skutečnosti Azure AD účty a můžete pomocí těchto přihlašovacích údajů v Průzkumníkovi graf.

**Spuštění dotazu**: spuštění dotazu zadejte dotaz v textovém poli požadavek a klikněte na tlačítko **získat** nebo klikněte na **zadat** kód. Výsledky se zobrazí v poli odpověď. Například `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` zobrazí seznam všech objektů skupina v adresáři ukázku.

Upozorňujeme na následující funkce a omezení Průzkumníka grafu:
- Nastavení možností funkce Automatické dokončování na zdroje. Se toto kliknutím na **Použití ukázku společnosti** a potom klikněte na textové pole žádost (adresa URL společnosti umístění). Můžete vybrat zdroj nastavení v rozevíracím seznamu.

- Podporuje "Já" a "TatoOrganizace" adresování aliasy. Můžete třeba použít `https://graph.windows.net/me?api-version=1.6` vrátíte objektu uživatele přihlášený uživatel nebo `https://graph.windows.net/myorganization/users?api-version=1.6` k vrácení všech uživatelů v adresáři aktuální. Všimněte si, že "Já" pomocí aliasu vrátí chybu pro firmu ukázku protože neexistuje žádný přihlášený uživatel díky žádost.

- Oddíl záhlaví odpověď. Lze použít při řešení problémů, ke kterým dochází při spouštění dotazů.

- Prohlížeč JSON pro odpověď s možnostmi rozbalit a sbalit.

- Žádná podpora zobrazení miniatur fotografii.

## <a name="using-fiddler-to-write-to-the-directory"></a>Použití Fiddler k zápisu do adresáře

Pro účely tohoto průvodce rychlým startem můžete použít Web ladění Fiddler k ověření "psaní operace proti vám Azure AD adresáře. Další informace a chcete nainstalovat Fiddler najdete v tématu [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

V následujícím příkladu použijete Fiddler Web ladění k vytvoření nové skupiny zabezpečení "MyTestGroup" v adresáři Azure AD.

**Získání přístupový token**: Azure AD graf zobrazíte klientů muset úspěšného ověření Azure AD nejdřív. Další informace najdete v tématu [Ověření scénáře Azure AD](active-directory-authentication-scenarios.md).

**Vytváření a spouštění dotazů**: proveďte následující kroky.

1. Otevřete Web ladění Fiddler a přejděte na kartu **autora** .
2. Když chcete vytvořit novou skupinu zabezpečení, vyberte **Publikovat** jako prostředek HTTP z rozevírací nabídky. Další informace o operace a oprávnění objektu skupiny najdete v článku [skupiny](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) v rámci [grafu Azure AD REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. V poli vedle **příspěvku**, zadejte následující požadavku na adresu URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Je třeba nahradit mytenantdomain s názvem domény adresáře Azure AD.

4. V oblasti přímo pod příspěvkem v rozevíracím seznamu zadejte tento příkaz:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] DOSADIT vaše &lt;přístupový token&gt; s přístupový token Azure AD adresáře.

5. V poli **požadavku** zadejte tento příkaz:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Další informace o vytváření skupin najdete v tématu [Vytvořit skupinu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Další informace o Azure AD entity a typ zadaných zveřejněné příslušným grafu a informace o operacích, které lze provádět na nich s grafu najdete v článku Principy [Azure AD grafu REST API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Další kroky

- Další informace o rozhraní [API Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Další informace o [rozsahů oprávnění rozhraní API Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
