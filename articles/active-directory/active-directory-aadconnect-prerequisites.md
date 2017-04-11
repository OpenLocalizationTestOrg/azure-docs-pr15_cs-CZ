<properties
   pageTitle="Azure AD Connect: Předpoklady a hardwaru | Microsoft Azure"
   description="Toto téma popisuje před požadavky, požadavky na hardware pro Azure AD Connect"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Požadavky pro Azure AD připojení
Toto téma popisuje před požadavky, požadavky na hardware pro Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Před instalací Azure AD Connect
Před instalací Azure AD Connect existuje několik věcí, které potřebujete.

### <a name="azure-ad"></a>Azure AD
- Předplatné Azure nebo [Azure zkušební předplatné](https://azure.microsoft.com/pricing/free-trial/). Jenom požadovaných pro přístup k portálu Azure a ne pro používání Azure AD Connect. Pokud používáte prostředí PowerShell nebo Office 365 nemusí Azure předplatné pro použití Azure AD Connect. Pokud máte licenci pro Office 365 můžete taky na portálu Office 365. Placené licence Office 365 taky dostanete do portálu Azure z portálu Office 365.
    - Můžete také pomocí funkce náhledu Azure AD v [Azure portálu](https://portal.azure.com). Toto je portál nevyžaduje licenci Azure.
- [Přidání a ověření domény](active-directory-add-domain.md) budete používat v Azure AD. Pokud budete chtít použít contoso.com pro uživatele a pak zkontrolujte ověřil tuto doménu a pouze nepoužíváte výchozí domény contoso.onmicrosoft.com.
- Azure AD klienta umožňuje výchozí 50k objekty. Při ověření vaší domény limit zvětšené tak, aby 300 k objektům. Pokud potřebujete ještě další objekty v Azure AD musíte otevřít případ podpory pro nemá omezení lepší te000130432 můžete dál. Pokud budete potřebovat víc než 500 objektů k musíte licenci, jako je Office 365, Azure AD základní, Azure AD Premium nebo Enterprise mobilitu sadu.

### <a name="prepare-your-on-premises-data"></a>Příprava místních dat
- Zkontrolujte [volitelné synchronizace funkcí, u kterých můžete povolit v Azure AD](active-directory-aadconnectsyncservice-features.md) a vyhodnocení funkcí, které byste měli povolit.

### <a name="on-premises-servers-and-environment"></a>Místní servery a prostředí
- AD schématu verze a struktury úroveň funkčnosti musí být systému Windows Server 2003 nebo novější. Řadiče domény můžete spustit všechny verze, dokud schématem a struktury úrovně požadavků.
- Pokud budete chtít použít funkci **heslo zpětným** řadiče domény musí být v systému Windows Server 2008 (plus pár nejnovější SP) nebo novější. Pokud jsou vaše řadiče domény na 2008 (pre R2) musí také použít [hotfix KB2386717](http://support.microsoft.com/kb/2386717).
- Řadiče domény používaný Azure AD musí být zapisovatelný. Není podporované použití jen pro čtení (řadiče domény jen pro čtení) a Azure AD Connect není postupujte podle jakékoli zápisu přesměrování.
- Azure AD Connect nejde nainstalovat na Small Business Server nebo Windows Server Essentials. Serveru musí používat Windows Server standardní nebo vyšší.
- Server Azure AD Connect musí mít úplné grafické nainstalovaný. Není podporovaná nainstalovat na server core.
- Azure AD Connect musí být nainstalovaný v systému Windows Server 2008 nebo novějším. Tento server může být řadiče domény nebo serveru člena používáte Expresní nastavení. Pokud používáte vlastní nastavení, serveru lze použít i samostatné a nemá být připojen k doméně.
- Pokud instalujete Azure AD Connect v systému Windows Server 2008, zkontrolujte, že použít nejnovější hotfix z webu Windows Update. Instalace není možné spustit bez opravy serverem.
- Pokud chcete použít funkci **Synchronizace hesel**, server Azure AD Connect musí být v systému Windows Server 2008 R2 SP1 nebo novější.
- Server Azure AD Connect musí mít [.NET Framework 4.5.1](#component-prerequisites) nebo novější a [Microsoft prostředí PowerShell 3.0](#component-prerequisites) nebo novější.
- Pokud nasazení Active Directory Federation Services servery služby AD FS a proxy serveru webové aplikace nainstalovanými musí být Windows serveru 2012 R2 nebo novější. [Vzdálená správa systému Windows](#windows-remote-management) musí být povolený na těchto serverech pro vzdálené instalace.
- Pokud nasazuje Active Directory Federation Services, musíte [Certifikáty SSL](#ssl-certificate-requirements).
- Pokud nasazuje Active Directory Federation Services, budete muset konfigurace [překlad](#name-resolution-for-federation-servers).
- Azure AD Connect vyžaduje databáze SQL serveru k ukládání dat identity. Ve výchozím nastavení je nainstalovaná SQL Server 2012 Express LocalDB (light verze SQL serveru Express) a vytvoření účtu služby pro službu v místním počítači. SQL Server Express má omezení velikosti 10GB, který umožňuje spravovat přibližně 100 000 objekty. Pokud budete potřebovat pro správu vyšší objemu objektů adresáře, budete muset přejděte na Průvodce instalací jiné instalace SQL serveru.
- Pokud používáte samostatné SQL serveru, pak použijte tyto požadavky:
    - Azure AD Connect podporuje všechny provedeních Microsoft SQL Server ze serveru SQL Server 2008 (plus pár SP4) na rok 2014 SQL serveru. Databázi Microsoft Azure SQL je jako databáze **nejsou podporované** .
    - Je nutné použít velká a malá písmena řazení SQL. Toto jsou označeny \_CI_ v jeho jméno. Je **není podporované** použití malá a velká písmena řazení označenými \_CS_ v jeho jméno.
    - Může mít jenom jeden sync engine za instance databázového. Je do instance databázového nasdílet FIM/MIM synchronizace, DirSync nebo Azure AD Sync **nejsou podporované** .

### <a name="accounts"></a>Účty
- Účet Azure AD globálního správce pro adresáři Azure AD, které chcete integrovat. Musí být **školním účtem nebo organizaci účtu** a nesmí být **účtu Microsoft**.
- Pokud používáte Expresní nastavení nebo upgrade z DirSync, musí mít účet podnikový správce pro místní služby Active Directory.
- [Účty ve službě Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) použijete instalační cesta vlastní nastavení.

### <a name="azure-ad-connect-server-configuration"></a>Konfigurace server Azure AD Connect
- Jestliže globální správci MFA povoleno, musí být URL **https://secure.aadcdn.microsoftonline-p.com** v seznamu důvěryhodných webů. Zobrazí se výzva Pokud není přidali dříve, než se zobrazí výzva k ověřovací kód MFA to přidat do seznamu důvěryhodných webů. Internet Explorer slouží k přidání mezi důvěryhodné servery.

### <a name="connectivity"></a>Připojení
- Server Azure AD Connect potřebuje služeb překládá názvy DNS k síti intranet a internet. Kontrola jmen i na adresářová služba Active Directory, jakož i koncové body Azure AD musíte mít serveru DNS.
- Pokud máte v síti Intranet bránách firewall a musíte otevřít porty mezi servery Azure AD Connect a řadiče domény, pak v tématu [Azure AD Connect porty](active-directory-aadconnect-ports.md) Další informace.
- Pokud vaše proxy nebo brány firewall limit adresy URL můžete k nim získat přístup, pak adresy URL uvedených v [oblasti Office 365 URL a IP adresu](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) musí být otevřena.
    - Pokud používáte v cloudu společnosti Microsoft ve Německo nebo cloudového Microsoft Azure pro státní správu, pak naleznete v tématu [Co byste měli zvážit Azure AD Connect synchronizace služby instance](active-directory-aadconnect-instances.md) adresy URL.
- Azure AD Connect je ve výchozím nastavení komunikace s Azure AD pomocí TLS 1.0. Můžete změnit to TLS 1.2 postupu v části [Povolit TLS 1.2 pro Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
- Pokud používáte proxy server pro odchozí připojení k Internetu, použité toto nastavení v souboru **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** musí být přidán u Průvodce instalací a Azure AD Connect synchronizace, které mají být k připojení k Internetu a Azure AD. Tento text musí být zadán v dolní části soubor. V tomto kódu &lt;PROXYADRESS&gt; představuje skutečný proxy IP adresu nebo název hostitele.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Pokud proxy server vyžaduje ověření, [účet služby](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) musí být umístěné v doméně a musíte použít vlastní nastavení Instalační cesta k zadání [vlastního účtu služby](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components). Budete potřebovat různé změnu machine.config. S tímto změn v souboru machine.config Průvodce instalací a sync engine odpovězte požadavky ověřování proxy serveru. Ve všech Průvodce instalací používají stránky, s výjimkou stránce **Configure** přihlášený v přihlašovací údaje uživatele. Na stránce **Konfigurovat** na konci Průvodce instalací kontextu je po přechodu na jiný [účet služby](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) , který byl vytvořen vy. V části machine.config by měl vypadat takto.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Další informace o [výchozí proxy prvek](https://msdn.microsoft.com/library/kd3cf2ex.aspx)najdete v článku MSDN.

Pokud máte problémy s připojením, přečtěte si téma [Poradce při potížích s připojením](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Další
- Volitelné: Test uživatelského účtu Ověřit synchronizaci.

## <a name="component-prerequisites"></a>Součásti požadavky

### <a name="powershell-and-net-framework"></a>Prostředí PowerShell a .net Framework
Azure AD Connect záleží na tom Microsoft Powershellu a .NET Framework 4.5.1. Potřebujete tato verze nebo jeho novější verzí na serveru nainstalovány. V závislosti na verzi systému Windows Server postupujte takto:

- Windows Server 2012R2
  - Ve výchozím nastavení je nainstalovaný Microsoft Powershellu, není nutná žádná akce.
  - .NET framework 4.5.1 a novějších verzích nabídnuta přes Windows Update. Zkontrolujte, že jste nainstalovali nejnovější aktualizace Windows Server v Ovládacích panelech.
- Windows Server 2008 R2 a Windows Server 2012
  - Nejnovější verzi aplikace Microsoft Powershellu najdete v **systému Windows Management Framework 4.0**dostupné na [Webu služby Stažení softwaru](http://www.microsoft.com/downloads).
  - .NET framework 4.5.1 a novějších verzích jsou dostupné na [Webu služby Stažení softwaru](http://www.microsoft.com/downloads).
- Windows Server 2008
  - Nejnovější podporovanou verzi Powershellu najdete v **systému Windows Management Framework 3.0**dostupné na [Webu služby Stažení softwaru](http://www.microsoft.com/downloads).
 - .NET framework 4.5.1 a novějších verzích jsou dostupné na [Webu služby Stažení softwaru](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Povolit TLS 1.2 Azure AD Connect
Azure AD Connect používá TLS 1.0 ve výchozím nastavení pro šifrování komunikace mezi sync engine serveru a Azure AD. Změníte konfigurace aplikací .net pro použití TLS 1.2 ve výchozím nastavení na serveru. Další informace o TLS 1.2 najdete v [Microsoft zabezpečení poradní 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 nelze povolit v systému Windows Server 2008. Budete potřebovat Windows Server 2008 R2 nebo novější. Zkontrolujte, budete mít .net 4.5.1 hotfix nainstalovaný operační systém najdete v článku [Microsoft zabezpečení poradní 2960358](https://technet.microsoft.com/security/advisory/2960358). Můžete mít takto nebo novější verzi na serveru nainstalovány? už.
2. Pokud používáte Windows Server 2008 R2, ujistěte se, zda že je povolen TLS 1.2. Na serveru Windows Server 2012 a novějších verzích TLS 1.2 by měl být zapnuto.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. U všech operačních systémech nastavte tento klíč registru a restartujte server.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Pokud chcete povolit TLS 1.2 mezi serverem sync engine a vzdálený Server SQL, pak zkontrolujte, jestli že máte požadovaných verze nainstalované pro [TLS 1.2 podpory pro Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Požadavky pro federaci instalace a konfigurace

### <a name="windows-remote-management"></a>Vzdálená správa systému Windows
Pokud chcete použít Azure AD Connect nasazení Active Directory Federation Services nebo proxy serveru webové aplikace, zkontrolujte požadavky na níže zajistit připojení a proběhne úspěšně konfigurace:

- Pokud je server cílové domény připojen, povolena vzdálené spravovaných systému Windows
    - V PSH příkaz okno se zvýšenými oprávněními použijte příkaz`Enable-PSRemoting –force`
- Pokud je server cílové mimo doménu připojeném počítači WAP, je několik další požadavky
    - Na cílovém počítači (WAP počítač):
         - Zajistit, aby winrm (Vzdálená správa systému Windows / WinRM) se službou prostřednictvím modulu snap-in služby
         - V PSH příkaz okno se zvýšenými oprávněními použijte příkaz`Enable-PSRemoting –force`
    - Na počítači, ve kterém je spuštěn Průvodce (Pokud je počítač cílové bez-spojených nebo nedůvěryhodných domény):
        - V PSH příkaz okno se zvýšenými oprávněními použijte příkaz`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - Ve Správci serveru:
             - Přidat DMZ WAP hostitele do fondu počítačů (Správce serveru -> Spravovat -> Přidat servery... použijte kartu DNS)
             - Karta správce serveru všechny servery: klikněte pravým tlačítkem myši WAP serveru a zvolte spravovat jako..., zadejte pověření místního (nikoli domény) na počítači WAP
             - Ověřte vzdálené PSH připojení na kartě servery všechny správce serveru: klikněte pravým tlačítkem myši WAP serveru a zvolte prostředí Windows PowerShell.  Vzdálenou relaci PSH otvírat zajistit navázáním vzdálený PowerShell relace.

### <a name="ssl-certificate-requirements"></a>Požadavky na certifikát SSL
**Důležité:** důrazně doporučujeme používat stejný certifikát SSL ve všech uzlů farmě služby AD FS a také všechny servery proxy serveru webové aplikace.

- Certifikát musí být X509 certifikát.
- Pomocí certifikátu podepsaného svým držitelem na federační servery v testovacím prostředí. Však provozním prostředí, doporučujeme získat certifikát veřejné certifikační autority.
    - Používáte-li certifikát, který není veřejně důvěryhodné, zajistěte, aby byl nainstalován na všech serverech proxy serveru webové aplikace certifikát důvěryhodných místního serveru a na všech serverech federace
- Identita certifikát musí shodovat s názvem služby federace (například sts.contoso.com).
    - Identita je buď předmět alternativní (SAN) příponu názvu Název_dns typ nebo, pokud nejsou žádné položky SAN, název subjektu zadaný jako běžný název.  
    - Více položek SAN existovat v certifikátu za předpokladu, že jeden z nich shoduje s názvem služby federace.
    - Pokud se chystáte používat pracovišti připojení, je potřeba pro hodnotu Další SAN **enterpriseregistration.** Následuje přípony hlavní název uživatele (UPN) pro vaši organizaci, například **enterpriseregistration.contoso.com**.
- Certifikáty na základě CryptoAPI další klávesy generování (CNG) a úložiště klíčů nejsou podporované. To znamená, že je nutné použít certifikát na základě CSP (zprostředkovatele šifrování) a nikoli KSP (Zprostředkovatel úložiště klíčů).
- Zástupné certifikáty jsou podporovány.

### <a name="name-resolution-for-federation-servers"></a>Překlad pro federační servery
- Nastavte záznamy DNS pro služby AD FS federace název služby (například sts.contoso.com) pro intranet (interní server DNS) a extranetové (veřejné DNS prostřednictvím svého doménového registrátora). Intranet DNS záznamu zajistit, že používáte A záznamy a ne CNAME záznamy. Toto je vyžaduje ověřování windows na správnou z počítače spojených domény.
- Pokud nasazujete víc server AD FS nebo webové aplikace Proxy server, přesvědčte se, že jste nakonfigurovali Vyrovnávání zatížení a, že záznamy DNS pro název služby federace AD FS (například sts.contoso.com) přejděte na Vyrovnávání zatížení.
- Ověřování systému windows pro práci v Internet Exploreru v síti intranet aplikace prohlížeče zajistit, že název služby federace AD FS (například sts.contoso.com) přidá do zóny intranet v aplikaci Internet Explorer. To můžete řídit prostřednictvím zásad skupiny a nasazena do všech počítačů domény připojen.

## <a name="azure-ad-connect-supporting-components"></a>Podpora součástí Azure AD Connect
Toto je seznam součástí, které jsou instalovány Azure AD Connect na serveru, kde je nainstalovaný Azure AD Connect. Tady je pro základní Expresní instalace.  Pokud se rozhodnete sdělit nám jiný SQL Server na stránce instalace synchronizace služby, není místně nainstalovaný SQL Express LocalDB.

- Azure AD Connect stavu
- Microsoft Online Services Pomocník pro přihlášení pro IT profesionály (nainstalovanou žádná závislost na něm)
- Microsoft SQL Server 2012 příkazového řádku nástroje
- Microsoft SQL Server 2012 Express LocalDB
- Microsoft SQL Server 2012 Native Client
- Microsoft Visual C++ 2013 redistribuci balíčku

## <a name="hardware-requirements-for-azure-ad-connect"></a>Požadavky na hardware pro Azure AD Connect
Následující tabulka ukazuje minimální požadavky na počítači synchronizační Azure AD Connect.

| Počet objektů ve službě Active Directory | PROCESOR | Paměti | Velikost pevného disku |
| ------------------------------------- | --- | ------ | --------------- |
| Méně než 10 000 | 1,6 GHz | 4 GB | 70 GB |
| 10 000 – 50000 | 1,6 GHz | 4 GB | 70 GB |
| 50000 – 100,000 | 1,6 GHz | 16 GB | 100 GB |
| Pro 100 000 nebo více objektů plnou verzi serveru SQL Server požaduje|  |  |  |
| 100,000 – 300 000 | 1,6 GHz | 32 GB | 300 GB |
| než 300 000 – 600,000 | 1,6 GHz | 32 GB | 450 GB |
| Více než 600,000 | 1,6 GHz | 32 GB | 500 GB |

Minimální požadavky na počítači se systémem AD FS nebo webové aplikace servery je následující:

- Procesor: Základní dvou 1,6 GHz nebo vyšší
- PAMĚTI: 2GB nebo vyšší
- Azure OM: Konfigurace A2 a novějších verzích

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
