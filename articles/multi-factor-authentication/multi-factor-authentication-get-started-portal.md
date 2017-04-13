<properties 
    pageTitle="Zavedení portálu uživatele pro službu Azure Multi-Factor Authentication"
    description="Tohle je stránka Azure Multi-Factor ověřování, který popisuje, jak začít s Azure MFA a portálu uživatele."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Zavedení portálu uživatele pro službu Azure Multi-Factor Authentication

Portál uživatel může správce nainstalujte a nakonfigurujte portálu Azure Multi-Factor Authentication uživatele. Portál uživatele je webu služby IIS, které umožňuje uživatelům zapsat v Azure Vícefaktorové ověřování a udržovat svých účtů. Uživatel může změnit jejich telefonního čísla, změnit jejich PIN kód nebo vynechat Azure Vícefaktorové ověřování během jejich další znak na.

Uživatelé se přihlásit k portálu uživatele pomocí své normální uživatelské jméno a heslo a bude dokončení hovoru Azure Vícefaktorové ověřování nebo odpovědi na otázky zabezpečení dokončete jejich ověření. Pokud je uživatel zápisu povoleno, uživatel nakonfiguruje jejich telefonního čísla a PIN kód při prvním přihlášení k portálu uživatele.

Správci portál uživatel může nastavení a oprávnění k přidání nových uživatelů a aktualizovat stávajícím uživatelům.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Nasazení portálu uživatele na stejný server jako Server Azure Multi-Factor Authentication

Podporují následující předpoklady pro instalaci portálu uživatelů na stejný server jako Server Azure Multi-Factor Authentication:

- Služby IIS je potřeba nainstalovat včetně prvků asp.net a meta 6 Internetové informační služby základní funkce pro kompatibilitu (pro službu IIS 7 nebo novější)
- Přihlášený uživatel musí mít oprávnění správce pro počítač a doménu, v případě potřeby.  Je to proto účet vyžaduje oprávnění k vytváření skupin zabezpečení služby Active Directory.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Nasazení portálu uživatele pro službu Azure Multi-Factor Authentication

