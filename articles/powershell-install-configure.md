<properties
    pageTitle="Instalace a konfigurace prostředí PowerShell Azure"
    description="Zjistěte, jak nainstalovat a nakonfigurovat Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Instalace a konfigurace prostředí PowerShell Azure

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">Prostředí PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure rozhraní příkazového řádku</a></div>

##<a name="what-is-azure-powershell"></a>Co je Azure PowerShell?
Azure Powershellu je sada modulů kontroly, které jsou zdrojem rutiny pro správu Azure pomocí prostředí Windows PowerShell. Pomocí rutin vytvářet, otestovat, nasazení a správu řešení a služeb Doručená prostřednictvím Azure platformy. Ve většině případů lze použít pro stejný úkoly, jako je portál Azure, například k vytváření a konfigurace cloudové služby, virtuálních počítačích, virtuálních sítí a webových aplikací web apps.

## <a name="how-versioning-works"></a>Jak funguje správa verzí

Azure Powershellu používá sémantického Správa verzí, což znamená, že pokud verze A > verzi B a potom verze A má nejaktuálnější rozhraní API. Také znamená to, že v hlavní verze střední hodnotu nejnovější změny v jedné nebo více rutiny.  Ano například verze 1.7.0 je hotfix při řešení jazycích změny ve verzích 1.x Azure Powershellu.

Další informace o sémantického správy verzí postupy v Azure Powershellu najdete v článku specifikace sémantického Správa verzí v: http://semver.org
 
Pokud chcete nejnovější rozhraní API, měli byste použít verzi 2.x. Ale pokud máte skriptů proti verze 1.x a nechcete, aby k zachycení nejnovějších změnách ve verzi 2.x podle 2.x [Poznámky](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), pak byste si měli nainstalovat 1.7.0.

Neshodu verze může dojít, pokud máte nainstalovanou nejnovější verzi modulu profil a starší verzi moduly, které závisí na tom je následně načíst. Nejjednodušší způsob, jak tento problém vyřešíte je si můžete nainstalovat z nejnovější MSI. Souboru MSI automaticky vyčistí starší verze aplikace moduly.
 
###<a name="installing-module-versions-side-by-side"></a>Instalace modulu verze vedle sebe

Verze 2.1.0 (i 1.2.6 pro AzureStack) jsou první verze modulu navržený je třeba nainstalovat a použít vedle sebe. Protože Azure PowerShell používá binární moduly, musí otevření nového okna prostředí PowerShell a použít **Import Module** k importu konkrétní verzi AzureRM rutiny:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Verze před 2.1.0 (jiné než 1.2.6) něm něco nefunguje dobře – souběžně s jinými verzemi modul Azure Powershellu. Při načítání starší verzi Azure PowerShell moduly pomocí příkazu podobné tomuto výše uvedené, kompatibilní verze modulu **AzureRM.Profile** se načte výsledkem rutiny s žádostí o přihlášení při každém spuštění rutina, i když jste přihlášení.

Nejjednodušší způsob, jak vyřešit, že toto je si můžete nainstalovat nejnovější Azure PowerShell z WebPI kanálu nebo MSI – umožňuje odebrat předchozích verzí aplikace modulů nainstalovaných v galerii. 

Všimněte si, moduly Azure a AzureRM máte závislosti společné, takže pokud použijete oba moduly při aktualizaci jednu, měli byste aktualizovat i. Starší verze modulu Azure mít stejnou problém s vedle sebe modul načítání které dřívější verze modulu AzureRM mít.

<a id="Install"></a>
## <a name="step-1-install"></a>Krok 1: instalace

Tady jsou dva způsoby, kterými můžete nainstalovat Azure Powershellu. Můžete nainstalovat z Galerie prostředí PowerShell nebo WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Instalace Azure Powershellu z Galerie prostředí PowerShell

