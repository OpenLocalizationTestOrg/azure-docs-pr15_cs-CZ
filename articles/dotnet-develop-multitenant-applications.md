<properties
    pageTitle="Více klienta webové aplikace vzorek | Microsoft Azure"
    description="Najděte architektonické přehledy a provedeních, která popisují, jak implementovat více klienta webovou aplikaci na Azure."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Víceklientské aplikací v Azure

Víceklientské aplikace je sdílený prostředek, který umožňuje samostatné uživatelů, nebo "klientů," Zobrazit aplikaci, jako kdyby byl vlastní. Běžný různě víceklientské aplikace je taková ve kterém všichni uživatelé aplikace chtít přizpůsobení uživatelského prostředí, ale jinak mít stejnou základní obchodní požadavky. Příklady velké víceklientské aplikace: Office 365, Outlook.com a visualstudio.com.

Z pohledu aplikace poskytovatele výhody multitenancy převážně se týkají efektivity provozní a náklady. Jedna verze aplikace můžete podle potřeby mnoho klienti/zákazníků povolení sloučení systému úlohy správy například sledování ladění výkonu, údržba softwaru a zálohovat data.

Následující obsahuje seznam nejdůležitější cíle a požadavky z hlediska poskytovatele.

- **Provisioning**: musíte mít zřízení nového klientů aplikace.  U víceklientské aplikací s velkým počtem klientů je nutné obvykle tento proces zautomatizovat povolením samoobslužné vytváření.
- **Udržovatelnost**: musíte mít upgradovat aplikaci a dalším úkolům údržbu okamžiku, kdy víc klientů používají.
- **Sledování**: musíte mít ke sledování aplikaci vždy určuje problémy a odstraňování problémů s nimi. Jedná se o sledování každého klienta je využití aplikací.

Aplikace správně implementovaná víceklientské poskytuje následující výhody uživatelům.

- **Izolace**: aktivity jednotlivé tenantů neovlivňují aplikaci pomocí dalších klientů použít. Klienti nelze datům dalších uživatelů. Klient jeví jako kdyby mají výhradní použití aplikace.
- **Dostupnost**: jednotlivé klienti má aplikace možné neustále, případně s záruky podle SLA. Aktivity jiných klienti znovu nedotýká dostupnosti aplikace.
- **Škálovatelnost**: aplikace rozšiřuje podle služba jednotlivé klienti. Informace o stavu a akce jiných klienti neměli vliv na výkon aplikace.
- **Náklady**: náklady jsou nižší než spuštěna aplikace vyhrazené, jednoho klienta, protože víceklientská umožňuje sdílení zdrojů.
- **Vlastní nastavení**. Možnost přizpůsobit aplikace pro jednotlivé klienta různými způsoby, například přidání nebo odebrání funkcí, změnou barev a loga nebo dokonce přidat svoje vlastní kód nebo skript.

Stručně řečeno i když jsou mnoho důležité informace, které je třeba vzít v úvahu vysoce scalable službu poskytovat, existuje také celá řada cílů a požadavky, které jsou společná pro mnoho víceklientské aplikací. Nemusí být některé důležité v konkrétních situacích a význam jednotlivých cíle a požadavky se budou lišit podle každého scénáře. Jako poskytovatele služby víceklientské aplikace se bude mít taky zajištěný cíle a požadavky jako klienti cíle a požadavky, ziskovosti, fakturace, více úrovně služeb, zřizování, udržovatelnost sledování a automatizaci.

