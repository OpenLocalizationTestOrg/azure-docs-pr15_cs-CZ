<properties
    pageTitle="Začínáme s Azure klíč trezoru | Microsoft Azure"
    description="Pomocí tohoto kurzu vám pomůžou začít s Azure klíč trezoru vytvoření kontejneru zesílený v Azure k ukládání a správa klíče a tajemství v Azure."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Začínáme s trezoru klíč Azure #
Azure trezoru klíč je k dispozici ve většině oblastech. Další informace najdete v tématu [klíč trezoru ceny stránky](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod  
Pomocí tohoto kurzu vám pomůžou začít s Azure klíč trezoru vytvoření kontejneru zesílený (trezoru) v Azure, ukládání a správu cryptographic klíče a tajemství v Azure. Ho vás provede proces vytvoření trezoru obsahující klíče nebo heslo, které pak můžete pomocí aplikace Azure pomocí prostředí PowerShell Azure. Ho pak se dozvíte, jak můžete aplikaci používat tento klíč nebo heslo.

**Předpokládaná doba dokončete:** 20 minut

>[AZURE.NOTE]  Tento kurz nezahrnuje pokyny, jak psát Azure aplikace, že jeden z kroků obsahuje, a to jak povolit aplikaci pro používání klíče nebo tajná klíčové trezoru.
>
>Tento kurz používá Azure Powershellu. Rozhraní příkazového řádku různé platformy, najdete v článku [odpovídající kurzu](key-vault-manage-with-cli.md).

Základní informace o trezoru klíč Azure, najdete v článku [Co je Azure klíč trezoru?](key-vault-whatis.md)

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz musí mít takto:

- Předplatné Microsoft Azure. Pokud nemáte jeden, můžete registraci [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
- Azure Powershellu, **minimální verze 1.1.0**. Instalace prostředí PowerShell Azure a přidružit Azure předplatné, najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Pokud jste již nainstalovali Azure PowerShell a nebudou vědět, požadovanou verzi z konzoly Azure Powershellu, zadejte `(Get-Module azure -ListAvailable).Version`. Když budete mít Azure PowerShell verze 0.9.1 prostřednictvím 0.9.8 nainstalovaný, můžete dál používat tento kurz se některé menší změny. Například, je nutné použít `Switch-AzureMode AzureResourceManager` příkaz a některé příkazy Azure klíč trezoru změnily. Seznam rutiny trezoru klíč pro verze 0.9.1 prostřednictvím 0.9.8 najdete v tématu [Rutiny trezoru klíč Azure](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Aplikace, která bude nakonfigurovaný na používání klíč nebo heslo, které vytvoříte v tomto kurzu. Ukázka aplikace je k dispozici z [Webu služby Stažení softwaru](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Pokyny najdete v tématu doprovodnou souboru Readme.


Tento kurz je určený pro začátečníky Azure Powershellu, ale předpokládá chápete základní koncepty, například modulů kontroly, rutin a relace. Další informace najdete v tématu [Začínáme používat Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Zobrazíte podrobnou nápovědu pro všechny rutina, která se zobrazí v tomto kurzu získáte pomocí rutiny **Get-Help** .

    Get-Help <cmdlet-name> -Detailed

Nápovědu k rutinu **AzureRmAccount přihlášení** , zadejte například:

    Get-Help Login-AzureRmAccount -Detailed

Můžete taky přečíst následující kurzy k seznámení s správce prostředků Azure v Azure Powershellu:

- [Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)
- [Pomocí prostředí PowerShell Azure pomocí Správce prostředků](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Připojení k předplatného ##

Zahájit relaci Powershellu Azure a přihlaste se k účtu Azure pomocí následujícího příkazu:  

    Login-AzureRmAccount 

Poznámka: Pokud používáte konkrétní instanci Azure, například Azure pro státní správu, použít parametr – prostředí pomocí tohoto příkazu. Příklad:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

V okně místní prohlížeče zadejte svůj účet Azure uživatelské jméno a heslo. Azure Powershellu získá všechna předplatná, které jsou přidružené k tomuto účtu a ve výchozím nastavení, použije první z nich.

Pokud máte víc předplatných a chcete zadat specifické pro účely trezoru klíč Azure, zadejte podle následujících pokynů v tématu předplatná pro váš účet:

    Get-AzureRmSubscription

Chcete-li předplatné pro použití, zadejte:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Další informace o konfiguraci Azure Powershellu najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).


## <a id="resource"></a>Vytvoření nové skupiny prostředků ##

Při použití Správce prostředků Azure všechny související materiály vzniká uvnitř skupiny zdrojů. Vytvoříme nové skupiny prostředků pro účely tohoto návodu s názvem **ContosoResourceGroup** :

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Vytvoření klíčových trezoru ##

Použijte rutinu [New-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) vytvořit klíčové trezoru. Tato rutina má tři povinné parametry: **název skupiny prostředků**, **klíčové trezoru název**a **zeměpisné polohy**.

Pokud používáte název trezoru **ContosoKeyVault**, název skupiny zdroje **ContosoResourceGroup**a umístění **jihovýchodní Asie**, zadejte například:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Výstup tuto rutinu zobrazí vlastnosti klíčové trezoru, který jste právě vytvořili. Jsou dva nejdůležitější vlastnosti:

- **Název trezoru**: V tomto příkladu je to **ContosoKeyVault**. Použijete tento název pro jiné rutiny klíč trezoru.
- **Identifikátor URI trezoru**: V tomto příkladu je to https://contosokeyvault.vault.azure.net/. Aplikace, které využívají trezoru prostřednictvím jeho rozhraní REST API musíte použít tento identifikátor URI.

Váš účet Azure je teď oprávnění provádět všechny operace s trezoru klíče. Co ještě nikdo jiný je.

>[AZURE.NOTE]  Pokud se zobrazí chyba **předplatné není registrovaný používat názvů "Microsoft.KeyVault"** při pokusu o vytvoření trezoru nové klíče, spusťte `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` a znovu spusťte příkaz Nový AzureRmKeyVault. Další informace najdete v tématu [AzureRmResourceProvider rejstříku](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a id="add"></a>Přidání klíče nebo tajná klíčové trezoru ##

Podle potřeby Azure klíč trezoru při vytváření klíče software chráněn můžete použít rutinu [AzureKeyVaultKey přidat](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) a zadejte tento příkaz:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Ale pokud máte existující software chráněn klíč v. Soubor PFX uložili na disk C:\ do souboru nazvaného softkey.pfx, který chcete nahrát do trezoru klíč Azure zadejte následující nastavení proměnné **securepfxpwd** k zadání hesla **123** pro. Soubor PFX:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Zadejte následující klíč z importovat. PFX soubor, který chrání klávesu softwarem ve službě trezoru klíč:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Teď můžete odkázat tento klíč, který jste vytvořili nebo nahrát trezoru klíč Azure pomocí jeho URI. Získáte pomocí **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy aktuální verzi a pomocí **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** této konkrétní verzi.  

Chcete-li zobrazí Uniform pro tento klíč, zadejte:

    $Key.key.kid

Přidat tajná do trezoru, která je hesla s názvem SQLPassword a má hodnotu Pa$ $w0rd trezoru klíč Azure, nejprve převést hodnotu z Pa$ $w0rd řetězec zabezpečené zadáním následujícího:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Zadejte následující:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Teď můžete odkázat toto heslo, které jste přidali do trezoru klíč Azure pomocí jeho URI. Získáte pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy aktuální verzi a pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** této konkrétní verzi.

Identifikátor URI pro tento tajná zobrazíte zadejte:

    $secret.Id

Pojďme zobrazte klávesu tajná, který jste právě vytvořili:

- Pokud chcete zobrazit kód, zadejte:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Chcete-li zobrazit skryté, zadejte:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Teď klíčové trezoru a klíč nebo tajná je připraven k aplikacím používat. Musí povolit aplikace jejich použití.  

## <a id="register"></a>Registrace aplikace pomocí služby Azure Active Directory ##

Tento krok by provést obvykle vývojář, v samostatném počítači. Se nevztahuje na Azure klíč trezoru, ale jsou zde uvedeny pro dokončení.


>[AZURE.IMPORTANT] Dokončete kurzu, váš účet, trezoru a aplikace, která bude registrovat v tomto kroku musí být stejného Azure adresáře.

Aplikace, které využívají klíčové trezoru musí ověřovat pomocí token ze služby Azure Active Directory. K tomuto účelu vlastník aplikace nejdřív zaregistrujte aplikace v Azure Active Directory. Na konci registrace vlastníka aplikace obdrží následující hodnoty:


- **ID aplikace** (označovaná taky jako ID klienta) a **ověřování klíč** (označovaná taky jako sdílené heslo). Aplikace musí prezentovat obě tyto hodnoty Azure Active Directory, chcete-li získat token. Konfigurace aplikace k tomuto závisí na aplikace. Ukázka aplikace trezoru klíč nastaví vlastníka aplikace tyto hodnoty v konfiguračního souboru.

Registrace aplikace v Azure Active Directory:

1. Přihlaste se k portálu Azure klasické.
2. Na levé straně klikněte na **Služby Active Directory**a vyberte v adresáři, ve kterém bude registrovat aplikace. <br> <br> **Poznámka:** Je nutné vybrat stejný adresář, který obsahuje Azure předplatné, ke které jste vytvořili trezoru klíče. Pokud si nejste jisti, které adresáře to je, klikněte na **Nastavení**, identifikovat předplatné, ke které jste vytvořili trezoru klíče a zapište si název adresáři zobrazené v posledním sloupci.

3. Klikněte na **aplikace**. Pokud není aplikace byly přidány do vašeho adresáře, tato stránka zobrazuje pouze na odkaz **Přidat aplikaci** . Klikněte na odkaz, nebo případně můžete kliknout **Přidat** na panelu s příkazy.
4.  V průvodci **Přidat aplikaci** na **Co chcete udělat?** stránky, klikněte na **Přidat aplikaci, které vyvíjí mé organizaci**.
5.  Na stránce **námi o aplikaci** zadejte název aplikace a pak vyberte **Rozhraní API webových aplikací a či nebo WEB** (výchozí). Klepněte na ikonu **Další** .
6.  Na stránce **Vlastnosti aplikace** , zadejte **Adresu URL na ODHLÁSIT** a **Identifikátor URI aplikace** webové aplikace. Pokud není aplikace tyto hodnoty, můžete jejich pro tento krok (je třeba zadat http://test1.contoso.com obou polí). Pokud existují těchto webů nezáleží. Důležité je, že se liší pro všechny aplikace v adresáři aplikace Identifikátor URI pro jednotlivé aplikace. V adresáři používá tento řetězec k identifikaci aplikace.
7.  Klikněte na ikonu **Hotovo** uložte provedené změny v průvodci.
8.  Na stránce **Snadné spuštění** klikněte na **KONFIGUROVAT**.
9.  Přejděte do oddílu **klíčů** , nastavte dobu trvání a klikněte na tlačítko **Uložit**. Na stránce aktualizuje a teď zobrazí hodnoty klíče. S touto hodnotou klíče a hodnotu **ID klienta** musíte nakonfigurovat aplikaci. (Pokyny k této konfiguraci jsou specifické pro aplikaci).
10. Zkopírujte hodnotu ID klienta z této stránky, které budete používat v dalším kroku nastavit oprávnění pro trezoru.

## <a id="authorize"></a>Povolit aplikaci pro použití klávesy nebo tajná ##

Povolit aplikaci pro přístup k klíč nebo tajná v trezoru použijte rutinu  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) .

Například pokud je název trezoru **ContosoKeyVault** a aplikaci, kterou chcete povolit má klienta ID 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed a vy chcete povolit aplikaci dešifrování a přihlaste se pomocí klávesy v trezoru, spusťte:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Pokud chcete povolit stejné aplikace číst tajemství v trezoru, spusťte následující:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Pokud chcete používat modul zabezpečení hardwaru (HSM) ##

Přidané assurance importovat nebo vygenerování klíčů v hardwaru zabezpečení modulech (moduly hardwarového zabezpečení), které nikdy opustit HSM okraj. Moduly hardwarového zabezpečení jsou FIPS ověřit 140-2 úrovně 2. Pokud tento požadavek neplatí pro vás, tuto část přeskočit a přejít na [Odstranit klíčové trezoru a související klíče a tajemství](#delete).

Pokud chcete vytvořit klávesy chráněné heslem HSM, je nutné použít [Azure klíč trezoru Premium osy služby pro podporu chráněné heslem HSM klíče](https://azure.microsoft.com/pricing/free-trial/). Poznámka: Kromě toho, že tato funkce není k dispozici pro Azure Čína.


Při vytváření klíčové trezoru přidejte parametr **– SKU** :


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Software chráněn klíčů (jak je znázorněno výše) a chráněné heslem HSM můžete přidat do tohoto klíče trezoru. Při vytváření chráněné heslem HSM klíče tvořeného nastavit **-cíl** parametr "HSM":

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Použijete příkaz klíč z importovat. PFX soubor v počítači. Příkaz importuje klávesu do moduly hardwarového zabezpečení ve službě trezoru klíč:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Další příkaz importuje "přenést vlastní klíč" balíček (BYOK). Tento scénář umožňuje generovat kód v místním HSM a odeslat jej moduly hardwarového zabezpečení ve službě klíč trezoru bez klíče necháte HSM okraj:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Podrobnější pokyny týkající se tohoto BYOK balíček najdete v článku [jak generovat a přenos chráněné heslem HSM klávesy pro Azure klíč trezoru](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Odstranit klíčové trezoru a související klíče a tajemství ##

Pokud už nepotřebujete klíčové trezoru a klíč nebo tajná ní obsaženými, můžete odstranit klíčové trezoru pomocí rutiny [AzureRmKeyVault odebrat](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) :

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Nebo můžete odstranit celý Azure zdroj skupinu, která obsahuje klíčové trezoru a dalších zdrojů, která jste zadali v dané skupině:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Další rutiny prostředí PowerShell Azure ##

Další příkazy, které mohou být užitečné pro správu trezoru Azure klíč:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Tento příkaz získá tabulkového zobrazení všech klíče a vybrané vlastnosti.
- `$Keys[0]`: Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč
- `Get-AzureKeyVaultSecret`: Tento příkaz zobrazí tabulkového zobrazení všechny skryté názvy a vybrané vlastnosti.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Příklad odebrání určitého klíče.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Příklad odebrání konkrétní tajná.


## <a id="next"></a>Další kroky ##

Zpracování kurzu, který používá Azure klíč trezoru ve webové aplikaci najdete v článku [Použití Azure klíč trezoru z webové aplikace](key-vault-use-from-web-application.md).

Jak trezoru klíčové držitelem se používá, najdete [Azure klíč trezoru protokolování](key-vault-logging.md).

Seznam nejnovější rutiny prostředí PowerShell Azure pro Azure klíč trezoru najdete v tématu [Rutiny trezoru klíč Azure](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Programování odkazy, najdete v článku [Příručka pro vývojáře Azure klíč trezoru](key-vault-developers-guide.md).
