<properties 
   pageTitle="Přidání služby Azure Active Directory pomocí připojené služby ve Visual Studiu | Microsoft Azure"
   description="Přidání služby Azure Active Directory pomocí dialogu Visual Studio přidat připojené služby"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Přidání služby Azure Active Directory pomocí připojené služby ve Visual Studiu 

##<a name="overview"></a>Základní informace
Pomocí služby Azure Active Directory (Azure AD) podporuje jednoho přihlašování (SSO) pro ASP.NET MVC webových aplikací nebo AD ověřování v rozhraní API webových služeb. Azure AD ověřování uživatelů pomocí svých účtů z Azure AD pro připojení k webovým aplikacím. Výhody Azure AD ověřování pomocí rozhraní API webových zahrnují zabezpečení rozšířené dat při zpřístupnění rozhraní API z webové aplikace. S Azure AD není nutné spravovat samostatné ověřování systému s řízení účet a uživatele.

## <a name="supported-project-types"></a>Typy podporovaného projektů

Dialogové okno připojené služby můžete se připojit k Azure AD v následujících typech projektu.

- MVC projekty technologie ASP.NET

- ASP.NET webového rozhraní API projekty


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Připojení k Azure AD pomocí dialogového okna připojené služby

1. Zkontrolujte, jestli že máte účet Azure. Pokud nemáte účet Azure, můžete zaregistrovat [bezplatnou zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Ve Visual Studiu otevřete v místní nabídce uzel **odkazy** v projektu a klikněte na **Přidat připojení služby**.
1. Zaškrtněte políčko **Azure AD ověření** a klikněte na **Konfigurovat**.

    ![Zvolte Přidat Azure AD ověřování](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Na první stránce **Konfigurace Azure AD ověřování**kontrola **Konfigurace jednotného přihlašování pomocí Azure AD**.

    Pokud váš projekt nakonfigurovaný s jinou konfiguraci ověřování, Průvodce vás upozorní, že pokračováním zakáže předchozí konfiguraci.

    ![Konfigurace Azure AD v Průvodci](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Na druhé stránce vyberte z rozevíracího seznamu **doménu** doménu. V seznamu domény najdete všechny domény přístupných osobám s postižením účty uvedené v dialogovém okně Nastavení účtu. Jako alternativu můžete zadat název domény, Pokud nenajdete tu, kterou hledáte, například mydomain.onmicrosoft.com. Vyberte možnost vytvořit novou aplikaci Azure AD nebo použijte nastavení z existující aplikace Azure AD. 

    ![Konfigurace Azure AD v Průvodci](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Na třetí stránce průvodce zkontrolujte, zda je zaškrtnuto políčko **Číst adresáře data** . Průvodce vyplní **tajná klienta**. 

    ![Konfigurace Azure AD v Průvodci](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Klikněte na tlačítko **Dokončit** . Dialogové okno přidá potřebné konfiguračního kódu a odkazy na povolit projektu pro Azure AD ověřování. Zobrazí se domény AD [Azure portálu](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Prohlédněte si stránce Začínáme, která se zobrazí v prohlížeči najdete návrhy na další kroky a co se stalo stránku, abyste viděli, jak změnila, projektu. Pokud chcete zkontrolovat, zda vše funguje, otevřete jednu ze souborů změněné konfigurace a ověřte, zda jsou nastavení podle příčinách. Hlavní web.config v projektu ASP.NET MVC například budou platit tato nastavení přidat:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Jak se mění projektu

Po spuštění průvodce Visual Studio přidá Azure AD a související odkazy na projektu. Přidání podpory Azure AD taky změnit konfigurační soubory a soubory kódu v projektu. Konkrétní změny, které provádí Visual Studio, závisí na typu projektu. Podrobné informace o jak změnit ASP.NET MVC projekty najdete v článku [Projekty MVC co se stalo –](http://go.microsoft.com/fwlink/p/?LinkID=513809). Rozhraní API webových projektů najdete v článku [Co se stalo – webového rozhraní API projekty](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Další kroky

Klást otázky a získat nápovědu.

 - [Fórum MSDN: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Příspěvek blogu: Úvod k Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

