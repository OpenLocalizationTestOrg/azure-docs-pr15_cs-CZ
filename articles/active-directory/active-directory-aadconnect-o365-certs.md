<properties
    pageTitle="Certifikát obnovení pokyny pro uživatele Office 365 a Azure Active Directory | Microsoft Azure"
    description="Tento článek popisuje uživatelům Office 365 k řešení problémů s e-mailů, které informovat o obnovení certifikátu."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Obnovení pro Office 365 a Azure Active Directory federation certifikátů

##<a name="overview"></a>Základní informace

Pro úspěšné federaci mezi Azure Active Directory (Azure AD) a Active Directory Federation Services (AD FS) se musí shodovat certifikáty podepsat tokenů zabezpečení Azure AD pomocí služby AD FS konfigurovat Azure AD. Všechny neshodu může vést k přerušení zabezpečení. Azure AD zaručuje, že tyto informace bude k dispozici synchronní při nasazujete službu AD FS a proxy serveru webové aplikace (přístup pomocí protokolu extranetové).

Tento článek poskytuje další informace ke správě tokenu podepisování certifikáty a jejich synchronizace s Azure AD v těchto případech:

* Nejsou nasazení proxy serveru webové aplikace, a proto není k dispozici v síti extranet federace metadata.
* Výchozí konfigurace služby AD FS nepoužíváte token podepisování certifikáty.
* Používáte zprostředkovatele identit třetích stran.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Výchozí konfigurace služby AD FS token podepisování certifikátů

Token přihlašování a token dešifrování certifikáty jsou obvykle podepsaný svým držitelem certifikáty a jsou vhodné pro jeden rok. Ve výchozím nastavení obsahuje služba AD FS automatického obnovení proces nazývaný **AutoCertificateRollover**. Pokud používáte službu AD FS 2.0 nebo novější, Office 365 a Azure AD automaticky aktualizované certifikátu před vypršením jeho platnosti.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Obnovení oznámení z portálu Office 365 nebo e-mailu

