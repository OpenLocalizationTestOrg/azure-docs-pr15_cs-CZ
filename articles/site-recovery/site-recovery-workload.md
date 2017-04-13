<properties
    pageTitle="K čemu úloh můžete chránit pomocí obnovení webu Azure?"
    description="Obnovení Azure webu chrání pracovního vytížení a aplikací koordinaci replikace, převzetí a obnovení místní virtuálních počítačích a fyzické servery Azure nebo vedlejší místního serveru"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>K čemu úloh můžete chránit pomocí obnovení webu Azure?


Tento článek popisuje pracovního vytížení a aplikace, které lze replikovat se službou Azure obnovení webu.

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="overview"></a>Základní informace

Organizace potřebovat strategii firmy kontinuitu a havárie obnovení (BCDR) pracovního vytížení a data bezpečných a k dispozici během zachovat plánované a neplánované výpadek služeb a obnovit na běžná pracovní podmínky co nejdříve.

Obnovení webu je služba Azure podíl strategie BCDR. Použití webu obnovení nástroje můžete nasazovat podporujících aplikaci replikace v cloudu nebo na vedlejší Web. Jestli vaše aplikace nacházejí Windows nebo na základě Linux, pracovního serverech fyzické, VMware nebo Hyper-V, můžete obnovení webu organizovat replikace, proveďte havárie testování obnovení a spuštění převzetí služeb při selhání a navrácení.


Obnovení webu lze integrovat s aplikací Microsoft, včetně Sharepointu, Exchange, Dynamics, SQL Server a služby Active Directory. Microsoft také úzce spolupracuje s počáteční dodavatelé včetně Oracle SAP, IBM a červené klobouk. Můžete přizpůsobit replikace řešení pro jednotlivé aplikace tak, že aplikace.

## <a name="why-use-site-recovery-for-application-replication"></a>Proč používat obnovení webu replikace aplikace?

Obnovení webu přispívá k ochraně úrovni aplikace a obnovení následujícím způsobem:

- Aplikace bez ohledu na, poskytnutí replikace pro všechny pracovní vytížení podporované počítače.
- V blízkosti synchronní replikace s RPOs co nejnižší 30 sekund potřebám nejdůležitější obchodních aplikací.
- Aplikace konzistentních snímků pro jednoho nebo vícevrstvého aplikace.
- Integrace se službou SQL Server AlwaysOn a partnerství s jinými úrovni aplikace replikace technologiemi, včetně AD replikace SQL AlwaysOn, skupiny dostupnost databáze Exchange (DAGs) a stráž dat Oracle.
- Flexibilní obnovení plánů, které umožňují obnovit celou aplikaci zásobníku jedním kliknutím, zahrnout zahrnout externí skripty a ruční akce v plánu.
- Rozšířené možnosti správy sítě obnovení webu a Azure zjednodušit požadavky aplikace sítě, včetně možnost rezervovat IP adresy nakonfigurujte Vyrovnávání zatížení a integrace s správce přenosy Azure zhoršeným switchovers RTO sítě.
-  Knihovna bohaté automatizaci poskytující řešení připravené na výrobní specifické pro aplikaci skriptů, které si můžete stáhnout a integrovaný s obnovení plány.



## <a name="workload-summary"></a>Souhrnné zátěží na projektu

Obnovení webu můžete replikovat nějakou aplikaci spuštěné v počítači podporované. Kromě toho jsme jste stala partnerem produktu týmy provádět další testování specifické pro aplikaci.

**Pracovní zátěž** | **Replikovat VMs Hyper-V sekundární Web** | **Replikovat VMs Hyper-V Azure** | **Replikovat VMware VMs sekundární Web** | **Replikace VMware VMs na Azure**
---|---|---|---|---
Služby Active Directory, DNS | Y | Y | Y | Y
Web apps (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
Služby SharePoint | Y | Y | Y | Y
SAP<br/><br/>Replikovat SAP server Azure pro bez obrázku | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft)
Exchange (bez DAG) | Y | Již brzy | Y | Y
Vzdálené plochy/VDI | Y | Y | Y | NENÍ K DISPOZICI
Linux (operačního systému a aplikací) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Již brzy | Y | Již brzy
Oracle | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft) | Y (testováno společnost Microsoft)
Soubor systému Windows Server | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Replikovat Active Directory a DNS

Jsou zásadní pro většinu podnikových aplikací služby Active Directory a infrastruktury služby DNS. Během obnovení havárie musíte ochrana a obnovit tyto součásti infrastruktury před obnovením pracovního vytížení a aplikace.

Obnovení webu slouží k vytvoření plánu havárie úplné automatické obnovení služby Active Directory a DNS. Například, pokud chcete selhání prostřednictvím služby SharePoint a SAP z primárního sekundární web, můžete nastavit obnovení plán, který nejdřív selhání služby Active Directory a další aplikace pro obnovení plán překlopení dalších aplikací, které jsou závislé na služby Active Directory.

[Další informace](site-recovery-active-directory.md) o ochraně služby Active Directory a DNS.

## <a name="protect-sql-server"></a>Ochrana SQL serveru

SQL Server obsahuje foundation services dat pro datové služby pro mnoho obchodních aplikací v datovém centru místní.  Obnovení webu lze spolu s technologií SQL serveru HA/DR chránit vícevrstevného podnikových aplikací, které používají SQL serveru. Obnovení webu poskytuje:

