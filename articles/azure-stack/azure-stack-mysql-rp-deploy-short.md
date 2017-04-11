<properties
    pageTitle="Použití databáze MySQL jako PaaS Azure zásobníku | Microsoft Azure"
    description="Princip rychlé kroky a nasazení zprostředkovatele prostředků MySQL poskytující MySQL jako služba Azure zásobníku."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>Použití databáze MySQL jako PaaS Azure zásobníku

> [AZURE.NOTE] Tyto informace platí jenom pro nasazení TP1 zásobníku Azure.

Můžete nasadit zprostředkovatele MySQL zdroje na Azure vrstvě. Po nasazení zprostředkovatele prostředků, můžete vytvořit MySQL servery a databází prostřednictvím Správce prostředků Azure nasazení šablon a zadat databáze MySQL jako služba. Databáze MySQL, které jsou běžné na webech, podporují spoustu platformách webu. Po nasazení poskytovatele zdroje, můžete vytvořit WordPress weby z platformu Azure Web Apps jako doplněk služby (PaaS) pro Azure vrstvě.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Rychlé kroky pro nasazení na poskytovatele zdroje
Pokud jste už znáte zásobníku Azure, postupujte následujícím způsobem: Pokud budete potřebovat víc informací, pomocí odkazů v každého oddílu nebo přejděte přímo na [Deploy databáze MySQL zdroje poskytovatele adaptér Koncepce zásobníku Azure](azure-stack-mysql-rp-deploy-long.md).

1.  Ujistěte se, které [dokončení všech instalačních kroků](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) před nasazením poskytovatele zdroje:

    - .NET 3.5 framework už nastavenou na základní systému Windows Server obrázku. (Pokud jste stáhli bitů Azure zásobníku po únor, 23, 2016, můžete tento krok přeskočit.)
    - [Je nainstalovaná verzi Azure PowerShell, který je kompatibilní se službou Azure vrstvě](http://aka.ms/azStackPsh).
    - V nastavení zabezpečení Internet Explorer na ClientVM [jsou povoleny Internet Explorer zvýšené zabezpečení je normálně vypnuté a soubory cookie](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Stáhněte si MySQL poskytovatele binární zdrojového souboru](http://aka.ms/masmysqlrp) a extrahujte na ClientVM v sadě Azure doklad o koncepce.

3. [Spuštění bootstrap.cmd a skriptů](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Sada skripty, která jsou seskupena podle dvě hlavní karty se otevře v prostředí integrovaném skriptování prostředí PowerShell (ISE). Spuštění všech načtené skriptů postupně zleva doprava na jednotlivých kartách.

    1. Zleva doprava spouštěly skripty na kartě **připravit** :

        - Vytvořte certifikát se zástupnými znaky k zabezpečení komunikace mezi zprostředkovatele prostředků a správce prostředků Azure.
        - Přijměte podmínky smlouvy EULA MySQL a stáhněte binární MySQL.
        - Certifikáty a všechny ostatní artefakty nahrávaly na účet Azure zásobníku úložiště.
        - Publikování balíčků Galerie tak, aby nástroje můžete nasazovat MySQL zdrojů v galerii.

        > [AZURE.IMPORTANT] Pokud některý z skripty zablokování pro není zřejmé, proč po odeslání vašeho klienta služby Azure Active Directory, nastavení zabezpečení, může blokuje DLL potřebného pro nasazení na spustit. Tento problém můžete vyřešit, vyhledejte Microsoft.AzureStack.Deployment.Telemetry.Dll ve složce poskytovatele zdroje pravým tlačítkem myši, klikněte na **Vlastnosti**a potom na kartě **Obecné** zaškrtněte **Odblokovat** .

    2. Zleva doprava spouštěly skripty na kartě **nasazení** :

        - [Nasazení virtuálního počítače (OM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , který bude hostitelem vašeho poskytovatele zdroje, MySQL servery i databází, které vytvoříte instanci. Tento skript odkazuje soubor parametr JSON, který byste potřebovali aktualizovat s některé hodnoty před spuštěním skriptu.
        - [Zaregistrujte místní záznam DNS](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) , který bude mapování na poskytovatele zdroje OM.
        - [Registrace poskytovatele zdroje](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) s místní zdroje správce Azure.

        > [AZURE.IMPORTANT] Všechny skripty se předpokládá, že obrázek základní operační systém splňuje požadavky (.NET 3.5 nainstalovali, JavaScript a soubory cookie zapnuta ClientVM a nejnovější verzi Azure PowerShell nainstalovaný). Pokud se zobrazí chyby při spuštění skripty, zkontrolujte splněny požadavky.

5. [Testování nového zdroje zprostředkovatele MySQL](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment)nasazení databáze MySQL z portálu Microsoft Azure vrstvě. Klikněte na **vytvořit** &gt; **vlastní** &gt; **MySQL Server a databáze**.

To by měla získat poskytovatele MySQL zdroje nahoru a spuštěné v přibližně 25 minut.
