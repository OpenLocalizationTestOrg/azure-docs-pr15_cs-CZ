<properties
    pageTitle="Aplikace služby Azure zásobníku Technical Preview 1 než začnete | Microsoft Azure"
    description="Postup dokončete před nasazením Web Apps na Azure zásobníku"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
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
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Než začnete s Azure vrstvě Web Apps

> [AZURE.NOTE] Tyto informace platí jenom pro nasazení TP1 zásobníku Azure.

Budete potřebovat pár věcí, na nainstalovat Azure zásobníku Web Apps:

- Dokončené nasazení [Azure zásobníku Technical Preview 1](azure-stack-run-powershell-script.md)
- Dost místa v systému Azure zásobníku malé nasazení Azure zásobníku Web Apps.  Požadované místo je přibližně 20GB místa na disku.
- [Server, který běží SQL Server](#SQL-Server).

>[AZURE.NOTE] Následující kroky, které všechny proběhnout na OM klienta.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Před nasazením Azure zásobníku Web Apps

Abyste mohli nasadit zprostředkovatele prostředků, musí Environment(ISE) prostředí PowerShell integrované skriptování spustili s oprávněními správce. Z tohoto důvodu musíte povolit soubory cookie a JavaScript v Internet Exploreru profilu, které používáte pro přihlášení k Azure Active Directory.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Vypnutí zabezpečení aplikace Internet Explorer rozšířeného

1.  Přihlaste se k počítači (Koncepce) ověření koncepce Azure zásobníku jako **AzureStack/správce**a potom spusťte **Správce serveru**.
2.  Vypněte **Rozšířeného zabezpečení aplikace Internet Explorer** pro správce a uživatele.
3.  Přihlaste se k virtuálního počítače jako správce ClientVM.AzureStack.local a spusťte **Správce serveru**.
4.  Vypněte **Rozšířeného zabezpečení aplikace Internet Explorer** pro správce a uživatele.

## <a name="enable-cookies"></a>Povolení souborů cookie

1.  Vyberte **Start**> **všechny aplikace**> **Příslušenství systému Windows**. Klikněte pravým tlačítkem na **Aplikaci Internet Explorer** > **Další** > **Spustit jako správce**.
2.  Pokud vás systém vyzve, zvolte možnost **použití doporučené zabezpečení**a pak vyberte **OK**.
3.  V Internet Exploreru, vyberte **Nástroje** (ikona ozubeného kola) > **Možnosti Internetu** > **ochrany osobních údajů** > **Upřesnit**.
4.  Vyberte **Upřesnit**. Zkontrolujte, zda jsou vybrány obě políčka **přijmout** a klikněte na **OK** a **OK** znova.
5.  Ukončete aplikaci Internet Explorer a restartujte ISE PowerShell jako správce.

## <a name="install-the-latest-version-of-azure-powershell"></a>Nainstalujte nejnovější verzi Azure PowerShell

1.  Přihlaste se k počítači Azure zásobníku Koncepce jako **Správce/AzureStack**.
2.  Připojení ke vzdálené ploše umožňuje přihlášení k ClientVM.AzureStack.local virtuálního počítače jako správce.
3.  Otevřete **Ovládací panely** a klikněte na **odinstalovat program**. 
4.  Klikněte pravým tlačítkem na **Microsoft Azure PowerShell - 2015 listopad** a klikněte na **odinstalovat**.
5.  Po odinstalaci dokončí, restartujte ClientVM.AzureStack.local virtuálního počítače
6.  Stáhněte a nainstalujte nejnovější [Azure Powershellu](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL Server

Ve výchozím nastavení Instalační služby Azure vrstvě Web Apps nastavena na použití SQL serveru, které máte nainstalované spolu s zprostředkovatele prostředků SQL Azure vrstvě. Při instalaci zprostředkovatele prostředků serveru SQL (SQL RP) zajistěte, aby že si poznamenejte databázi jména a hesla správce. Potřebujete i při instalaci Azure zásobníku Web Apps.
Poznámka: Můžete bude mít taky zajištěný možnost nasazení a pomocí jiného serveru SQL Server. Můžete použít po dokončení možnosti instalační služby Azure vrstvě Web Apps instanci systému SQL Server.
