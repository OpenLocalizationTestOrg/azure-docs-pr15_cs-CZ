<properties
    pageTitle="Nasazení zprostředkovatele prostředků MySQL Azure zásobníku | Microsoft Azure"
    description="Podrobný postup pro nasazení zprostředkovatele prostředků MySQL Azure zásobníku."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>Nasazení zprostředkovatele prostředků MySQL zásobníku Azure pomocí služby WebApps

> [AZURE.NOTE] Tyto informace platí jenom pro nasazení TP1 zásobníku Azure.

Použití tohoto článku ke sledování podrobný postup pro nastavení poskytovatele MySQL zdroje na Azure zásobníku doklad koncepce tak, že můžete začít [používat databáze MySQL](azure-stack-mysql-rp-deploy-short.md) zásobníku Azure, včetně použití MySQL jako back-end pro WordPress weby vytvářené pomocí [Azure Web Apps](azure-stack-webapps-deploy.md).

## <a name="set-up-steps-before-you-deploy"></a>Postup nastavení před nasazením

Před nasazením zprostředkovatele prostředků, musíte:

- Máte výchozí obrázek systému Windows Server s .NET 3.5
- Vypnutí rozšířené zabezpečení aplikace Internet Explorer (IE)
- Nainstalujte nejnovější verzi Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Vytvořit obrázek systému Windows Server, včetně .NET 3.5

Když jste si stáhli bitů Azure zásobníku po 2/23/2016 Jelikož výchozí obrázek základní Windows serveru 2012 R2 .NET 3.5 framework v tomto ke stažení a v novějších verzích můžete tento krok přeskočit.

Pokud jste si stáhli před 2/23/2016, je potřeba vytvořit Windows serveru 2012 R2 Datacentra virtuálního pevného disku s obrázkem po posunutí .NET 3.5 a je nastavení jako výchozího obrázku v úložišti obrázek platformy.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Vypnout IE rozšířené zabezpečení a povolit soubory cookie

Abyste mohli nasadit zprostředkovatele zdroje, prostředí integrovaném Scripting prostředí PowerShell (ISE) spustit jako správce, proto je potřeba povolit soubory cookie a JavaScript v Internet Exploreru profil, který používáte pro přihlášení k Azure Active Directory pro správce a uživatele sign in.

**Pokud chcete vypnout IE rozšířené zabezpečení:**

1. Přihlaste se k počítači (koncepce) ověření koncepce Azure zásobníku jako AzureStack/správce a potom spusťte Správce serveru.

2. Vypněte **Rozšířeného zabezpečení aplikace Internet Explorer** pro správce a uživatele.

3. Přihlaste se k **ClientVM.AzureStack.local** virtuálního počítače jako správce a potom spusťte Správce serveru.

4. Vypněte **Rozšířeného zabezpečení aplikace Internet Explorer** pro správce a uživatele.

**Povolení souborů cookie:**

1. Na obrazovce Start systému Windows klikněte na **všechny aplikace**, klikněte na **Příslušenství systému Windows**, klikněte pravým tlačítkem **Internet Exploreru**, přejděte na **Další**a pak vyberte **Spustit jako správce**.

2. Pokud se zobrazí výzva, zkontrolujte **použití doporučené zabezpečení**a klikněte na tlačítko **OK**.

3. V Internet Exploreru klikněte na **Nástroje (ozubené) ikonu** &gt; **Možnosti Internetu** &gt; kartu **Ochrana osobních údajů** .

4. Klikněte na **Upřesnit**, ujistěte se, že nejsou vybrány obě tlačítka **přijmout** , klepněte na tlačítko **OK**a klikněte na tlačítko **OK** .

5. Ukončete aplikaci Internet Explorer a restartujte ISE PowerShell jako správce.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Instalace Azure zásobníku kompatibilní verzi Azure PowerShell

1. Odinstalujte všechny existující prostředí PowerShell Azure z klienta OM.

2. Přihlaste se k počítači Azure zásobníku Koncepce jako AzureStack/správce.

3. Pomocí vzdálené plochy, přihlaste se k **ClientVM.AzureStack.local** virtuálního počítače jako správce.

4. Otevřete ovládací panely, klikněte na **odinstalovat program** &gt; klikněte na **Azure PowerShell** &gt; klikněte na **odinstalovat**.

