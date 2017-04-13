<properties
    pageTitle="Jak používat relay Service Bus s .NET | Microsoft Azure"
    description="Informace o použití služby pro přenos dat Bus služby Azure připojení dvěma aplikacemi hostovaný na různých místech."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Použití služby pro přenos dat Bus služby Azure

Tento článek popisuje, jak používat službu relay Service Bus. Vzorky napsané v jazyce C# a použít Windows Communication Foundation (WCF) rozhraní API s příponami obsažené v sestavení Bus služby. Další informace o relay Service Bus najdete v tématu Přehled [zasílání zpráv služby Bus přenos](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Co je relay Service Bus?

[Služby Bus *relay* ](service-bus-relay-overview.md) service umožňuje vytvářet hybridní aplikace, které se spouštějí v Azure datacentra a vlastní místní organizace prostředí. Relay Service Bus usnadňuje to tím, že umožňuje bezpečně vystavit služby Windows Communication Foundation (WCF), které se nacházejí v rámci podnikové sítě veřejné cloudu bez nebudete muset otevřít připojení brány firewall nebo vyžadování rušivá změny infrastrukturu podnikové sítě.

![Předávací koncepty](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Relay Service Bus umožňuje hostitele služby WCF existující prostředí organizace. Pak můžete delegovat listening pro příchozí požadavky na tyto služby WCF ke službě Bus služeb spuštěných v rámci Azure a relace. Umožňuje zobrazit tyto služby kódu aplikace spuštěné v Azure nebo mobilním pracovníkům nebo extranetové partnera prostředí. Služba Bus umožňuje bezpečně ovládací prvek, kdo má přístup k těchto služeb jemně odstupňovaná úrovni. Poskytuje výkonné a bezpečný způsob, jak vystavit funkcí aplikace a data z existujících řešení enterprise a jej využít z cloudu.

Tento článek ukazuje, jak používat relay Service Bus k vytvoření k webové službě WCF, zveřejněným pomocí vazby kanálu TCP, implementující zabezpečené konverzace mezi dvěma stranami.

## <a name="create-a-service-namespace"></a>Vytvoření obor názvů služby

Pokud chcete začít relay Service Bus v Azure, je nutné nejprve vytvořit obor. Obor názvů poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci.

Při vytváření názvů služby:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Získání balíček NuGet Bus služby

[Služba Bus NuGet balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je nejjednodušší a chcete nakonfigurovat aplikaci se všemi závislostí služby Bus si rozhraní API Bus služby. Pokud chcete nainstalovat balíček NuGet v projektu, postupujte takto:

1.  V Průzkumníku klikněte pravým tlačítkem na **odkazy**a potom klikněte na **Spravovat balíčky NuGet**.
2.  Vyhledejte "Služby Bus" a vyberte položku **Bus služby Microsoft Azure** . Klikněte na **nainstalovat** a dokončete instalaci a potom zavřete dialogové okno takto:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Pomocí služby Bus vystavit a používání webové služby protokolu SOAP s TCP

Pokud chcete vystavit existující webové službě WCF SOAP pro externí spotřebu, musíte provedete změny vazby služby a adresy. Může být nutné změny konfiguračního souboru nebo ho může vyžadovat kód změny, podle toho, jak máte nastavení a nakonfigurovány služby WCF. Všimněte si, že WCF umožňuje více koncové body sítě přes stejné služby, takže zachovat stávající interní koncové body při přidávání koncové body Bus služby pro externí přístup ve stejnou dobu.

V tomto úkolu Vytvoření jednoduchého služby WCF a přidat posluchače Bus služby do ní. Tento cvičení předpokládá některé znalost Visual Studiu a proto nejsou projděte si postup všechny podrobnosti o vytváření projektu. Místo toho zaměřen na kód.

Před spuštěním těchto kroků, proveďte následující postup pro nastavení prostředí:

1.  Ve Visual Studiu vytvořte konzoly aplikace, která obsahuje dva projekty "Klienta" a "Služby" v rámci řešení.
2.  Přidáte do aplikace Microsoft Azure služby Bus NuGet oba projekty. Tento balíček přidá všechny potřebné sestavení odkazy na projekty.

### <a name="how-to-create-the-service"></a>Jak vytvořit službu

Nejprve vytvořte samotnou službu. Jiné služby WCF se skládá ze nejméně tři různých částí:

-   Definice smlouvy, který popisuje výměna jaké zpráv a co mají operace uplatnit.
-   Provádění uvedené smlouvy.
-   Host (hostitel), který je hostitelem služby WCF a poskytuje několik koncové body.

Příklady v této části adresy každou z těchto složek

Smlouva definuje jeden operace `AddNumbers`, která přidá dvěma čísly a vrátí výsledek. `IProblemSolverChannel` Rozhraní umožňuje klientovi snadněji spravovat životnost proxy serveru. Vytváření neznámé rozhraní se považuje za doporučený postup. Je dobré dát této smlouvy definice do samostatného souboru tak, aby tento soubor můžete odkázat z "Klienta" a "Služby" projektů, ale můžete taky zkopírovat kód do oba projekty.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Implementace se smlouvou na místě je důvodu.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Konfigurace hostitele služby programově

Smlouvy a implementaci na místě můžete teď hostujete službu. Hostování nastane uvnitř objektu [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) , který má na starosti Správa instancí služby a hostuje koncové body, které přijímat zprávy. Následující kód konfiguruje službu se pravidelné místní koncový bod a koncového bodu služby Bus ilustrovat vzhled vedle sebe, vnitřní a vnější koncové body. Nahraďte řetězec *obor názvů* s názvem názvů a *yourKey* klávesu přidružení zabezpečení, který jste získali v předchozím kroku nastavení.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

V tomto příkladu vytvoříte dva koncové body, které jsou na stejnou implementaci smlouvy. Reprodukujte místní a jednu plánované prostřednictvím služby Bus. Důležité rozdíly mezi nimi jsou vazby; [Článku](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) pro místní a [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) koncového bodu služby Bus a adresy. Místní koncový bod má adresu místní síti s portem distinct. Koncový bod služby Bus má adresa koncového bodu složené řetězce `sb`, vaše jméno názvů a cestu "Řešitele." Výsledkem identifikátor URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifikační koncový bod služby jako koncový bod služby Bus TCP plně kvalifikovaný název externí DNS. Pokud umístíte kód nahrazení zástupných do `Main` funkce aplikace **služby** , budou mít funkční služby. Pokud má vaše služba poslouchat výhradně na služby Bus odeberte deklaraci místní koncový bod.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Konfigurace hostitele služby v konfiguračního souboru

Můžete taky nakonfigurovat hostitele pomocí konfiguračního souboru. V následujícím příkladu se zobrazí v tomto případě hostování kód služby.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Definice koncový bod přesunout do konfiguračního souboru. Balíček NuGet již přidala oblasti definice do souboru App.config, které jsou požadovaná konfigurace příponu Bus služby. Následujícím příkladu je přesně odpovídá předchozí kód, by se měly přímo pod **system.serviceModel** element. Tento příklad kódu předpokládá, že oboru názvů projektu C# jmenuje **služby**.
Nahrazení zástupných symbolů obor názvů služby Bus služby a přidružení zabezpečení klíče.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Po provedení těchto změn spuštění služby stejně jako před, ale s dvěma živou koncové body: jeden místní a jeden listening v cloudu.

### <a name="create-the-client"></a>Vytvoření klienta

#### <a name="configure-a-client-programmatically"></a>Konfigurace klienta programově

Pokud chcete používat službu, můžete vytvořit klienta WCF pomocí [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektu. Služba Bus používá model tokeny zabezpečení implementovaná pomocí přidružení zabezpečení. [Poskytovatel TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) třídy představuje Poskytovatel tokenu zabezpečení pomocí metody předdefinované factory, jejichž výsledkem je nějaký známý tokenu poskytovatelů. Metoda [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) zpracovávání pořízení odpovídající token přidružení zabezpečení v následujícím příkladu. Název a klíč jsou získány z portálu podle popisu v předchozí části.

Nejdřív vytvořte odkaz nebo zkopírujte `IProblemSolver` smlouvy kód služby do projektu klienta.

Potom nahraďte kód v `Main` metoda klienta, znovu nahrazení zástupného textu služby Bus názvů a klíč přidružení zabezpečení.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Nyní je možné vytvářet klienta a službách, ukažte je (spustit službu nejdřív) a klient volá službu a vytiskne **9**. Klienta a serveru můžete spustit na různých počítačích i v rámci sítě a komunikace bude dál fungovat. Kód klienta můžete taky spustit v cloudu nebo místně.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Konfigurace klienta v konfiguračního souboru

Následující kód ukazuje, jak nakonfigurovat klienta pomocí konfiguračního souboru.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Definice koncový bod přesunout do konfiguračního souboru. Následující příklad, který je shodný s kódem uvedených dříve, by se měly přímo pod **system.serviceModel** element. Tady, musíte nahradit zástupné symboly s služby Bus názvů a klíčem přidružení zabezpečení.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby pro přenos dat služby Bus, tyto odkazy vedou na další informace.

- [Služba Bus přenos zasílání zpráv – přehled](service-bus-relay-overview.md)
- [Přehled Azure Bus služby](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Stažení služby Bus ukázek z [Azure vzorků][] nebo najdete v článku [Přehled ukázky Bus služby][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure vzorky]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Přehled služeb Bus vzorky]: ../service-bus-messaging/service-bus-samples.md