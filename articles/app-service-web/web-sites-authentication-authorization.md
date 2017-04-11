<properties 
    pageTitle="Ověření se místní službou Active Directory v aplikaci Azure | Microsoft Azure" 
    description="Další informace o různých možností pro řádek obchodních aplikací v aplikaci služby Azure ověření s místní služby Active Directory" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Ověření se místní službou Active Directory v aplikaci Azure #

Tento článek popisuje, jak ověřit s místním systémem v [Aplikaci služby Azure](../app-service/app-service-value-prop-what-is.md)Active Directory (AD). Aplikace Azure je hostovaný v cloudu, ale existuje způsoby, jak ověřit místní uživatele AD bezpečné. 

## <a name="authenticate-through-azure-active-directory"></a>Ověření prostřednictvím služby Azure Active Directory
Tenanta služby Azure Active Directory může být adresáře synchronizují s místního AD. Tento přístup uživatelům AD přístup k aplikaci z Internetu a ověřovat pomocí svých přihlašovacích údajů místní. Kromě toho aplikaci služby Azure poskytuje [zapnout klíč řešení pro tento způsob](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Několika kliknutími tlačítka můžete povolit ověřování s tenantem synchronizovali adresáře Azure aplikace. Tento přístup má následující výhody:

-   Není nutné zadávat všechny ověřovací kód v aplikaci. Povolení aplikace služby proveďte ověření za vás a váš čas strávený poskytnout podpoře funkcí v aplikaci.
-   [Rozhraní API Azure AD grafu](http://msdn.microsoft.com/library/azure/hh974476.aspx) umožňuje přístup k datům adresáře z aplikace Azure.
-   Poskytuje jednotné přihlašování k [všech aplikací podporovaných Azure Active Directory](/marketplace/active-directory/), včetně služeb Office 365 Dynamics CRM Online, Microsoft Intune a tisíce aplikací cloudu společnosti Microsoft. 
-   Azure Active Directory podporuje řízení přístupu na základě rolí. Můžete vzorek [Authorize(Roles="X")] s minimálními změny kódu.

Jak psát řádku obchodní Azure aplikace, která ověří s Azure Active Directory najdete [vytvořit řádek obchodní Azure aplikace pomocí ověřování služby Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Ověření prostřednictvím místní STS
Pokud máte místní zabezpečené tokenu služby (STS) jako Active Directory Federation Services (AD FS), můžete pomocí nich federování ověřování Azure aplikace. Tento přístup je nejvhodnější, když zásady společnosti zakazuje neukládaly v Azure AD data. Však takto:

-   Služba tokenů zabezpečení topologie musí být nasazené na místním prostředí, s režijních nákladů a řízení.
-   Pouze správci služby AD FS můžete nakonfigurovat [předávající vztahy důvěryhodnosti stran a deklarace pravidla](http://technet.microsoft.com/library/dd807108.aspx), které pravděpodobně omezí možnosti vývojáře. Na druhé straně je možné Správa a přizpůsobení [nároky](http://technet.microsoft.com/library/ee913571.aspx) na základě jednotlivé aplikace.
-   Aplikace Access do místního nasazení AD data vyžadují samostatné řešení prostřednictvím podnikovou bránu firewall.

Jak psát řádku obchodní Azure aplikace, která ověří s místním STS najdete [vytvořit řádek obchodní Azure aplikace pomocí služby AD FS ověřování](web-sites-dotnet-lob-application-adfs.md).
 
