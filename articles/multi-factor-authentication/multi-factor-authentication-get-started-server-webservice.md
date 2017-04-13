<properties 
    pageTitle="Začínáme webová služba MFA serveru mobilní aplikaci"
    description="Aplikaci Azure Multi-Factor Authentication nabízí možnost Další ověření mimo pásma.  Je možné MFA serveru a používat nabízená oznámení pro uživatele."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>Začínáme webová služba MFA serveru mobilní aplikaci

Aplikaci Azure Multi-Factor Authentication nabízí možnost Další ověření mimo pásma. Místo umístěním automatizovaným telefonní hovor nebo služby SMS uživateli během přihlašování, Azure Vícefaktorové ověřování posune oznámení k aplikaci Azure Multi-Factor Authentication na smartphonu nebo tabletu uživatele. Uživatel jednoduše klepne "Ověřit" (nebo zadá kód PIN a klepne se na "Ověřit") v aplikaci k přihlášení.

Abyste mohli používat aplikaci Azure Multi-Factor Authentication následující jsou potřeba, aby aplikace úspěšně komunikovat s mobilní aplikace webové služby:

- Najdete v tématu Hardware a požadavky na Software pro požadavky na hardware a software
- Musíte používat verze 6.0 nebo vyšší Server Azure Multi-Factor Authentication
- Mobilní aplikace webová služba musí být nainstalovaná na webovém serveru internetového systém Microsoft® Internet informace služby Internetové informační služby IIS 7.x nebo vyšší.  Další informace o serveru IIS najdete v článku [IIS.NET](http://www.iis.net/).
- Zajištění ASP.NET v4.0.30319 je nainstalovaný, evidenci a nastavit na hodnotu povoleno
- Zahrnout povinných rolí služby ASP.NET a Kompatibilita metabáze služby IIS 6
- Mobilní aplikace webová služba musí být přístupný pomocí veřejné adresy URL
- Mobilní aplikace webová služba musí zabezpečená certifikát SSL.
- V Azure Multi-Factor Authentication webové služby SDK musí být nainstalovaná ve službě IIS 7.x nebo vyšší na serveru, který Server Azure Multi-Factor Authentication
- V Azure Multi-Factor Authentication webové služby SDK musí zabezpečená certifikát SSL.
- Mobilní aplikace webové služby, musíte mít pro připojení k v Azure Multi-Factor Authentication webové služby SDK přes připojení SSL
- Mobilní aplikace webová služba musí být ověření v Azure Multi-Factor Authentication webové služby SDK pomocí přihlašovacích údajů účet služby, který je členem skupiny zabezpečení s názvem "PhoneFactor správci". Tento účet služby skupiny existují a ve službě Active Directory Pokud Server Azure Multi-Factor Authentication běží na serveru doméně. Tento účet služby a skupiny existovat místně na Server Azure Multi-Factor Authentication Pokud není připojen k doméně.


Instalace portálu uživatele na serveru, než Server Azure Multi-Factor Authentication vyžaduje následující tři kroky:

1. Instalace webové služby SDK
2. Instalace mobilních aplikací webové služby
3. Konfigurace nastavení mobilní aplikace na Server Azure Multi-Factor Authentication
4. Aktivace aplikací Azure Multi-Factor Authentication pro koncové uživatele

## <a name="install-the-web-service-sdk"></a>Instalace webové služby SDK

Pokud v Azure Multi-Factor Authentication webové služby SDK už není nainstalovaná na Server Azure Multi-Factor Authentication, přejděte na tento server a otevřete Server Azure Multi-Factor Authentication. Klikněte na ikonu SDK webové služby, klikněte na SDK instalace webové služby... tlačítko a postupujte podle pokynů prezentovat. Web služby SDK musí zabezpečená certifikát SSL. K tomuto účelu nevadí certifikátu podepsaného svým držitelem, ale musí být importují do úložiště "Důvěryhodných kořenových certifikátů" účtu místního počítače na webovém serveru portál uživatele tak, aby ho bude důvěřovat při zahájení připojení SSL.

<center>![Nastavení](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>Instalace mobilních aplikací webové služby
Před instalací webová služba mobilní aplikaci, mějte na paměti toto:

- Pokud portálu Azure Multi-Factor Authentication uživatele je už nainstalovaná na serveru internetového, uživatelské jméno, heslo a adresu URL webové služby SDK můžete zkopírovali z portálu uživatele nastavení(Web.config)).
- Je vhodné otevřete webový prohlížeč na webovém serveru internetového a přejděte na adresu URL webové služby SDK, napsaného do nastavení(Web.config)). Pokud v prohlížeči můžete k webové službě úspěšně, by měl výzvu k zadání přihlašovacích údajů. Zadejte uživatelské jméno a heslo, které byly vloženy do nastavení(Web.config)) přesně tak, jak se zobrazí v souboru. Ujistěte se, že se zobrazí bez varování certifikát a chyby.
- Pokud reverzního proxy nebo brány firewall umístěným před mobilní aplikace webové služby Webový server a provádění SSL odstranění, můžete upravit nastavení(Web.config)) mobilní aplikace webové služby a přidejte následující klávesy odkaz <appSettings> oddílu tak, aby webová služba mobilní aplikaci můžete využívat http místo https. Ale SSL požaduje pořád v mobilní aplikaci pro proxy brány firewall/obrácené pořadí. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>Instalace mobilních aplikací webové služby

