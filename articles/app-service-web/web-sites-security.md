<properties
    pageTitle="Zabezpečení aplikace služby Azure aplikace"
    description="Zjistěte, jak zajistit web app, mobilní aplikace back-end nebo rozhraní API aplikace v aplikaci služby Azure."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Zabezpečení aplikace služby Azure aplikace

Tento článek vám pomůže začít o zabezpečení v prohlížeči, mobilní aplikace back-end nebo rozhraní API aplikace v aplikaci služby Azure. 

Zabezpečení v aplikaci služby Azure má dvě úrovně: 

- **Infrastruktura, tak i platformu zabezpečení** – zabezpečení Azure mít služby potřebujete skutečně spustit věci bezpečně v cloudu.
- **Zabezpečení aplikace** – je třeba navrhnout samotnou aplikaci bezpečné. Platí to i jak integrovat s Azure Active Directory, jak spravovat certifikáty a jak zajistíte, že bezpečně hovořit můžete různé služby. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktura, tak i platformu zabezpečení
Protože aplikace služba udržuje Azure VMs úložiště, síťových připojení, předlohy webu, Správa a funkce integrace a mnoha dalších je aktivně zabezpečená a zesílený prochází důkladná dodržování předpisů a zkontroluje průběžně, abyste měli jistotu, že:

- Aplikace služby aplikace jsou izolace, jak z Internetu a od ostatních zákazníků Azure zdrojů.
- Komunikace tajemství (například spojení řetězců) mezi aplikace pro aplikaci služby a dalších Azure zdrojů (například databáze SQL) ve skupině zdroje nepřekročil Azure a nemá přes hranice všechny sítě. Tajemství jsou vždy šifrované.
- Všechny komunikace mezi aplikaci služby aplikace a externích zdrojů, například Správa Powershellu, rozhraní příkazového řádku, SDK Azure, rozhraní REST API a hybridní připojení jsou správně šifrovaná.
- 24 hodin hrozbou správy chrání aplikaci služby zdrojů z malwaru, distributed odmítnutí služby (Denial) člověka v – středním (MITM) a jiných hrozeb. 

Další informace o infrastruktury, tak i platformu zabezpečení v Azure najdete v tématu [Centrum zabezpečení Azure](/support/trust-center/security/).

#### <a name="application-security"></a>Zabezpečení aplikace

Při zodpovědní za infrastrukturu a platformy, která spuštění aplikace na Azure je zodpovědní za to zabezpečit samotnou aplikaci. Jinými slovy budete muset vývoj, nasazení a správu kód aplikace a obsah v bezpečný způsob. Bez toho kódu aplikace nebo obsah může být ohroženo hrozeb jako:

- Vkládání příkazu SQL
- Zneužití relace
- Křížové webu skriptování
- MITM úrovni aplikace
- Denial úrovni aplikace

