<properties
   pageTitle="Implementace služby AD FS v Azure | Microsoft Azure"
   description="Jak implementovat zabezpečené hybridní architektura sítě s služba Active Directory Federation se tak mohli ověřovat v Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Implementace v Azure Active Directory Federation Services (služby AD FS)

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené postupy pro implementaci, který slouží k rozšíření v místní síti k Azure a [Active Directory Federation Services služby AD FS ()] , který používá sítě zabezpečené hybridní[ active-directory-federation-services] provádět federované a tak mohli ověřovat součásti spuštěné v cloudu. Tato architektura rozšiřuje konstrukce označená [Rozšíření Active Directory, aby Azure][implementing-active-directory].

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento odkaz architektura používá správce zdroje, který Microsoft doporučuje nové nasazení.

Služby AD FS mohlo by umožnit spuštění místní, ale ve scénáři hybridní umístění aplikací v Azure může být efektivnější k provedení této funkce v cloudu. Typické použití případů pro tuto architekturu patří:

- Hybridní aplikace kde úloh spustit částečně místní a částečně v Azure.

- Řešení, která využívají federované povolení vystavit webových aplikací do organizace partnera.

- Systémy, které podporují přístup z webové prohlížeče systém mimo bránou firewall.

- Systémy, které umožňují uživatelům přístup k webovým aplikacím připojením z oprávněnými externí zařízení například vzdálených počítačů, poznámkových bloků a další mobilní zařízení. 

Podrobnosti o tom, jak funguje služby AD FS, najdete v článku [Přehled služby Active Directory Federation][active-directory-federation-services-overview]. Kromě toho článek [nasazení služby AD FS v Azure] [ adfs-intro] obsahuje podrobné podrobné Úvod k provádění služby AD FS v Azure.

## <a name="architecture-diagram"></a>Diagram architektury