<ol>
<li>Spusťte Průzkumníka Windows na Server Azure Multi-Factor Authentication a přejděte do složky, kde je nainstalovaný Server Azure Multi-Factor Authentication (například C:\Program Files\Azure Multi-Factor Authentication). Volba 32bitové nebo 64bitové verze systému instalační soubor Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup podle potřeby na serveru, nainstaluje mobilní aplikace webové služby na. Zkopírujte instalační soubor internetového server.</li>

<li>Na webovém serveru internetového instalační soubor spuštěním s oprávněními správce. Nejjednodušší způsob je otevřete okno příkazového řádku jako správce a přejděte do umístění, kde se zkopíroval instalační soubor.</li>  

<li>Spusťte instalaci soubor Multi-Factor AuthenticationMobileAppWebServiceSetup, pokud budete chtít změnit na web a změňte virtuální adresář krátký název, třeba "PA". Název krátké virtuální adresáře se doporučuje od uživatele zadejte adresu URL mobilní aplikace webové služby do mobilního zařízení při aktivaci.</li>

<li>Po dokončení instalace AuthenticationMobileAppWebServiceSetup Multi-Factor Azure, vyhledejte C:\inetpub\wwwroot\PA (odpovídající adresáře nebo podle názvu virtuální adresář) a upravte nastavení(Web.config)).</li>  

<li>Vyhledejte WEB_SERVICE_SDK_AUTHENTICATION_USERNAME a WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD klíče a je nastavena hodnoty pro uživatelské jméno a heslo účtu služby, který je členem skupiny zabezpečení PhoneFactor správců seskupení (viz požadavky výše). Může to být stejný účet použitý jako identita uživatele portálu Azure Multi-Factor Authentication Pokud, která byla dříve nainstalována. Nezapomeňte zadejte uživatelské jméno a heslo mezi znak uzavírající na konci řádku (hodnota = "" / >). Doporučujeme používat kvalifikovaný uživatelské jméno (například doména\uživatelské jméno nebo machine\username).</li>  

<li>Vyhledejte nastavení pfMobile App Web Service_pfwssdk_PfWsSdk a změňte hodnotu z "http://localhost:4898/PfWsSdk.asmx" adresu URL webové služby SDK, na kterém běží na Azure Multi-Factor ověřování serveru (například https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Protože SSL se používá pro připojení, musí odkazovat SDK webové služby tak, že název serveru a nikoli IP adresu vzhledem k tomu certifikát SSL bude byla vydána název serveru a adresu URL použít musí shodovat s názvem na certifikát. Pokud název serveru nevyřeší k IP adrese z internetového serveru, přidejte položku do souboru hosts na tomto serveru namapujte název serveru Azure Multi-Factor Authentication na IP adresu. Uložte nastavení(Web.config)) po provedení změn.</li>  

<li>Pokud na webu, aby mobilní aplikace webová služba byl nainstalovaný v části (například výchozí web) už nebylo binded certifikátem veřejně přihlášení, instalace certifikátu na serveru v opačném případě už nainstalovaný otevřete Správce služby IIS a svázat certifikát na web.</li>  

<li>Otevřete webový prohlížeč na libovolném počítači a přejděte na adresu URL nainstalovanou mobilní aplikace webové služby (například https://www.publicwebsite.com/PA). Ujistěte se, že se zobrazí bez varování certifikát a chyby.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurace nastavení mobilní aplikace na Server Azure Multi-Factor Authentication
Teď je nainstalována služba web mobilní aplikaci, musíte nastavit Server Azure Multi-Factor Authentication pro práci s na portálu.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurace nastavení mobilní aplikace na serveru Azure Multi-Factor Authentication

1. Na Azure Multi-Factor ověřování serveru klikněte na ikonu portál uživatele. Pokud uživatelé budou moci určit jejich metody ověřování zaškrtněte na kartě nastavení v části Povolit uživatelům, zvolte metodu mobilní aplikaci. Bez tuto funkci povolit koncoví uživatelé se budou muset obraťte se na podporu Nápověda k provedení aktivace pro mobilní aplikaci.
2. Zaškrtněte políčko Povolit uživatelům aktivovat pole pro mobilní aplikaci.
3. Zaškrtněte políčko Povolit přihlášení uživatele.
4. Klikněte na ikonu mobilní aplikace.
5. Zadejte adresu URL společně s virtuální adresář, která je vytvořená při instalaci AuthenticationMobileAppWebServiceSetup Multi-Factor Azure. Název účtu lze zadávat do příslušného pole. Tento název společnosti se zobrazí v mobilní aplikaci. Pokud necháte prázdné, zobrazí se název poskytovatele Auth Multi-Factor vytvořené v portálu pro správu Azure.



<center>![Nastavení](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