1. V rámci Azure ověřování serveru Multi-Factor: klikněte na ikonu portál uživatele v nabídce nalevo, klikněte na tlačítko nainstalovat portál uživatele.
1. Klikněte na další.
1. Klikněte na další.
1. Pokud je počítač připojen k doméně a konfiguraci služby Active Directory k zabezpečení komunikace mezi portálu uživatele a služba Azure Vícefaktorové ověřování není úplný, zobrazí se v kroku služby Active Directory. Kliknutím na tlačítko Další automatické dokončování tuto konfiguraci.
1. Klikněte na další.
1. Klikněte na další.
1. Klikněte na Zavřít.
1. Na libovolném počítači otevřete webový prohlížeč a přejděte na adresu URL nainstalovanou portál uživatele (například https://www.publicwebsite.com/MultiFactorAuth). Ujistěte se, že se zobrazí bez varování certifikát a chyby.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Nasazení portálu Azure Vícefaktorové ověřování serveru uživatele na samostatném serveru

Abyste mohli používat aplikaci Azure Multi-Factor Authentication následující jsou potřeba, aby aplikace úspěšně komunikovat s uživateli portálu:

Požadavky na hardware a software najdete Hardware a Software požadavky:

- Musíte používat verze 6.0 nebo vyšší Azure Multi-Factor ověřování serveru.
- Portál uživatel musí být nainstalovaná na webovém serveru internetového systém Microsoft® Internetové informační služby (IIS) 6.x, IIS 7.x nebo vyšší.
- Při použití služby IIS 6.x, zajistit ASP.NET v2.0.50727 je instalována, registrované a povolené.
- Povinné služeb rolí při používání služby IIS 7.x nebo vyšší zahrnout ASP.NET a Kompatibilita metabáze služby IIS 6.
- Portál uživatele by měl zabezpečená certifikát SSL.
- V Azure Multi-Factor Authentication webové služby SDK musí být nainstalovaná ve službě IIS 6.x, IIS 7.x nebo vyšší na Server Azure Multi-Factor Authentication nainstalovaného na serveru.
- V Azure Multi-Factor Authentication webové služby SDK musí zabezpečená certifikát SSL.
- Portál uživatel musí být možné se připojit k v Azure Multi-Factor Authentication webové služby SDK přes připojení SSL.
- Portál uživatel musí být ověření v Azure Multi-Factor Authentication webové služby SDK pomocí přihlašovacích údajů účet služby, který je členem skupiny zabezpečení s názvem "PhoneFactor správci". Tento účet služby skupiny existují a ve službě Active Directory Pokud Server Azure Multi-Factor Authentication běží na serveru doméně. Tento účet služby a skupiny existovat místně na Server Azure Multi-Factor Authentication Pokud není připojen k doméně.

Instalace portálu uživatele na serveru, než Server Azure Multi-Factor Authentication vyžaduje následující tři kroky:

1. Instalace webové služby SDK
2. Instalace portálu uživatele
3. Konfigurace nastavení portálu uživatele na Server Azure Vícefaktorové ověřování


### <a name="install-the-web-service-sdk"></a>Instalace webové služby SDK

Pokud v Azure Multi-Factor Authentication webové služby SDK už není nainstalovaná na Server Azure Multi-Factor Authentication, přejděte na tento server a otevřete Server Azure Multi-Factor Authentication. Klikněte na ikonu SDK webové služby, klikněte na SDK instalace webové služby... tlačítko a postupujte podle pokynů prezentovat. Web služby SDK musí zabezpečená certifikát SSL. K tomuto účelu nevadí certifikátu podepsaného svým držitelem, ale je nutné importovat do úložiště "Důvěryhodných kořenových certifikátů" účtu místního počítače na webovém serveru portál uživatele tak, aby ho bude důvěřovat při zahájení připojení SSL.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Instalace portálu uživatele

Před instalací portálu uživatele na samostatném serveru, mějte na paměti toto:

- Je vhodné otevřete webový prohlížeč na webovém serveru internetového a přejděte na adresu URL webové služby SDK, napsaného do nastavení(Web.config)). Pokud v prohlížeči dostanou webová služba úspěšně, by měl výzvu k zadání přihlašovacích údajů. Zadejte uživatelské jméno a heslo, které byly vloženy do nastavení(Web.config)) přesně tak, jak se zobrazí v souboru. Ujistěte se, že se zobrazí bez varování certifikát a chyby.
- Pokud reverzního proxy nebo brány firewall umístěným před webový server portál uživatele a provádění SSL odstranění, můžete upravovat nastavení(Web.config)) portál uživatele a přidat následující klávesy odkaz <appSettings> oddílu tak, aby portál uživatele můžete využívat http místo https. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>Chcete-li nainstalovat portálu uživatele

