<properties 
   pageTitle="Přidání služeb Mobile ve službách připojení ve Visual Studiu | Microsoft Azure"
   description="Přidání služby Mobile pomocí dialogu Visual Studio přidat připojené služby"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Přidání mobilní služby Podnikové připojení Visual Studio

Pomocí aplikace Visual Studio 2015 můžete připojit k mobilní služby Azure pomocí dialogového okna **Přidat službu připojení** . Můžete připojit z libovolné aplikaci klienta C#, všechny JavaScript aplikace nebo různé platformy Cordova aplikace. Po připojení, můžete vytvořit a přístup k datům, vytvářet vlastní rozhraní API a naplánovaných úloh nebo můžete přidat podpora nabízených oznámení.  Připojené služby operace přidá všechny příslušné odkazy a kód připojení. Můžete taky využít předdefinované podpory pro ověřování s různými schémata Oblíbené identit, jako je Azure AD Facebooku, Twitteru a Accounts Microsoft.

## <a name="supported-project-types"></a>Typy podporovaného projektů

>[AZURE.NOTE] Přidání služby Azure Mobile do Windows univerzální (Windows 10) projektů pomocí dialogového okna Přidat připojené služby není podporována ve Visual Studiu 2015. Můžete přidat služby Azure Mobile – nainstalujte odpovídajících balíčků správcem NuGet balíček pro váš projekt.

Dialogové okno připojené služby můžete se připojit k mobilní služby Azure v následujících typech projektu.

- .NET Windows 8.1 úložiště, telefonu a univerzální aplikace projekty

- Projekty JavaScript Windows 8.1 úložiště, telefonu a univerzální aplikace

- Projekty vytvořené pomocí aplikace Visual Studio nástroje pro Apache Cordova


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Připojení ke službě Mobile Azure pomocí dialogového okna Přidat připojené služby

1. Zkontrolujte, jestli že máte účet Azure. Pokud nemáte účet Azure, můžete zaregistrovat [bezplatnou zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Otevření dialogového okna **Přidat připojené služby** .
 - Pro .NET aplikace otevřete projekt ve Visual Studiu, otevřete místní nabídku pro uzel **odkazy** v okně Průzkumník a pak zvolte **Přidat připojené služby**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Apache Cordova aplikace projektů otevřete projekt ve Visual Studiu, otevřete místní nabídku uzel projektu v Průzkumníku řešení a pak zvolte **Přidat službu připojení**.

1. V dialogovém okně **Přidat službu připojení** zvolte **Mobilní služby Azure**a potom klikněte na tlačítko **Konfigurace** . Můžete být vyzváni k přihlášení do Azure, pokud jste tak již neučinili.

    ![Přidání Azure mobilní služby](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. V dialogovém okně **Služby Azure Mobile** vyberte stávající Mobilní služba Pokud nějaké máte. Pokud potřebujete k vytvoření nové Azure mobilních služeb, postupujte podle následujících k tomu nevyzve. Opačném případě přejděte k dalšímu kroku.

    Vytvoření nového účtu mobilních služeb:
    1. Zvolte **Vytvořit služby **odkaz v dolní části dialogového okna.
        ![Přidání nového mobilní připojené služby](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. V dialogovém okně **Vytvořit Mobile Service** můžete JavaScript Mobilní služba back-end nebo .NET Mobilní služba back-end z rozevíracího seznamu **Runtime** . 
  
        ![Vytvoření mobilní služby](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Služba back-end JavaScript je jednoduchý a výkonných. Pokud vytváříte JavaScript mobile service back-end kód v JavaScriptu straně serveru uložený v cloudu, ale můžete upravit skripty serveru pomocí Průzkumníka serveru nebo portálu pro správu Azure. 

        .NET mobile service back-end nabízí celou power a flexibilní rozhraní API webových a Entity Framework. Pokud vytváříte .NET Mobilní služba back-end projektu vytvoří se přidají do vašeho řešení. 

    1. Vyberte **oblast** místo, kam chcete mobilních služeb a potom zadejte uživatelské jméno a heslo pro server.
 
    1. Po zadání všech požadovaných informací, klikněte na tlačítko **vytvořit** vytvořte mobilních služeb.
    2. Nová Mobilní služba by se měly v seznamu služby v dialogovém okně **Služby Azure Mobile** . V seznamu vyberte nové mobilních služeb a klikněte na tlačítko **Přidat** pro přidání služby do projektu.
    

1. Zkontrolujte úvodní stránce, která se zobrazí a zjistěte, jak byl upraven projektu. Stránka Začínáme se zobrazí ve vašem prohlížeči pokaždé, když přidáte připojený služby. Projděte si doporučené další kroky a příklady kódu nebo přejděte na stránku příčinách zobrazíte jaké odkazy se přidala do vašeho projektu a jak se změnily kódu a konfigurace soubory.

1. Použití ukázek kódu jako vodítko, zahájíte psaní kódu pro přístup k mobilní služba.

## <a name="how-your-project-is-modified"></a>Jak se mění projektu

Jak aplikace Visual Studio upravuje projektu závisí na typu projektu. C# klientské aplikace najdete v článku [Co se stalo – C# projekty](http://go.microsoft.com/fwlink/p/?LinkId=513119). Klient aplikace JavaScript najdete v článku [Co se stalo – JavaScript projekty](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova aplikací najdete v článku [Co se stalo – Cordova projekty](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Další kroky

Klást otázky a dostávat nápovědy: 

 - [Fórum MSDN: Azure mobilní služby](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure mobilních služeb v týmovém blogu Microsoft Azure](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure mobilních služeb azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure.microsoft.com následující dokumentaci pro Azure mobilní služby](https://azure.microsoft.com/documentation/services/mobile-services/)