5. [Stáhněte si nejnovější Azure PowerShell, který podporuje Azure zásobníku](http://aka.ms/azstackpsh) a nainstalujte ji.

    Po instalaci prostředí PowerShell můžete spustit toto ověření skript Powershellu abyste měli jistotu, že můžete připojit k instanci aplikace Azure zásobníku (objevit na webovou stránku přihlášení).

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Zavádění nasazení poskytovatele zdroje prostředí PowerShell

1. Připojení vzdálené plochy Koncepce Azure zásobníku k clientVm.AzureStack.Local a přihlaste se jako azurestack\\azurestackuser.

2. [Stažení MySQL RP binární](http://aka.ms/masmysqlrp) soubor a extrahujte do D:\\MySQLRP.

3. Spuštění D:\\MySQLRP\\Bootstrap.cmd soubor jako správce (azurestack\administrator).

    Otevře se soubor Bootstrap.ps1 v prostředí PowerShell ISE.

4. Dokončení windows PowerShell ISE načítání, klikněte na tlačítko "přehrávání" nebo stiskněte klávesu F5.

    Dvě hlavní karty načte, každý obsahuje všechny skripty a soubory budete muset nasazení poskytovatele MySQL zdroje.

## <a name="prepare-prerequisites"></a>Příprava požadavky

Kliknutím na kartu **Příprava požadavky** :

- Vytvoření požadovaných certifikátů
- Stažení MySQL binární do zásobníku Azure
- Nahrání artefakty k účtu úložiště Azure zásobníku
- Publikování položek galerie

### <a name="create-the-required-certificates"></a>Vytvoření požadovaných certifikátů
Tento skript **Nový SslCert.ps1** přidá \_. Certifikát AzureStack.local.pfx SSL D:\\MySQLRP\\požadavky\\BlobStorage\\složek kontejneru. Certifikát zabezpečení komunikace mezi poskytovatelem zdroje a místní instanci správce Azure prostředků.

1. Na kartě Hlavní **Příprava požadavky** klikněte na kartu **Nový SslCert.ps1** a ho spusťte.

2. Na příkazovém řádku, která se zobrazí zadejte heslo PFX chrání privátním klíčem a **poznamenejte si toto heslo**. Je potřeba ho později.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>Stažení MySQL binární do zásobníku Azure

1. Vyberte kartu **Stáhnout MySqlServer.ps1** a ho spusťte.
2. Po zobrazení výzvy klikněte na **Ano** v dialogovém okně Potvrdit přijmout smlouvu EULA.

    Příkaz přidá dva zip soubory do složky D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Nahrání všech artefakty k účtu úložiště Azure zásobníku

1. Klikněte na kartu **Odesílání Microsoft.MySql RP.ps1** a ho spusťte.

2. V dialogovém okně žádost o přihlašovacích údajů Windows PowerShell zadejte přihlašovací údaje Správce služby Azure vrstvě.

3. Po zobrazení výzvy ID klienta Azure Active Directory, zadejte svůj Azure Active Directory klienta plně kvalifikovaný název domény: například microsoftazurestack.onmicrosoft.com.

    Automaticky otevírané okno požádá o přihlašovacích údajů.

    > [AZURE.TIP] Pokud místní nabídce nezobrazí, které buď nevypnuli IE lepší zabezpečení a Povolit JavaScript v tomto počítači a uživatele nebo nepřijal souborů cookie v aplikaci Internet Explorer. V tématu [Nastavení kroky před nasazením](#set-up-steps-before-you-deploy).

4. Zadejte svoje přihlašovací údaje Správce služeb zásobníku Azure a potom klikněte na **Sign In**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Publikovat položky galerie pro pozdější vytvoření prostředku

Vyberte kartu **Publikovat GalleryPackages.ps1** a ho spusťte. Tento skript přidá dvě položky marketplace portálu Koncepce zásobníku Azure marketplace, využívající nasazení zdroje databáze jako položky marketplace.

## <a name="deploy-the-mysql-resource-provider-vm"></a>Nasazení zprostředkovatele MySQL prostředků OM

Teď jsou připraveni koncepce zásobníku Azure s potřebné certifikáty a marketplace položky nástroje můžete nasazovat zprostředkovatele prostředků serveru SQL. Kliknutím na kartu **poskytovatele MySQL nasazení** :

   - Zadejte hodnoty v JSON soubor, který odkazuje na procesu nasazení
   - Nasazení zprostředkovatele prostředků
   - Aktualizace místní DNS
   - Registrace poskytovatelem adaptér SQL Server zdroje

### <a name="provide-values-in-the-json-file"></a>Zadejte hodnoty v souboru JSON

Klikněte na tlačítko **Microsoft.MySqlprovider.Parameters.JSON**. Tento soubor obsahuje parametrů, které správce prostředků Azure šablony je potřeba správně nasadit do zásobníku Azure.

1. Vyplňte **prázdné** parametrů v souboru JSON:

    - Zkontrolujte, že zadáte **adminusername** a **adminpassword** OM MySQL zdroje poskytovatele.

    - Zkontrolujte, že zadat heslo pro parametr **SetupPfxPassword** , která vyrobila poznamenat v kroku [připravit prequisites](#prepare-prerequisites) .

    - Zkontrolujte, že zadáte **basicAuthUserName** a **basicAuthPassword** parametry. **Poznamenejte si tyto hodnoty.** Musíte je později k registraci poskytovatele zdroje.

2. Klikněte na **Uložit**.

### <a name="deploy-the-resource-provider"></a>Nasazení zprostředkovatele prostředků

1. Klikněte na kartu **nasazení Microsoft.Mysql provider.PS1** a následujícím způsobem.
2. Do Azure Active Directory po zobrazení výzvy zadejte své jméno klienta.
3. Automaticky otevírané okno odešlete přihlašovacích údajů správce služby Azure vrstvě.

Úplné nasazení může trvat mezi 15 a 45 minut na některé vysoce utilized zásobníku POCs Azure.

### <a name="update-the-local-dns"></a>Aktualizace místní DNS

1. Klikněte na kartu **Register Microsoft.MySQL fqdn.ps1** a následujícím způsobem.
2. Po zobrazení výzvy Azure Active Directory klienta ID pro zadávání vašeho Azure Active Directory klienta plně kvalifikovaný název domény: například **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Registrace zprostředkovatele prostředků SQL RP

1. Klikněte na kartu **Register Microsoft.My provider.ps1** a následujícím způsobem.

2. Po zobrazení výzvy k zadání přihlašovacích údajů použijte označeny jako **basicAuthUserName** a **basicAuthPassword** parametry.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Ověření nasazení na portálu zásobníku Azure

1. Odhlaste se z ClientVM a znovu se přihlaste jako **AzureStack\User**.

2. Na ploše klikněte na **Portál Koncepce zásobníku Azure** a přihlaste se k portálu jako správce služby.

3. Ověřte, že bylo úspěšné zavedení. Klikněte na tlačítko **Procházet** &gt; **Skupiny zdrojů**, klikněte na skupina zdroje se používá (výchozí hodnota je **MySQLRP**) a zkontrolujte essentials část zásuvné (horní polovina) přečte **nasazení proběhla úspěšně**.


4. Ověřte, že registrace úspěšně. Klikněte na tlačítko **Procházet** &gt; **poskytovatelů zdroje**a vyhledejte **Místní MySQL**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Vytvoření první databáze MySQL testování nasazení

1. Přihlaste se k portálu Azure zásobníku Koncepce jako správce služby.

2. Klikněte **+** tlačítko &gt; **vlastní** &gt; **MySQL Server & databáze**.

3. Vyplnění formuláře s podrobnostmi databáze.

    **Poznamenejte si "název serveru" zadáte.** Řetězec připojení databáze obsahuje název"server" v rámci uživatelské jméno: například ** "user@ <ServerName>"**. Budete muset při připojení k databázi při zadávání uživatelské jméno v tomto formátu: například, když nasadíte MySQL webového serveru pomocí zprostředkovatele prostředků webu Azure


## <a name="next-steps"></a>Další kroky

Vyzkoušejte další [PaaS služby](azure-stack-tools-paas-services.md) , jako třeba [zprostředkovatele prostředků serveru SQL Server](azure-stack-sql-rp-deploy-short.md) a [zprostředkovatele prostředků Web Apps](azure-stack-webapps-deploy.md).
