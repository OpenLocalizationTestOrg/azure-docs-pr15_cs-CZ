<properties
    pageTitle="Jak vytvořit a převést chráněné heslem HSM klíče Azure klíč trezoru | Microsoft Azure"
    description="Použijte tento článek vám pomůže plánování, generovat a pak ho přepojit vlastní chráněné heslem HSM klávesy pro práci s Azure klíč trezoru."
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
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Jak vytvořit a přenos chráněné heslem HSM klíčích pro trezoru klíč Azure

##<a name="introduction"></a>Úvod

Pro přidané assurance při použití klávesy trezoru Azure, můžete importovat nebo vygenerování klíčů v hardwaru zabezpečení modulech (moduly hardwarového zabezpečení), které nikdy opustit HSM okraj. Tento scénář je někdy označovány jako *přenést vlastní klíč*nebo BYOK. Moduly hardwarového zabezpečení jsou FIPS ověřit 140-2 úrovně 2. Azure trezoru klíč používá společnosti Thales nShield řadu moduly hardwarového zabezpečení chránit vaše klíče.

Umožňuje informace v tomto tématu vám usnadní plánování, generovat a pak ho přepojit vlastní chráněné heslem HSM klávesy pro práci s Azure klíč trezoru.

Tato funkce není k dispozici pro Azure Čína. 

>[AZURE.NOTE] Další informace o Azure klíč trezoru najdete v tématu [Co je Azure klíč trezoru?](key-vault-whatis.md)  
>
>Výukové dosáhnout toho, aby Začínáme, které můžete vytvořit klíčové trezoru chráněné heslem HSM klíčů naformátovat, najdete v článku [Začínáme s Azure klíč trezoru](key-vault-get-started.md).

Další informace o vytváření a přenos klíč chráněné heslem HSM přes Internet:

- Generování klíče z offline workstation snižuje napadení.

- Klíč musí být zašifrovaný s klíč Exchange klíč (KEK), které zůstává šifrované, až bude převedena na moduly hardwarového zabezpečení Azure klíč trezoru. Pouze šifrované verzi kód ponechá původní workstation.

- Sadu nástrojů nastaví vlastnosti kód klienta spojující kód ve světě zabezpečení Azure klíč trezoru. Takže po moduly hardwarového zabezpečení Azure klíč trezoru přijímání a dešifrování kód, jenom tyto moduly hardwarového zabezpečení můžete používat. Kód není možné exportovat. Tuto vazbu nevynucují tak, že moduly hardwarového společnosti Thales zabezpečení.

- Klíč Exchange klíč (KEK), který slouží k šifrování kód generováno uvnitř moduly hardwarového Azure klíč trezoru zabezpečení a není možné exportovat. Moduly hardwarového zabezpečení vynutit, zda může být bez vymazat verzi KEK mimo moduly hardwarového zabezpečení. Kromě toho sadu nástrojů zahrnuje potvrzení, že KEK není možné exportovat a vygenerované uvnitř originální HSM, která byla vyrobila společnosti Thales společnosti Thales.

- Sadu nástrojů obsahuje potvrzení, že na světě zabezpečení Azure klíč trezoru vygenerované taky na originální HSM společnost společnosti Thales společnosti Thales. Tento potvrzení ukáže vám, že používá Microsoft genuine hardwaru.

- Společnost Microsoft používá samostatné KEKs a oddělte světě zabezpečení v jednotlivých zeměpisnou oblast. Oddělení zajistí, že kód můžete použít jenom v datacentrech v oblasti, ve kterém je šifrovaná. Například klíč od Evropský zákazníka nelze použít v datacentrech v Severní Americe a Asii.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Další informace o službách společnosti Thales moduly hardwarového zabezpečení a Microsoft

Společnosti Thales e zabezpečení je počáteční globální poskytovatel šifrování internetovým řešení a zabezpečení finančních služeb, maximum technologie, výrobních, government, a technologie odvětví. S 40 rok sledování záznam ochrany podnikové a pro státní správu informace společnosti Thales řešení používají čtyři z pěti největší energie a šestinásobné společností. Jejich řešení taky používá 22 NATO země a zabezpečit více než 80 % transakce Celosvětová dostupnost plateb.

