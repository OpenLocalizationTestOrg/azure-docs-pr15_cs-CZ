<properties
    pageTitle="Přidání webové aplikace zdroje zprostředkovatele Azure zásobníku | Microsoft Azure"
    description="Podrobné pokyny pro nasazení Web Apps ve vrstvě Azure"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Přidání webových aplikací Web Apps zdroje zprostředkovatele Azure zásobníku

> [AZURE.NOTE] Tyto informace platí jenom pro nasazení TP1 zásobníku Azure.

Přidání zdroje poskytovatele webových aplikací do zásobníku Azure obsahuje sedm kroky:

1.  Stáhněte součásti potřebné.
2.  Vytvoření certifikátů pro použití v Azure zásobníku Web Apps.
3.  Instalační program umožňuje stáhnout fáze a nainstalovat Azure zásobníku Web Apps. 
4.  Ověření webová aplikace instalace.
5.  Vytvoření DNS záznamů pro front-end a vyrovnávání zatížení Server pro správu.
6.  Registraci nově nasazeném poskytovatele zdroje Web Apps s ARM.
7.  Vyzkoušení poskytovatele webových aplikací zdroje.

## <a name="download-required-components"></a>Stáhnout součásti potřebné

1.  Stáhněte si [aplikaci služby Azure zásobníku náhled Instalační služby systému](http://aka.ms/azasinstaller). 
2.  Stáhněte si [aplikaci služby Azure zásobníku nasazení helper skriptů](http://aka.ms/azashelper). 
3.  Extrahujte soubory ze souboru zip skripty Pomocník, by měl být tři skripty:
    - Vytvoření AppServiceCerts.ps1
    - Vytvoření AppServiceDnsRecords.ps1
    - Register-AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Vytvoření certifikátů pro použití v Azure zásobníku Web Apps

První skript spolupracuje certifikační autorita Azure zásobníku k vytvoření 3 certifikáty, které jsou vyžadovány Web Apps. Spusťte skript na ClientVM zajistit, že používáte prostředí PowerShell jako azurestack\administrator:
1.  V relaci Powershellu spuštěný jako **azurestack\administrator**spusťte skript **AppServiceCerts.ps1 vytvořit** .  Tím vytvoříte tři certifikáty do stejné složky jako skript, které jsou potřebné tak, že Web Apps.
2.  Zadejte heslo k zabezpečení soubory pfx a poznamenejte si ho jako je třeba ho zadat do instalační služby Azure zásobníku webové aplikace.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Instalační program umožňuje stáhnout a nainstalovat Azure zásobníku Web Apps

Instalační program appservice.exe udělejte toto:
1.  Výzva k zadání přijměte Microsoft třetích stran EULA.
2.  Shromažďování informací nasazení Azure vrstvě.
3.  Vytvoření kontejneru objektů blob v zadaný účet Azure zásobníku úložiště.
4.  Stahování souborů potřebné k instalaci zprostředkovatele prostředků Azure zásobníku Web Appu.
5.  Příprava instalace pro nasazení zprostředkovatele prostředků v prohlížeči v prostředí Azure vrstvě.
6.  Nahrajte soubory do účtu úložiště služby aplikace.
7.  Prezentovat informace potřebné k zahájit šabloně správce prostředků Azure.

Následující postup vám pomůže s fází instalace:

>[AZURE.NOTE] Spuštění instalačního programu je třeba použít zvýšenými účet (místní nebo domény správce). Pokud se přihlásit jako azurestack\azurestackuser, zobrazí se výzva k zadání přihlašovacích údajů zvýšenými oprávněními. 

1.  Spusťte appservice.exe jako **azurestack\administrator**. 
2.  Klikněte na **nasazení pomocí Správce prostředků Azure**.

![Instalační program Technical Preview 1 aplikaci služby Azure zásobníku][1]

3.  Kontrola a přijměte licenční podmínky pro předběžnou verzi softwaru společnosti Microsoft a klikněte na tlačítko **Další**.
4.  Kontrola a souhlasit s podmínkami třetí partylicense a klikněte na tlačítko **Další**.
5.  Zkontrolujte informace o konfiguraci aplikace Cloudová služba a klikněte na tlačítko **Další**.

![Konfigurace cloudu aplikaci služby Technical Preview 1 aplikaci služby Azure zásobníku][2]

6. Klikněte na **Připojit** (vedle rozevíracího seznamu Azure zásobníku předplatná).

![Azure zásobníku aplikaci služby Technical Preview 1 aplikaci služby cloudu konfigurace obrazovku][3]

7.  V okně Azure zásobníku ověřování zadat **Azure Active Directory služby správy účtu** a **heslo**a klepněte na **Přihlásit**.
**Poznámka:** Toto je účet Azure Active Directory, kterou jste použili při nasazení Azure zásobníku.
    - Klikněte na **Šipku dolů** vpravo od pole vedle **Azure zásobníku předplatná** a pak vyberte předplatné.

![Výběr předplatné Technical Preview 1 aplikace služby Azure zásobníku][5]

8.  Klikněte na **Šipku dolů** vpravo od pole vedle **Azure zásobníku umístění**.
    - Vyberte **místní**.
9. Zadejte **název** pro správce.
10. Zadejte **heslo** pro správce.
11. Zkontrolujte **Podrobnosti o SQL serveru** a v případě potřeby proveďte změny.
12. Zkontrolujte **Přihlašovací účet členem** a proveďte změny v případě potřeby.
13. Zadejte **Heslo**.
14. Klikněte na tlačítko **Další**.  V tomto okamžiku instalační program bude teď zkontrolujte podrobnosti o připojení pro systém SQL Server k dispozici.

![Výběr předplatné Technical Preview 1 aplikace služby Azure zásobníku][4]    

15. Klikněte na tlačítko **Procházet** vedle **Soubor certifikátu SSL výchozí aplikace Web** a přejděte na **webapps. AzureStack.Local** certifikát [vytvořili](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
16. Zadejte **heslo certifikátu** nastavené při vytváření certifikáty.
17. Klikněte na tlačítko **Procházet** vedle **Soubor certifikátu SSL poskytovatele zdroje** a přejděte na Správa **. AzureStack.Local** certifikát [vytvořili](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
18. Zadejte **heslo certifikátu** nastavené při vytváření certifikáty.
19. Klikněte na tlačítko **Procházet** vedle **Soubor zdroje poskytovatele kořenového certifikátu** a přejděte na Správa **. AzureStack.Local** certifikát [vytvořili](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
20. Klikněte na **Další** instalační program ověří zadané heslo certifikát.

![Podrobnosti o certifikátu Technical Preview 1 aplikaci služby Azure zásobníku][6]

Nasazení bude trvat asi 45 až 60 minut dokončení.

![Průběh instalace Technical Preview 1 aplikaci služby Azure zásobníku][7]

21. Po úspěšném dokončení instalační program klikněte na **Konec**.

## <a name="validate-web-apps-installation"></a>Ověření instalace webové aplikace

1.  **Azure zásobníku hostitelském počítači** otevřete **Hyper-V správce**.
2.  Vyhledejte **CN0 OM** a **Připojit** v angličtině.
![Správce Hyper-V Technical Preview 1 aplikace služby Azure zásobníku][8]
3.  Na ploše tento OM poklikejte na **Web cloudu Management Console**.
![Konzolu Správa Technical Preview 1 aplikace služby Azure zásobníku][9]
4.  Přejděte na **Správa serverů**.
5.  Když jsou všechny počítače **jste připraveni** přejít k dalšímu kroku. 
![Stav 1 spravovaných serverů Technical Preview aplikace služby Azure zásobníku][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>Vytvoření DNS záznamů pro Server pro správu a front-end Vyrovnávání zatížení
1.  Spuštění instance prostředí PowerShell jako **azurestack\administrator**.
2.  Přejděte do umístění skriptů stáhnout a extrahovaných v [základní krok](#Download-Required-Components).
3.  Spustit **AppServiceDnsRecords.ps1 vytvořit** skript, vytvoří se položky DNS pro povolení portálem a webové aplikace požadavků na směrovány serverům přední konce.  Zprostředkovatele zdroje vytvořená během ARM nasazení webových aplikací, dvě Vyrovnávání zatížení Software (SLBs). Ukazovaly na servery správy a front-end serverů. Portál a žádosti o ARM založené Azure zásobníku aplikace zdroje poskytovatele přejděte na serveru správy.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Registraci nově nasazeném poskytovatele Azure zásobníku Web Apps s ARM
1.  Spuštění instance prostředí PowerShell jako **azurestack\administrator**.
2.  Přejděte do umístění skriptů stáhnout a extrahovaných v [základní krok](#Download-Required-Components).
3.  Spusťte skript **AppServiceResourceProvider.ps1 rejstříku** . 

>[AZURE.NOTE] Zadejte uživatelské jméno a heslo **přesně (včetně velká a malá písmena)** , případně byl zadán pro **Správce virtuální počítače** a pole **heslo** v průběhu instalace se nepovede registrace zprostředkovatele zdroje.

## <a name="test-drive-azure-stack-web-apps"></a>Test disku Azure zásobníku Web Apps

Teď nasazené a registrován zprostředkovatele prostředků Web Apps můžete otestovat a ujistěte se, že klienti můžete nasazení web apps.

1.  Na portálu zásobníku Azure klikněte na nový, klikněte na Web + Mobile a klikněte na Web App.
2.  V prohlížeči zásuvné zadejte název do pole webové aplikace.
3.  V části pole Skupina zdroje klikněte na nový a potom zadejte název do pole Skupina zdroje. 
4.  Klikněte na aplikaci služby plán a umístění a klikněte na vytvořit nový.
5.  V aplikaci služby plán zásuvné zadejte název do pole plán aplikaci služby.
6.  Klikněte na tlačítko vrstvy ceny klikněte na Free sdílené nebo sdílené sdílené, klikněte na vybrat, klikněte na tlačítko OK a klikněte na vytvořit.
7.  V za minutu, na dlaždici pro nové webové aplikace se zobrazí na řídicím panelu. Klikněte na dlaždici.
8.  Ve webové aplikaci zásuvné klikněte na Procházet a zobrazit výchozí web pro tuto aplikaci.


**Nasazení WordPress, DNN nebo Django web (volitelné)**

1. Na portálu zásobníku Azure klikněte na "a", přejděte k webu Azure Marketplace nasadit na web Django a počkejte, aby instalaci zvládli. Platformu Django web používá databázi založený na systému souborů a nevyžaduje žádné další zdroje poskytovatele jako SQL nebo MySQL.  

2. Pokud taky nainstalovali zprostředkovatele prostředků MySQL nástroje můžete nasazovat WordPress webu z Marketplace. Po zobrazení výzvy parametrů databáze, zadávání uživatelské jméno jako *User1@Server1* (s uživatelské jméno a název serveru podle svého výběru).

3. Pokud i nainstalovali zprostředkovatele prostředků serveru SQL Server, nástroje můžete nasazovat DNN webu z Marketplace. Po zobrazení výzvy parametrů databáze, vyberte databázi v počítači se systémem serveru SQL Server, ke kterému je připojen váš poskytovatel zdroje.

## <a name="next-steps"></a>Další kroky

Můžete taky vyzkoušet jiné [platformy jako služba (PaaS) služby](azure-stack-tools-paas-services.md), jako [zprostředkovatele prostředků serveru SQL Server](azure-stack-sql-rp-deploy-short.md) a [zprostředkovatele MySQL prostředků](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
