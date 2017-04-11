<properties
    pageTitle="Rozšířené žádosti o omezení službou Azure rozhraní API Management"
    description="Informace o vytvoření a použití flexibilní kvóty a sazby omezení zásady službou Azure rozhraní API Management."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Rozšířené žádosti o omezení službou Azure rozhraní API Management

Možnost omezení příchozí žádosti je klíčové rolí Správa rozhraní API Azure. Buď kontrolou sazbu žádosti o nebo celkový žádosti o/data převedena rozhraní API správa umožňuje rozhraní API poskytovatelů Chraňte zneužití jejich rozhraní API a vytvářet hodnoty pro různé úrovně rozhraní API produktu.

## <a name="product-based-throttling"></a>Omezení na základě produktu
Datum omezení sazba funkce byly omezené právě obor vymezený na konkrétní předplatné s kódem Product (v podstatě klíč), definice na portálu Správa rozhraní API aplikace publisher. To je užitečné pro rozhraní API poskytovatele použít limity pro vývojáře, kteří se přihlásili k jejich rozhraní API, ale to nepomůže, například v omezení jednotlivé koncoví uživatelé rozhraní API. Je možné, že pro jednotlivé uživatele vývojáře aplikace používat celý kvóty a potom znemožnit ostatních zákazníků vývojář k používání aplikace. Také několik zákazníci, kteří mohou generovat velkého množství žádosti o omezit přístup k občas uživatele.

## <a name="custom-key-based-throttling"></a>Vlastní klíč na základě omezení
Nové zásady [sazba limit tak, že klíč](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [kvóta tak, že klíč](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) poskytují výrazně flexibilnější řešení ovládacímu prvku přenosy. Tyto nové zásady umožňují definování výrazů k identifikaci klávesy, pomocí kterých se použijí k sledování využití provozu. Způsob, jak to funguje pro je nejsnažší znázorněn příklad. 

## <a name="ip-address-throttling"></a>Omezení IP adresa
Tyto zásady omezit IP adresa jednoho klienta jenom 10 hovory každou minutu s celkem 1,000,000 volání a 10 000 kilobajtů šířky pásma za měsíc. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Pokud všechny klienty na Internetu použili jedinečnou IP adresu, může to být efektivní způsob omezení použití uživatelem. Však je velmi pravděpodobné, aby několik uživatelů se sdílení jednu veřejnou IP adresu z důvodu je přístup k Internetu přes zařízení. Bez ohledu na to, pro rozhraní API, pomocí kterých neověřené Accessu `IpAddress` může být nejlepší možností.

## <a name="user-identity-throttling"></a>Omezení identita uživatele
Pokud ověření koncový uživatel a omezení klíč lze vytvořit na základě informací, které jednoznačně identifikuje, která uživatele.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

V tomto příkladu jsme extrahovat se tak mohli ověřovat záhlaví, převeďte ho do `JWT` objektu a předmět tokenu identifikaci uživatele a použít je jako sazba omezení klíče. Pokud identita uživatele uložený v `JWT` jako jednu z druhé tvrdí, pak, že hodnota lze použít v jeho umístění.

## <a name="combined-policies"></a>Kombinované zásady
Přestože nové zásady omezení poskytuje větší kontrolu než existující omezení zásady, je pořád hodnota kombinace obou funkcí. Omezení předplatné klíčem product ([sazba volání Limit tak, že předplatné](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavte kvóta využití prostředků tak, že předplatné](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) je skvělý způsob, jak povolit monetizing z rozhraní API tak, že nabíjení podle úrovně použití. Lepší nebo barev ovládací prvek moci omezení uživatelem je doplňková a zabrání snížení prostředí jiného chování jednoho uživatele. 

## <a name="client-driven-throttling"></a>Klient řízený úsilím omezení
Když klávesu omezení je definována pomocí [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx), pak je poskytovatele rozhraní API, který je výběr jak omezení má obor vymezený. Však může vývojář chtít určit, jak budou ohodnocení limit vlastní zákazníky. To může povolit rozhraní API poskytovatele zavedením vlastní záhlaví umožňuje vývojáře klientské aplikaci komunikovat klávesy rozhraní API.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Díky vývojáře klientské aplikaci rozhodnout se, jak se mají vytvořit sazbu omezení klíče. Pomocí pár věcí správně vynalézavosti vývojář klienta vytvářet své vlastní vrstvy sazby přidělování sady kláves pro uživatele a otočení použití klíče.

## <a name="summary"></a>Souhrn
Azure rozhraní API Správa poskytuje sazby a nabídky omezení chránit i přidejte hodnotu do rozhraní API služeb. Nové omezení zásady s vlastní oboru pravidla umožňují lepší nebo barev publikum nemůže ovládat tyto zásady Povolit vaši zákazníci k vytváření ještě lepších aplikací. Příklady v tomto článku ukazují použití tyto nové zásady výrobních míru omezení s klienta IP adresy, identity uživatele a hodnoty klienta generovaného klíče. Můžou ale nastat mnoho části zprávy, která lze použít například uživatelského agenta neúplné cesta URL velikost zprávy.

## <a name="next-steps"></a>Další kroky
Zkontrolujte sdělte nám svůj názor v vlákna Disqus tohoto tématu. Je skvělé přehrajte o jiné potenciální klíčové hodnoty, které byly logické volba ve vašich scénářích.

## <a name="watch-a-video-overview-of-these-policies"></a>Podívejte se na video přehled tyto zásady
Další informace o [sazba limit tak, že klíč](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [kvóta tak, že klíč](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady uvedené v tomto článku podívejte se prosím následující video.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