>[AZURE.NOTE] Pokud jste dostali e-mailu nebo portálu oznámení s dotazem, obnovíte certifikát pro Office, najdete v článku [Správa se změní na token podepisování certifikátů](#managecerts) na zaškrtněte, pokud budete muset provádět žádné akce. Microsoft známa možné problém, který může vést k oznámení publikacích odesílaného, i když není nutná žádná akce.

Azure AD pokusí ke sledování metadata federaci a aktualizace tokenu podepisování certifikáty podle tato metadata. 30 dní před vypršením platnosti token podepisování certifikáty, Azure AD zkontroluje, pokud nové certifikáty jsou k dispozici po dotazování federace metadata.

* Pokud můžete úspěšně hlasování metadata federace a získat nové certifikáty, bez e-mailového oznámení nebo upozornění na portálu Office 365 vystaven uživateli.
* Pokud nemůžete načíst nový token podepisování certifikáty, buď protože metadata federace není k dispozici nebo při přechodu myší automatické certifikát není povolená, Azure AD problémy e-mailové oznámení a upozornění na portálu Office 365.

![Upozornění na portálu Office 365](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Pokud používáte službu AD FS zajistit nepřerušený, ověřte, že serverech tyto aktualizace tak, že není docházet k chybám ověření známé problémy. To snižuje známé problémy služby AD FS proxy serveru pro tuto prodloužení a budoucí obnovování:
>
>Server 2012 R2 - [systému Windows Server květen 2014 kumulativní](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 a 2012 – [ověřování proxy server selže v systému Windows Server 2012 nebo Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Zaškrtněte, pokud certifikáty třeba aktualizovat<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Krok 1: Kontrola stavu AutoCertificateRollover

Na server služby AD FS otevřete Powershellu. Zkontrolujte, zda je hodnota AutoCertificateRollover nastavena na hodnotu True.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Pokud používáte službu AD FS 2.0, prvního spuštění Microsoft.Adfs.Powershell Pssnapin přidat.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Krok 2: Zkontrolujte, že je synchronizovaná AD FS a Azure AD

Na server služby AD FS otevřete příkazovém řádku prostředí PowerShell Azure AD a připojte k Azure AD.

>[AZURE.NOTE] Azure AD PowerShell můžete stáhnout [tady](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Zaškrtněte políčko certifikáty konfigurovat AD FS a Azure AD důvěřovat vlastnosti pro zadanou doménu.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Pokud odpovídá thumbprints v obou výstupy se nesynchronizují s Azure AD vlastních certifikátů.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Krok 3: Ověřte, zda certifikát blíží se vypršení platnosti

Do výstupu Get-MsolFederationProperty nebo Get-AdfsCertificate zkontrolujte datum v části "Není po." Pokud data je menší než 30 dní jinam, byste měli vzít akce.

| AutoCertificateRollover | Synchronizace s Azure AD certifikátů | Federace metadat veřejně přístupný | Platnost | Akce |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Ano | Ano | Ano | - | Není nutná žádná akce. V tématu [prodloužit token automaticky podpisového certifikátu](#autorenew). |
| Ano | Ne  | - | Menší než 15 dní | Obnovení okamžitě. V tématu [prodloužit token ručně podpisového certifikátu](#manualrenew). |
| Ne | - | - | Menší než 30 dní. | Obnovení okamžitě. V tématu [prodloužit token ručně podpisového certifikátu](#manualrenew). |

\[-] Nezáleží.

## Obnovení automaticky podpisový certifikát tokenů (doporučeno)<a name="autorenew"></a>

Nemusíte provádět jakékoli kroky ručně při splnění obou těchto podmínek:
- Nasadili Proxy webové aplikace, které umožní přístup metadata federace z extranetové.
- Používáte službu AD FS výchozí konfigurace (AutoCertificateRollover je povoleno).

Zkontrolujte podle následujících pokynů potvrďte, že certifikát automaticky aktualizován.

**1. na AD FS AutoCertificateRollover nastavena na hodnotu True.** To znamená služby AD FS automaticky vygeneruje nové tokenu přihlašování a tokenu dešifrování certifikáty, před starého fáze platnosti.

**2. metadata federace AD FS veřejně přístupný.** Zkontrolujte, zda je federace metadat veřejně přístupný tak, že přejdete na následující adresu URL z počítače na veřejné Internetu (mimo podnikovou síť):


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

kde `(your_FS_name) `nahrazen název hostitele služby federace vaše organizace používá, například fs.contoso.com.  Pokud jste ověřit obě tato nastavení povede, nemáte něco dalšího dělat.  

Příklad: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Obnovení ručně podpisový certifikát tokenů<a name="manualrenew"></a>

Můžete prodloužit token podepisování certifikáty ručně. Například v následujících scénářích lépe vašem případě mohlo fungovat ruční obnovení:
* Token podepisování certifikáty jsou automatickým podpisem certifikáty. Nejběžnější důvodem je, že vaše organizace má na starosti AD FS certifikáty zapsané od organizace certifikační autority.
* Zabezpečení sítě neumožňuje metadata federačního veřejně přístupný.

V těchto případech pokaždé, když aktualizujete token podepisování certifikáty, je nutné taky aktualizovat domény Office 365 pomocí příkazu Powershellu MsolFederatedDomain aktualizace.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Krok 1: Zkontrolujte, že služba AD FS nainstalovaný nový token podepisování certifikátů

**Jiné než výchozí konfigurace**

Pokud používáte jiného než výchozího konfiguraci služby AD FS (kde **AutoCertificateRollover** je nastavena na **hodnotu False**), pravděpodobně pomocí vlastních certifikátů (vlastním nepodepsané). Další informace o tom, jak prodloužit tokenu služba AD FS podepisování certifikáty najdete v článku [pokyny pro zákazníky, ne pomocí služby AD FS automatické přihlášení certifikáty](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federace metadat není veřejně dostupný**

Na druhé straně Pokud **AutoCertificateRollover** nastavena na **hodnotu True**, ale metadata federace není veřejně přístupný, nejdřív zkontrolujte, že byly vytvořeny nové token podepisování certifikátů službou AD FS. Zkontrolujte, jestli že máte nový token podepisování certifikáty podle provedením následujících kroků:

1. Ověřte, že jste přihlášeni do primární server služby AD FS.
2. Zkontrolujte aktuální podpisový certifikáty ve službě AD FS otevřením okna příkazového prostředí PowerShell a tohoto příkazu:

    PS C:\>podepisování token Get-ADFSCertificate – CertificateType

    >[AZURE.NOTE] Pokud používáte službu AD FS 2.0, měli byste nejdřív spustit přidat Pssnapin Microsoft.Adfs.Powershell.

3. Podívejte se na příkaz výstup všech certifikátů uvedené. Pokud služby AD FS vygenerovala nový certifikát, měli byste zkontrolovat dva certifikáty do výstupu: které **IsPrimary** má hodnotu **True** a datem **neplatí po** reprodukujte do 5 dnů a pro kterou **IsPrimary** hodnotu **False** a **neplatí po** reprodukujte o budoucí rok.

4. Pokud se zobrazí pouze jeden certifikát a **neplatí po** datum do 5 dní, budete muset generovat nový certifikát.

5. Pro vytvoření nového certifikátu, spusťte následující příkaz na příkazovém řádku prostředí PowerShell: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Zkontrolujte aktualizace opětovným spuštěním následujícího příkazu: PS C:\>podepisování token Get-ADFSCertificate – CertificateType

Dva certifikáty měl seznam obsahovat nyní, jedna z nich obsahuje **neplatí po** datu přibližně jeden rok v budoucnu a jehož **IsPrimary** hodnotu **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Krok 2: Aktualizace nový token podepisování certifikáty pro zabezpečení Office 365

Aktualizace Office 365 tokenu nový podpis certifikáty určené důvěřovat, následujícím způsobem.

1.  Otevřete modul Microsoft Azure Active Directory pro Windows PowerShell.
2.  Spuštění $cred = Get-pověření. Pokud tuto rutinu zobrazí výzvu k zadání přihlašovacích údajů, zadejte přihlašovací údaje účtu vaše cloudové služby správce.
3.  Spuštění připojit MsolService – pověření $cred. Tato rutina připojí ke cloudové službě. Vytvoření kontextu, který se připojuje ke službě cloud je potřeba před spuštěním další rutin kliknutím na nástroj nainstalovaný.
4.  Pokud tyto příkazy jsou spuštěné v počítači, který není primární federačního serveru AD FS, spusťte Set MSOLAdfscontext-počítač <AD FS primary server>, kde <AD FS primary server> je vnitřní úplný název domény je primární server služby AD FS. Tato rutina vytvoří kontextu, který se připojuje k AD FS.
5.  Spuštění aktualizace MSOLFederatedDomain – název_domény <domain>. Tato rutina aktualizace nastavení ze služby AD FS do cloudové služby a konfiguruje vztah důvěryhodnosti mezi těmito dvěma.


>[AZURE.NOTE] Pokud potřebujete podporu víc domén nejvyšší úrovně, například contoso.com a fabrikam.com, můžete přepínačem **SupportMultipleDomain** pomocí libovolné rutiny. Další informace najdete v tématu [podpory víc domén nejvyšší úrovně](active-directory-aadconnect-multiple-domains.md).

## Opravte zabezpečení Azure AD pomocí Azure AD Connect<a name="connectrenew"></a>

Pokud jste nakonfigurovali farmě služby AD FS a zabezpečení Azure AD pomocí Azure AD Connect, můžete Azure AD Connect ke zjištění potřebnosti provádět žádnou akci tokenu podepisování certifikáty. Pokud potřebujete obnovení certifikáty, můžete to udělat Azure AD Connect.

Další informace najdete v tématu [oprava důvěřovat](./active-directory-aadconnect-federation-management.md#repairing-the-trust).