Upřednostňovaný způsob je pomocí prostředí PowerShell galerie. Potřebujete modulu PowerShellGet použít galerii Powershellu. Tato možnost je dostupná tady: [PowerShellGallery.com](https://www.powershellgallery.com/)

Instalace modulu Azure PowerShell 1.3.0 nebo vyšší z Galerie prostředí PowerShell pomocí prostředí Windows PowerShell nebo integrované skriptování prostředí (ISE PowerShell) řádek se zvýšenými oprávněními pomocí následujících příkazů:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Další informace o příkazech

- **Instalovat modul AzureRM** instalace modulu kumulativní pro správce prostředků Azure rutiny. Závisí na modulu AzureRM 
- konkrétní verzi oblast pro každý modul Azure správce prostředků. Rozsah však započítávány verze zajišťuje, žádné změny modul meze může obsahovat při instalaci AzureRM moduly se stejnou hlavních verzí. Při instalaci modulu AzureRM všechny správce prostředků Azure moduly, které není dříve nainstalována se stáhne a nainstalovali z Galerie Powershellu. Další informace o sémantického správy verzí používaný moduly Azure Powershellu najdete v článku [semver.org](http://semver.org). 
- **Nainstalujte modul Azure** instalace modulu Azure. Tento modul je ten služba Správa z Azure PowerShell 0.9.x. Musí mít bez hlavní změny a být zaměnit v předchozí verzi modulu Azure.

###<a name="installing-azure-powershell-from-webpi"></a>Instalace Azure Powershellu z WebPI

Instalace Azure PowerShell 1.0 a větší z WebPI odpovídá bylo pro 0.9.x. Stažení [Prostředí PowerShell Azure](http://aka.ms/webpi-azps) a spusťte instalaci. Pokud máte Azure PowerShell 0.9.x nainstalovaný, verze 0.9.x odinstaluje v rámci upgradu. Nainstalovaného Azure PowerShell moduly prostředí PowerShell galerii instalační program automaticky odstraní moduly před instalací zajistit konzistentní prostředí Azure PowerShell.

> [AZURE.NOTE] Pokud jste nainstalovali dříve Azure moduly z Galerie Powershellu, instalační program, budou automaticky odebrány. Zapomíná a to tím, o které verze modulu máte nainstalované a kde se nacházejí. Galerie prostředí PowerShell moduly obvykle nainstaluje **%ProgramFiles%\WindowsPowerShell\Modules**. Naopak WebPI instalační program nainstaluje Azure moduly v * *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Pokud dojde k chybě při instalaci, můžete odebrat ručně Azure* složky ve složce **%ProgramFiles%\WindowsPowerShell\Modules** a zkuste instalaci znova.

Po dokončení instalace vaše ```$env:PSModulePath``` nastavení by měl obsahovat adresáře obsahující rutiny prostředí PowerShell Azure.

> [AZURE.NOTE] Je známý problém s prostředím PowerShell **$env: PSModulePath** , může dojít při instalaci ze WebPI. Pokud váš počítač vyžaduje restartování počítače z důvodu aktualizace systému nebo jiných zařízeních, může se stát, aktualizace **$env: PSModulePath** tak, aby neobsahoval cesta nainstalovanou Azure Powershellu. V takovém případě se může zobrazit zpráva "rutina nebyl rozpoznán" při pokusu o pomocí rutin prostředí PowerShell Azure po instalaci nebo upgrade. V takovém případě restartování počítače by měl vyřešit.

Pokud se zobrazí zprávu podobnou následující, když se pokusíte načíst nebo spuštění rutiny:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Tento problém můžete odstranit tak, že restartování počítače nebo import rutin z C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ jako následující (kde XXXX je verzi prostředí PowerShell nainstalovaný:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Krok 2: spuštění
Rutiny můžete spustit ze standardní konzoly Windows PowerShell nebo z prostředí PowerShell integrovanou skriptování prostředí (ISE).
Metody, kterou otevřete buď konzoly závisí na verzi systému Windows používáte:

- V počítači se systémem aspoň Windows 8 nebo Windows Server 2012, můžete pomocí integrovaného vyhledávání. Na obrazovce **Start** začněte psát power. Tento příkaz vrátí obory seznam aplikací, který obsahuje prostředí Windows PowerShell. Spusťte konzolu, klikněte na některý z aplikace. (Připnutí aplikace na **úvodní** obrazovku Windows, klikněte pravým tlačítkem myši na ikonu.)

- V počítači se systémem verze dřívější než Windows 8 nebo Windows Server 2012 pomocí **nabídky Start**. V nabídce **Start** klikněte na položky **Všechny programy**, klikněte na **položku Příslušenství**, klikněte na složku **Prostředí Windows PowerShell** a klikněte na **Prostředí Windows PowerShell**.

Můžete taky spustit **Windows PowerShell ISE** použití položek nabídky a klávesové zkratky pro provádějí řadu stejné úkoly, které chcete provést v konzole prostředí Windows PowerShell. Použít ISE v konzole prostředí Windows PowerShell Cmd.exe, nebo do pole **Spustit** , zadejte, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Příkazy, které vám pomůžou začít

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Krok 3: připojení
Rutiny třeba předplatného, aby mohl spravovat vaše služby. Pokud už nemáte, můžete si koupit předplatné Azure. Pokyny najdete v tématu [Jak koupit Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Typ **AzureRmAccount přihlášení**

2. Zadejte e-mailové adresy a hesla přidruženého k vašemu účtu. Azure ověří uloží přihlašovacími údaji a potom slouží k zavření okna.

– NEBO –

Přihlaste se k svůj pracovní nebo školní účet:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Pokud máte víc než jednoho klienta přidružený k účtu vaší organizace, zadejte parametr TenantId:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Tento interaktivním přihlášení metodu jde použít pouze pomocí pracovního nebo školního účtu. Pracovní nebo školní účet je uživatel, který je spravuje váš pracovní nebo školní a definované v instanci služby Azure Active Directory pro práci a studium. Pokud zrovna nemáte pracovní nebo školní účet a používáte účet Microsoft k přihlášení k předplatnému Azure, můžete můžete snadno vytvořit pomocí následujících kroků.

> 1. Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com)a klikněte na **Služby Active Directory**.

> 2. Pokud existuje žádný adresář, vyberte možnost **vytvořit adresář** a zadejte požadované informace.

> 3. Vyberte svůj adresář a přidání nového uživatele. Tomuto novému uživateli můžete Přihlaste se pomocí pracovního nebo školního účtu. Při vytváření uživatel bude dodaného s obou e-mailovou adresu uživatele a dočasného hesla. Uložte tyto informace, jak se používá v kroku 5 níže.

> 4. Z portálu Microsoft Azure klasické vyberte **Nastavení** a potom vyberte **Správce**. Klikněte na **Přidat**a přidání nového uživatele jako spolu správce. Díky pracovní nebo školní účet pro správu předplatného Azure.

> 5. Nakonec odhlášení z portálu Azure klasické a klikněte znova se přihlaste pomocí pracovní nebo školní účet. Pokud je první čas přihlašování pomocí tohoto účtu, se výzva ke změně hesla.

> Další informace o přihlašování k Microsoft Azure pomocí pracovního nebo školního účtu najdete v článku [registraci Microsoft Azure jako organizace](./active-directory/sign-up-organization.md).

> Další informace o správě ověřování a předplatné v Azure najdete v článku [Správa účtů, předplatná a správní role](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Zobrazení podrobností o účtech a předplatného

Můžete mít víc účtů a předplatná umožňující nepoužívá Azure Powershellu. Můžete přidat víc účtů spuštěním **AzureRmAccount přidat** více než jednou.

Zobrazit dostupné Azure účty, zadejte **Get-AzureAccount**.

Zobrazíte předplatné Azure zadejte **Get-AzureRmSubscription**.

##<a id="Help"></a>Získání nápovědy##

Tyto materiály nápovědu pro konkrétní rutiny:


-   Z v konzole, můžete použít předdefinované systém nápovědy. **Získání nápovědy** rutina poskytuje přístup k systému. 

- Pokud potřebujete pomoc od komunity vyzkoušejte tyto Oblíbené fóra:

 - [Azure fóru na webu MSDN]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Víc se uč


Naleznete v následujících zdrojích Další informace o používání rutin:

Základní informace o používání Windows Powershellu najdete v článku [Pomocí Windows Powershellu](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Referenční informace o rutinách najdete v článku Principy [Azure rutiny](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Ukázky skriptů a pokyny týkající se Naučte se používat ke správě Azure skriptování najdete v tématu [Centrum skriptů](http://go.microsoft.com/fwlink/p/?LinkId=321940).