Úplné informace o otázky bezpečnosti pro webové aplikace je předmětem tohoto dokumentu. Jako výchozí bod pro další informace o zabezpečení aplikace najdete v článku [Otevření webové aplikace zabezpečení projektu (OWASP)](https://www.owasp.org/index.php/Main_Page), konkrétně [10 hlavních projektů.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), kde jsou uvedeny aktuální horní 10 kritické webové aplikace zabezpečení chyby, podle OWASP členy.

## <a name="perform-penetration-testing-on-your-app"></a>Provedení průniku testování v aplikaci

Jedním ze způsobů nejjednodušší ještě nejdřív testování na chyby v aplikaci služby aplikace je pomocí [Integrace se službou Tinfoil zabezpečení](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) provádět chyby jedním kliknutím prohledávání na aplikace. Zobrazení výsledků testu v sestavě aplikace snadno porozumět a zjistěte, jak opravit každou chybu s podrobnými pokyny.

Pokud chcete provést vlastní penetrační testy nebo chcete použít jiný skeneru suite nebo poskytovatele, musíte projděte si [proces schvalování testování Azure průniku](https://security-forms.azure.com/penetration-testing/terms) a získat předchozí schválení provádět požadovanou penetrační testy.

##<a name="https"></a>Zabezpečená komunikace se zákazníky

Pokud používáte ** \*. azurewebsites.net** název domény vytvořené pro aplikaci služby aplikace může hned začít používat HTTPS, podle certifikát SSL pro všechny ** \*. azurewebsites.net** názvy domén. Pokud váš web používá [vlastní název domény](web-sites-custom-domain-name.md), můžete odeslat certifikát SSL [Povolit HTTPS](web-sites-configure-ssl-certificate.md) pro vlastní doménu.

Povolení [HTTPS](https://en.wikipedia.org/wiki/HTTPS) můžete chránit před útoky MITM komunikace mezi aplikace a její uživatele.

## <a name="secure-data-tier"></a>Zabezpečené osy dat

Aplikace služby lze integrovat vysoce s databáze SQL tak, že jsou řetězce připojení jsou šifrované v na vývěsku a pouze dešifrovat na OM aplikaci kompatibilní s *a* pouze při spuštění aplikace. Kromě toho databáze SQL Azure obsahuje mnoho funkcí zabezpečení týkající se zabezpečení dat aplikace z internetových hrozeb, včetně [šifrování v ostatních](https://msdn.microsoft.com/library/dn948096.aspx), [Vždy zašifrovaných](https://msdn.microsoft.com/library/mt163865.aspx), [Maskování dynamických dat](../sql-database/sql-database-dynamic-data-masking-get-started.md)a [Hrozbou, že zjišťování](../sql-database/sql-database-threat-detection-get-started.md). Pokud máte citlivá data nebo požadavky, najdete v článku [zabezpečení databáze SQL](../sql-database/sql-database-security.md) Další informace o tom, jak zabezpečit data.

Pokud používáte databáze jiného poskytovatele, například ClearDB, měli byste požádat si přečtěte následující dokumentaci poskytovatele přímo na doporučené postupy pro zabezpečení.  

##<a name="develop"></a>Vývoj bezpečných a nasazení

### <a name="publishing-profiles-and-publish-settings"></a>Publikování profily a nastavení publikování

Při vytváření aplikací, provádění úkolů správy nebo Automatizace úkolů pomocí nástroje ATP **Visual Studiu**, **Web matice**, **Azure PowerShell** **rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)**, můžete použít *Nastavení publikování* souboru nebo *publikování profilu*. Oba typy souborů ověřit s Azure a by měl být zabezpečený abyste zabránili neoprávněnému přístupu.

* Soubor **Nastavení publikování** obsahuje

    * ID Azure předplatného

    * Správa certifikát, který umožňuje provádět úlohy správy pro vaše předplatné *aniž by bylo nutné zadat heslo nebo název účtu*.

* **Publikování profilu** soubor obsahuje

    * Informace pro publikování do aplikace

Pokud používáte nástroj, který používá publikovat nastavení souboru nebo soubor s profilem publikování, importujte soubor obsahující nastavení publikování nebo profilu na nástroj a **Odstranění** souboru. Musí mít soubor sdílet s ostatními pracující na projektu například, uložte ho na zabezpečeném místě například *šifrované* adresáři s omezenými oprávněními.

Navíc nezapomeňte, jestli že jsou správně zapojené importovaných přihlašovacích údajů. Například **Azure PowerShell** a **rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)** obsahují importované informace v **adresáři** (*~* na Linux nebo OS X systémy a */users/yourusername* v systémech Windows.) Navíc cenného papíru budete pravděpodobně chtít **šifrování** těchto umístěních nástroje šifrování k dispozici pro váš operační systém.

### <a name="configuration-settings-and-connection-strings"></a>Nastavení a připojovací řetězec
Je obvyklé ukládat připojovací řetězec, přihlašovacími údaji a další citlivé informace v konfiguraci soubory. Bohužel tyto soubory může být vystaven na vašem webu nebo zaškrtnuté do veřejné úložiště vystavení tyto informace. Jednoduchý hledaný na [GitHub](https://github.com), například můžete objevovat některé soubory konfigurace s vystaveného tajemství v veřejné úložištích.

Doporučený postup je mít tyto informace z vašeho aplikace konfigurační soubory. Aplikace služby umožňuje obsahují informace o konfiguraci jako součást prostředí runtime jako **Nastavení aplikace** a **připojovací řetězec**. Hodnoty jsou k dispozici aplikaci za běhu pomocí *proměnných* pro většinu jazyky. V aplikacích .NET jsou tyto hodnoty vložit do konfiguraci .NET za běhu. Kromě těchto situací bude platit toto nastavení šifrovaného není-li zobrazit nebo nakonfigurovat pomocí [Portálu Azure](https://portal.azure.com) nebo nástroje například prostředí PowerShell nebo Azure rozhraní příkazového řádku. 

Ukládání informací o konfiguraci služby aplikace umožňuje správcům v aplikaci uzamknout citlivých informací aplikace výroby. Mohou vývojáři samostatnou sadu konfigurace nastavení pro vývoj aplikací a nastavení mohou být automaticky nahrazena s nastavením v aplikaci služby. Ani vývojáři měli byste vědět tajemství nakonfigurován pro aplikaci výroby. Další informace o konfiguraci nastavení aplikace a připojovací řetězec v aplikaci služby najdete v článku [Konfigurace webové aplikace](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Azure aplikace služba poskytuje zabezpečený přístup pomocí protokolu FTP k systému souborů aplikace prostřednictvím **FTPS**. To umožňuje zabezpečený přístup k kód aplikace na web appu, stejně jako diagnostiky protokoly. Doporučujeme vždy používat FTPS místo FTP. 

Odkaz FTPS aplikace najdete pomocí následujícího postupu:

1. Otevřete [portál Azure](https://portal.azure.com).
2. Vyberte **Procházet vše**.
3. Z zásuvné **Procházet** vyberte **Aplikaci služby**.
4. V **Aplikaci služby** zásuvné vyberte požadovanou aplikaci.
5. Z aplikace na zásuvné vyberte **všechna nastavení**.
6. V **Nastavení** zásuvné vyberte **Vlastnosti**.
7. Odkazy FTP a FTPS jsou uvedeny na zásuvné **Nastavení** . 

Další informace o FTPS najdete v tématu [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Další kroky

Další informace o zabezpečení Azure informacemi o platformě, informace o vytváření sestav k **incidentu zabezpečení nebo zneužívání**nebo informovat Microsoft, že budete provádět **průniku testování** webu, klikněte v části zabezpečení [Centrum zabezpečení aplikace Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

Další informace o **web.config** **applicationhost.config** souborů v aplikacích pro aplikaci služby najdete v článku [Možnosti konfigurace odemknout ve webových aplikacích pro aplikaci služby Azure](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Na informace o protokolování pro aplikaci služby aplikace, které mohou být užitečné při zjišťování útoky, přečtěte si téma [protokolování diagnostiky povolte](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), kde můžete okamžitě vytvořit aplikaci krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo

* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
