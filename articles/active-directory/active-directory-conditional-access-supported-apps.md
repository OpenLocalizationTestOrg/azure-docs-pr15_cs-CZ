<properties
    pageTitle="Aplikace, které využívají pravidla podmíněného přístupu v Azure Active Directory | Microsoft Azure"
    description="Řízení přístupu podmíněné zkontroluje Azure Active Directory pro určité podmínky když ověřuje uživatele a umožníte aplikaci access."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Aplikace, které využívají pravidla podmíněného přístupu v Azure Active Directory

Pravidla podmíněného přístupu podporují služby Azure Active Directory (Azure AD)-připojení aplikace, předem integrované federované software jako aplikací služby (SaaS), aplikace, které využívají heslo jednotné přihlašování (SSO) řádek podnikových aplikací a aplikace, které využívají proxy server Azure AD aplikace. Podrobný seznam aplikací, které můžete použít podmíněné přístup najdete v článku [službu povolenou s podmíněným přístupem](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Podmíněný přístup funguje v obou případech s přenosných a stolních aplikace, které používají moderní ověřování. V tomto článku jsme vysvětlovat, jak access funguje v aplikacích přenosných a stolních.

Azure AD přihlašovací stránky můžete v aplikacích, které používají moderní ověřování. Na přihlašovací stránce výzvě pro vícefaktorové ověřování. Zprávy se zobrazí, pokud je blokován přístup uživatele. Moderní ověřování je nutný pro zařízení ověření s Azure AD, jsou vyhodnoceny zásad podmíněné přístupu zařízení.

Je důležité vědět aplikace, které můžete použít pravidla podmíněného přístupu a kroky, které možná budete muset přijmout zajistit další vstupní body aplikace.

## <a name="applications-that-use-modern-authentication"></a>Aplikace, které využívají moderní ověřování

> [AZURE.NOTE] Pokud máte podmíněné přístupu v Azure AD, který má ekvivalent v Office 365, konfigurujte zásady obou podmíněné přístupu. To platí, například zásady podmíněné přístupu pro Exchange Online nebo SharePoint Online.

V následujících aplikacích podmíněného přístupu podpory Office 365 nebo jiných aplikacích pro službu Azure AD připojení:

| Cíl služby  | Platformy  | Aplikace                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online | Windows 10|Aplikace pošta a kalendář nebo lidé, Outlook 2016, Outlook 2013 (plus pár moderní ověřování), Skype pro firmy (plus pár moderní ověřování)|
|Office 365 Exchange Online| Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (plus pár moderní ověřování), Skype pro firmy (plus pár moderní ověřování)|
|Office 365 Exchange Online|iOS, Android|  Mobilní aplikace Outlook|
|Office 365 Exchange Online|Mac OS X| Outlook 2016 pro vícefaktorové ověřování a umístění. Podpora zařízení zásad plánované pro budoucí Skype pro podporu firem plánované v budoucnosti|
|Office 365 SharePoint Online|Windows 10| Aplikace Office 2016, Office univerzální aplikace Office 2013 (plus pár moderní ověřování), OneDrive pro firmy (Další generování synchronizačního klienta nebo NGSC) pro školy plánované pro budoucí skupin Office podpory plánované pro budoucí podpora aplikace SharePoint plánované v budoucnosti|
|Office 365 SharePoint Online|Windows 8.1, Windows 7|Aplikace Office 2016, Office 2013 (plus pár moderní ověřování), aplikace OneDrive pro firmy (pracovní synchronizačního klienta)|
|Office 365 SharePoint Online|iOS, Android|  Mobilní aplikace Office |
|Office 365 SharePoint Online|Mac OS X| Aplikace Office 2016 pro vícefaktorové ověřování a umístění. Podpora zařízení zásad plánované v budoucnosti|
|Office 365 Yammeru|Windows 10, iOS a Android | Aplikace Yammer Office|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS a Android | Aplikace Dynamics CRM|
|PowerBI služby|Windows 10, Windows 8.1, Windows 7, iOS a Android | PowerBI aplikace|
|Azure vzdálené aplikace služby|Windows 10, Windows 8.1, Windows 7, iOS, Android a Mac OS X |Azure vzdálené aplikace|
|Jiné služby aplikace Moje aplikace|IOS a Android|Jiné služby aplikace Moje aplikace |


## <a name="applications-that-do-not-use-modern-authentication"></a>Aplikace, které nepoužívají moderní ověřování

V současné době musíte použít jiné metody blokovat přístup k aplikacím, které nepoužívají moderní ověřování. Pravidla přístupu k aplikacím, které nepoužívají moderní ověřování nejsou nevynucují podmíněné přístupem. Toto je primárně pozornost pro přístup pomocí protokolu Exchange nebo SharePoint. Většina dřívějších verzích aplikace pomocí starší protokoly řízení přístupu.

### <a name="control-access-in-office-365-sharepoint-online"></a>Řízení přístupu v Office 365 SharePoint Online
Starší verze protokoly pro přístup k Sharepointu můžete zakázat pomocí rutinu Set-SPOTenant. Tahle rutina slouží nechcete, aby klienti Office, které používají protokoly – moderní ověřování z přístupu k prostředkům v Sharepointu Online.

**Příklad**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Řízení přístupu v Office 365 Exchange Online

Exchange nabízí dvě hlavní kategorie protokoly. Zkontrolujte následující možnosti a potom vyberte zásadu přesně pro vaše organizace.

-   **Exchange ActiveSync**. Ve výchozím nastavení nejsou pro Exchange ActiveSync nevynucují zásady podmíněné přístupu pro vícefaktorové ověřování a umístění. Potřebujete ochrana přístup k těmto službám nakonfigurováním zásady Exchange ActiveSync přímo nebo blokováním Exchange ActiveSync pomocí pravidel Active Directory Federation Services (AD FS).
-   **Starší verze protokoly**. Můžete blokovat starší protokoly se službou AD FS. Toto je blokován přístup k starších klientů Office, například Office 2013 bez povoleno moderní ověřování a starších verzích Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Blokovat starší protokol pomocí služby AD FS

Následující příklady pravidel můžete použít k blokování starší protokol přístupu na úrovni AD FS. Vyberte dvě běžné konfigurace.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Možnost 1: Povolit Exchange ActiveSync a povolit starší verze aplikace, ale jenom v síti intranet

Použitím následující tři pravidla pro službu AD FS může stran zabezpečení pro Microsoft Office 365 Identity platformu přenosy protokolu Exchange ActiveSync a prohlížeč a moderní ověřování přenosy mít přístup. Starší verze aplikace jsou blokovány extranetové.

##### <a name="rule-1"></a>Pravidlo 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Pravidlo 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Pravidlo 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Možnost 2: Povolit Exchange ActiveSync a blokovat starší verze aplikace

Použitím následující tři pravidla pro službu AD FS může stran zabezpečení pro Microsoft Office 365 Identity platformu přenosy protokolu Exchange ActiveSync a prohlížeč a moderní ověřování přenosy mít přístup. Starší verze aplikace jsou blokovány odkudkoli.

##### <a name="rule-1"></a>Pravidlo 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Pravidlo 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Pravidlo 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
