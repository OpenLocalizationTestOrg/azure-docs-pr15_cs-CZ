<properties
    pageTitle="MFA serveru v systému Windows Server 2012 R2 AD FS | Microsoft Azure"
    description="Tento článek popisuje, jak začít s Azure Vícefaktorové ověřování a služby AD FS ve Windows serveru 2012 R2."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Zabezpečení cloudu a místní prostředků Azure Multi-Factor ověřování serveru pomocí služby AD FS ve Windows serveru 2012 R2

Pokud používáte Active Directory Federation Services (AD FS) a chcete zabezpečené cloudu nebo místních zdrojů, můžete nakonfigurovat Azure Multi-Factor ověřování serveru pro práci se službou AD FS. Konfigurace spustí dvoustupňového ověření pro koncové body vysoké hodnoty.

V tomto článku probereme tato pomocí služby AD FS ve Windows serveru 2012 R2 Azure Multi-Factor ověřování serveru. Podrobnosti přečtěte si o tom, jak [zabezpečené cloudu a místní zdroje pomocí Azure Multi-Factor ověřování serveru se službou AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Zabezpečení systému Windows Server 2012 R2 AD FS s Azure Vícefaktorové ověřování serveru

Při instalaci Azure Multi-Factor Authentication serveru máte následující možnosti:

- Místně nainstalovat na stejný server služby AD FS Azure Multi-Factor ověřování serveru
- Instalace adaptér Azure Vícefaktorové ověřování místně na server služby AD FS a nainstalujte Multi-Factor ověřování serveru na jiný počítač

Než začnete, mějte na paměti následující informace:

- Není nutné nainstalovat Azure Multi-Factor Authentication Server na server služby AD FS. Vícefaktorové ověřování adaptér však nutné nainstalovat na AD FS Windows Server 2012 R2, na kterém běží služba AD FS. Server můžete nainstalovat na jiný počítač, pokud je podporovanou verzi a nainstalujete adaptér AD FS samostatně federačního serveru AD FS. Postupujte podle následujících pokynů se dozvíte, jak nainstalovat adaptér samostatně.

- Pokud byl vytvořen adaptér MFA Server AD FS, byla předpokládá, že služba AD FS může předat název řídicí strany adaptér. Potom předávající název strany může jako název aplikace. Však to vypadá pozor, abyste v případě. Pokud vaše organizace používá textovou zprávu nebo mobilní aplikaci metody ověřování, řetězce společnosti nastavení obsahovat zástupného symbolu, < $*název_aplikace*>. Tento zástupný symbol není automaticky nahradit, pokud používáte službu AD FS adaptér. Doporučujeme odebrat zástupného symbolu z řetězců vhodná při zabezpečené AD FS.

- Účet, který používáte pro přihlášení musí mít oprávněními pro vytváření skupin zabezpečení ve službě Active Directory.

- Průvodce instalací adaptér Vícefaktorové ověřování AD FS vytvoří skupinu zabezpečení s názvem PhoneFactor správce v instanci služby Active Directory. Potom přidá účet služby AD FS federace služby do této skupiny. Doporučujeme, abyste je řadiče domény ověřit, že skupiny správců PhoneFactor skutečně vytvořili a že účet služby AD FS členové této skupiny. V případě potřeby účet služby AD FS ručně přidáte do skupiny správců PhoneFactor řadiče domény.

- Informace o instalaci SDK webové služby pomocí portálu uživatel, přečtěte si o [nasazení portálu uživatele pro Server Azure Multi-Factor Authentication.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Instalace Azure Multi-Factor Authentication serveru místně na server služby AD FS

1. Stáhněte a nainstalujte Azure Multi-Factor ověřování serveru na server služby AD FS. Informace o instalaci přečtěte si o [začínáte pracovat s Azure Multi-Factor ověřování serveru](multi-factor-authentication-get-started-server.md).
2. V konzole Správa Azure Multi-Factor ověřování serveru klikněte na ikonu **služby AD FS** a pak vyberte požadované možnosti **přihlášení uživatele povolit** a **Povolit uživatelům vyberte způsob**.
3. Vyberte další možnosti, které chcete zadat pro vaši organizaci.
4. Klikněte na **instalovat AD FS adaptér**.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Pokud se zobrazí okno služby Active Directory, to znamená dvě věci. V počítači připojeném do domény a konfiguraci služby Active Directory k zabezpečení komunikace mezi adaptér AD FS a služba Vícefaktorové ověřování neúplná. Klikněte na tlačítko **Další** pro automatické dokončení této konfiguraci nebo vyberte **Přeskočit Automatická konfigurace služby Active Directory a konfigurovat nastavení ručně** zaškrtněte políčko a potom na tlačítko **Další**.
6. Pokud se zobrazí windows místní skupiny, to znamená dvě věci. Počítač připojen k doméně a konfiguraci místního skupiny k zabezpečení komunikace mezi adaptér AD FS a služba Vícefaktorové ověřování je neúplné. Klikněte na tlačítko **Další** pro automatické dokončení této konfiguraci nebo vyberte **Přeskočit Automatická konfigurace místní skupiny a konfigurovat nastavení ručně** zaškrtněte políčko a potom na tlačítko **Další**.
7. V Průvodci instalací klikněte na **Další**. Azure Multi-Factor ověřování serveru vytvoří skupiny správců PhoneFactor a přidá do skupiny správců PhoneFactor účet služby AD FS.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Na stránce **Spustit instalační služby systému** klikněte na **Další**.
9. V Vícefaktorové ověřování AD FS adaptér instalační služba systému klikněte na **Další**.
10. Po dokončení instalace klikněte na tlačítko **Zavřít** .
11. Po instalaci adaptér Zaregistrujte ho se službou AD FS. Otevřete Windows PowerShell a spusťte tento příkaz:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Můžete adaptér nově registrovaných upravte zásadu globální ověřování ve službě AD FS. V konzole pro správu služby AD FS přejděte na uzel **Zásad ověřování** . V části **Vícefaktorové ověřování** klikněte na odkaz **Upravit** vedle části **Globální nastavení** . V okně **Upravit zásadu globální ověřování** vyberte **Vícefaktorové ověřování** jako další ověřování a klikněte na **OK**. Adaptér je registrovaná jako WindowsAzureMultiFactorAuthentication. Restartujte službu AD FS pro registraci se projeví.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

V tomto okamžiku Multi-Factor Authentication Server je nastavena na být poskytovatele další ověřování pomocí služby AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Instalaci samostatného instanci služby AD FS adaptér pomocí webové služby SDK
1. Instalace SDK webové služby na serveru, na kterém běží Server Multi-Factor Authentication.
2. Zkopírujte následující soubory z \Program Files\Multi-adresář faktor ověřování serveru Server, podle kterého chcete nainstalovat na AD FS adaptér:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Zrušení registrace MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Spuštění instalačního souboru MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. V instalační služby systému Vícefaktorové ověřování ADFS adaptér klepnutím na tlačítko **Další** zahájíte instalaci.
5. Po dokončení instalace klikněte na tlačítko **Zavřít** .

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Upravte soubor MultiFactorAuthenticationAdfsAdapter.config

Tyto kroky k jeho úpravám MultiFactorAuthenticationAdfsAdapter.config:

1. Uzel **UseWebServiceSdk** nastavena na **hodnotu true**.  
2. Nastavte hodnotu pro **WebServiceSdkUrl** na adresu URL služby SDK Multi-Factor Authentication Web. Příklad: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, kde certificatename je název vašeho certifikátu.  
3. Úpravou registru MultiFactorAuthenticationAdfsAdapter.ps1 skriptu přidáním *- ConfigurationFilePath &lt;cestu&gt; * na konec `Register-AdfsAuthenticationProvider` příkaz, kde * &lt;cestu&gt; * je úplnou cestu k souboru MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Konfigurace webové služby SDK pomocí jména a hesla

Konfigurace webové služby SDK dvěma způsoby. První z nich je uživatelské jméno a heslo, druhý certifikátem klienta. Tyto kroky pro první možnost nebo přejděte rovnou vteřin.  

1. Nastavte hodnotu pro **WebServiceSdkUsername** na účet, který je členem skupiny zabezpečení PhoneFactor správců. Použití &lt;domény&gt;& #92; &lt;uživatelské jméno&gt; formát.  
2. Nastavte hodnotu pro **WebServiceSdkPassword** na příslušný heslo.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Konfigurace služby SDK Web s certifikátem klienta

Pokud nechcete používat uživatelské jméno a heslo, postupujte ke konfiguraci webové služby SDK certifikát klienta.

1. Získejte certifikát klienta od certifikační autority na serveru, na kterém běží SDK webové služby. Zjistěte, jak [získat certifikátů](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importujte certifikátu klienta do místního počítače osobní úložiště certifikátů na serveru, na kterém běží SDK webové služby. Ujistěte se certifikát veřejného certifikační autorita ve úložiště certifikátů důvěryhodné kořenových certifikátů.  
3. Exportujte do souboru .pfx veřejné a privátní klíče certifikát klienta.  
4. Veřejný klíč ve formátu ve formátu Base 64 exportujte do souboru .cer.  
5. Ve Správci serveru ověřte, že je webový Server (IIS) \Web Server\Security\IIS ověřování mapování klientských certifikátů nainstalována. Pokud není nainstalovaný, vyberte **Přidat rolí a funkcí** přidáte tuto funkci.  
6. Ve Správci služby IIS poklikejte **Konfigurace editoru** webu, který obsahuje virtuální adresář SDK webové služby. Je důležité akce na úrovni webu a ne na úrovni virtuální adresář.  
7. Přejděte do části **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Sada povolený na **hodnotu true**.  
9. OneToOneCertificateMappingsEnabled nastavena na **hodnotu true**.  
10. Klikněte na tlačítko **…** vedle oneToOneMappings a potom klikněte na odkaz **Přidat** .  
11. Otevřete soubor ve formátu Base 64 .cer, který jste vyexportovali dříve. Odebrání *---začít certifikát---* *---UKONČIT certifikát---*a všechny konce řádků. Zkopírujte výslednou řetězec.  
12. Nastavení certifikátu za účelem řetězec zkopírovali v předchozím kroku.  
13. Sada povolený na **hodnotu true**.  
14. Nastavit uživatelské jméno účtu, který je členem skupiny zabezpečení PhoneFactor správců. Použití &lt;domény&gt;& #92; &lt;uživatelské jméno&gt; formát.  
15. Nastavení hesla pro příslušný heslo a potom zavřete Editor konfigurace.  
16. Klikněte na **použít** odkaz.  
17. V adresáři virtuální webové služby SDK poklikejte na položku **ověřování**.  
18. Ověřte, zda **povoleno**nastaveny ASP.NET zosobnění a základní ověřování a, že všechny další položky jsou nastavené **Zakázané**.  
19. V adresáři virtuální webové služby SDK poklikejte na položku **Nastavení SSL**.  
20. Nastavte certifikátů na **přijmout**a potom klikněte na **použít**.  
21. Zkopírujte soubor .pfx, který jste exportovali dříve na serveru, na kterém běží služba AD FS adaptér.  
22. Importujte soubor .pfx k úložišti osobních certifikátů místního počítače.  
23. Klikněte pravým tlačítkem myši a vyberte **Spravovat privátních klíčů**a potom udělit přístup pro čtení k účtu, který jste použili pro přihlášení ke službě AD FS.  
24. Otevření certifikát klienta a kopírování miniaturou na kartu **Podrobnosti** .  
25. V souboru MultiFactorAuthenticationAdfsAdapter.config nastavte **WebServiceSdkCertificateThumbprint** řetězec zkopírovali v předchozím kroku.  


Nakonec zaregistrovat adaptér, spustit \Program Files\Multi-faktor ověřování Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 skript Powershellu. Adaptér je registrovaná jako WindowsAzureMultiFactorAuthentication. Restartujte službu AD FS pro registraci se projeví.

## <a name="related-topics"></a>Příbuzná témata

Poradce při potížích, najdete v tématu [Nejčastější dotazy týkající se Azure Multi-Factor Authentication](multi-factor-authentication-faq.md)
