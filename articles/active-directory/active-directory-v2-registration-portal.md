<properties
    pageTitle="Témata nápovědy k portálu registrace aplikace | Microsoft Azure"
    description="Popis různých funkcí v portálu Microsoft aplikace registrace."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>Odkaz registrace aplikace
Tento dokument obsahuje kontext a popis různých funkcí v portálu registrace aplikace Microsoft [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Moje aplikace
Tento seznam obsahuje všechny aplikace registrované pro použití s koncový bod Azure AD verze 2.0.  Tyto aplikace dokážou přihlásit uživatelé, kteří mají obou osobní účty z účtu Microsoft a pracovní/školní účty z Azure Active Directory.  Další informace o koncový bod Azure AD verze 2.0, najdete v článku naší [verze 2.0 – přehled](active-directory-appmodel-v2-overview.md).  Tyto aplikace taky lze integrovat s koncovým ověření účtu Microsoft, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Aplikace Live SDK
Tento seznam obsahuje všechny aplikace registrované výhradně pomocí účtu Microsoft.  Není povoleno pro použití s Azure Active Directory jakékoliv.  Toto je místo, kam bude hledat všechny aplikace, které bylo dřív registrovaný u portálu pro vývojáře MSA na `https://account.live.com/developers/applications`.  Všechny funkce, které dřív prováděny `https://account.live.com/developers/applications` lze provádět teď tento nový portál `https://apps.dev.microsoft.com`.  Pokud máte další otázky o aplikace účtu Microsoft, kontaktujte nás.

## <a name="application-secrets"></a>Aplikace tajemství
Aplikace zajišťují přihlašovací údaje, které umožňují aplikace provádět spolehlivé [ověření klienta](http://tools.ietf.org/html/rfc6749#section-2.3) s Azure AD.  V OAuth & OpenID připojení aplikace tajemství je obecně označované jako `client_secret`.  Ve skupinovém rámečku verze 2.0 protokol libovolné aplikaci, která přijímá tokenu zabezpečení jsme narazili na místo s možností zadání web (pomocí `https` schématu) musí používat aplikaci tajná identifikuje Azure AD po zaručené ceny, které tokenu zabezpečení.  Kromě toho všechny native client – tohoto recieves tokeny na zařízení se zakázáno pomocí aplikace tajná ověřování klienta, aby se zamezilo úložiště tajemství v zabezpečený prostředí.

Jednotlivé aplikace může obsahovat dva tajemství platná aplikace v libovolném bodě v čase.  Udržovat dvou tajemství, máte ablilty provádět pravidelných klíče při přechodu myší přes celou prostředí aplikace.  Po migraci jako celek aplikace nové tajná odstraňte staré tajná a můžete vytvořit novou.

V tomto okamžiku jsou povoleny pouze dva typy tajemství aplikace na portálu registrace aplikace.  Výběr **Generovat nové heslo** generovat a uložte sdílený tajná v úložišti svých dat, které můžete použít v aplikaci.  Výběr **Generovat nový klíč pár** vytvoří nový soukromá/pár klíče, můžete stáhnout a použít k ověření klienta Azure AD.

## <a name="profile"></a>Profil
V části profil portálu registrace aplikace mohou sloužit k přizpůsobení přihlašovací stránky pro aplikace.  V současné době můžete změnit přihlašovací stránky aplikace loga, podmínky adresy URL služby a zásady ochrany osobních údajů.  Logo musí být průhledný obrázek bod 48 x 48 nebo 50 × 50 v soubor GIF, PNG nebo JPEG, který je 15 KB nebo menší.  Zkuste změnách hodnot a zobrazování výsledný symbol stránce!

## <a name="live-sdk-support"></a>Podpora živou SDK
Pokud zapnete "Live SDK podpory", vytvoříte všechny aplikace tajemství bude zřízení do Azure AD a úložiště dat Account Microsoft.  To vám umožní aplikace integrovat přímo ke službě Microsoft Account (login.live.com).  Pokud chcete vytvořit aplikaci pomocí Microsoft Account přímo (na rozdíl od koncového bodu verze 2.0 Azure AD pomocí), nezapomeňte, že je povolená Live SDK podpory.

Zákaz Live SDK podpory bude zapsat tajná aplikace jenom do dat Azure AD ukládat.  Azure AD datový úložiště zahrne podnikové předpisy, které umožňují splňují určité organizace pro standardy, například dodržováním ustanovení zákona FISMA.  Pokud povolíte Live SDK podporu, nemusí aplikace dosáhnout některé z těchto standardy dodržování.

Pokud jenom plánujete používat koncový bod Azure AD verze 2.0, můžete zakázat bezpečně Live SDK podpory.