Microsoft spolupracuje s společnosti Thales k vylepšení stav obrázek pro moduly hardwarového zabezpečení. Tato vylepšení umožňují získat typické výhody hostované služby bez vzdát publikum nemůže ovládat své klíče. Konkrétně tato vylepšení nechat Microsoft spravovat moduly hardwarového zabezpečení tak, aby se nemusíte. Jako do cloudové služby Azure klíč trezoru škálování krátkodobě plnit vrcholy pole používání vaší organizace. Ve stejnou dobu, po zamknutí kód uvnitř moduly hardwarového zabezpečení společnosti Microsoft: zachování kontroly nad tím klíčové životního cyklu, protože generování klíče a přenést do moduly hardwarového zabezpečení společnosti Microsoft.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Provádění přenést vlastní klíč (BYOK) pro trezoru klíč Azure

Použijte následující informace a postupy, pokud bude vygenerovat vlastní chráněné heslem HSM klíč a potom ho přepojte na Azure klíč trezoru – přenést klíč (BYOK) nefunguje.


##<a name="prerequisites-for-byok"></a>Požadavky pro BYOK

Najdete v následující tabulce seznam požadavcích na přenést vlastní klíč (BYOK) pro Azure klíč trezoru.

|Požadavek|Další informace|
|---|---|
|Předplatné Azure|Pokud chcete vytvořit trezoru klíč Azure, musíte předplatné Azure: [registrace pro bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)|
|Vrstvy služby Azure klíč trezoru Premium podporuje chráněné heslem HSM klíče|Další informace o – úrovně služeb a funkcí pro Azure klíč trezoru najdete na webu [Azure klíč trezoru ceny](https://azure.microsoft.com/pricing/details/key-vault/) .|
|Společnosti Thales HSM čipové karty a podporu softwarových|Musí mít přístup k společnosti Thales hardwaru zabezpečení modulu a základní provozní znalost společnosti Thales moduly hardwarového zabezpečení. Seznam kompatibilních modely nebo koupit HSM, pokud nemáte jeden najdete v článku [Společnosti Thales hardwaru zabezpečení modulu](https://www.thales-esecurity.com/msrms/buy) .|
|Následující hardware a software:<ol><li>Offline x64 workstation s minimální operační systém Windows z Windows 7 a společnosti Thales nShield software minimální verze 11.50.<br/><br/>Pokud je tento workstation spuštěn Windows 7, musíte [nainstalovat Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Workstation, který je připojený k Internetu a má minimální operační systém Windows systému Windows 7.</li><li>Jednotka USB nebo jiné přenosného zařízení, které obsahuje aspoň 16 MB volného místa.</li></ol>|Z bezpečnostních důvodů doporučujeme, aby první stanici není připojený k síti. Toto doporučení však není nevynucují programově.<br/><br/>Všimněte si, že v pokyny, které následují za tento workstation se označuje jako odpojeno workstation.</p></blockquote><br/>Kromě toho kód klienta produkční sítě, doporučujeme používat druhou, samostatnou stanici ke stažení nástroje a nahrajte klávesu klienta. Ale pro účely testování je možné použít stejné workstation jako první.<br/><br/>Všimněte si, že v pokyny, které následují za tento druhý workstation se označuje jako workstation připojeného k Internetu.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generovat a přenášet kód HSM trezoru klíč Azure

Generovat a převeďte kód do Azure klíč trezoru HSM použijete pět takto:

- [Krok 1: Příprava počítači připojeného k Internetu](#step-1-prepare-your-internet-connected-workstation)
- [Krok 2: Příprava počítači odpojeno](#step-2-prepare-your-disconnected-workstation)
- [Krok 3: Generovat kód](#step-3-generate-your-key)
- [Krok 4: Příprava klíč pro přenos](#step-4-prepare-your-key-for-transfer)
- [Krok 5: Přenášet kód trezoru klíč Azure](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Krok 1: Příprava počítači připojeného k Internetu
U tohoto prvního kroku proveďte následující postupy k počítači, který je připojený k Internetu.


###<a name="step-11-install-azure-powershell"></a>Krok 1.1: Instalace Azure Powershellu

Z workstation připojeného k Internetu stáhněte a nainstalujte modul Azure PowerShell, který obsahuje rutiny pro správu Azure klíč trezoru. Při této akci musí minimální verze 0.8.13.

Pokyny k instalaci zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Krok 1.2: Stáhněte si ID Azure předplatného

Zahájit relaci Powershellu Azure a přihlaste se k účtu Azure pomocí tento příkaz:

        Add-AzureAccount
V okně místní prohlížeče zadejte svůj účet Azure uživatelské jméno a heslo. Pak pomocí příkazu [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Z výstupu vyhledejte ID pro předplatné, které budete používat pro Azure klíč trezoru. Toto ID předplatného budete později potřebovat.

Není zavřete okno Azure Powershellu.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Krok 1.3: Stažení BYOK nástrojů pro trezoru klíč Azure

Klikněte na Microsoft Download Center a [Stáhnout Azure klíč trezoru BYOK nástrojů](http://www.microsoft.com/download/details.aspx?id=45345) pro zeměpisná oblast nebo výskyt Azure. Zadejte název balíčku stáhnout a jeho odpovídající balíčku hash SHA 256 pomocí následující informace:

---

**Severní Amerika:**

KeyVault-BYOK-nástroje spojené States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Evropa:**

KeyVault BYOK nástroje Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Země Asie:**

KeyVault BYOK nástroje AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Latinská Amerika:**

KeyVault BYOK nástroje LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japonsko:**

KeyVault BYOK nástroje Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Austrálie:**

KeyVault BYOK nástroje Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault BYOK nástroje USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault BYOK nástroje Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Německo:**

KeyVault BYOK nástroje Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Indie:**

KeyVault BYOK nástroje India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Ověření integrity nástrojů stažené BYOK z relaci Powershellu Azure získáte pomocí rutiny [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) .

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Sadu nástrojů patří:

- Balíček klíč Exchange klíč (KEK), který má název začínající **BYOK-KEK - pkg-.**
- Zabezpečení světě balíčku, který má název začínající **BYOK-SecurityWorld - pkg-.**
- Skript python s názvem v**erifykeypackage.py.**
- Příkazového řádku spustitelný soubor pojmenované **KeyTransferRemote.exe** a související DLL.
- Vizuální C++ Redistributable balíček, s názvem **vcredist_x64.exe.**

Zkopírujte balíček jednotka USB nebo jiné přenosné úložiště.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Krok 2: Příprava počítači odpojeno

Tento druhý krok proveďte následující postupy v workstation, který není připojený k síti (Internetu nebo vnitřní síti).


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Krok 2.1: Příprava odpojeno workstation s společnosti Thales HSM

Instalace softwaru podpory nCipher (společnosti Thales) na počítači s Windows a ten pak připojíte HSM společnosti Thales na tomto počítači.

Ujistěte se, že jsou nástroje společnosti Thales v parametru path (**%nfast_home%\bin** a **%nfast_home%\python\bin**). Zadejte například takto:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Další informace najdete v uživatelské příručce zahrnutý v sadě HSM společnosti Thales.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Krok 2.2: Instalace nástrojů BYOK na odpojeno workstation

Zkopírujte balíček nástrojů BYOK z jednotka USB nebo jiné přenosné úložiště a pak udělejte toto:

1. Extrahujte soubory z balíčku stažený do libovolné složky.
2. V této složce spusťte vcredist_x64.exe.
3. Postupujte podle pokynů na stránce instalovat modul runtime součásti Visual C++ Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Krok 3: Generovat kód

V tomto kroku třetí postupujte podle následujících na odpojeno workstation.

###<a name="step-31-create-a-security-world"></a>Krok 3.1: Vytvoření prezentace zabezpečení

Spusťte příkazový řádek a spuštění programu společnosti Thales nové prezentace.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Tento program vytvoří soubor **Prezentace zabezpečení** v % NFAST_KMDATA%\local\world, který odpovídá C:\ProgramData\nCipher\Key správy aplikací\Místní složce. Můžete použít jiné hodnoty pro kvorum, ale v našem příkladu se zobrazí výzva k zadání tři prázdné karty a PIN kódy pro každý z nich. Potom dvě karty dát plný přístup na světě zabezpečení. Tyto vizitky se stanou má **Správce karty nastavit** pro nové prezentace zabezpečení.

Pak postupujte takto:

- Vytvořte záložní kopii souboru prezentace. Zabezpečení a ochrana souborů světě, správce karty a jejich PIN a, jedna osoba musí mít přístup k více karet.

###<a name="step-32-validate-the-downloaded-package"></a>Krok 3,2: Ověřte stažených balíčku

Tento krok je volitelný, ale doporučuje tak, že můžete ověřit takto:

- Klíč Exchange klíč, který je součástí sadu nástrojů vygenerování z originální HSM společnosti Thales.
- V pravé HSM společnosti Thales byl vytvořen hash světa zabezpečení, která je součástí sadu nástrojů.
- Klíč Exchange klíč je není možné exportovat.

>[AZURE.NOTE]Potvrďte balíček stažený HSM, musíte být připojeni, zapnutá a musí mít zabezpečení prezentace na něm (třeba takové, jaké, který jste právě vytvořili).

Ověřte balíček stažený:

1.  Spusťte skript verifykeypackage.py jednu z následujících kroků podle toho, zeměpisná oblast nebo výskyt Azure ovládání:
    - Severní Americe:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Evropské:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Pro země Asie:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Pro Latinská Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Pro Japonsko:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Pro Austrálii:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - Pro [Státní správu Azure](https://azure.microsoft.com/features/gov/), které využívá instance government USA Azure:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Pro Kanadu:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - U Německo:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Pro Indii:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Software společnosti Thales zahrnuje python na %NFAST_HOME%\python\bin

2.  Potvrďte, že se zobrazí následující příkaz, který označuje úspěšném ověření: **výsledek: Úspěch**

Tento skript ověří řetězec podepisující osoba až kořenový klíč společnosti Thales. Hash tento klíč kořenové je vložen do skript a jeho hodnota by měla být **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Můžete taky ověřit tuto hodnotu samostatně navštivte [web společnosti Thales](http://www.thalesesec.com/).

Teď jste připravení na vytvoření nového klíče.

###<a name="step-33-create-a-new-key"></a>Krok 3.3: Vytvořte nový klíč

Vygenerujte klíč pomocí programu **generatekey** společnosti Thales.

Spusťte tento příkaz Generovat klíč:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Když spustíte tento příkaz, použijte tyto pokyny:

- Parametr *chránit* musí být nastavena na hodnotu **modulu**viz. Tím vytvoříte chráněné heslem modul klíče. Sada nástrojů BYOK nepodporuje chráněné heslem OCS klíče.

- Nahraďte hodnotu *contosokey* **ident** a **plainname** libovolnou hodnotu řetězce. Minimalizace pro správu režijních nákladů a snížit riziko chyby, doporučujeme vám použít stejnou hodnotu pro obě. Hodnota **ident** musí obsahovat pouze čísla typ čáry a malá písmena.

- Pubexp ještě zbývá prázdnou hodnotu (výchozí) v tomto příkladu, ale můžete zadat určité hodnoty. Další informace najdete v dokumentaci společnosti Thales.

Tento příkaz vytvoří soubor tokenizovaný klíč ve složce %NFAST_KMDATA%\local název začíná **key_simple_**, následovaný **ident** zadaného v příkazu. Příklad: **key_simple_contosokey**. Tento soubor obsahuje šifrovaného klíče.

Obecnějším údajům tento tokenizovaný soubor klíč bezpečné místo.

>[AZURE.IMPORTANT] Po později přenášet kód Azure klíč trezoru Microsoft nemůžete exportovat tento klíč zpět tak z něj stal velice důležité zálohovat klíče a zabezpečení světě bezpečné. Požádejte společnosti Thales pokyny a osvědčené postupy pro zálohování kód.

Teď jste připraveni přejít kód Azure klíč trezoru.

##<a name="step-4-prepare-your-key-for-transfer"></a>Krok 4: Příprava klíč pro přenos

V tomto kroku čtvrtou postupujte podle následujících na odpojeno workstation.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Krok 4.1: Vytvoření kopie kód s omezená oprávnění

Omezit oprávnění na váš klíč z příkazového řádku, spusťte jednu z následujících kroků podle toho, zeměpisná oblast nebo výskyt Azure:

- Severní Americe:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Evropské:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Pro země Asie:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Pro Latinská Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Pro Japonsko:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Pro Austrálii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- Pro [Státní správu Azure](https://azure.microsoft.com/features/gov/), které využívá instance government USA Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Pro Kanadu:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- U Německo:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Pro Indii:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Když spustíte tento příkaz, nahraďte *contosokey* stejné hodnoty uvedené v **3.3 krok: Vytvořte nový klíč** od kroku [Generovat kód](#step-3-generate-your-key) .

Zobrazí se výzva pro zapojení karty zabezpečení světě správce.

Po dokončení příkazu, uvidíte **výsledek: Úspěch** a kopii klíče s omezená oprávnění jsou v souboru nazvaného key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Krok 4.2: Kontrola novou kopii klíč

Volitelně můžete spusťte společnosti Thales nástroje potvrďte minimální oprávnění na nový klíč:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Když spustíte tyto příkazy, nahraďte contosokey stejné hodnoty uvedené v **3.3 krok: Vytvořte nový klíč** od kroku [Generovat kód](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Krok 4.3: Šifrování kód pomocí klávesy Exchange klíč společnosti Microsoft

Spusťte některou z následujících příkazů, podle toho, zeměpisná oblast nebo výskyt Azure:

- Severní Americe:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Evropské:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro země Asie:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro Latinská Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro Japonsko:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro Austrálii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro [Státní správu Azure](https://azure.microsoft.com/features/gov/), které využívá instance government USA Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro Kanadu:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- U Německo:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Pro Indii:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Když spustíte tento příkaz, použijte tyto pokyny:

- Nahraďte *contosokey* identifikátoru, který jste použili ke generování klíč v **3.3 krok: Vytvořte nový klíč** od kroku [Generovat kód](#step-3-generate-your-key) .

- Nahraďte *SubscriptionID* ID Azure předplatné, které obsahuje trezoru klíče. Načíst tuto hodnotu dříve, **1.2 krok: získání Azure předplatné ID** od kroku [připravit počítači připojeného k Internetu](#step-1-prepare-your-internet-connected-workstation) .

- Nahraďte *ContosoFirstHSMKey* popisek, který se používá pro váš soubor takto jmenuje výstupu.

Když to úspěšném dokončení zobrazuje **výsledek: Úspěch** a je v současné složce, která má následující název nového souboru: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Krok 4.4: Zkopírujte balíčku klíčové přenos stanici připojeného k Internetu

Použijte jednotka USB nebo jiné přenosné úložiště a zkopírujte ve výstupním souboru v předchozím kroku (KeyTransferPackage ContosoFirstHSMkey.byok) na počítači připojeného k Internetu.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Krok 5: Přenášet kód trezoru klíč Azure

Pro tento poslední krok na workstation připojeného k Internetu rutina [AzureKeyVaultKey přidat](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) k uložení balíčku klíčové přenos, kterou jste zkopírovali z odpojeno workstation k HSM trezoru klíč Azure:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Pokud úspěšného nahrávání se zobrazí zobrazí vlastnosti klíč, který jste právě přidali.


##<a name="next-steps"></a>Další kroky

Teď můžete tento klíč chráněné heslem HSM v trezoru klíče. Další informace naleznete v části **Chcete-li použít zabezpečení modulu hardwaru (HSM)** v tomto kurzu [Začínáme s aplikacemi trezoru klíč Azure](key-vault-get-started.md) .
