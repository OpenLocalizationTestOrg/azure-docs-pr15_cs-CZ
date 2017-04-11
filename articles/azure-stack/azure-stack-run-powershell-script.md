<properties
    pageTitle="Nasazení Azure zásobníku Koncepce | Microsoft Azure"
    description="Naučte se připravit Koncepce zásobníku Azure a spustit skript Powershellu pro nasazení Koncepce zásobníku Azure."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Nasazení Azure zásobníku Koncepce
Abyste mohli nasadit Koncepce zásobníku Azure, musíte nejdřív pro [přípravu nasazení počítače](#prepare-the-deployment-machine) a potom [Spustit nasazení skript Powershellu](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Stáhněte si a extrahovat Microsoft Azure zásobníku Koncepce TP2

Než začnete, ujistěte se, že se aspoň 85 GB místa.

1. Stažení Azure zásobníku Koncepce TP2 se skládá ze souboru zip obsahují následující 12 soubory sčítání ~ 20 GB:
    - 1 MicrosoftAzureStackPOC.EXE
2. Seznamte se s obrazovky podmínky licenční smlouvy a informace o Průvodci vlastní extrakci a klikněte na tlačítko **Další**.
3. Seznamte se s obrazovky zásady ochrany osobních údajů a informace o Průvodci vlastní extrakci a klikněte na tlačítko **Další**.
4. Výběr cíle pro soubory, které chcete extrahovat, klikněte na tlačítko **Další**.
    - Výchozí hodnota je: <drive letter>:\<aktuální složka > \Microsoft Koncepce zásobníku Azure
5. Seznamte se s cílovou umístění obrazovku a informace o Průvodci vlastní extrakci a klikněte na **extrahovat** extrahovat CloudBuilder.vhdx (~44.5 GB) a ThirdPartyLicenses.rtf soubory.

> [AZURE.NOTE] Po extrahujte soubory můžete odstranit soubor zip obnovíte místo na počítači. Nebo můžete přesunout zip soubor do jiného umístění, takže pokud potřeba nasadit se nemusíte stahovat soubory zip znovu.

## <a name="prepare-the-deployment-machine"></a>Příprava počítače nasazení

1. Ujistěte se, že fyzicky připojit k počítači, nasazení, nebo mají přístup fyzické konzoly (například KVM). Tento přístup budete potřebovat po restartování počítače nasazení v kroku 9.

2. Zkontrolujte, že nasazení počítač splňuje [minimální požadavky](azure-stack-deploy.md). [Kontrola nasazení Azure zásobníku Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) můžete použít k potvrzení vašim požadavkům.

3. Přihlaste se jako správce místní do počítače Koncepce.

4. Zkopírujte soubor CloudBuilder.vhdx C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Pokud se rozhodnete používat doporučené skript Příprava hostitelském počítači Koncepce (krok 5 – kroku 7), není třeba vkládat libovolné klávesy licenci na stránce s aktivací. Zkušební verzi Windows serveru 2016 obrázek je součástí a zadáním licenční klíč způsobí vypršení platnosti zprávy s upozorněním.

5. Na počítači Koncepce spusťte tento skript Powershellu stahovat soubory Azure zásobníku TP2 technické podpory:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Tento skript stáhne Azure zásobníku TP2 podpůrné soubory do složky určené parametrem $LocalPath.
    
6. Otevřete zvýšenými konzoly PowerShell a změňte adresář na místo, kam jste zkopírovali soubory.
    - 11 MicrosoftAzureStackPOC N.BIN (kde N je 1-11)
7. Klikněte pravým tlačítkem myši na MicrosoftAzureStackPOC.EXE > Spustit jako správce.

8. Spusťte skript PrepareBootFromVHD.ps1. Tento skript a unattend soubory, které jsou k dispozici další podporu skripty obsažených v této Tvůrce dotazů.
    Existuje pět parametrů pro tento skript Powershellu:
    - CloudBuilderDiskPath (povinné) – cesta k CloudBuilder.vhdx na HOSTITELI.
    - (Volitelné) – DriverPath můžete přidat další ovladače hostitele v virtuální HD.
    - (Volitelné) – ApplyUnattend zadejte tento parametr k automatizaci konfigurace operačního systému. Pokud je zadaná, musí zadat AdminPassword konfigurace operační systém při spuštění (vyžaduje za předpokladu, že soubor unattend_NoKVM.xml je průvodních).
    Pokud nepoužíváte tento parametr, se používá obecný zachováni bez další přizpůsobení. Budete muset KVM úplného přizpůsobení po restartování ho.
    - (Volitelné) – AdminPassword používat pouze v případě ApplyUnattend parametr nastaven, vyžaduje aspoň šest znaků.
    - VHDLanguage (volitelné) – určuje jazyk virtuální pevný disk na "en US".
    Skript uvedených a obsahuje příklad použití, když je nejčastější použití:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Pokud spustíte tento příkaz přesné, je nutné zadat AdminPassword příkazového řádku.

9. Po dokončení skript musí potvrdit restartování počítače. Pokud jsou jiní uživatelé přihlášeni, tento příkaz se nezdaří. Pokud se příkaz nezdaří, spusťte tento příkaz:`Restart-Computer -force` 

10. HOSTITELI restartuje do OS CloudBuilder.vhdx, pokud nasazení trvá.

> [AZURE.IMPORTANT] Azure zásobníku vyžaduje přístup k Internetu, přímo nebo prostřednictvím průhledné proxy serveru. Nasazení TP2 Koncepce podporuje právě jeden NIC pro připojení k síti. Pokud máte víc nic, ujistěte se, že je povolený pouze jeden (a všechny ostatní jsou zakázány) před spuštěním skript pro nasazení v další části.

## <a name="run-the-powershell-deployment-script"></a>Spustit skript Powershellu nasazení

1. Přihlaste se jako správce místní do počítače Koncepce. Použijte přihlašovací údaje zadané v předchozích krocích.

2. Otevřete zvýšenými konzoly Powershellu.

3. V prostředí PowerShell spusťte tento příkaz:`cd C:\CloudDeployment\Configuration`

4. Příkaz nasazení:`.\InstallAzureStackPOC.ps1`

5. Do příkazového řádku **zadat heslo** zadejte heslo a pak ho potvrďte ho. To je heslo k virtuálních počítačích. Ujistěte se, že ho nahrát.

6. Zadejte přihlašovací údaje pro váš účet Azure Active Directory. Tento uživatel musí být globální správce v klientovi adresář.

7. Nasazení může trvat několik hodin, ve kterých automaticky restartování jednou.

    > [AZURE.IMPORTANT] Pokud chcete sledovat nasazení, přihlaste se jako azurestack\AzureStackAdmin. Pokud se přihlásit jako místní správce po počítač připojen k doméně, neuvidíte průběh nasazení. Nemáte spusťte nasazení, místo toho Přihlaste se jako azurestack\AzureStackAdmin ověřte, jestli je spuštěný.

    Pokud nasazení podaří, zobrazí konzole Powershellu: **DOKONČENO: akce "Nasazení"**.

    Pokud se nezdaří nasazení, můžete zkusit [znovu](azure-stack-rerun-deploy.md). Nebo můžete [přeinstalujte](azure-stack-redeploy.md) ho od začátku.

### <a name="deployment-script-examples"></a>Příklady skriptů nasazení

Pokud svoji identitu AAD pouze přidružené jeden adresář AAD:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Je-li vaši identitu AAD přidružené k větší než jeden AAD adresář:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Pokud vaše prostředí nemá DHCP povoleno, musí obsahovat další parametry na jednu z možností nad (příklad použití k dispozici):

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Volitelné parametry InstallAzureStackPOC.ps1

| Parametr | Povinné a nepovinné | Popis |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Volitelné | Nastaví Azure Active Directory uživatelské jméno a heslo. ID organizace nebo Account Microsoft může být těchto Azure přihlašovacích údajů. Použít přihlašovací údaje Microsoft Account, nevytvářejte tento parametr rutiny. V rozevírací nabídce Azure ověřování vynechání tento parametr vyzve během nasazení. Tím vytvoříte ověřování a aktualizace tokeny používaný během nasazení. |
| AADDirectoryTenantName | Povinné | Nastaví adresáři klienta. Tento parametr použijte k určení určitých adresáři mají oprávnění ke správě více adresářů AAD účtu. Celé jméno klienta adresáře AAD ve formátu <directoryName>. onmicrosoft.com. |
| AdminPassword | Povinné | Nastaví místního účtu správce a všechny ostatní uživatelské účty ve všech virtuálních počítačích vytvořen jako součást Koncepce nasazení. Toto heslo se musí shodovat aktuální heslo místního správce na hostiteli. |
| AzureEnvironment | Volitelné | Vyberte Azure prostředí, se kterým chcete zaregistrovat tohoto Azure zásobníku nasazení. Možnosti zahrnují *Veřejné Azure*, *Azure - Čína* *Azure - vládní organizace*. |
| EnvironmentDNS | Volitelné | DNS server se vytvoří jako součást nasazení Azure vrstvě. Umožňuje počítači uvnitř řešení mimo označení Kontrola jmen poskytují existující infrastruktury DNS server. Server DNS v razítko předá Neznámý název řešení požadavků na tento server. |
| NatIPv4Address | Požadovaný pro podporu DHCP překladu síťových adres | Nastaví statickou IP adresu pro MAS BGPNAT01. Tento parametr použijte jenom DHCP nelze přiřadit platné IP adrese povolíte přístup k Internetu. |
| NatIPv4DefaultGateway | Požadovaný pro podporu DHCP překladu síťových adres | Nastaví použitá výchozí brána statickou IP adresu pro MAS BGPNAT01. Tento parametr použijte jenom DHCP nelze přiřadit platné IP adrese povolíte přístup k Internetu.  |
| NatIPv4Subnet | Požadovaný pro podporu DHCP překladu síťových adres | Předpona IP adres podsítí použitá pro DHCP přes překladu síťových adres podpory. Tento parametr použijte jenom DHCP nelze přiřadit platné IP adrese povolíte přístup k Internetu.  |
| PublicVLan | Volitelné | Nastaví VLAN ID. Použijte tento parametr, pouze pokud hostitele a MAS BGPNAT01 musíte nakonfigurovat VLAN ID pro přístup k fyzické síti (a Internetu). Například`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Znovu spustit | Volitelné | Pomocí tohoto příznaku spusťte nasazení.  Používá se všechny předchozí vstup. Znovu zadávat data dřív k dispozici není podporováno, protože vytvářejí a použita pro nasazení několik jedinečné hodnoty. |
| TimeServer | Volitelné | Tento parametr použijte v případě potřeby zadejte konkrétní časového serveru. |

## <a name="next-steps"></a>Další kroky

[Připojení k Azure zásobníku](azure-stack-connect-azure-stack.md)
