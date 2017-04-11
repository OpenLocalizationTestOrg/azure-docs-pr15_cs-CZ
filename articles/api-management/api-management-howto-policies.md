<properties 
    pageTitle="Zásady správy Azure rozhraní API | Microsoft Azure" 
    description="Naučte se vytvořit, upravit a konfigurace zásad správy API." 
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


#<a name="policies-in-azure-api-management"></a>Zásady správy Azure rozhraní API

V části Správa rozhraní API Azure jsou zásady výkonné schopnosti, pomocí kterých vydavatele, kterého chcete změnit chování rozhraní API prostřednictvím konfigurace systému. Zásady jsou kolekce příkazy, které jsou proveden sebou na žádost nebo odpovědi rozhraní API. Oblíbené příkazy obsahuje převodu formátu XML na JSON a volání sazba omezení omezení počtu příchozí hovory od vývojáře. Mnoho dalších zásad jsou k dispozici mimo pole.

V tématu [Zásady odkaz][] úplný seznam zásad příkazy a jejich nastavení.

Zásady použijí uvnitř brány, které najdete mezi spotř rozhraní API a spravované rozhraní API. Brána obdrží všechny žádosti o a obvykle předá je nezměněného základní rozhraní API. Zásady však můžete použít změny na příchozí žádosti a odchozí odpověď.

Zásady výrazů lze použít jako hodnoty atributu nebo textové hodnoty v některém z zásady správy rozhraní API, dokud zásadu určuje jinak. Několik zásad například zásady [řízení toku][] a [nastavit proměnnou][] vycházejí zásad výrazů. Další informace najdete v tématu [Upřesnit zásady][] a [zásady výrazů][].

## <a name="scopes"> </a>Konfigurace zásad
Zásad je možné konfigurovat globálně nebo v rozsahu [produktu][], [rozhraní API][] nebo [operace][]. Konfigurace zásad, přejděte do editoru zásady na portálu aplikace publisher.

![Nabídka zásady][policies-menu]

Editor zásady se skládá ze tří hlavních částí: obor zásad (nahoře), definici zásad kde zásady se upravují (vlevo) a příslušným seznamu (vpravo):

![Zásady editor][policies-editor]

Chcete-li začít konfigurace zásad je potřeba nejdřív vybrat obor jakou má zásadu použít. Snímek obrazovky pod **Starter** vybere produktu. Všimněte si, že Čtvereček symbol u názvu zásad označuje, že je zásada použita už na této úrovni.

![Rozsah][policies-scope]

Protože již byla použita zásada, konfiguraci zobrazený v zobrazení definice.

![Konfigurace][policies-configure]

Zásady se zobrazí jen pro čtení na začátku. Chcete-li upravit definici klikněte na akce **Konfigurace zásad** .

![Úpravy][policies-edit]

Definice zásad je jednoduchý dokument XML, který popisuje posloupnost příkazů příchozí a odchozí. XML můžete upravovat přímo v okně definice. Seznam příkazů slouží k pravému a příkazy k dispozici oboru jsou a povoleny zvýrazněný. jak je ukázáno **Limit volání sazba** příkazem snímek výše.

Klepnutí na příkaz povolené přidá odpovídající XML umístění kurzoru v definice zobrazení. 

>[AZURE.NOTE] Pokud není povolený zásad, které chcete přidat, ujistěte se, že jste v správný rozsah pro tuto zásadu. Každý prohlášení o zásadách slouží k použití v některých obory a zásady oddíly. Zobrazíte zásad oddíly a obory pro zásady zaškrtněte části **použití** pro tuto zásadu [Zásad odkaz][].

Úplný seznam zásad příkazy a jejich nastavení jsou k dispozici v [Zásad odkaz][].

Například pokud chcete přidat nový příkaz omezit příchozí žádosti na určené IP adresy, umístěte kurzor jenom do obsahu `inbound` elementů XML a klikněte na příkaz **omezit volající IP adresy** .

![Zásady omezení][policies-restrict]

Tím se přidá fragmentu kódu XML k `inbound` prvek, který obsahuje pokyny ke konfiguraci příkazu.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Pokud chcete omezit příchozí žádosti a přijmout jenom ty z adresy IP 1.2.3.4 upravit XML následujícím způsobem:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Uložení][policies-save]

Po dokončení konfigurace příkazy pro zásady, klikněte na tlačítko **Uložit** a změny se rozšíří do bránu pro správu rozhraní API okamžitě.

##<a name="sections"> </a>Konfigurace zásad Principy

Zásady je posloupnost příkazů, které spustit, aby žádost a odpověď. Konfigurace rozdělen řádně podporovat `inbound`, `backend`, `outbound`, a `on-error` sections uvedeno v následující konfiguraci.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Pokud dojde k chybě během zpracování požadavku, zbývající kroky v `inbound`, `backend`, nebo `outbound` oddíly se přeskočilo a spuštění přejde k příslušným v `on-error` oddíl. Tak, že zaškrtnete zásad příkazy v `on-error` oddíl chybu můžete procházet pomocí `context.LastError` vlastnost, zkontrolovat a přizpůsobit pomocí chyby odpověď `set-body` zásady a konfigurace, co se stane, když dojde k chybě. Existuje kódy chyb předdefinované postup a chyby, ke kterým může dojít během zpracování zásad příkazy. Další informace najdete v tématu [zpracování chyb v zásady správy rozhraní API](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Protože zásady může být zadán na různých úrovních (globální produkt, rozhraní api a operace) konfigurace umožňuje můžete určit pořadí, ve kterém příkazy definice zásad spustit z hlediska zásad nadřazené. 

V následujícím pořadí vyhodnoceny zásad obory.

1. Globální rozsah
2. Rozsah produktu
3. Rozhraní API oboru
4. Operace oboru

Příslušným částem je jsou vyhodnoceny podle umístění `base` prvek, pokud je k dispozici.

Například pokud máte zásadu globální úrovni a zásada nakonfigurovaná pro rozhraní API, pak při každém této konkrétní API obě zásady se použije. Rozhraní API správy umožňuje deterministický řazení kombinované zásad příkazy prostřednictvím základní prvek. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

V příkladu definici zásad výše uvedená `cross-domain` údajů by provedení před následovat vyšší zásad, které chcete zapnout, `find-and-replace` zásad.

Pokud stejné zásady se zobrazí v prohlášení o zásadách dvakrát, naposledy vyhodnocené zásada použita. Můžete to přepsat zásad, které jsou definovány na obor vyšší. Zásady v aktuálním oboru v editoru zásad zobrazíte kliknutím na **Přepočet efektivní zásad pro vybraný rozsah**.

Globální zásad má žádné zásady nadřazené a práce s nimi `<base>` prvek v něm nemá žádný vliv. 

## <a name="next-steps"></a>Další kroky

Podívejte se na následující video zásad výrazů.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Odkaz na zásad]: api-management-policy-reference.md
[Produktu]: api-management-howto-add-products.md
[ROZHRANÍ API]: api-management-howto-add-products.md#add-apis 
[Operace]: api-management-howto-add-operations.md

[Rozšířené zásady]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Řízení toku]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Nastavit proměnnou]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Zásady výrazů]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