- Jednoduchý a efektivní havárie obnovovací řešení pro systém SQL Server. Replikovat více verzí a edice systému SQL Server – samostatné servery a clusterů Azure nebo vedlejší webu.  
- Integrace se službou SQL AlwaysOn dostupnost skupinám tak, aby Správa převzetí a navrácení plánech obnovení obnovení webu Azure.
- Obnovení začátku do konce plány jednotného zasílání zpráv pro všechny úrovně v aplikaci, včetně databáze systému SQL Server.
- Měřítko SQL serveru ve špičce načte s obnovení webu tak, že "roztržení" je do větších IaaS virtuálního počítače v Azure.
- Testování snadné obnovení havárie SQL serveru. Spuštěním testovací převzetí služeb při selhání k analýze dat a spuštění kontroly kompatibility bez vlivu na provozním prostředí.

[Další informace](site-recovery-sql.md) o ochraně SQL serveru.

##<a name="protect-sharepoint"></a>Ochrana služby SharePoint

Obnovení Azure webu chrání při nasazení Sharepointu, následujícím způsobem:

- Eliminuje potřeb a přidružená infrastruktura nákladů na farmě samostatné podle havárie obnovení. Obnovení webu umožňuje replikovat celou farmu (Web app a databáze úrovní) Azure nebo vedlejší webu.
- Zjednodušuje nasazení aplikace a správy. Aktualizace nasazení primární web automaticky replikovat a tedy po jsou dostupné převzetí a obnovení farmy na vedlejší webu. Sníží taky správy složitostí i náklady spojené s farmě samostatné podle udržovat aktuální.
- Zjednodušuje vývoj aplikací služby SharePoint a testování vytvořením prostředí výrobní profesionálové kopie na vyžádání otevřené pro testování a ladění.
- Přechod do cloudu zjednodušuje pomocí obnovení webu migrace při nasazení Sharepointu do Azure.

[Další informace](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) o ochraně SharePoint.


## <a name="protect-dynamics-ax"></a>Ochrana Dynamics AX

Obnovení Azure webu pomáhá chránit vaše řešení Dynamics AX ERP podle:

- Orchestrating replikace celý Dynamics AX prostředí (Web a AOS úrovní, databáze úrovní Sharepointu) Azure nebo vedlejší webu.
- Zjednodušení migrace Dynamics AX nasazení s cloudem (Azure).
- Zjednodušení vývoj aplikací Dynamics AX a testování vytvořením kopírovat výrobní profesionálové na vyžádání k testování a ladění.

[Další informace](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) o ochraně dynamické AX.

## <a name="protect-rds"></a>Ochrana služby Remote Data Service

Vzdálené plochy Services (služby Remote Data Service) umožňuje infrastruktura virtuálních (klientských počítačů VDI), na základě relace plochy a aplikací umožňuje uživatelům pracovat kdekoli. Obnovení webu Azure můžete:

- Spravované nebo nespravované fondu virtuální plochy replikujte na vedlejší web a vzdálené aplikace a relace k sekundární webu nebo Azure.
- To, co můžete replikovat:

**SLUŽBY REMOTE DATA SERVICE** | **Replikovat VMs Hyper-V sekundární Web** | **Replikovat VMs Hyper-V Azure** | **Replikovat VMware VMs sekundární Web** | **Replikace VMware VMs na Azure** | **Replikace fyzické servery na vedlejší webu** | **Replikace fyzické servery na Azure**
---|---|---|---|---|---|---
**Fondu virtuální plocha (nespravované)** | Ano | Ne | Ano | Ne | Ano | Ne
**Fondu virtuální plocha (spravovaných i bez UDP)** | Ano | Ne | Ano | Ne | Ano | Ne
**Vzdálené aplikace a stolní cvičení (bez UDP)** | Ano | Ano | Ano | Ano | Ano | Ano


[Další informace](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) o ochraně RDS.


## <a name="protect-exchange"></a>Ochrana Exchange

Obnovení webu chrání Exchange, následujícím způsobem:

- Malé nasazení Exchange, jako je jedna nebo samostatné servery obnovení webu replikovat a převzetí Azure nebo vedlejší webu.
- Obnovení webu lze integrovat s Exchange DAGS větší nasazení.
- Exchange DAGs jsou doporučené řešení pro obnovení havárie serveru Exchange v podniku.  Plány obnovy obnovení webu mohou obsahovat DAGs k organizovat DAG převzetí na webech.


[Další informace](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) o ochraně Exchange.

## <a name="protect-sap"></a>Ochrana SAP

Obnovení webu umožňuje chránit nasazením SAP, následujícím způsobem:

- Povolení ochrany celé nasazení SAP replikace různých nasazení vrstvy Azure nebo vedlejší webu.
- Zjednodušte cloudu migrace pomocí obnovení webu migrace SAP nasazení do Azure.
- Zjednodušení SAP vývoje a testování vytvořením výrobní profesionálové zkopírujte na vyžádání testování a ladění aplikací.

[Další informace](http://aka.ms/asr-sap) o ochraně SAP.

## <a name="next-steps"></a>Další kroky

[Příprava na nasazení obnovení webu](site-recovery-best-practices.md) 