1. Spusťte Průzkumníka Windows na serveru Azure Multi-Factor Authentication a přejděte do složky, kde je nainstalovaný Server Azure Multi-Factor Authentication (například C:\Program Files\Multi faktorem ověřování serveru). Volba 32bitové nebo 64bitové verze systému instalační soubor MultiFactorAuthenticationUserPortalSetup podle potřeby na serveru, který uživatel portál nainstaluje na. Zkopírujte instalační soubor internetového server.
2. Na webovém serveru internetového instalační soubor spuštěním s oprávněními správce. Nejjednodušší způsob je otevřete okno příkazového řádku jako správce a přejděte do umístění, kde se zkopíroval instalační soubor.
3. Spusťte instalační soubor MultiFactorAuthenticationUserPortalSetup64, pokud budete chtít změnit název serveru a virtuální adresář.
4. Po dokončení instalace portálu uživatel, přejděte k C:\inetpub\wwwroot\MultiFactorAuth (nebo příslušného adresáře podle názvu virtuální adresář) a upravit nastavení(Web.config)).
5. Vyhledejte klávesu USE_WEB_SERVICE_SDK a změňte hodnotu z NEPRAVDA true (pravda). Vyhledejte WEB_SERVICE_SDK_AUTHENTICATION_USERNAME a WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD klíče a je nastavena hodnoty pro uživatelské jméno a heslo účtu služby, který je členem skupiny zabezpečení PhoneFactor správců seskupení (viz požadavky výše). Nezapomeňte zadejte uživatelské jméno a heslo mezi znak uzavírající na konci řádku (hodnota = "" / >). Je vhodné použít kvalifikovaný uživatelské jméno (například doména\uživatelské jméno nebo machine\username)
6. Vyhledejte nastavení pfup_pfwssdk_PfWsSdk a změňte hodnotu z "http://localhost:4898/PfWsSdk.asmx" adresu URL webové služby SDK, na kterém běží na Azure Multi-Factor ověřování serveru (například https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Protože SSL se používá pro připojení, musí odkazovat SDK webové služby tak, že název serveru a nikoli IP adresu vzhledem k tomu certifikát SSL bude byla vydána název serveru a adresu URL použít musí shodovat s názvem na certifikát. Pokud název serveru nevyřeší k IP adrese z internetového serveru, přidejte položku do souboru hosts na tomto serveru namapujte název serveru Azure Multi-Factor Authentication na IP adresu. Uložte nastavení(Web.config)) po provedení změn.
7. Pokud na webu, aby uživatel portál byl nainstalovaný v části (například výchozí web) už nebylo binded certifikátem veřejně přihlášení, instalace certifikátu na serveru v opačném případě už nainstalovaný otevřete Správce služby IIS a svázat certifikát na web.
8. Na libovolném počítači otevřete webový prohlížeč a přejděte na adresu URL nainstalovanou portál uživatele (například https://www.publicwebsite.com/MultiFactorAuth). Ujistěte se, že se zobrazí bez varování certifikát a chyby.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurace nastavení portálu uživatele na Server Azure Multi-Factor Authentication
Teď portálu je nainstalovaný, musíte nakonfigurovat Server Azure Multi-Factor Authentication pro práci s na portálu.

Server Azure Vícefaktorové ověřování nabízí několik možností portálu uživatele.  Následující tabulka obsahuje seznam těchto možností a explaination z jejich použití.

Uživatelské nastavení portálu|Popis|
:------------- | :------------- |
Adresa URL portálu uživatele| Umožňuje zadejte adresu URL které je právě hostovaný na portálu.
Primární ověřování| Umožňuje určit typ ověřování používat při přihlášení k portálu.  Ověřování systému Windows, Radius nebo LDAP.
Povolit uživatelům přihlášení|Umožňuje uživatelům zadejte uživatelské jméno a heslo na přihlašovací stránce portálu uživatele.  Pokud není tato možnost vybrána, bude zašedlé polí.
Povolit přihlášení uživatele|Umožňuje uživateli do programu zaregistrovat vícefaktorové ověřování tak, že je pořizování obrazovka s výzvou k další informace, například telefonní číslo.  Objeví se výzva pro záložní Telefon umožňuje uživatelům zadat sekundární telefonní číslo.  Výzva pro třetích stran MÍSTOPŘÍSEŽNÉM token umožňuje uživatelům zadat token 3 MÍSTOPŘÍSEŽNÉM stran.
Umožňuje uživatelům spustit jednorázová přemostění| To umožňuje uživatelům zahájit jednorázovou přemostění.  Pokud uživatel sady, které si ho bude trvat ovlivnit při příštím uživatel přihlásí.  Výzva pro přemostění sekund, než poskytuje uživatel s polem, takže můžete změnit výchozí 300 sekund.  V opačném jednorázové přemostění je pouze vhodný k 300 sekund.
Povolit uživatelům vyberte metodu| Umožňuje uživatelům můžete určit způsob jejich primárního kontaktu.  To může být telefonní hovor, textovou zprávu, mobilní aplikace nebo MÍSTOPŘÍSEŽNÉM token.
Umožnit uživatelům vybrat jazyk|  Umožňuje uživateli změnit jazyk, který se používá pro telefonní hovor, textovou zprávu, mobilní aplikace nebo MÍSTOPŘÍSEŽNÉM token.
Povolit uživatelům aktivovat pro mobilní aplikaci| Umožňuje generovat aktivační kód při dokončování procesu aktivace mobilní aplikace, který se používá se serverem.  Můžete také nastavit počet zařízení, která jsou to aktivace na.  1 až 10.
Použití dotazy zabezpečení jako základní|Umožňuje dotazy zabezpečení v případě, že vícefaktorové ověřování se nezdaří.  Můžete zadat počet bezpečnostní otázky, které musí být úspěšně zodpovězené.
Povolit uživatelům přiřadit token MÍSTOPŘÍSEŽNÉM třetích stran| Umožňuje uživatelům vybírat token MÍSTOPŘÍSEŽNÉM třetích stran.
Použití MÍSTOPŘÍSEŽNÉM tokenu jako základní|Umožňuje použití token MÍSTOPŘÍSEŽNÉM v případě, že vícefaktorové ověřování se nezdaří.  Můžete taky určit časový limit relace v minutách.
Povolení protokolování|Umožňuje přihlásit na portál uživatele.  Soubory protokolu jsou umístěny na: C:\Program Files\Multi faktory ověřování Server\Logs.

Většinou tato nastavení jsou viditelné pro uživatele, až se jim povolí a znaménka uživatele do portálu uživatele.

![Uživatelské nastavení portálu](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurace nastavení portálu uživatele v Server Azure Multi-Factor Authentication




1. Na Azure Multi-Factor ověřování serveru klikněte na ikonu portál uživatele. Na kartě nastavení zadejte adresu URL portálu uživatele do textového pole Adresa URL portálu uživatele. Tato adresa URL bude vložen do e-mailů odesílaných uživatelům při importu na Server Azure Multi-Factor Authentication povolil funkci e-mailu.
2. Vyberte nastavení, která chcete použít na portálu uživatele. Například pokud uživatelé budou moci určit jejich metody ověřování, ujistěte se, že je zaškrtnuté políčko Povolit uživatelům vyberte metodu spolu s metody, pomocí kterých můžete vybírat.
3. Klikněte na odkaz Nápověda v pravém horním rohu nápovědu k nastavení zobrazen principy.

<center>![Nastavení](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Karta správci
Tato karta umožňuje jednoduše přidat uživatele, kteří mají oprávnění správce.  Při přidávání správce, můžete optimalizovat oprávnění, která se zobrazí.  Tímto způsobem lze zajistit jenom poskytnout potřebná oprávnění správce.  Jednoduše klikněte na tlačítko Přidat a pak vyberte a uživatelů a jejich oprávnění a potom klikněte na Přidat.

![Správce portálu uživatelů](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Dotazy zabezpečení
Tato karta umožňuje určit, které uživatelé budou muset-li vybrán dotazy zabezpečení používání náhradní možnost poskytnout odpovědi na otázky zabezpečení.  Server Azure Authenticaton Multi-Factor získáváte výchozí dotazy, které můžete použít.  Můžete taky změnit pořadí, nebo můžete přidat své vlastní otázky.  Po přidání vlastní otázky, můžete určit jazyků se mají tyto otázka zobrazit v taky.

![Dotazy zabezpečení portálu uživatele](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Předaná relací

## <a name="saml"></a>SAML
Umožňuje nastavení portálu uživatele přijmout deklarací od některého poskytovatele identit SAML pomocí.  Můžete zadat časový limit relace, zadejte ověřovací certifikát a odhlášení přesměrování adresy URL.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Tato karta umožňuje zadat jednu IP adresy nebo rozsahy IP adres, které lze přidat tak, že pokud uživatel je přihlašování jednu z těchto adres IP vícefaktorové ověřování obejít.

![Portál uživatele důvěryhodné IP adresy](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Uživatel samoobslužné registrace
Pokud chcete, aby uživatelé se přihlásit a zapsat musíte vybrat Povolit uživatelům přihlášení a povolení možností uživatele pro zápis. Myslete na to, že vyberete nastavení se projeví přihlášení uživatele.

Třeba když uživatel přihlášení k portálu uživatele a klepne na tlačítko přihlásit, jsou potom přesměrováni na stránku nastavení Azure Multi-Factor ověřování uživatelů.  V závislosti na tom, jak jste nakonfigurovali Azure Vícefaktorové ověřování uživatel může ovlivnit výběr jejich metody ověřování.  

Pokud vyberte metodu ověření hlasových hovorů nebo byly předkonfigurovaná a používá příslušný způsob, na stránce bude zobrazí výzvu k zadání jejich primární telefonní číslo a rozšíření v případě potřeby.  Jsou taky smějí záložní telefonní číslo zadejte.  

![Portál uživatele důvěryhodné IP adresy](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Pokud uživatel je potřeba k použití kódu PIN při ověřování, na stránce taky vyzve je k zadání PIN kódu.  Po zadání jejich telefonní čísla a PIN kódu (pokud existuje), uživatel klikne Já zavolat na tlačítko ověřit.  Azure Vícefaktorové ověřování provede zavolat ověřování uživatele primární telefonní číslo.  Uživatel musí přijetí telefonního hovoru a zadejte svůj kód PIN (když to jde) a stiskněte klávesu # k přesunutí na další krok procesu vlastní registrace.   

Pokud uživatel vybere Text zprávy SMS metody ověřování nebo byl předkonfigurovaná a používá příslušný způsob, na stránce vyzvat uživatele k jejich číslo mobilního telefonu.  Pokud uživatel je potřeba k použití kódu PIN při ověřování, na stránce taky vyzve je k zadání PIN kódu.  Po zadání jejich telefonního čísla a PIN kódu (pokud existuje), uživatel klikne Text mi teď na tlačítko ověřit.  Azure Vícefaktorové ověřování provede služby SMS ověřování uživatele mobilní telefon.  Uživatel musí přijímání SMS, která obsahuje jednu čas heslo (OTP) a odpovědět na zprávu, které OTP plus svůj PIN kód v případě potřeby) přejděte další krok procesu vlastní registrace.

![Portál uživatele služby SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Pokud uživatel vybere mobilní aplikace metody ověřování nebo byl předkonfigurovaná a používá příslušný způsob, na stránce se zobrazí výzvu k nainstalujete Azure Vícefaktorové ověřování na svém zařízení a generovat aktivační kód.  Po instalaci aplikace Azure Vícefaktorové ověřování, uživatel klikne na tlačítko Generovat aktivační kód.    

>[AZURE.NOTE]Abyste mohli používat aplikaci Azure Vícefaktorové ověřování, musíte povolit uživatel nabízených oznámení pro svého zařízení.

Na stránce zobrazí aktivační kód a adresu URL spolu s obrázkem čárového kódu.  Pokud uživatel je potřeba k použití kódu PIN při ověřování, na stránce taky vyzve je k zadání PIN kódu.  Uživatel zadá aktivační kód a adresu URL do aplikace Azure Vícefaktorové ověřování nebo používá skener čárových kódů naskenujte obrázek čárového kódu a klepne na tlačítko aktivovat.    

Po dokončení aktivaci uživatel klikne na tlačítko ověření nyní.  Azure Vícefaktorové ověřování provede ověřování pro uživatele mobilní aplikace.  Uživatel musí zadat kód PIN (když to jde) a stiskněte tlačítko Ověřit své mobilní aplikaci k přesunutí na další krok procesu vlastní registrace.  


Pokud správce nakonfigurovali Server Azure Multi-Factor ověřování získat informace o zabezpečení otázek a odpovědí, uživatel přejde na stránku dotazy zabezpečení.  Uživatel musí vyberte čtyři dotazy zabezpečení a zadejte odpovědí na vybranou otázky.    

![Dotazy zabezpečení portálu uživatele](./media/multi-factor-authentication-get-started-portal/secq.png)  

Vlastní přihlášení uživatele je dokončena a uživatel je přihlášena k portálu uživatele.  Uživatelé mohou přihlásit zpět k portálu uživatele kdykoli v budoucnosti změnit telefonní čísla, spojky, metody ověřování nebo dotazy zabezpečení Pokud povolené na základě jejich správci.