Následující diagram zvýrazní důležitá součástí tato architektura (*klikněte na zvětšit*). Další informace o všech prvků nesouvisí s služby AD FS, přečtěte si [Implementace zabezpečeného hybridní architektura sítě v Azure][implementing-a-secure-hybrid-network-architecture], [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]a [Implementace zabezpečeného hybridní architektura sítě s identitami služby Active Directory v Azure][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Tento diagram znázorňuje následující případy použití:
>
>- Kód aplikace spuštěné v organizaci partnera přistupuje webovou aplikaci umístěn uvnitř Azure VNet.
>
>- Externí, registrované uživatele (pomocí přihlašovacích údajů uložených uvnitř přidá) přístup k webové aplikace umístěn uvnitř Azure VNet.
>
>- Uživateli, který vaše VNet připojuje pomocí oprávněnými zařízení a spuštění webové aplikace umístěn uvnitř Azure VNet.
>
>Kromě toho tato architektura se zaměřuje na pasivní federace, kde na federační servery rozhodnout, kdy a jak k ověření uživatele. Očekává se, že uživatel zadat přihlašovací informace při spuštění aplikace. Toto je mechanismus nejčastěji používané webových prohlížečích která zahrnuje protokol, který přesměrovává prohlížeči na web, kde můžete uživatele zadejte své přihlašovací údaje. Služby AD FS taky podporuje federaci aktivní kterým aplikace převezme zodpovědnost zadání přihlašovacích údajů bez zásahu další uživatele, ale tento případ je mimo rozsah Tato architektura.

- **Přidá podsítě.** Přidá servery jsou obsaženy v vlastní podsítě. Pravidla NSG chránit přidá servery a umožňují bránu firewall proti z neočekávané zdrojů.

- **Přidá servery.** Toto jsou řadiče domény se systémem jako VMs v cloudu. Tyto servery můžete přihlašovacích údajů místních identit v rámci domény.

- **Podsítě služby AD FS.** Servery služby AD FS mohou být umístěny ve své vlastní podsítě s pravidly NSG budou sloužit jako bránu firewall.

- **Servery služby AD FS.** Servery služby AD FS poskytují federované autorizace a ověření. V tomto architektura budou provádět následující úkoly:

    - Zobrazuje se jim tokenů zabezpečení obsahující deklarací, které udělali federačního serveru partner jménem uživatele partnera. Služby AD FS můžete ověřit, že těchto tokenů platí před odesláním deklarací webové aplikace spuštěné v Azure. Podnikové webové aplikace (v Azure) můžete tyto požadavky povolte žádosti. V tomto scénáři podnikové webové aplikace je, *může stran*a je zodpovědnost federačního serveru partner o vystavení tvrdí, že jsou srozumitelné podnikové webové aplikace. Federační servery partnera pokládány za *partnery* protože odesláním žádosti o přístup jménem ověřené účty v organizaci partnera. Servery služby AD FS se označují jako *partnery zdroje* , protože poskytnutí přístupu k prostředkům (v tomto případě webových aplikací).

    - Může ověřovat (přes přidá a [Služba registrace zařízení Active Directory][ADDRS]) a povolte příchozí žádosti z externích uživatelů spuštěná webového prohlížeče nebo zařízení, které potřebuje přístup k podnikové webových aplikací. 

    Servery služby AD FS nakonfigurovaná jako farmě prostřednictvím Vyrovnávání zatížení Azure. Tuto strukturu pomáhají zlepšit dostupnost a škálovatelnost. Taky Všimněte si, že servery služby AD FS nejsou přístupné přímo k Internetu raději veškerý internetový provoz filtrována prostřednictvím služby AD FS webové aplikace proxy servery a DMZ.

- **Podsítě proxy server služby AD FS.** Proxy servery služby AD FS může být obsažen v vlastní podsítě s pravidly NSG zajistit ochranu. Servery v tomto podsítě jsou vystaven na Internetu přes sadu sítě virtuální zařízení, které jsou zdrojem mezi Azure virtuální síť a Internet brána firewall.

- **Servery proxy (WAP) webové aplikace služby AD FS.** Tyto počítače zajišťovat služby AD FS serverů příchozí žádosti z partnerských organizací a externích zařízení. Servery WAP fungují jako filtr stínění servery služby AD FS z přímý přístup z veřejného Internetu. Stejně jako u servery služby AD FS nasazení WAP serverech ve farmě s Vyrovnávání zatížení vám větší dostupnost a škálovatelnost než nasazení kolekce samostatné servery.

    >[AZURE.NOTE] Podrobné informace o instalaci WAP servery najdete v tématu [instalace a konfigurace Proxy serveru webové aplikace][install_and_configure_the_web_application_proxy_server]

- **Organizace partnera.** Toto je příklad organizace partnera, která běží webovou aplikaci, která požádá o přístup k webové aplikace spuštěné v Azure. Federačního serveru v organizaci partnera ověří žádosti o místně a odešle tokenů zabezpečení obsahující deklarací do služby AD FS spuštěné v Azure. Služby AD FS v Azure ověří tokenů zabezpečení a pokud jsou platné ji předejte deklarací u webové aplikace spuštěné v Azure je povolit.

    >[AZURE.NOTE] Můžete taky nakonfigurovat VPN tunelem pomocí Azure brány stanovit přímý přístup do služby AD FS důvěryhodných partnerů. Požadavky dostali z těchto partnerů není procházejí WAP servery.

## <a name="recommendations"></a>Doporučení

Tato část obsahuje souhrn doporučení pro implementaci služby AD FS v Azure, překrývajícím:

- Doporučení OM.

- Doporučení sítě.

- Dostupnost doporučení.

- Doporučení týkající se zabezpečení.

- Doporučení instalace služby AD FS.

- Služby AD FS zabezpečení doporučení.

### <a name="vm-recommendations"></a>Doporučení OM

Vytvoření VMs s dostatečné prostředky pro zpracování očekávané objem provozu. Použijte velikost existující počítačích hostitelské služby AD FS místně jako výchozí bod. Sledujte využití prostředků. Můžete změnit velikost VMs a měřítko, jako jsou příliš velká.

Postupujte podle [systém Windows OM na Azure]doporučení[vm-recommendations].

### <a name="networking-recommendations"></a>Doporučení sítě

Konfigurace rozhraní sítě pro každou VMs hostitelské služby AD FS a WAP servery s statické soukromé IP adres.

Nedávají veřejné IP adresy služby AD FS VMs. Další informace najdete v tématu [otázky bezpečnosti pro][security-considerations].

Nastavení IP adresu upřednostňované a vedlejší servery DNS sítě rozhraní pro každé služby AD FS a WAP OM neodkazuje VMs přidá (která by měla běžet DNS). Tento krok je třeba povolit každý OM k doméně.

### <a name="availability-recommendations"></a>Dostupnost doporučení

Vytvoření farmě služby AD FS se aspoň dva servery pro zvýšení použitelnosti službu služby AD FS.

Použití různých úložiště účtů pro každou OM služby AD FS ve farmě. Tento přístup pomáhá zajistit, abyste, pokud se nepovede v účtu jednoho úložiště celou farmu nedostupné.

Vytvoření sady samostatné Azure dostupnost pro služby AD FS a WAP VMs. Ujistěte se, že jsou ve všech sadách aspoň dva VMs. Každá dostupnost musí mít nejméně dvě aktualizace domény a dvě poruch domény.

Konfigurace vyrovnávání zatížení VMs služby AD FS a WAP VMs následujícím způsobem:

- Použití vyrovnávání zatížení Azure poskytovat WAP VMs externí přístup a vyrovnávání zatížení interní k distribuci načíst na serverech služby AD FS ve farmě služby AD FS.

- Zobrazená port 443 (HTTPS) na servery služby AD FS/WAP pouze k přenášena.

- Dejte Vyrovnávání zatížení statické IP adresy.

- Vytvořte zkušební stavu použití protokolu TCP namísto HTTPS. Použití příkazu ping port 443 ověřte, že server služby AD FS pracuje.

    >[AZURE.NOTE] Servery služby AD FS protokol serveru název označení (SNI), takže pokusíte probe pomocí HTTPS koncový bod z dojde k chybě Vyrovnávání zatížení.

- Přidejte záznam DNS *A* domény pro vyrovnávání zatížení služby AD FS. 

    Zadejte IP adresu Vyrovnávání zatížení a zadejte název domény (třeba adfs.contoso.com). To je název, podle kterého klienty a servery WAP přístup k serverové farmě služby AD FS.

### <a name="security-recommendations"></a>Doporučení týkající se zabezpečení

Můžete zabránit přímé působení servery služby AD FS k Internetu. Servery služby AD FS jsou doméně počítačích s celou povolení udělit tokenů zabezpečení. Pokud je server služby AD FS ohroženo, můžete se zlými úmysly problémů tokeny plný přístup na všech webových aplikací a federační servery, které jsou chráněny pomocí služby AD FS. Pokud systému musí zpracovat žádosti o externí uživatelé připojující se nemusí být nutně z důvěryhodného partnerských webů, použijte WAP servery ke zpracování tyto požadavky. Další informace najdete v tématu [umístění Proxy federačního serveru][where-to-place-an-fs-proxy].

Místo služby AD FS servery a WAP v samostatném podsítí s vlastní brány firewall. Pravidla NSG lze definovat pravidla brány firewall. Pokud požadujete komplexnější ochranu využijete větší zabezpečení obvod kolem servery pomocí pár prvků podsítí a NVAs, podle popisu v dokumentu [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]. Všimněte si, že všechny brány firewall by měl přenosy na port 443 (HTTPS).

Omezení přístupu přímé přihlášení na servery služby AD FS a WAP. Pouze DevOps pedagogy by měla připojení.

Nepřipojovat WAP servery na doménu.

### <a name="adfs-installation-recommendations"></a>Doporučení instalace služby AD FS

Článek [nasazení farma federačních serverů] [ Deploying_a_federation_server_farm] poskytuje podrobné pokyny k instalaci a konfiguraci služby AD FS. Před konfigurací první server služby AD FS ve farmě proveďte následující úkoly:

1. Získejte certifikát veřejně důvěryhodné k provádění ověřování serveru. *Název subjektu* musí obsahovat název prodloužením doby klienti přístup ke službě federace. To je název DNS pro vyrovnávání zatížení, například *adfs.contoso.com* (eliminování možnosti použití zástupných znaků názvy jako **. contoso.com*, z bezpečnostních důvodů). Použijte stejný certifikát na všechny VMs server služby AD FS. Máte možnost si zakoupit certifikát od důvěryhodnou certifikační autoritou, ale pokud vaše organizace používá Active Directory Certificate Services můžete vytvořit vlastní. 

    *Alternativní název subjektu* používá DRS povolit přístup z externích zařízení. Musí být ve formuláři *enterpriseregistration.contoso.com*.

    Další informace najdete v tématu [získání a konfigurace certifikátu SSL pro služby AD FS][adfs_certificates].

2. U řadiče domény generovat kořenové klíč pro službu klíč rozdělení. Nastavte hodnotu time efektivní je aktuální čas mínus 10 hodin (Toto nastavení snižuje zpoždění, která může nastat ve distribuce a synchronizace klíče přes domény). Tento krok je nutný pro podporu vytváření účet služby skupiny, který slouží ke spuštění služby služby AD FS. Následujícího příkazu Powershellu ukazuje příklad udělejte toto:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Přidání jednotlivých server služby AD FS OM do domény.

>[AZURE.NOTE] Instalace služby AD FS, musí být spuštěný emulaci roli FSMO u domény řadiče domény spuštěna a přístupné z VMs služby AD FS.

### <a name="adfs-trust-recommendations"></a>Služby AD FS zabezpečení doporučení

Vytvořte federaci vztah důvěryhodnosti mezi instalace služby AD FS a federační servery žádné organizace partnera. Konfigurace deklarací filtrování a potřebné mapování. Tento postup vyžaduje:

- DevOps zaměstnanců **na každé organizace partnera** přidáte předávající stran zabezpečení pro přístupných servery služby AD FS webových aplikací.

- DevOps zaměstnanců **ve vaší organizaci** konfigurace zabezpečení deklarací poskytovatele povolit servery služby AD FS s informacemi o důvěryhodnosti deklarací, které jsou zdrojem organizace partnera.

- DevOps zaměstnanců **ve vaší organizaci** ke konfiguraci služby AD FS předat deklarací k vaší organizace webových aplikací.

    Další informace najdete v tématu [Vytvoření důvěryhodnosti federace][establishing-federation-trust].

Publikovat webové aplikace vaší organizace a zpřístupníte je na externí partnery pomocí předběžné ověření prostřednictvím WAP servery. Další informace najdete v tématu [publikování aplikace pomocí služby AD FS používající][publish_applications_using_AD_FS_preauthentication]

Všimněte si, že služby AD FS podporuje tokenu transformace a rozšíření. Azure Active Directory neposkytuje tuto funkci. Pomocí služby AD FS když nastavíte vztahy důvěryhodnosti, můžete:

- Konfigurace transformace deklaraci identity pro ověřovacích pravidel. Můžete třeba namapovat skupinu zabezpečení z vyjádření organizace partnera společnosti Microsoft na něco, co tento přidá můžete povolit ve vaší organizaci používá.

- Transformace deklarací z jednoho formátu do druhého. Například můžete namapovat z SAML 2.0 SAML 1.1 Pokud aplikace podporuje pouze deklarací SAML 1.1. 

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

SQL Server nebo Windows interní databáze souboru () slouží k uložení informací o konfiguraci služby AD FS. Souboru obsahuje základní redundance. Změny zapsán přímo do jenom jedna z databáze služby AD FS v clusteru služby AD FS, zatímco ostatní servery pomocí vyžádané replikace aktuálnost jejich databází. Pomocí serveru SQL Server umožníte redundance celou databázi a dostupnost pomocí převzetí clusterů nebo odrážející strukturu.

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Služby AD FS používá protokol HTTPS, proto se ujistěte, NSG pravidla pro podsítě obsahující webu úroveň požadavky HTTPS povolení VMs. Tyto požadavky může pocházet z místní síti podsítí obsahující web osy, obchodní osy, osy dat, soukromé DMZ, veřejné DMZ a podsítě obsahující servery služby AD FS.

Zvažte použití sadu virtuální spotřebiče síťové protokoly podrobné informace o přenosu procházení okraj virtuální sítě pro účely auditování.

## <a name="scalability-considerations"></a>Co byste měli zvážit škálovatelnost:

Následující skutečnosti shrnuté z dokumentu [plánu zavedení služby AD FS][plan-your-adfs-deployment], co výchozí bod pro pro změnu velikosti služby AD FS farmy:

- Pokud máte míň než 1 000 uživatelů, nevytvářejte vyhrazené servery služby AD FS, ale místo toho instalace služby AD FS na všech serverech přidá v cloudu (zkontrolujte, jestli máte aspoň dva přidá servery, abyste zachovali dostupnost). Vytvoření jednoho WAP serveru.

- Pokud máte 15000 až 1 000 uživatelů, vytvořte dvě vyhrazené služby AD FS a dvě vyhrazené WAP servery.

- Pokud máte 15000 až 60000 uživatelů, vytvořte mezi tři až pět vyhrazené servery služby AD FS a aspoň dva vyhrazené WAP servery.

Tyto údaje použijeme že používáte duální procesory VMs (standardní D4_v2 nebo lepší) hostovat servery v Azure.

Poznámka: Pokud používáte interní databáze systému Windows pro uložení dat konfigurace služby AD FS, můžete se omezí na osm servery služby AD FS ve farmě. Pokud očekáváte nutností informace, pak pomocí serveru SQL Server. Další informace najdete v tématu [Rolí služby AD FS konfigurační databáze][adfs-configuration-database].

## <a name="management-considerations"></a>Důležité informace

Pedagogy DevOps máme ještě počítat provádět následující úkoly:

- Správa federační servery (Správa farmě služby AD FS Správa zásad zabezpečení na federační servery a správa certifikátů používaný federation services).

- Správa WAP servery (Správa farmy WAP Správa certifikátů WAP).

- Správa webových aplikací (Konfigurace mapování deklarací předávající strany, metody ověřování a).

- Zálohování součástí služby AD FS.

## <a name="monitoring-considerations"></a>Sledování co byste měli zvážit

[Microsoft System Center Management Pack pro Active Directory Federation Services 2012 R2] [ oms-adfs-pack] obsahuje aktivní i informace sledování nasazení služby AD FS pro federačním serveru. Tento management pack sleduje:

- Události služby AD FS služby záznamy v protokolu událostí služby AD FS.

- Údaje o výkonu, shromáždit výkonnosti služby AD FS. 

- Celkový stav služby AD FS systému a webových aplikací (předávající strany) a poskytuje upozornění pro kritické problémy a upozornění.

## <a name="solution-components"></a>Součásti řešení

Ukázkový skript řešení [Nasazení ReferenceArchitecture.ps1][solution-script], neexistuje, které můžete provádět architektura, který následuje doporučení popisované v tomto článku. Tento skript používá správce prostředků Azure šablony. Šablony jsou k dispozici jako sady základní stavební bloky, z nichž každá provádí konkrétní akce, jako jsou vytváření VNet a konfigurace NSG. Účelem skript je organizovat nasazení šablony.

Šablony jsou s parametry, s parametry v samostatných souborech JSON. Můžete změnit parametrů v těchto souborů, které chcete konfigurovat nasazení vlastní požadavkům. Nemusíte změnit šablony sami. Poznámka: aby nesmí změnit schémata objektů v soubory parametrů.

Při úpravách šablony vytvořit objekty, které následují za konvence podle [Doporučené názvů zdrojů Azure][naming-conventions].

Ukázka řešení vytvoří a nakonfiguruje prostředí v cloudu zahrnující přidá podsítě a servery služby AD FS podsítě a servery, podsítě proxy server služby AD FS a servery, DMZ, web osy, firmy osy a komponenty osy dat aplikace access, brána VPN a správy osy. Ukázka řešení obsahuje taky volitelná konfigurace pro vytvoření simulovaný místního prostředí.

Následující části popisují prvky místní i cloudových konfigurace.

### <a name="on-premises-components"></a>Místní součásti

>[AZURE.NOTE] Tyto součásti nejsou hlavní výběr architektury popsané v tomto dokumentu a jsou k dispozici pouze vám umožní otestovat prostředí cloudu, ne pomocí skutečné provozním prostředí. Z tohoto důvodu v této části pouze shrnuje klíčové parametr soubory. Můžete změnit nastavení, například velikosti VMs nebo IP adresy, ale je vhodné ponechat spoustu dalších parametrů beze změny.

Toto prostředí zahrnuje doménové AD pro doménu s názvem contoso.com. Doména obsahuje dva přidá servery s IP adresy 192.168.0.4 a 192.168.0.5. Tyto dva servery taky spustit službu DNS. S názvem místního účtu správce na obou VMs `testuser` heslem `AweS0me@PW`. Kromě toho konfigurace nastaví Brána VPN připojit se k VNet v cloudu. Konfigurace můžete změnit úpravou následující JSON soubory umístěné ve [**parametry/místní edici**] [ on-premises-folder] složky:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Tento soubor definuje sítě adresní prostor pro místního prostředí.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Tento soubor obsahuje konfiguraci místního VMs hostitelské služby přidá. Ve výchozím nastavení jsou vytvářeny dvě *Standardní DS3 v2* VMs.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** a ** [connection.parameters.json][on-premises-connection-parameters]**. Tyto soubory podržte nastavení pro připojení VPN k bráně Azure VPN v cloudu, včetně sdílené klávesy použít k ochraně přenosy procházení brány.

Zbývající soubory ve složce obsahují informace o konfiguraci použitých k vytvoření místní domény pomocí tohoto infrastruktury. Je používat k instalaci přidá nastavení DNS, vytvořte strukturu a konfigurace webů replikace pro struktuře.

### <a name="cloud-components"></a>Součásti cloudu

Tyto součásti tvoří základní tato architektura. [**Parametry/azure**] [ azure-folder] složka obsahuje následující soubory parametr pro konfiguraci tyto součásti:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Tento soubor definuje strukturu VNet VMs a jiné součásti v cloudu. Obsahuje nastavení, například název, adresní prostor, podsítí a adresy všech serverů DNS povinné. Všimněte si, že adresy DNS uvedeno v následujícím příkladu referenční IP adresy místní servery DNS a výchozí server Azure DNS. Změna tyto adresy Pokud nepoužíváte místního prostředí ukázkové neodkazuje nastavení DNS:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Tento soubor nakonfiguruje VMs systém přidá v cloudu. Nastavení se skládá ze dvou VMs. Změňte správce uživatelské jméno a heslo v `virtualMachineSettings` část notebooku a můžete upravit velikost OM, aby odpovídala požadavky na domény:

    Další informace najdete v článku [Rozšíření Active Directory, aby Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [Přidat přidá domény controller.parameters.json][add-adds-domain-controller-parameters]**. Tento soubor obsahuje nastavení pro vytváření domény společnosti CONTOSO trvající přidá servery. Použití vlastní příponami vytvořte domény a přidání přidá serverů. Není-li vytvořit další přidá servery (v tomto případě je, aby měli přidat `vms` matice), změnit jejich jména z výchozího nebo chcete vytvořit doménu s jiným názvem nemusíte upravit tento soubor.

- ** [loadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Soubor obsahuje dvě sady parametry konfigurace. `virtualMachineSettings` Část definuje VMs hostujících službu služby AD FS v cloudu. Ve výchozím nastavení skript vytvoří dva z těchto VMs ve stejnou sadu dostupnost:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    `loadBalancerSettings` Část obsahuje popis Vyrovnávání zatížení pro tyto VMs. Vyrovnávání zatížení přenosy v síti, která se zobrazí na port 443 (HTTPS) předává některého VMs:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [služby AD FS farmy domény join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Tento soubor obsahuje nastavení slouží k přidání domény společnosti CONTOSO servery služby AD FS. Potřebujete upravit tento soubor, pokud jste vytvořili další servery služby AD FS (aktualizovat `vms` pole v tomto případě), nebo změně názvu domény.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [služby AD FS farmy first.parameters.json][adfs-farm-first-parameters]**, a ** [služby AD FS farmy rest.parameters.json][adfs-farm-rest-parameters]**. Skript používá nastavení tyto soubory k vytvoření serverové farmě služby AD FS. 

    Soubor *gmsa.parameters.json* obsahuje nastavení pro skupinu spravovat účet služby používaný službou služby AD FS. Pokud chcete změnit název účtu nebo doménu, můžete upravit tento soubor.

    Soubor *služby AD FS farmy first.parameters.json* obsahuje informace potřebné k vytvoření serverové farmě služby AD FS a přidání prvního serveru. Pokud jste změnili domény nebo název účtu služby skupinu spravovat, měli byste aktualizovat tento soubor.

    Soubor *služby AD FS farmy rest.parameters.json* slouží k přidání zbývající servery služby AD FS farmy. Znovu Pokud jste změnili domény nebo název účtu služby skupinu spravovat, měli byste aktualizovat tento soubor. Aktualizace `vms` pole Pokud jste vytvořili další servery služby AD FS.

- ** [loadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Tento soubor je podobná struktury a obsahu souboru *loadBalancer adfs.parameters.json* . Obsahuje data pro vytváření proxy servery služby AD FS a vyrovnávání zatížení.

- ** [adfsproxy farmy first.parameters.json][adfsproxy-farm-first-parameters]**, a ** [adfsproxy farmy rest.parameters.json][adfsproxy-farm-rest-parameters]**. Skript používá nastavení tyto soubory k vytvoření serverové farmy proxy server služby AD FS. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Tento soubor obsahuje nastavení slouží k vytváření brána Azure VPN na cloud používá pro připojení k místní síti. Změňte `sharedKey` hodnota argumentu `connectionsSettings` oddílu, aby odpovídal, místní VPN zařízení. Další informace najdete v tématu [implementace hybridní architektura sítě s Azure a místních VPN][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** a ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Tyto soubory konfigurace příchozích (veřejné) a odchozí (soukromé) straně VMs, které tvoří DMZ ochrana servery v cloudu. Další informace o těchto prvků a jejich konfigurace v tématu [implementace DMZ mezi Azure a Internetu][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters json][loadBalancer-biz-parameters]**, a ** [loadBalancer data.parameters json][loadBalancer-data-parameters]**. Tyto soubory parametry obsahují OM specifikace pro úrovní přístupu pro web, business a data a konfigurace vyrovnávání zatížení pro každou úroveň. Toto jsou VMs, které hostovat webových aplikací web apps a databází a provádění úloh business pro organizaci. Můžete změnit velikost a počet VMs v každé osy podle vašich požadavků.

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Tento soubor obsahuje konfiguraci pole a Správa odkazů VMs. Je možné získat přihlášení a přístup pro správu k VMs web, business a úrovní dat v poli odkaz. Ve výchozím nastavení vytvoří skript jednoho *Standard_DS1_v2* OM, ale můžete upravit tento soubor vytvořit VMs zvětšit nebo další, pokud je pravděpodobně významné pracovní zátěž správy.

## <a name="solution-deployment"></a>Nasazení řešení

Řešení předpokládá následující požadavky:

- Máte existující Azure předplatné, ve kterém můžete vytvořit skupiny zdrojů.

- Jste stáhli a nainstalovali nejnovější sestavení Azure Powershellu. Najdete [tady] [ azure-powershell-download] pokyny.

Spuštění skript, který nasadí řešení:

1. Přesunutí do pohodlný složky na místním počítači a vytvořte následující podsložky:

    - Skriptů

    - Skripty a parametrů

    - Skripty/parametry/místní edici

    - Skripty/parametry/Azure

2. Stažení [Nasazení ReferenceArchitecture.ps1] [ solution-script] souboru do složky skriptů

3. Stáhnout obsah [místní parametry/edici] [ on-premises-folder] skripty/parametry/místní edici složky:

4. Stáhnout obsah [Parametry/azure] [ azure-folder] skripty/parametry/Azure složky.

5. Upravte soubor nasazení ReferenceArchitecture.ps1 ve složce skripty a změňte následující čar a umožňuje určit, které by měly být vytvořené nebo používá k ukládání prostředků vytvořené pomocí skriptu skupiny zdroje:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Upravte soubory parametrů v skripty/parametry/místní edici a skripty/parametry/Azure složek. Aktualizujte odkazy skupina zdroje v těchto souborech podle názvů skupin zdrojů přiřazených k proměnné v souboru ReferenceArchitecture.ps1 nasazení. Následující tabulka uvádí soubory, které parametr odkazovat skupiny jako zdroje. Poznámka: *Vzdálená pomoc služby AD FS pracovní zátěž rg* *Vzdálená pomoc služby AD FS zabezpečení rg*, *Vzdálená pomoc-služby AD FS přidá rg*, *Vzdálená pomoc služby AD FS služby AD FS rg*a *Vzdálená pomoc služby AD FS proxy rg* skupiny se používají v skript Powershellu nevyskytují parametr souborů se změnami.

  	|Pole Skupina zdroje|Parametr soubory|
  	|--------------|--------------|
  	|Vzdálená pomoc služby AD FS místní edici rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|Vzdálená pomoc služby AD FS sítě rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-Private.Parameters.JSON<br />parameters\azure\dmz-Public.Parameters.JSON<br />parameters\azure\loadBalancer-ADFS.Parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.Parameters.JSON<br />parameters\azure\loadBalancer-Biz.Parameters.JSON<br />parameters\azure\loadBalancer-data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*dvou výskytů*)

    Navíc nastavení místním i cloudových součásti, jak je popsáno v části [součásti řešení] [součásti řešení].

7. Otevřete okno Azure Powershellu, přejděte do složky skripty, spusťte tento příkaz:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Nahrazení `<subscription id>` pomocí svého ID Azure předplatného.

    Pro `<location>`, zadejte Azure oblast, jako například `eastus` nebo `westus`.

    `<mode>` Parametr můžete mít jednu z následujících hodnot:

    - `Onpremise`, k vytvoření simulovaný místního prostředí.

    - `Infrastructure`, a vytvořte infrastruktury VNet přejít pole v cloudu.

    - `CreateVpn`, sestavovat Azure virtuální sítě brány a připojte ji k místní síti.

    - `AzureADDS`, vytvářet VMs budou sloužit jako přidá servery, nasadit služby Active Directory do těchto VMs a vytvoření domény v cloudu.

    - `AdfsVm`Vytvoření VMs služby AD FS a připojte k doméně v cloudu.

    - `ProxyVm`k vytvoření proxy server služby AD FS VMs a připojte k doméně v cloudu.

    - `Prepare`, která provede všech výše uvedených úkolů. **Pokud vytváříte zcela novou nasazení a nemáte infrastruktury místní jde doporučené možnost.**

    >[AZURE.NOTE] Můžete taky spustit skript s `<mode>` parametr `Workload` vytvořit web, business a osy dat VMs a sítě. Toto nastavení není součástí `Prepare` režimu.

    Pokud používáte `Prepare` možnost, skript má několik hodin a dokončí se zprávou *Příprava dokončení. Nainstalujte certifikát pro všechny služby AD FS a proxy VMs.*

8.  Restartujte pole odkazů (*Vzdálená pomoc služby AD FS Správa vm1* ve skupině *Vzdálená pomoc služby AD FS zabezpečení rg* ) umožňuje nastavení DNS se projeví.

9.  [Získejte certifikát SSL pro služby AD FS] [ adfs_certificates] a nainstalovat VMs služby AD FS certifikát. Všimněte si, že se můžete připojit k VMs služby AD FS až do pole odkaz. IP adresy se *10.0.5.4* *10.0.5.5*. Výchozí uživatelské jméno je *contoso\testuser* heslem *AweSome@PW*.

    >[AZURE.NOTE] Skript ReferenceArchitecture.ps1 nasazení se v komentářích v tomto okamžiku poskytují podrobné pokyny k vytvoření certifikátu podepsaného svým držitelem test nebo pomocí autorita `makecert` příkaz. Ale nepoužívejte certifikátů vytvořených makecert v pracovním prostředí.

10. Spusťte konfigurace serverové farmy služby AD FS následujícího příkazu Powershellu:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. V dialogovém okně odkaz Procházet *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* k otestování instalace služby AD FS (zobrazí se upozornění certifikát, který můžete ignorovat pro tento test). Ověřte, že se zobrazí na přihlašovací stránku společnosti Contoso. Přihlaste se jako *contoso\testuser* heslem *AweS0me@PW*.

12. Nainstalujte na VMs proxy server služby AD FS certifikát SSL. IP adresy se *10.0.6.4* *10.0.6.5*.

13. Spusťte tento příkaz Powershellu ke konfiguraci první proxy server služby AD FS:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Postupujte podle instrukcí skriptem k otestování instalace první proxy server služby AD FS.

15. Spusťte tento příkaz Powershellu pro nastavení na druhý první proxy server služby AD FS:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Postupujte podle instrukcí skriptem testování dokončení konfigurace proxy server služby AD FS.

## <a name="next-steps"></a>Další kroky

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Zabezpečené hybridní architektura sítě se službou Active Directory"
