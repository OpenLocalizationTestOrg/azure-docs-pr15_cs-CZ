<properties
    pageTitle="Práce s deklarací vědět aplikace v aplikaci Proxy"
    description="Popisuje, jak rychle začít pracovat s Azure AD aplikace Proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Práce s deklarací vědět aplikace v aplikaci Proxy

Nároky vědět aplikace provádět přesměrování na zabezpečení tokenu Service (Služba tokenů zabezpečení), požadující zase přihlašovací údaje uživatele za token před přesměrováním uživatele k aplikaci. Povolit aplikaci Proxy pro práci s těmito přesměrování, je třeba přijmout podle těchto kroků.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Před provedením tohoto postupu, ujistěte se, že služba tokenů zabezpečení aplikaci vědět deklarované přesměruje neexistuje mimo vaší místní síti.

## <a name="azure-classic-portal-configuration"></a>Konfigurace Azure klasické portálu

1. Publikování aplikace podle pokynů v [aplikacích publikovat s Proxy aplikace](active-directory-application-proxy-publish.md).
2. V seznamu aplikací vyberte aplikaci vědět deklarací a klikněte na **Konfigurovat**.
3. Pokud jste se rozhodli **průchod** jako **Používající metody**, je nutné vybrat **HTTPS** jako schéma **Externí adresu URL** .
4. Pokud jste se rozhodli **Azure Active Directory** jako **Používající metody**, vyberte **žádné** jako **Interní metody ověřování**.


## <a name="adfs-configuration"></a>Konfigurace služby AD FS

1. Otevřete Správa služby AD FS.
2. Přejděte na **Může důvěryhodnosti stran**, klikněte pravým tlačítkem na aplikaci, kterou publikujete s Proxy aplikací a zvolte **Vlastnosti**.  
  ![Vztah důvěryhodnosti předávající stran klikněte pravým tlačítkem myši na název aplikace - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Na kartě **koncové body** ve skupinovém rámečku **typ koncového bodu**vyberte **WS Federation**.
4. V části **Důvěryhodné adresa URL** zadejte adresu URL jste zadali v aplikaci proxy serveru v části **Externí adresu URL** a klikněte na **OK**.  
  ![Přidání koncového bodu - hodnotu důvěryhodné URL - snímek](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Viz taky

- [Publikování aplikací s Proxy aplikace](active-directory-application-proxy-publish.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Řešení potíží s aplikací Proxy](active-directory-application-proxy-troubleshoot.md)
- [Povolení aplikace native client – chcete provést interakci s proxy aplikací](active-directory-application-proxy-native-client.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
