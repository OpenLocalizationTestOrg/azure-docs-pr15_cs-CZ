<properties
   pageTitle="Použití Elasticsearch jako úložiště služby struktury aplikace trasování | Microsoft Azure"
   description="Popisuje, jak můžete služby struktury aplikace pomocí Elasticsearch a Kibana ukládat, index, a prohledávat aplikace trasování (protokoly)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Použití Elasticsearch jako trasování aplikace služby struktury ukládání
## <a name="introduction"></a>Úvod
Tento článek popisuje, jak [Struktury služby Azure](https://azure.microsoft.com/documentation/services/service-fabric/) aplikace mohou použít **Elasticsearch** a **Kibana** pro uložení sledování aplikace indexování a vyhledávání. [Elasticsearch](https://www.elastic.co/guide/index.html) je otevřít zdroj distribuované a scalable v reálném čase vyhledávání a technologie pro analýzu modul, který je vhodné pro daný úkol. Možné nainstalovat na virtuálních počítačích Windows a Linux spuštěné v Microsoft Azure. Elasticsearch můžete efektivně zpracovat *strukturovaných* trasování vytvořené pomocí technologií například **Události trasování pro Windows (trasování událostí pro Windows)**.

Trasování událostí pro Windows používanou runtime služby struktury zdroj diagnostické informace (stopy). Je doporučeno pro službu struktury aplikace jako zdroj diagnostické informace o jejich příliš. Použití stejné mechanismus umožňuje korelace předávaných runtime a poskytované aplikací trasování a něco v ní při odstraňování problémů. Šablony projektů služby struktury ve Visual Studiu zahrnout API protokolování (na základě .NET **Zdroj_události** třídy) posílá trasování trasování událostí pro Windows ve výchozím nastavení. Obecné základní informace o sledování aplikací služby struktury pomocí trasování událostí pro Windows najdete v článku [sledování a diagnostice služby v nastavení vývoj místního počítače](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Trasování objevit v Elasticsearch budou muset nezaznamenávají na uzlech struktury služby v reálném čase (aplikace je spuštěná) a odešle Elasticsearch koncový bod. Pro sledování zachycení dva hlavní způsoby:

+ **Proces sledování zachycení**  
Aplikace nebo přesněji proces služby je zodpovědný za odesláním diagnostiky data, která chcete sledování úložiště (Elasticsearch).

+ **Zachycení mimo proces sledování**  
Samostatné agent zachycení od proces služby nebo procesy a posílat úložišti sledování.

Tady vidíte jsme popisují, jak nastavit Elasticsearch na Azure, diskutovat o odborníků a nevýhody obou zachytit možnosti a je vysvětleno, jak nakonfigurovat službu služby struktury odeslání dat do Elasticsearch.


## <a name="set-up-elasticsearch-on-azure"></a>Nastavení Elasticsearch na Azure
Nejjednodušší způsob, jak nastavit službu Elasticsearch na Azure je prostřednictvím [**Správce prostředků Azure šablon**](../azure-resource-manager/resource-group-overview.md). Úplný [Správce prostředků Azure rychlý úvod šablony pro Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) je k dispozici rychlý úvod Azure šablony úložiště. Tato šablona používá samostatné úložiště účty pro jednotkách (skupiny uzlů). Také je možné zajistit samostatné klienta a serveru uzly s různé konfigurace a různých čísel disků dat připojených.

Tady budeme používat jinou šablonu s názvem **ES MultiNode** z [úložiště Azure diagnostických nástrojů](https://github.com/Azure/azure-diagnostics-tools). Tato šablona je snadněji používaly a vytvoří clusteru služby Elasticsearch chráněny HTTP základní ověřování. Než budete pokračovat, stáhněte si úložišti z GitHub na vašem počítači (podle klonováním úložiště nebo stahování souboru zip). Šablona ES MultiNode se nachází ve složce se stejným názvem.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Připravit počítač tak, aby spouštěly skripty Elasticsearch instalace
Nejjednodušší způsob, jak používat šablonu ES MultiNode je prostřednictvím poskytnutého skriptu prostředí PowerShell Azure s názvem `CreateElasticSearchCluster`. Použít tento skript, musíte nainstalovat moduly prostředí PowerShell a nástroji s názvem **openssl**. Ten je potřeba při vytváření SSH klíč, který slouží ke správě svůj cluster Elasticsearch vzdáleně.

`CreateElasticSearchCluster`skript je určený pro snadnější použití šablonou ES MultiNode z počítače Windows. Je možné použít šablonu na počítač systému Windows, ale tento scénář je nad rámec tohoto článku.

1. Pokud jste nenainstalovali je už, nainstalujte [**prostředí PowerShell Azure moduly**](http://aka.ms/webpi-azps). Po zobrazení výzvy klikněte na **Spustit**, pak **instalovat**. Vyžaduje se Azure Powershellu 1.3 nebo novější.

2. Nástroj **openssl** je součástí rozdělení [**Libovolná pro Windows**](http://www.git-scm.com/downloads). Pokud jste to ještě neudělali, nainstalujte teď [Libovolná pro systém Windows](http://www.git-scm.com/downloads) . (Výchozí možnosti instalace jsou v pořádku.)

3. Za předpokladu, že libovolná byla nainstalovaný, ale není součástí systému cesty, otevřete okno Microsoft Azure PowerShell a následující příkazy:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Nahrazení `<Git installation folder>` libovolná umístění ve vašem počítači; Výchozí hodnota je **"C:\Program Files\Git"**. Poznámka: středník na začátek první cesty.

4. Ujistěte se, že jste přihlášeni k Azure (přes [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) rutina) a že jste vybrali předplatné, které mají být použity k vytvoření pružná hledání obrázku. Můžete ověřit, že je vybraná správná předplatného pomocí `Get-AzureRmContext` a `Get-AzureRmSubscription` rutiny.

5. Pokud jste to ještě neudělali, přejděte do složky ES MultiNode aktuálního adresáře.

### <a name="run-the-createelasticsearchcluster-script"></a>Spustit skript CreateElasticSearchCluster
Před spuštěním skript, otevřete `azuredeploy-parameters.json` souborů a ověřte nebo zadejte hodnoty parametrů skriptu. Následujících parametrů jsou k dispozici:

|Název parametru           |Popis|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Název, který slouží k vytvoření veřejně zobrazovaný název DNS pružná hledání clusteru (přidáním domain Azure oblasti k zadané jméno). Například pokud je tato hodnota parametru "myBigCluster" a zvolené Azure oblast je západní USA, název výsledné DNS clusteru je myBigCluster.westus.cloudapp.azure.com. <br /><br />Tento název slouží také jako název kořenové pro mnoho artefakty přidružené pružná hledání obrázku, například názvy uzlů data.|
|adminUsername           |Název účtu správce pro správu clusteru pružná hledání (odpovídající SSH klíčů automaticky).|
|dataNodeCount           |Počtu uzlů v clusteru pružná vyhledávání. Aktuální verze skript nerozlišuje mezi uzly dat a dotazů. všechny uzly přehrát obě role. Výchozí hodnota je 3 uzlů.|
|dataDiskSize            |Velikost dat disků (GB) přidělený pro každý uzel data. Každý uzel obdrží 4 disků dat výhradně pružná vyhledávací služby serveru.|
|oblast                  |Název Azure oblast místo, kam mají být umístěny pružná hledání obrázku.|
|esUserName              |Uživatelské jméno uživatele, který je nakonfigurovaný přístup k ES obrázku (vyměřené poplatky za jeho základní ověřování HTTP). Heslo není součástí souboru parametrů a třeba zadat při `CreateElasticSearchCluster` skript spouštěn.|
|vmSizeDataNodes         |Velikost Azure virtuálního počítače uzlech pružná vyhledávání. Výchozí nastavení Standard_D2.|

Teď jste připraveni následujícím způsobem. Problému tento příkaz:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

kde 

|Název parametru skriptu    |Popis|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Název skupiny Azure zdroje, který bude obsahovat všechny zdroje obrázku pružná vyhledávání.|
|`<azure-region>`         |Název oblasti Azure tam, kde by měl vytvořili pružná hledání obrázku.|         
|`<es-password>`          |Heslo uživatele pružná vyhledávání.|

>[AZURE.NOTE] Pokud se zobrazí NullReferenceException z rutinu Test AzureResourceGroup jste zapomněli pro přihlášení k Azure (`Add-AzureRmAccount`).

Pokud dojde k chybě spustila skript a zjistíte, že je chyba způsobená hodnoty parametru nepovedlo šablony, opravte parametrem soubor a znova spusťte skript s názvem jiný zdroj. Můžete taky znovu použít stejný název skupiny zdrojů a skript vyčistit starý přidáním `-RemoveExistingResourceGroup` parametr vyvolání skriptu.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Výsledek spuštění CreateElasticSearchCluster skriptu
Po spuštění `CreateElasticSearchCluster` skriptu následující hlavní artefakty se vytvoří. V tomto příkladu jsme se předpokládá, že jste použili "myBigCluster" jako hodnotu `dnsNameForLoadBalancerIP` parametry a oblasti, které jste vytvořili clusteru je západní US.

|Artefakt|Název, umístění a poznámky|
|----------------------------------|----------------------------------|
|SSH klíč pro vzdálená správa |myBigCluster.key soubor (v adresáři, ze kterého byla CreateElasticSearchCluster spustit). <br /><br />Tento soubor klíče slouží k připojení k uzlu správce a (přes uzel správce) do dat uzlů v clusteru.|
|Správce uzel                        |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Vyhrazené OM Vzdálená správa clusteru Elasticsearch – pouze ten, který umožňuje připojení k externím SSH. Běží ve stejné síti virtuální jako všech Elasticsearch uzlech, ale nespustí žádné Elasticsearch služby.|
|Uzly dat                        |myBigCluster1... myBigCluster*N* <br /><br />Uzly dat, které se službami Elasticsearch a Kibana. Můžete se připojit pomocí SSH jednotlivých uzlech, ale jenom přes uzel správce.|
|Elasticsearch obrázku             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Primární koncový bod pro Elasticsearch obrázku (Všimněte si přípony /es). Je chráněn základní ověřování HTTP (pověření byly zadané parametry esUserName/esPassword šablony ES MultiNode). Clusteru má taky vedoucí Plug-inu nainstalovaný (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) pro správu základní obrázku.|
|Kibana služby                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Služba Kibana nastavenou zobrazení dat z obrázku vytvořeného Elasticsearch. Je chráněn stejné přihlašovacími údaji jako samotného clusteru.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>V procesu versus zachycení mimo proces sledování
V úvodu jsme uvedené dva základní způsoby shromažďování dat diagnostiky: a mimo proces. Má každá slabých a silných.

Výhody **proces sledování zachycení** , patří:

1. *Snadno konfigurace a nasazení*

    * Konfigurace shromažďování diagnostiky dat je jenom část konfiguraci aplikace. Není těžké si vždy uchovat "synchronní" s ostatními rohu aplikace.

    * Konfigurace jednotlivé aplikace nebo za služby je snadno dosáhnout.

    * Zachycení mimo proces sledování obvykle vyžaduje samostatné nasazení a konfiguraci diagnostiky Agent, což je administrativní úkolu a potenciální zdroje chyby. Technologie konkrétní agent často umožňuje jenom jedna instance agent jednotlivé počítače virtuální (uzly). To znamená pro tuto konfiguraci kolekce konfiguraci diagnostiky sdíleny všechny aplikace a služby na uzel.

2. *Flexibilitu*

    * Aplikaci můžete odeslat data, ať je nutné přejít, dokud je knihovna klienta, který podporuje systému cílových datový úložiště. Přidáním nového propadů podle potřeby.

    * Komplexní pravidla zaznamenání, filtrování a agregace dat lze provést.

    * Mimo proces sledování zachycení je často omezen propadů dat, které podporuje agent. Některé agenti jsou extensible.

3. *Přístup k datům interní aplikace a kontextu*

    * Diagnostické subsystému běží uvnitř procesu aplikace a služby můžete snadno rozšířit trasování kontextové informacemi.

    * Při přístupu mimo proces data musí být odeslány agent přes některé mechanismus komunikaci mezi procesy, třeba trasování událostí pro systém Windows Tento postup můžou uložit další omezení.

Výhody **mimo proces sledování zachycení** patří:

1. *Vypíše možnost Sledujte aplikaci a Automatický sběr dojde k chybě*

    * Zachycení trasování v procesu, nemusí být úspěšný Pokud aplikace nespustí nebo dojde k chybě. Nezávislé agent má mnoho větší šanci zachycení důležité informace o řešení potíží.<br /><br />

2. *Splatnost, odolnosti a osvědčené výkonu*

    * Agent vyvinutý prodejcem platformu (například agenta diagnostických nástrojů Microsoft Azure) byla vyměřené poplatky za jeho náročné testování a bitvy zabezpečení.

    * S trasování v procesu zachycení péče třeba zajistit, aby aktivity odeslat diagnostické dat z aplikace procesu rušit hlavní úkoly aplikace nebo dejte časování nebo výkonu problémů. Nezávisle na sobě pracovního agent je menší chybám tyto otázky a speciálně omezit jeho dopad na systém.

Je možné ke sloučení a těžit z obou přístupů. Nakonec je nejlepším řešením pro mnoho aplikací.

Tady používáme **Microsoft.Diagnostic.Listeners knihovny** a v proces sledování zachycení odeslání dat z aplikace služby struktury Elasticsearch clusteru.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Odeslání dat diagnostiky do Elasticsearch pomocí knihovnu posluchače
Knihovna Microsoft.Diagnostic.Listeners není součástí PartyCluster ukázková služby struktury aplikace. Používat:

1. Stáhněte si [Ukázkový PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) z GitHub.

2. Zkopírujte Microsoft.Diagnostics.Listeners a Microsoft.Diagnostics.Listeners.Fabric projektů (celé složky) z adresáře ukázkové PartyCluster ke složce řešení aplikace, která se má odeslat data Elasticsearch.

3. Otevřete cílový řešení, klikněte pravým tlačítkem myši na uzel řešení v Průzkumníku řešení a zvolte **Přidat existující projekt**. Přidáte projekt Microsoft.Diagnostics.Listeners řešení. Opakujte stejný Microsoft.Diagnostics.Listeners.Fabric projektu.

4. Přidejte do projektu odkaz ze služby projekt přidané dva projekty. (Každé služby, který se má odeslat data do Elasticsearch by měl odkaz Microsoft.Diagnostics.EventListeners a Microsoft.Diagnostics.EventListeners.Fabric).

    ![Projekt odkazy na Microsoft.Diagnostics.EventListeners a Microsoft.Diagnostics.EventListeners.Fabric knihoven][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Verze služby struktury obecné dostupnost a Microsoft.Diagnostics.Tracing Nuget balíčku
Aplikace vytvořené pomocí služeb struktury obecné dostupnost vydání (2.0.135 uvolnění 31 březen 2016) zacílit **.NET Framework 4.5.2**. Tato verze je nejvyšší verze .NET Framework nepodporuje Azure v době GA vydání. Tato verze Framework bohužel nemá určitých EventListener rozhraní API, která potřebuje knihovnu Microsoft.Diagnostics.Listeners. Protože ZDROJ_UDÁLOSTI (součást, který tvoří základ protokolování rozhraní API aplikace struktury) a EventListener pevně svázáno, musíte použít každý projekt, který používá knihovnu Microsoft.Diagnostics.Listeners alternativní provádění ZDROJ_UDÁLOSTI. Tato implementace poskytuje společnost **Microsoft.Diagnostics.Tracing Nuget balíčku**autorem Microsoft. Balíček je plně zpětně kompatibilní s ZDROJ_UDÁLOSTI zahrnuté v rámci, takže žádné změny kód by měl být nutné než odkazované názvů změny.

Pomocí implementaci Microsoft.Diagnostics.Tracing třídy ZDROJ_UDÁLOSTI zahájíte takto pro jednotlivé projekty služby, kterou je potřeba odeslat data do Elasticsearch:

1. Klikněte pravým tlačítkem myši na projekt služby a zvolte **Spravovat balíčků Nuget**.

2. Přejděte k tomuto zdroji balíčku nuget.org (pokud ještě není zaškrtnuté) a vyhledejte "**Microsoft.Diagnostics.Tracing**".

3. Instalace `Microsoft.Diagnostics.Tracing.EventSource` balíčku (a její závislosti).

4. Otevřete soubor **ServiceEventSource.cs** nebo **ActorEventSource.cs** v projektu služby a nahraďte `using System.Diagnostics.Tracing` směrnice nad soubor nasdílet `using Microsoft.Diagnostics.Tracing` směrnice.

Tento postup nebude potřeba po **.NET Framework 4.6** nepodporuje Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Pro vytvoření instance: Elasticsearch posluchače a konfigurace
Posledním krokem při odesílání dat diagnostiky Elasticsearch je pro vytvoření instance `ElasticSearchListener` a nakonfigurujte ji s daty Elasticsearch připojení. Posluchače automaticky zaznamenává všechny události mocninu prostřednictvím ZDROJ_UDÁLOSTI tříd definovaných v projektu služby. Je třeba jej aktivní po dobu platnosti službu, tedy nejlepší místo, kde ji vytvořit v kódu inicializace služby. Tady je mohl vzhled kód inicializace příslušnosti služby po potřebné změny (dodatky zdůraznit v komentářích počínaje `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Připojení dat Elasticsearch měly být umístěny na samostatný oddíl v souboru konfigurace služby (**PackageRoot\Config\Settings.xml**). Název oddílu musí odpovídat hodnota předaná `FabricConfigurationProvider` konstruktoru, například:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Hodnoty `serviceUri`, `userName` a `password` parametry odpovídají adresa koncového bodu clusteru Elasticsearch, Elasticsearch uživatelské jméno a heslo v uvedeném pořadí. `indexNamePrefix`je předponu Elasticsearch indexy; Knihovna Microsoft.Diagnostics.Listeners vytvoří nový index pro svá data denně.

### <a name="verification"></a>Ověření
Je to! Teď kdykoli spustit službu začne trasování posílat Elasticsearch službu určenou v konfiguraci. Můžete ověřit, že to otevřením uživatelského rozhraní Kibana přidružený k instanci Elasticsearch cílové. V našem příkladu je adresa stránky http://myBigCluster.westus.cloudapp.azure.com/. Zkontrolujte, zda indexy s předponou název pro `ElasticSearchListener` instance skutečně byly vytvořeny a vyplněné s daty.

![Zobrazení Kibana PartyCluster aplikace události][2]

## <a name="next-steps"></a>Další kroky
- [Další informace o diagnostikování a sledování služby struktury služby](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
