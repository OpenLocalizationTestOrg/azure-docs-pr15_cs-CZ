<properties
   pageTitle="Sledování stavu v služby struktury | Microsoft Azure"
   description="Úvod do struktury služby Azure stavu sledování model, který obsahuje sledování clusteru a jeho aplikací a služeb."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Úvod do služby struktury sledování stavu
Azure struktury služby zavádí model stavu, které obsahuje hodnocení bohaté flexibilní a extensible zdraví a vytváření sestav. Model umožňuje v reálném čase sledování stavu clusteru a služeb spuštěných v nich. Můžete snadno získat informace o stavu a opravit potenciální problémy před jejich použití kaskádových a způsobit rozsáhlé výpadků. V části typické model služby odesílat sestavy založené na místní zobrazení a že informace agregovaný poskytnout celkové obrázku úrovně zobrazení.

Tento model bohaté stavu služby struktury součásti použít vykazování jejich aktuální stav. Stejný mechanismus můžete sestavy stavu z aplikace. Pokud investovat ve stavu vysoce kvalitní vykazování, který popisuje vlastních podmínek můžete zjišťovat a řešení problémů pro spuštěnou aplikaci mnohem snadnější.

> [AZURE.NOTE] Můžeme začít podsystém stavu lze řešit třeba upgradech sledované. Služba struktury obsahuje sledované aplikace a clusteru upgradů zajištění úplné dostupnosti, bez výpadek služeb a minimální bez zásahu uživatele. K dosažení těchto cílů, upgrade zkontroluje stav na základě nakonfigurované zásady upgradu a umožňuje upgrade pokračovat, jenom když stavu s ohledem na požadované mezní hodnoty. V opačném upgradu se automaticky vrátit zpět nebo pozastavenou správcům umožňující opravit problémy. Další informace o upgrade aplikace, najdete v [tomto článku](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Stav úložiště
Úložiště stavu pořád lékařským informace o entity clusteru snadno načítání a hodnocení. Bude použita jako služby struktury zachován stavová služba ověřit dostupnost a škálovatelnost. Stav úložiště je součástí **struktury: / systém** aplikaci a je k dispozici, když je zapnut cluster a spuštěna.

## <a name="health-entities-and-hierarchy"></a>Stav entity a hierarchie
Entity stavu se seřazenými do logická hierarchie, který popisuje interakce a závislosti mezi různé entity. Při ukládání stav na základě sestav dostali ze struktury služby součástí jsou automaticky vytvořené entity a hierarchie.

Stav entity odrážet služby struktury entity. (Například **Aplikační entita stavu** odpovídá instance aplikace používaný v clusteru, zatímco **stavu uzel entity** odpovídá clusteru struktury služby.) Hierarchie stavu zaznamenává interakcí systémové entity a je základ pro vyhodnocení rozšířené stavu. Se dozvíte o klíčové služby struktury koncepty [technický přehled struktury služby](service-fabric-technical-overview.md). Další informace o aplikaci najdete v tématu [služby struktury aplikace model](service-fabric-application-model.md).

Stav entity a hierarchie umožňují obrázku a aplikací se efektivně ohlásit, ladění a sledovat. Model stavu poskytuje přesně, *podrobného* znázorňuje stav mnoho přesunutí použitelné clusteru.

![Stav entity.][1]
Stav entity se seřazenými do hierarchie podle vztahy nadřazenosti a podřízenosti.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Stav entity jsou:

- **Obrázku**. Představuje stavu služby struktury obrázku. Sestavy stavu clusteru popisují podmínky, které ovlivňují celého obrázku a nelze zúžit na jedno nebo více chybná dětí. Jako příklad lze uvést mozku clusteru rozdělení kvůli problémům se sítí oddílů nebo komunikace.

- **Uzel**. Představuje stavu služby struktury uzel. Sestavy stavu uzel popisují podmínky, které mít vliv na uzel. Obvykle ovlivňují všechny nasazeném entity spuštěno. Jako příklad lze uvést kdy uzel je odhlášení z disku mezeru (nebo jiné vlastnosti celého systému, například paměti, připojení) a uzel je dolů. Entita uzel je určen název uzlu (řetězec).

- **Aplikace**. Představuje stavu instance aplikace spuštěné v clusteru. Sestavy stavu aplikace popisují podmínky, které vliv na celkový stav aplikace. Nemůže být zúžit na jednotlivé děti (službami či aplikacemi nasazeném). Jako příklad lze uvést začátku do konce interakce mezi různé služby v aplikaci. Aplikační entita je určen název aplikace (URI).

- **Služby**. Představuje stavu služby spuštěné v clusteru. Sestavy stavu služby popisy podmínky, které ovlivňují celkový stav služby a jejich nemůže být zúžit oddíl nebo otevřené. Jako příklad lze uvést konfiguraci služby (například port nebo externí sdílené složky), který způsobuje problémy týkající se všechny oddíly. Entitu služby je určen název služby (URI).

- **Oddíl**. Představuje stavu služby oddíl. Sestavy stavu oddíl popisují podmínky, které ovlivňují sadu celý otevřené. Jako příklad lze uvést kdy číslo repliky je menší než počet cílové a oddíl je ztrátu kvora. Entita oddíl je určen oddílu Identifikátor GUID ().

- **Otevřené**. Znázorňuje stav otevřené stavová služba nebo instance příslušnosti služby. Nejmenší jednotku, která watchdogs a součástí systému můžete vykázat aplikace. Stavová služeb, jako příklad lze uvést primární otevřené hlášení při nemůže replikovat operace druhotné a v případě replikace není řízení očekávané tempu. Příslušnosti instanci můžete nahlásit taky, když není dost zdrojů nebo má problémy s připojením. Entita otevřené je určen oddílu Identifikátor GUID a ID otevřené nebo instance (dlouhé).

- **DeployedApplication**. Představuje stavu z *aplikace spuštěna na uzel*. Sestavy stavu nasazení aplikace popisují podmínky specifické pro tuto aplikaci na uzel, který nelze zúžit balíčků služeb nasazený ve stejném uzlu. Jako příklad lze uvést staženého balíčku aplikace nemůže být na uzel a po problém nastavení objekty zabezpečení aplikace na uzel. Nasazení aplikace je určen název aplikace (URI Uniform) a název uzlu (řetězec).

- **DeployedServicePackage**. Představuje stavu balíku služeb spuštěných uzel clusteru. Popisuje podmínky specifické pro balíčku služby, které nemají vliv na ostatní služby balíčků ve stejném uzlu stejné aplikace. Jako příklad lze uvést balíček kódu v balíček služby, kterou nelze spustit a konfigurace balíčku, nemůže přečíst. Balíček nasazeném služby je určen aplikace jména (URI Uniform), uzel (řetězec) a název seznamu služby (řetězec).

Rozlišení modelu stavu usnadňuje rozpoznávání a oprava problémů. Například pokud službu nereaguje, je možné zprávu, že je chybná instance aplikace. Vytváření sestav v, že není úroveň ideální, ale, protože problém nemusí být vliv na všechny služby v rámci této aplikace. Sestavy by měl použít ke službě chybná nebo konkrétní podřízený oddíl, pokud informace, odkazuje na tento oddíl. Data automaticky ploch prostřednictvím hierarchie a chybná oddíl je nastavená jako viditelný na úrovni služby a aplikace. Tento agregace umožňuje zdůraznit a příčina problém vyřešit rychleji.

Hierarchie stavu se skládá z vztahy nadřazenosti a podřízenosti. Clusteru je tvořen uzly a aplikace. Aplikace služby a nasazené aplikace. Nasazení aplikace nasadili balíčků služeb. Služby mají oddíly a každý oddíl obsahuje jednu nebo více replikách. Existuje zvláštní vztah mezi uzly a nasazené entity. -Li uzel chybná vykázání své autorita systémovou součást (převzetí Správce služby), ovlivní nasazeném aplikací, balíčků služeb a repliky používaný v něm.

Hierarchie stavu představuje poslední stav systému podle nejnovější sestav stavu, které je prakticky v reálném čase.
Vnitřní a vnější watchdogs možné vykazovat ve stejných entit na základě specifická použití logických operátorů nebo vlastních sledované podmínek. Uživatele sestav existovat společně se sestavami systému.

Plánování investovat v článku jak vykazování zpráv a odpovídání na stav průběhu návrhu velké cloudové služby, aby služba líp ladění, sledovat a pracovat.

## <a name="health-states"></a>Stav státy
Struktury služby využívá k popisu, jestli je nebo není správný entita třech stavech stavu: OK, upozornění a chyb ve vzorcích. Každé zprávy odeslané do úložiště stavu zadejte jednu z těchto států. Výsledek vyhodnocení stavu je jedním z těchto států.

Možné [státy stavu](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) patří:

- **OK**. Entita je funkční. Neexistují známé problémy vykázanému nebo jeho podřízeným objektům (je-li k dispozici).

- **Upozornění**. Entita dojde některé problémy, ale zatím není chybná (například je, ale nezpůsobí ještě všech problémů, funkční). V některých případech podmínka upozornění samotné stanovit bez zásahu jinak a je užitečný pro poskytují přehled o co se děje. V ostatních případech může podmínka upozornění snížit do špatných problém bez zásahu uživatele.

- **Chyba**. Entita je chybná. Opatření opravíte stav entitu, protože nefunguje správně.

- **Neznámý**. V úložišti stavu neexistuje entity. Tento výsledek lze získat z distribuované dotazů, které sloučit výsledky z více složek. Například získáte dotazu seznamy uzel přejde na **FailoverManager** a **HealthManager**, získání aplikace seznam dotazu, půjde **ClusterManager** a **HealthManager**. Tyto dotazy sloučit výsledky z více součásti systému. Pokud jiný systémovou součást vrátí entitu, která nedosáhl nebo byly vyčistí z obchodu stavu, výsledného má neznámý stav.

## <a name="health-policies"></a>Zásady stavu
Úložiště stavu platí zásady stavu k určení entita správný podle svých sestav a jeho podřízeným objektům.

> [AZURE.NOTE] Zásady stavu může být zadán v clusteru instalované (obrázku a uzel stavu hodnocení) nebo v aplikaci instalované (hodnocení aplikace a všech podřízených). Žádostí o zkušební stavu můžete taky předat zásady hodnocení vlastní stavu, které se používá pouze pro hodnocení.

Ve výchozím nastavení služby struktury slouží k použití striktních pravidel (všechno, co musí být správný) pro hierarchické vztahy nadřazenosti a podřízenosti. Pokud ještě jedna dětí obsahuje jednu chybná událost, nadřazeného je považován za chybná.

### <a name="cluster-health-policy"></a>Zásady stavu obrázku
[Zásady stavu clusteru](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) slouží k vyhodnocení uzel stavu stavy a stavu obrázku. Zásady možné definovat ve manifest obrázku. Pokud nenachází, použije se výchozí zásady (nulové povolenou selhání).
Zásady stavu clusteru obsahuje:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Určuje, zda zacházet s upozorněním stavu sestavy jako chybné při vyhodnocování stavu. Výchozí: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Určuje maximální povolenou procento aplikace, které můžete předcházet chybná clusteru je považován za zobrazuje chyba.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Určuje maximální povolenou procento uzlů, které můžete předcházet chybná clusteru je považován za zobrazuje chyba. Ve velkých clusterů některé uzly jsou vždy dolů či oddálení pro opravy, tak tento procento by měl být nakonfigurované pro tolerovat.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Zásady mapy aplikací typ stavu lze při vyhodnocování stavu clusteru popisují typy zvláštní aplikací. Ve výchozím nastavení jsou všechny aplikace přepněte do fondu a vyhodnocena s MaxPercentUnhealthyApplications. Pokud některé typy aplikací zacházet odlišně, lze pořizovat z fondu globální. Místo toho jsou vyhodnoceny proti procent spojené s jeho názvem typu aplikace v mapě. Například v clusteru mívají tisíce aplikací různých typů několika instancí aplikace ovládacího prvku typu zvláštní aplikace. Ovládací prvek aplikace by nikdy neměla být zobrazuje chyba. Můžete zadat globální MaxPercentUnhealthyApplications 20 % tolerovat některých chyb, ale pro typ aplikace, které "ControlApplicationType" MaxPercentUnhealthyApplications na hodnotu 0. Tímto způsobem, pokud některé z mnoha aplikací jsou chybná, ale pod globální chybná procento, clusteru má být vyhodnocen jako upozornění. Upozornění stavu nemá vliv na upgrade obrázku nebo jiné sledování spouštěný kliknutím chyby stavu. Ale i jeden ovládací prvek aplikaci chyby by cluster chybná, který spustí vrátit zpět změny nebo pozastaví upgrade obrázku v závislosti na upgrade konfigurace.
Pro typy aplikací definované v mapě všechny instance aplikace přesměrováni mimo globální fondu aplikací. Jsou vyhodnoceny podle celkový počet aplikace typu aplikace pomocí konkrétní MaxPercentUnhealthyApplications z mapy. Ostatní aplikace, zůstanou v globální fondu a jsou vyhodnoceny s MaxPercentUnhealthyApplications.

V následujícím příkladu je úryvek z manifestu obrázku. Definování položky v mapě typ aplikace, předpona název parametru s "ApplicationTypeMaxPercentUnhealthyApplications-", následovaný zadejte název aplikace.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Zásady stavu aplikace
[Zásady použití stavu](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) popisuje, jak to dělá hodnocení událostí a agregace podřízené státy pro aplikace a jeho podřízeným objektům. Je to možné definovat ve manifest aplikace, **ApplicationManifest.xml**balíček aplikace. Jsou-li žádné zásady, struktury služby předpokládá, že entitu chybná, pokud má sestavy stavu nebo nadpis na podřízené úrovni na stavu upozornění nebo chyby.
Konfigurovat zásad jsou:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Určuje, zda zacházet s upozorněním stavu sestavy jako chybné při vyhodnocování stavu. Výchozí: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Určuje maximální povolenou procento nasazeném aplikace, které můžete předcházet chybná aplikace je považován za zobrazuje chyba. Procentuální hodnota se vypočítá vydělením počtu nefunkčních aplikací nasazena přes počtu uzlů, které aplikace jsou v současnosti nasazená na clusteru. Výpočtu zaokrouhluje nahoru k tolerovat jeden selhání na malé počtu uzlů. Výchozí procentuální hodnoty: nula.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Určuje služby typ stavu zásady, které nahrazují výchozí zásady stavu pro všechny typy služeb v aplikaci.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Poskytuje mapy zásad stav služby na typ služby. Tyto zásady nahraďte výchozí zásady typ stavu služby každý zadaný typ služby. Například pokud aplikace typu služby příslušnosti brány a typ služby stavového stroje, můžete nakonfigurovat zásady stavu pro jejich hodnocení jinak. Při určování zásad za typ služby můžete získat přesnější kontrolu stavu služby.

### <a name="service-type-health-policy"></a>Zásady typu stavu služby
[Zásady stavu služby typu](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) Určuje, jak jsou vyhodnoceny a agregovat služeb a děti služeb. Zásady obsahuje:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Určuje maximální povolenou procento chybná oddíly před služby je považován za chybná. Výchozí procentuální hodnoty: nula.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Určuje maximální povolenou procento chybná repliky před oddíl se považuje za nebezpečné. Výchozí procentuální hodnoty: nula.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Určuje maximální povolenou procento chybná služeb před aplikace se považuje za nebezpečné. Výchozí procentuální hodnoty: nula.

V následujícím příkladu je výpisem z manifest aplikace:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Hodnocení stavu
Uživatele a automatické služby můžete kdykoli vyhodnotit stav všech entity. K vyhodnocování entity stavu, agregace úložiště stav všech stavu zprávy o entita a vyhodnotí všech podřízených (je-li k dispozici). Algoritmus agregace stavu používá zásady stavu, které určují způsob vyhodnocení sestav stavu a jak agregovat podřízené stavu USA (je-li k dispozici).

### <a name="health-report-aggregation"></a>Agregace sestav stavu
Jedna osoba může obsahovat více sestav stavu odeslány pomocí různých jednotky vykazující (součásti systému nebo watchdogs) na různé vlastnosti. Agregace používá zásady přidružené stavu zejména ConsiderWarningAsError členem zásad stavu aplikace nebo obrázku. ConsiderWarningAsError Určuje, jak chcete zjistit upozornění.

Souhrnné stavu je spouštěný kliknutím *nejhorší* sestav stavu entity. Pokud je nejméně jeden chybách stavu, agregované stavu je chyba.

![Agregace sestavy stavu s zprávu o chybách.][2]

Stav osoba, která obsahuje jednu nebo víc sestav stavu chyby je vyhodnocena jako chyba. Platí totéž i pro zprávu o stavu vypršela platnost, bez ohledu na jeho stavu.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Pokud nejsou žádné zpráv o chybách a jedno nebo více upozornění, je agregované stavu upozornění nebo chyba, v závislosti na příznaku zásad ConsiderWarningAsError.

![Agregace sestavy stavu s grafem upozornění a ConsiderWarningAsError NEPRAVDA.][3]

Stav agregace sestavu s grafem upozornění a ConsiderWarningAsError nastavena na hodnotu false (výchozí).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Podřízené stavu agregace
Souhrnné stavu entita odráží podřízené bezpečnostní stavy (je-li k dispozici). Algoritmus pro agregaci podřízené stavu státy používá příslušných stavu zásady založený na typu entity.

![Podřízené entity stavu agregace.][4]

Podřízené agregace v závislosti na zásadách stavu.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Po úložišti stavu vyhodnotil všechny podřízené, sloučí obnovit stav stavu podle nakonfigurovaného maximální procento chybná dětí. Toto procento, se považuje ze zásad v závislosti na typu entita a podřízené.

- Všechny podřízené OK států, stavu podřízené agregované je OK.

- Máte děti OK a států upozornění upozornění stavu podřízené agregované.

- Pokud jsou děti s chyby stavy, které neodpovídají maximální povolenou procento chybná dětí, agregované stavu je chyba.

- Pokud děti s chyby státy dodržovat maximální povolený procento chybná dětí, je agregované stavu upozornění.

## <a name="health-reporting"></a>Vykazování stavu
Součásti systému, aplikací systému struktury a watchdogs interní/externí můžete vykázat služby struktury entity. Jednotky vykazující proveďte *místní* zjištění stavu sledované entity na základě podmínek, které jsou sledování. Není potřeba podívejte se na každý globální stát nebo agregovat data. Požadované chování je jednoduchý jednotky vykazující a ne složité organismy, které je potřeba zkontrolovat mnoho věci odvodit jaké informace chcete odeslat.

Odeslání stavu dat k úložišti stavu, musí zpravodaj identifikovat problémového entita a vytvářet sestavy stavu. Sestava potom můžete odeslat prostřednictvím rozhraní API pomocí [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx), pomocí prostředí PowerShell nebo prostřednictvím ZBÝVAJÍCÍ.

### <a name="health-reports"></a>Sestavy stavu
[Sestavy stavu](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) pro každý entit clusteru obsahují následující informace:

- **ID zdroje**. Řetězec, který jednoznačně identifikuje zpravodaj událost stavu.

- **Identifikátor entity**. Určuje použití sestavy entity. Liší se podle [entity zadejte](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Obrázku. Žádná.

  - Uzel. Název uzlu (řetězec).

  - Aplikace. Název aplikace (URI). Představuje název instance aplikací nasazena clusteru.

  - Služba. Service název (URI). Představuje název instance služby nasazenou v clusteru.

  - Oddíl. Oddíl Identifikátor (GUID). Představuje jedinečný identifikátor oddíl.

  - Otevřené. Stavová služba otevřené ID nebo ID instance příslušnosti služby (INT64).

  - DeployedApplication. Název aplikace (URI Uniform) a název uzlu (řetězec).

  - DeployedServicePackage. Název (řetězec) manifestu aplikace jména (URI Uniform), uzel (řetězec) a služby.

- **Vlastnost**. *Řetězec* (ne pevné výčet), která umožňuje zpravodaj zařadit do nějaké kategorie události stavu pro určitou vlastnost entity. Například zpravodaj A můžete nahlásit že stavu vlastnost Node01 "úložiště" a zpravodaj B mohli vykazovat stav vlastnost Node01 "připojením". V úložišti stavu jsou tyto sestavy považovány za události samostatné stavu entity Node01.

- **Popis**. Řetězec, který umožňuje zpravodaj obsahuje podrobné informace o stavu události. **ID zdroje**, **Vlastnosti**a **HealthState** plně popisují sestavy. Popis přidá čitelné informace o sestavě. Text usnadňuje správcům a uživatelům zprávu o stavu.

- **HealthState**. [Výčet](service-fabric-health-introduction.md#health-states) , který popisuje stavu sestavy. Přijaté hodnoty jsou OK, upozornění a chyby.

- **TimeToLive**. Časový interval, který označuje, jak dlouho sestava stavu je platný. Spolu s **RemoveWhenExpired**umožňuje úložišti stavu vědět, jak chcete zjistit vypršela platnost události. Ve výchozím nastavení je argument hodnota nekonečné a sestavy je platný trvale.

- **RemoveWhenExpired**. Logická hodnota. Pokud je nastavena na hodnotu true, sestava vypršela platnost stavu automaticky odeberou z úložišti zdraví a sestavy nemá vliv hodnocení stavu entity. Vyvolají sestavu platí stanovený počet minut jenom a zpravodaj nevyžaduje explicitně vymazání. Slouží také k odstranění sestavy z obchodu stavu (například sledovacích dojde ke změně a ukončí odesílání sestavy s předchozí zdroj a vlastnosti). Můžete poslat zprávu s stručný TimeToLive spolu s RemoveWhenExpired zrušte předchozí stavu z obchodu stavu. Pokud je hodnota NEPRAVDA, vypršela platnost sestavy zpracován jako chyba týkající se stavu hodnocení. Hodnotu false signalizuje k úložišti stavu, že zdroje vykazují pravidelně na tuto vlastnost. Pokud ne, je nutné něco správné snažit se sledovacích. Stav sledovacích zachyceny zvažovat událost jako chyba.

- **SequenceNumber**. Kladné celé číslo, které není nutné stále narůstající, představuje pořadí sestavy. Podle stavu úložiště slouží ke zjištění zastaralé sestavy, které jsou doručeny pozdě kvůli zpoždění sítě nebo jiné problémy. Pokud je pořadové číslo že menší nebo rovna nejčastěji naposledy použité číslo pro stejný subjekt, zdroje a vlastnost odmítnutý sestavy. Pokud není zadán, je automaticky generované pořadové číslo. Je třeba umístit pořadové číslo pouze ve zprávě o stavu přechody. V takovém případě zdroji musí pamatovat které sestavy se odesílají a zachovat informace pro obnovení o převzetí.

Tyto čtyři použitelné informací – jsou potřeba pro každý sestava stavu ID zdroje, identifikátor entity, vlastnosti a HealthState –. ID zdroje, které řetězec není povolená začínat předpona "**systém.**", který je rezervován pro systém sestavy. Pro stejný subjekt existuje pouze jedné sestavy pro stejný zdroj a vlastnosti. Víc sestav pro stejný zdroj a vlastnost přepsat sobě, buď na straně klienta stavu (pokud jsou dávce) nebo o stavu ukládání straně. Nahrazení je založená na pořadová čísla; novější sestavy (s vyšší pořadová čísla) nahraďte starší sestavy.

### <a name="health-events"></a>Události stavu
Interně úložišti stavu pořád [události stavu](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), které obsahují všech informací ze zprávy a další metadata. Metadata obsahuje dobu přidělenou sestavy ke klientovi zdraví a čas, kdy byl upraven na straně serveru. Události stavu jsou vrácená [stavu](service-fabric-view-entities-aggregated-health.md#health-queries).

Přidané metadata obsahuje:

- **SourceUtcTimestamp**. Čas předaná sestavy stavu klienta (Coordinated Universal Time).

- **LastModifiedUtcTimestamp**. Čas, kdy sestavu naposledy změnil na straně serveru (Coordinated Universal Time).

- **IsExpired**. Příznak označující, zda sestavu vypršelo při ověříte spuštění dotazu v úložišti stavu. Zvláštní události můžete platnost jenom v případě, že RemoveWhenExpired vyhodnotí jako NEPRAVDA. V opačném událost není vrácená dotazem a se odebere z úložiště.

- **LastOkTransitionAt**, **LastWarningTransitionAt** **LastErrorTransitionAt**. Čas poslední pro přechody OK / / chybové upozornění. Tato pole Zobrazit historie zdravotní stav přechody události.

Pole stavu přechodu lze použít pro efektivnější upozornění nebo informace o událostech "historických" stavu. Scénáře, jako umožňují:

- Upozornění při vlastnost bylo v upozornění nebo Chyba víc než X min. Kontrola stavu u určitého časového období zabráněno upozornění na dočasné podmínky. Například upozornění Pokud stavu byla upozornění dobu delší než pět minut můžete převést na (HealthState == upozornění a teď - LastWarningTransitionTime > 5 minut).

- Upozornění jenom na podmínky, které byly změněny za poslední X min. Pokud sestavy už na chyby před zadané doby, můžete ho ignorovat, protože byl již signál dříve.

- Pokud je vlastnost přepínání mezi upozornění a chyby, zjistit, jak dlouho byly chybná (to znamená špatně). Například upozornění Pokud vlastnost není správný dobu delší než pět minut můžete převést na (HealthState! = Ok a teď - LastOkTransitionTime > 5 minut).

## <a name="example-report-and-evaluate-application-health"></a>Příklad: Sestav a vyhodnocování stavu aplikace
Následující příklad odešle zprávu o stavu pomocí prostředí PowerShell o používání **struktury: / WordCount** ze zdroje **MyWatchdog**. Sestava stavu obsahuje informace o stavu vlastnost "dostupnost" v chybovém stavu stavu, s nekonečné TimeToLive. Potom dotazů aplikace stavu, který vrátí agregované zdravotní stav chyby a události nahlášeného stavu v seznamu události stavu.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Použití modelu stavu
Model stavu umožňuje cloudovými službami a platformu struktury služby základní zobrazit, protože sledování a stav rozděleny mezi různými monitory v rámci clusteru.
Jiných systémů službu z jediné, centralizované úrovni obrázku, které rozdělí všechny *potenciálně* užitečné informace které služby. Tento přístup omezuje jejich škálovatelnost. Je taky nedovoluje shromažďování specifických informací k identifikaci problémy a potenciální problémy jako zavřít příčina nejvíce podporovat.

Model stavu slouží velkém pro monitorování a Diagnostika pro vyhodnocení obrázku a použití stavu a upgradech sledované. Další služby data stavu umožňuje provádět automatické opravy, vytváření historie zdravotní obrázku a problémů upozornění za určitých podmínek.

## <a name="next-steps"></a>Další kroky
[Zobrazení sestav stavu služby struktury](service-fabric-view-entities-aggregated-health.md)

[Odstraňování potíží s pomocí sestav stavu systému](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Jak sestavy a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Přidání vlastních sestav stavu služby struktury](service-fabric-report-health.md)

[Sledování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace služby struktury](service-fabric-application-upgrade.md)
