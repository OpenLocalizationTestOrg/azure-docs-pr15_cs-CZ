<properties 
    pageTitle="Kurz Service Bus Relay | Microsoft Azure"
    description="Vytvoření klienta služby Bus aplikace a přihlašovacích údajů pomocí služby Bus předávací."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Kurz Service Bus Relay

Tento kurz popisuje, jak vytvořit jednoduchý služby Bus klientské aplikace a služby pomocí funkce "relay" Bus služby. Na odpovídající kurz, který využívá služba Bus [brokered zpráv](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)najdete v tématu [Služby Bus Brokered zpráv .NET kurz](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Pracovních přes tento kurz vám pochopení kroky, které jsou potřeba k vytvoření aplikace služby Bus klienta a služby. Stejně jako jejich protějšky WCF služba je konstrukce, která poskytuje jeden nebo více koncové body, z nichž každá zpřístupňuje jeden nebo více operace služeb. Koncový bod služby určuje adresu kde najdete službu, vazbu, která obsahuje informace, které klienta komunikovat službu a smlouvy, který definuje funkce poskytované službu svým klientům. Hlavní rozdíl mezi WCF a služby Bus služby je, že koncový bod se zobrazí v cloudu místo místně na vašem počítači.

Po dokončení práce řadou témata v tomto kurzu, budete mít spuštěné služby a klienta, který může vyvolat operace služby. První téma popisuje, jak nastavit účet. Další kroky popisují, jak definovat služba, která používá smlouvy, jak implementovat službu a postup při konfiguraci služby v kódu. Jsou taky popisují hostovat a spusťte službu. Je vlastním hostovanou službu, která se vytvoří a klienta a služby spuštění na stejném počítači. Konfigurace služby pomocí kódu nebo konfiguračního souboru.

Konečný tři kroky popisují, jak vytvořit klientské aplikace a konfigurovat klientské aplikaci a vytvářet a používat klienta, který může mít přístup k funkcím hostiteli.

Všechna témata v tomto oddílu se předpokládá, že používáte Visual Studio jako vývojové prostředí.

## <a name="sign-up-for-an-account"></a>Registrace účtu

Cílem prvního kroku je vytvořit obor názvů a získat klíč sdílené přidružení (zabezpečení aplikace Access podpis). Obor názvů obsahuje hranice aplikace pro každou aplikaci vystaven prostřednictvím služby Bus. Přidružení zabezpečení klíč vytváří automaticky systém při vytváření názvů služby. Kombinace obor názvů služby a přidružení zabezpečení klíč poskytuje pro službu Bus ověření přístup k aplikaci její přihlašovací údaje.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Definování WCF smlouvy o poskytování služeb pro práci s Bus služby

Smlouvu určuje jaké operace podporuje službu (Web služby terminologie pro metod nebo funkcí). Smlouvy se vytvářejí tím, že definujete rozhraní C++, C# nebo Visual Basic. Obou metod v rozhraní odpovídá operace konkrétní službu. Každé rozhraní musí mít atribut [atribut ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) použitá a každá operace musí mít atribut [atribut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) použitý. Pokud není metodu v rozhraní, který obsahuje atribut [atribut ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atribut [atribut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) , nebude vystaven příslušný způsob. Kód pro tyto úkoly je k dispozici v příkladu postupu. Větší diskuse smlouvy a služeb najdete v článku [Návrh a implementace služby](https://msdn.microsoft.com/library/ms729746.aspx) v dokumentaci WCF.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>K vytvoření smlouvy služby Bus s rozhraní

1. Jako správce otevřete aplikaci Visual Studio pravým tlačítkem myši program v nabídce **Start** a vyberte **Spustit jako správce**.

2. Vytvořte nový projekt aplikace konzoly. Klikněte na nabídku **soubor** a vyberte **Nový**a potom klikněte na **projekt**. V dialogovém okně **Nový projekt** klikněte na tlačítko **Visual Basic** (Pokud **Visual Basic** nezobrazí, vyhledejte v části **Další jazyky**). Klikněte na položku Šablona **Aplikace konzoly** a pojmenujte ho **EchoService**. Klikněte na **OK** vytvořte projekt.

    ![][2]

3. Nainstalujte balíček NuGet Bus služby. Tento balíček automaticky přidá odkazy na knihovnu služby Bus, jakož i WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který umožňuje programově přístup k základních funkcí WCF. Služba Bus používá spoustu objekty a atributy WCF k definování smluv.

    V Průzkumníku klikněte pravým tlačítkem myši na řešení a potom klikněte na **Spravovat NuGet balíčků řešení**. Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Ujistěte se, že je název projektu vybrané v poli **verze** . Klikněte na tlačítko **instalovat**a přijměte podmínky použití.

    ![][3]

3. V Průzkumníku poklikejte na soubor Program.cs a otevřete ji v editoru, pokud ještě není otevřený.

4. Přidejte následující pomocí příkazů v horní části souboru:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Změna názvu názvů z jeho výchozí název **EchoService** **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Tento kurz používá C# oboru **Microsoft.ServiceBus.Samples**, což je obor názvů typu smlouvy spravovaných, který se používá v souboru konfigurace v kroku [Konfigurace klienta WCF](#configure-the-wcf-client) . Můžete použít libovolný obor názvů, které chcete při vytváření Tato ukázka; kurz však nebude fungovat, pokud pak upravíte obory názvů smlouvy a služby toho v souboru konfigurace aplikace. Obor názvů podle konfiguračního souboru musí být stejný jako obor názvů zadaný v souborech C#.

1. Přímo za `Microsoft.ServiceBus.Samples` deklarace názvů, ale v rámci oboru, definujte nový rozhraní s názvem `IEchoContract` a použít ho `ServiceContractAttribute` atribut rozhraní s hodnotou oboru názvů **http://samples.microsoft.com/ServiceModel/Relay/**. Obor názvů hodnota se liší od obor názvů, který používáte v rámci rozsahu kódu. Místo toho hodnotou oboru názvů slouží jako jedinečný identifikátor pro tuto smlouvu. Určování oboru explicitně zabrání výchozí obor názvů hodnotu vás někdo přidá k názvu smlouvy.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Obor názvů smlouvy služby obvykle obsahuje naming schéma, které obsahuje informace o verzi. Včetně informací o verzi v oboru smlouvy služby povoluje služby vyčlenění nejdůležitějších změn tak, že definujete nové smlouvu k novému oboru názvů a vystavení na nový koncový bod. Tímto způsobem můžete dál používat staré smlouvu aniž byste museli být aktualizovány klienty. Informace o verzi mohou být tvořeny data nebo čísla Tvůrce dotazů. Další informace najdete v tématu [Správa verzí služby](http://go.microsoft.com/fwlink/?LinkID=180498). Pojmenování schéma obor názvů smlouvy služby pro účely tohoto kurzu neobsahuje informace o verzi.

1. V rámci `IEchoContract` rozhraní, deklarovat metodu jedné operace `IEchoContract` smlouvy zpřístupňuje v rozhraní a použít `OperationContractAttribute` atribut metody, kterou chcete vystavit jako součást veřejné služby Bus smlouvy.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Hned za `IEchoContract` rozhraní definice, deklarovat kanálu, které dědí z obou `IEchoContract` a taky `IClientChannel` rozhraní, jak je znázorněno zde:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanál je WCF objekt, kterým Host (hostitel) a klientský projít informace k sobě. Později budete psát kód proti kanálu, který chcete vypsat informace mezi těmito dvěma aplikacemi.

1. V nabídce **vytvořit** klikněte na tlačítko **Sestavit řešení** nebo stisknutím **Kombinace kláves Ctrl + Shift + B** zatím potvrďte přesnost vašem čísle do zaměstnání.

### <a name="example"></a>Příklad

Následující kód ukazuje základní rozhraní, který definuje služby Bus smlouvy.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Teď, když je vytvořeno rozhraní, můžete používat rozhraní.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Implementace smlouvy WCF používat službu Bus

Vytvoření relay Service Bus vyžaduje nejprve vytvořit smlouvy, který je definován pomocí nějakého rozhraní. Další informace o vytváření rozhraní naleznete v předchozím kroku. Dalším krokem je implementovat rozhraní. Tento postup představuje vytvoření třídy s názvem `EchoService` , která implementuje definované uživatelem `IEchoContract` rozhraní. Po provedení rozhraní poté nastavíte rozhraní pomocí konfigurační soubor aplikace konfigurace. Konfigurační soubor obsahuje potřebných informací pro aplikací, jako je název služby název smlouvy a zadejte protokol, který slouží ke komunikaci s Bus služby. Kód použité u těchto úkolů je k dispozici v příkladu postupu. Další obecné informace o tom, jak implementovat smlouvu o poskytování služeb naleznete v dokumentaci WCF [Provádění smluv](https://msdn.microsoft.com/library/ms733764.aspx) .

1. Vytvoření nové třídy s názvem `EchoService` přímo za definici `IEchoContract` rozhraní. `EchoService` Implementuje třída `IEchoContract` rozhraní. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Podobně jako jiné implementace rozhraní využijete definici v jiný soubor. Pro účely tohoto návodu však provedení se nachází ve stejném souboru jako definici rozhraní a `Main` metody.

1. Použít atributy [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) , které mají `IEchoContract` rozhraní. Atribut určuje název a služby názvů. Po tom, `EchoService` třídy vypadat takto:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementace `Echo` metodou definovanou `IEchoContract` rozhraní v `EchoService` předmětu. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Klikněte na **vytvořit**a potom klikněte na tlačítko **Sestavit řešení** potvrďte přesnost vašem čísle do zaměstnání.

### <a name="to-define-the-configuration-for-the-service-host"></a>Definování konfigurace hostitele služby

1. Konfigurační soubor je velmi podobné konfiguračního souboru WCF. Obsahuje název služby, koncového bodu (to znamená umístění služby Bus zpřístupňuje pro zařízení a hosts vzájemně komunikovat) a vazby (typ protokol, který slouží ke komunikaci). Hlavní rozdíl je, že tento koncový bod nakonfigurovanou službu odkazuje [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) vazba, která není součástí .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) je taková vazeb definované Bus služby.

1. V **Okně Průzkumník**poklikejte na soubor App.config a otevřete ji v editoru Visual Studio.

2. V `<appSettings>` prvek, nahrazení zástupných symbolů s názvem obor názvů služby a přidružení zabezpečení základní jste zkopírovali v předchozím kroku. 

1. V `<system.serviceModel>` značek, přidejte `<services>` prvek. Více aplikací služby Bus můžete definovat v jedné konfiguračního souboru. Tento kurz však definuje pouze jeden.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. V rámci `<services>` prvek, přidejte `<service>` prvek, který chcete definovat název služby.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. V `<service>` prvek, definovat umístění smlouvy koncového bodu a typ vazby koncového bodu.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Koncový bod definuje, kde bude vypadat klient aplikace Host (hostitel). Kurz později slouží k vytváření identifikátor URI, který plně zpřístupňuje hostiteli prostřednictvím služby Bus tento krok. Vazba deklaruje, jsme používáte TCP protokol komunikovat s Bus služby.

1. Klikněte v nabídce **vytvořit** na **Sestavit řešení** potvrďte přesnost vašem čísle do zaměstnání.

### <a name="example"></a>Příklad

Následující kód ukazuje provádění smlouvu.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Následující kód ukazuje základní formát konfiguračního souboru přidružené hostitele služby.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Hostování a spuštění služby základní k registraci Bus služby

Tento krok popisuje, jak provádět služby základní Bus služby.

### <a name="to-create-the-service-bus-credentials"></a>Vytvoření pověření Bus služby

1. V `Main()`, vytvořte dvě proměnné pro uložení oboru a přidružení zabezpečení klíč, které se budou číst z okna konzoly.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Klávesu přidružení zabezpečení se později používat pro přístup k projektu Bus služby. Obor názvů předané jako parametr `CreateServiceUri` vytvořit službu URI.

4. Použití objektu [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) deklarujte, že budete používat klávesu přidružení zabezpečení psaní přihlašovacích údajů. Přidejte následující kód přímo za kód přidali v posledním kroku.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Chcete-li vytvořit základní adresu pro službu

1. Po kódu, který jste přidali v posledním kroku vytvořit `Uri` instanci služby základní adresu. Tento identifikátor URI určuje schématu Bus služby, obor názvů a cestu rozhraní služby.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" je zkratka pro službu Bus schéma a označuje, že TCP používáme jako protokol. To byla také dříve uvedeno v souboru konfigurace při [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) zadaná jako vazba.
    
    Pro účely tohoto návodu identifikátor URI je `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Vytvoření a konfigurace hostitele služby

1. Nastavení režimu připojení `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Režim připojení popisuje protokol služba používá pro komunikaci s Bus služeb; Nastavit informace HTTP nebo TCP. Použitím výchozího nastavení `AutoDetect`, služba pokouší připojit k Bus služby protokolu TCP, pokud je k dispozici a HTTP Pokud TCP není k dispozici. Poznámku, že to se liší od protokol službu určuje pro komunikaci s klientem. Protokolu je určený podle vazby použít. Například služba můžete použít [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) vazba, která určuje, že jeho koncového bodu (vystaven na služby Bus) informuje uživatele o s klienty přes HTTP. Stejnou službu mohli byste určit **ConnectivityMode.AutoDetect** tak, aby služba informuje uživatele o s Bus služby prostřednictvím protokolu TCP.

1. Vytvoření hostitele služby pomocí URI vytvořený v této části.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hostitele služby je WCF objekt, který vytváří instanci služby. Tady předáte typ služby, které chcete vytvořit ( `EchoService` typu) a taky na adresu, pro niž chcete vystavit službu.

1. V horní části souboru Program.cs přidáte odkazy na [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) a [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Po návratu do `Main()`, nakonfigurujte koncový bod k povolení přístupu veřejnosti.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Tento krok informuje Bus služby, že aplikace můžete najít veřejně porovnáním služby ATOM Bus kanálu plánu projektu. Pokud má argument **DiscoveryType** **soukromé**, klienta by pořád moct přístup ke službě. Služba však neobjeví při hledání oboru Bus služby. Místo toho klienta musel předem znát cestu koncového bodu.

1. Použití pověření služby pro koncové body služby podle konfiguračního souboru:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Jak je uvedeno v předchozím kroku, může vykázání více služeb a koncové body v souboru konfigurace. Pokud jste měli byste Procházet tento kód konfigurační soubor a vyhledejte každý koncový bod, do kterého se má použít svoje přihlašovací údaje. Pro tento kurz obsahuje konfiguračního souboru pouze jeden koncový bod.

### <a name="to-open-the-service-host"></a>Chcete-li otevřít hostitele služby

1. Otevřete službu.

    ```
    host.Open();
    ```

1. Informujte uživatele, aby službu běží a je vysvětleno, jak vypnout službu.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Po dokončení zavřete hostitele služby.

    ```
    host.Close();
    ```

1. Stiskněte **Kombinaci kláves Ctrl + Shift + B** sestavení projektu.

### <a name="example"></a>Příklad

V následujícím příkladu obsahuje smlouvu a implementaci z předchozích kroků v tomto kurzu a hostitelem služby do konzoly aplikací. Kompilace následující do spustitelný soubor s názvem EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Vytvoření WCF klienta pro smlouvu

Dalším krokem je vytvoření základní klientské aplikaci služby Bus a definovat smlouvu, kterou provede v dalších krocích. Poznámka řadu kroků vypadat postup slouží k vytvoření služby: Definování smlouvy, úpravy App.config souboru, pomocí přihlašovacích údajů pro připojení k Bus služby a tak dál. Kód použité u těchto úkolů je k dispozici v příkladu postupu.

1. Vytvoření nového projektu v aktuálním Visual Studio řešení pro klienta následujícím způsobem:
    1. V Průzkumníku ve stejném řešení, která obsahuje služby klikněte pravým tlačítkem myši na aktuální řešení (není projekt) a klikněte na **Přidat**. Potom klikněte na **Nový projekt**.
    2. V dialogovém okně **Přidat nový projekt** klikněte na tlačítko **Visual Basic** (Pokud **Visual Basic** nezobrazí, vyhledejte v části **Další jazyky**), vyberte šablonu **Aplikace konzoly** a nazvěte ji **EchoClient**.
    3. Klikněte na **OK**.
<br />

1. V Průzkumníku poklikejte na soubor Program.cs v projektu **EchoClient** a otevřete ji v editoru, pokud ještě není otevřený.

1. Změna názvu názvů z jeho výchozí název `EchoClient` k `Microsoft.ServiceBus.Samples`.

1. Nainstalujte [Service Bus NuGet balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus). V Průzkumníku klikněte pravým tlačítkem **EchoClient** projektu a potom klikněte na **Spravovat balíčků NuGet**. Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **instalovat**a přijměte podmínky použití.

    ![][3]

1. Přidat `using` údajů pro názvů [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) v souboru Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Přidání definice smlouvy služby na obor, jak je vidět v následujícím příkladu. Všimněte si, že tato definice je stejná jako definice použité v projektu **služby** . Měli byste přidat tento kód v horní části `Microsoft.ServiceBus.Samples` názvů.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Stisknutím **Kombinace kláves Ctrl + Shift + B** vytvářet klienta.

### <a name="example"></a>Příklad

Následující kód zobrazuje aktuální stav Program.cs soubor v aplikaci project EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfigurace klienta WCF

V tomto kroku vytvoříte soubor App.config pro základní klientská aplikace, která přistupovat ke službě vytvořili dříve v tomto kurzu. Tento soubor App.config definuje smlouvy, vazby a název koncového bodu. Kód použité u těchto úkolů je k dispozici v příkladu postupu.

1. V Průzkumníku v projektu **EchoClient** poklikejte **App.config** otevřete soubor v editoru Visual Studio.

2. V `<appSettings>` prvek, nahrazení zástupných symbolů s názvem obor názvů služby a přidružení zabezpečení základní jste zkopírovali v předchozím kroku.

1. V rámci system.serviceModel element přidejte `<client>` prvek.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Tento krok deklaruje, že definujete klientské aplikaci WCF styl.

1. V `client` elementu definovat název, smlouvy a typ vazby koncového bodu.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Tento krok definuje název koncového bodu smlouvy definovat ve službě a na to, že klientské aplikace používá pro komunikaci s služby Bus TCP. Název koncového bodu je v dalším kroku používaný k propojení této konfiguraci koncový bod se službou URI.

1. Klikněte na **soubor**, potom na **Uložit vše**.

## <a name="example"></a>Příklad

Následující kód ukazuje konfiguračního souboru pro zobrazování výsledků klienta.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Implementace klient WCF volání Bus služby

V tomto kroku implementaci základní klientské aplikace, který má přístup k na službu, kterou jste vytvořili dříve v tomto kurzu. Podobně jako služby klienta provede spoustu stejných operací pro přístup k Bus služby:

1. Nastaví režim připojení.
1. Vytvoří identifikátory URI, najde hostitelskou službu.
1. Definuje zabezpečovací přihlašovací údaje.
1. Platí přihlašovací údaje pro připojení.
1. Otevře připojení.
1. Provede úkoly specifické pro aplikaci.
1. Slouží k zavření připojení.

Jeden z hlavního rozdílů je však klientské aplikaci používané v kanálu k připojení služby Bus, že služba používá volání **ServiceHost**. Kód použité u těchto úkolů je k dispozici v příkladu postupu.

### <a name="to-implement-a-client-application"></a>Implementace klientské aplikace

1. **Automatické zjištění**nastavte režim připojení. Přidejte následující kód uvnitř `Main()` metoda **EchoClient** aplikace.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. K uložení hodnot obor názvů služby a klíč přidružení zabezpečení, které se budou číst z konzoly definujte proměnné.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Vytvoření identifikátor URI, který definuje umístění hostitele služby Bus projektu.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Vytvoření objektu přihlašovací údaje pro koncový bod služby názvů.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Vytvoření kanálu, který se načítá při konfiguraci podle konfiguračního souboru.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanálu je WCF objekt, který vytvoří na kanál, přes který služeb a klientských aplikací komunikovat.

1. Použití služby Bus pověření.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Vytvoření a otevřete kanálu služby.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Napište základní uživatelské rozhraní a funkce pro ozvěnu.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Všimněte si, že kód používá instanci kanálu objektu jako proxy server služby.

1. Zavřete kanálu a výroby.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Ke spuštění aplikace

1. Stiskněte **Kombinaci kláves Ctrl + Shift + B** sestavte řešení. Tímto způsobem projektu klienta a projekt služby, kterou jste vytvořili v předchozích krocích.

2. Před spuštěním klientské aplikace, ujistěte se, jestli je spuštěný aplikaci služby. V Průzkumníku ve Visual Studiu klikněte pravým tlačítkem myši na **EchoService** řešení a potom klikněte na **Vlastnosti**.

3. V dialogovém okně Vlastnosti řešení klikněte na **Projekt při spuštění**a potom klikněte na tlačítko **více projektů po spuštění** . Zkontrolujte, že **EchoService** se objeví na začátku seznamu. 

4. Nastavte **Akce** pro **EchoService** a **EchoClient** projekty **začít**.

    ![][5]

5. Klikněte na tlačítko **závislosti projektů**. V dialogovém okně **projekty** vyberte **EchoClient**. V dialogovém okně **na** zkontrolujte, že je zaškrtnuté políčko **EchoService** .

    ![][6]

6. Klikněte na **OK** zavřete dialogové okno **Vlastnosti** .

7. Stisknutím klávesy **F5** spustit oba projekty.

8. Obě konzoly windows otevřete a výzvu k zadání názvu názvů. Služba musí spustit první, takže v okně konzoly **EchoService** zadejte obor názvů a stiskněte klávesu **Enter**.

9. Potom se zobrazí výzva k kód přidružení zabezpečení. Zadejte klávesu přidružení zabezpečení a stiskněte ENTER.

    Tady je příklad výstup z okna konzoly. Všimněte si, že hodnoty za předpokladu, že tady je příklad pouze.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Aplikace služby se vytiskne do okna konzoly adresu, na kterém je přijímá, jak je vidět v následujícím příkladu.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. V okně konzoly **EchoClient** zadejte stejné informace, které jste předtím zadali pro aplikaci služby. Postupujte podle předchozích kroků pro klientské aplikaci zadáte stejné obor názvů služby a hodnoty klíče přidružení zabezpečení.

11. Po zadání tyto hodnoty, klienta otevře kanál ke službě a zobrazí výzvu k zadání nějaký text, jak je vidět v následujícím příkladu výstup konzoly.

    `Enter text to echo (or [Enter] to exit):` 

    Zadejte text, který chcete odeslat do aplikace služby a stiskněte klávesu Enter. Tento text odeslaný do služby prostřednictvím operace služby ozvěnu a zobrazí se v okně služby konzoly jako následující příklad výstupu.

    `Echoing: My sample text`

    Klientská aplikace obdrží vrácenou hodnotu `Echo` operace, je původní text, který bude vytištěn spolu oknu jeho konzoly. Následujícím obrázku je příklad výstupu z okna konzoly klienta.

    `Server echoed: My sample text`

12. Odesílání textových zpráv z klienta služby tímto způsobem můžete pokračovat. Po dokončení stiskněte klávesu Enter konzoly Windows klienta a služby ukončit obě aplikace.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak vytvořit klientské aplikaci, jak volat operace služby a jak zavřít klient po dokončení operace volání.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Další kroky

Tento kurz ukázal k vytváření klienta služby Bus aplikace a přihlašovacích údajů pomocí funkce "předávací" Bus služby. Výukové podobné využívající služby Bus [zprávy](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)najdete v článku [Služby Bus Brokered zpráv .NET kurz](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Další informace o Bus služby, najdete v těchto tématech.

- [Přehled služeb Bus zasílání zpráv](../service-bus-messaging/service-bus-messaging-overview.md)
- [Základy Bus služby](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Služba Bus architektura](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