Další informace o dalších navrhování víceklientské aplikace najdete v článku [hostitelské aplikace více klienta na Azure][]. Další informace o běžných vzorků dat architektura více klienta software jako služby (SaaS) databázové aplikace najdete v článku [Provedeních více klienta SaaS aplikací se databáze SQL Azure](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure obsahuje řadu funkcí, které umožňují adresy klíčové problémy při návrhu víceklientské systému.

**Izolace**

- Klienti segmentu webu tak, že záhlaví Host pomocí hesla nebo bez SSL komunikace
- Klienti segmentu webu tak, že parametry dotazu
- Webové služby v jednotlivých rolích kolegy
    - Role pracovníka. která obvykle zpracování dat v serverové části aplikace.
    - Web role, které obvykle představovat frontend pro aplikace.

**Úložiště**

Správa dat jako jsou databáze SQL Azure nebo úložišti Azure služeb, jako je službu tabulky, která obsahuje služby pro ukládání velkých objemů Nestrukturovaná data a služba objektů Blob poskytující služby pro ukládání velké množství nestrukturovaná textu nebo binární data například video, zvuk a obrázky.

- Zabezpečení Multitenant dat v SQL databázi odpovídající SQL Server přihlášení za klienta.
- Použití Azure tabulek aplikace zdroje zadáním kontejneru úroveň přístupu, můžete možnost Upravit oprávnění bez problémů nové adresy URL pro zdroje chráněné s sdílených přístup podpisy.
- Azure fronty fronty Azure zdrojů aplikace se běžně používají jednotka zpracování jménem klienty, ale může taky slouží k distribuci práce potřebných pro vytváření a správa.
- Fronty Bus služby pro zdroje aplikace, která posune pracovat sdílených službu, můžete použít jednu fronty každému odesílateli klienta pouze mají oprávnění (jak je odvozeno z deklarací vystaveného ACS) posunout do této fronty, zatímco jen příjemce ze služby smíte spotřebovávat fronty dat pocházejících z víc klientů.


**Připojení a zabezpečení služeb**

- Azure služby Bus infrastrukturu zasílání zpráv mezi aplikací povolením vymění volně vázaný výzvu vylepšené měřítko a odolnost proti chybám.

**Síť služby**

Azure obsahuje několik síťové služby, které podporuje ověřování a zjednodušíte hostovanou aplikací. Tyto služby patřit následující úkoly:

- Azure virtuální sítě umožňuje, můžete vytvořit a spravovat virtuální privátní sítě (VPN) v Azure i bezpečně propojení s místním IT infrastrukturu.
- Virtuální správce sítě přenosy umožňuje načíst zůstatek příchozích napříč několika hostované služby Azure, jestli máte systém ve stejném datacentru nebo přes různé datacentrech ve světě.
- Azure Active Directory (Azure AD) je moderní, na základě ZBÝVAJÍCÍ služba, která poskytuje možnosti řízení správu a přístup identity získáte cloudu. Použití Azure AD pro prostředky aplikací Azure AD pro poskytují snadný způsob, jak ověřování a ověřování uživatelů k získání přístupu k webovým aplikacím a službám zároveň funkce a mohli ověřovat promítnou mimo váš kód.
- Azure Bus služby poskytuje zabezpečené zasílání zpráv a funkce toku dat pro distributed a hybridní aplikacemi, například komunikaci mezi Azure hostovaný aplikací a místní aplikací a služeb, aniž by bylo složité infrastruktury bránu firewall a zabezpečení. S použitím Service Bus Relay pro prostředky aplikací služby, které jsou k dispozici jako koncové body může patřit ke klientovi (například hostovaný mimo systému, například na místní) nebo může být k dispozici konkrétně klienta (protože citlivá data specifické pro klienta přenášena mezi nimi) služby.



**Zřízení zdroje**

Azure obsahuje řadu způsobů zřízení nového klienti aplikace. U víceklientské aplikací s velkým počtem klientů je nutné obvykle tento proces zautomatizovat povolením samoobslužné vytváření.

- Role pracovníka vám umožní poskytování a rušení poskytování jednoho tenanta zdroje (třeba když nového klienta znaménka zdola nahoru nebo zruší), shromáždit metriky měření používání a správě měřítko podle určitých harmonogramu nebo v odpovědi překročením prahových hodnot klíčové ukazatele výkonu. Tato stejné role taky lze vysunout aktualizace a inovací na řešení.
- Objekty BLOB Azure lze použít zřízení výpočetním nebo předem inicializován prostředků úložiště pro nové klienti zároveň zásady přístupu na úrovni kontejneru chránit jej služby balíčků virtuální pevný disk obrázky a další zdroje informací.
- Možnosti pro vytváření databáze SQL materiály pro klienta, kterého patří:

    -   DDL skripty nebo vloženého jako zdroje v rámci sestavení
    -   SQL Server 2008 R2 DAC balíčků nasazený programově.
    -   Kopírování z databáze předlohy odkaz
    -   Použití databáze Import a Export zřízení nového databází ze souboru.



<!--links-->

[Hostitelské aplikace více klienta na Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
