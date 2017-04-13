<properties
    pageTitle="Výuková pomocí služby Bus ZBÝVAJÍCÍ přenos zpráv | Microsoft Azure"
    description="Vytvoření jednoduchého aplikaci služby Bus relay Host (hostitel), která poskytuje rozhraním REST."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>ZBÝVAJÍCÍ Relay Service Bus kurz

Tento kurz popisuje, jak vytvořit jednoduchý služby Bus hostitele aplikace, která poskytuje rozhraním REST. ZBÝVAJÍCÍ klientovi web, například ve webovém prohlížeči, prostřednictvím protokolu HTTP žádosti o přístup k rozhraní API Bus služby.

Tento kurz používá ZBYTEK Windows Communication Foundation (WCF) modelu programování vytvářet služba REST na Bus služby. Další informace najdete v tématu [Modelu programování ZBÝVAJÍCÍ WCF](https://msdn.microsoft.com/library/bb412169.aspx) a [Návrh a implementace služby](https://msdn.microsoft.com/library/ms729746.aspx) v dokumentaci WCF.

## <a name="step-1-create-a-service-namespace"></a>Krok 1: Vytvoření obor názvů služby

Cílem prvního kroku je vytvořit obor názvů a získat klíč sdílené přidružení (zabezpečení aplikace Access podpis). Obor názvů obsahuje hranice aplikace pro každou aplikaci vystaven prostřednictvím služby Bus. Přidružení zabezpečení klíč vytváří automaticky systém při vytváření názvů služby. Kombinace obor názvů služby a přidružení zabezpečení klíč poskytuje pro službu Bus ověření přístup k aplikaci její přihlašovací údaje.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Krok 2: Definujte na základě ZBÝVAJÍCÍ WCF smlouvy o poskytování služeb pro práci s Bus služby

Jako s jinými službami služby Bus při vytváření ZBÝVAJÍCÍ styl služby, musíte definovat smlouvy. Smlouva určuje jaké operace hostiteli podporuje. Operace služby můžete považovat za metody webové služby. Smlouvy se vytvářejí tím, že definujete rozhraní C++, C# nebo Visual Basic. Obou metod v rozhraní odpovídá operace konkrétní službu. [Atribut ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atributu musí být použity pro každé rozhraní a atribut [atribut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) musí být použity pro každou akci. Pokud není metodu v rozhraní, který obsahuje [atribut ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) [atribut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), nebude vystaven příslušný způsob. Kód použít pro tyto úkoly se zobrazují v příkladu postupu.

Primární rozdíl mezi základní smlouvy Bus služby a smlouvy ZBÝVAJÍCÍ styl je přidání odpovídající vlastnost [atribut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Tato vlastnost umožňuje namapovat metodu ve vašem rozhraní na metodu na druhé straně rozhraní. V tomto případě použijeme [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) odkaz metodu na HTTP GET. Díky služby Bus přesně načíst a interpretaci příkazů posílat rozhraní.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>K vytvoření smlouvy služby Bus s rozhraní

1. Otevřete aplikaci Visual Studio jako správce: klikněte pravým tlačítkem aplikaci v nabídce **Start** a potom klikněte na **Spustit jako správce**.

2. Vytvořte nový projekt aplikace konzoly. Klikněte na nabídku **soubor** a vyberte **Nový**a pak vyberte **projekt**. V dialogovém okně **Nový projekt** klikněte na tlačítko **Visual Basic**, vyberte šablonu **Aplikace konzoly** a pojmenujte ho **ImageListener**. Použijte výchozí **umístění**. Klikněte na **OK** vytvořte projekt.

3. Pro projekt C# Visual Studio vytvoří `Program.cs` soubor. Tato třída obsahuje prázdnou `Main()` metoda požadovaná pro projekt aplikace konzoly vytvářet správně.

4. Přidáte odkazy na služby Bus a **System.ServiceModel.dll** projektu instalací balíček NuGet Bus služby. Tento balíček automaticky přidá odkazy na knihovnu služby Bus, jakož i WCF **System.ServiceModel**. V Průzkumníku klikněte pravým tlačítkem **ImageListener** projektu a potom klikněte na **Spravovat balíčků NuGet**. Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **instalovat**a přijměte podmínky použití.

5. Odkaz na **System.ServiceModel.Web.dll** musí explicitně přidejte do projektu:

    na. V Průzkumníku klikněte pravým tlačítkem na složku **odkazy** ve složce projektu a potom klikněte na **Přidat odkaz**.

    b. V dialogovém okně **Přidat odkaz na** klikněte na kartu **Framework** na levé straně a do **vyhledávacího** pole, zadejte **System.ServiceModel.Web**. Zaškrtněte políčko **System.ServiceModel.Web** a potom klikněte na **OK**.

6. Přidejte následující `using` příkazy v horní části souboru Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který umožňuje programový přístup k základních funkcí WCF. Služba Bus používá spoustu objekty a atributy WCF k definování smluv. Ve většině aplikací relay Service Bus použijete tento obor názvů. Podobně [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) pomáhá definovat kanálu, což je objekt kterými komunikujete Bus služby a váš webový prohlížeč klienta. Nakonec [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) obsahuje typy, které umožňují vytvářet webových aplikací.

7. Přejmenování `ImageListener` názvů **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Přímo po levá složená závorka prohlášení názvů definovat nové rozhraní s názvem **IImageContract** a použít ho atribut **atribut ServiceContractAttribute** rozhraní s hodnotou `http://samples.microsoft.com/ServiceModel/Relay/`. Obor názvů hodnota se liší od obor názvů, který používáte v rámci rozsahu kódu. Obor názvů hodnota se používá jako jedinečný identifikátor pro tuto smlouvu a by měla být informace o verzi. Další informace najdete v tématu [Správa verzí služby](http://go.microsoft.com/fwlink/?LinkID=180498). Určování oboru explicitně zabrání výchozí obor názvů hodnotu vás někdo přidá k názvu smlouvy.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. V rámci `IImageContract` rozhraní, deklarovat metodu jedné operace `IImageContract` smlouvy zpřístupňuje v rozhraní a použít `OperationContractAttribute` atribut metody, kterou chcete vystavit jako součást veřejné služby Bus smlouvy.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Atribut **OperationContract** přidejte hodnotu **WebGet** .

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Umožní služby Bus ke směrování HTTP GET požadavků `GetImage`a chcete-li přeložit návratové hodnoty `GetImage` do odpověď protokolu HTTP GETRESPONSE. Dále v tomto kurzu budou používat ve webovém prohlížeči přístup k této metody a zobrazí obrázek v prohlížeči.

11. Hned za `IImageContract` definice deklarovat kanálu, které dědí z obou `IImageContract` a `IClientChannel` rozhraní.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanál je WCF objekt, kterým tuto službu a klient projít informace k sobě. Kanál později, vytvoříte hostitelská aplikace. Služba Bus pomocí tohoto kanálu předat požadavků HTTP GET z prohlížeče vaší implementací **GetImage** . Služba Bus taky používá kanál vrácenou hodnotu **funkce GetImage** a přeložit do HTTP GETRESPONSE pro prohlížeče klienta.

12. Klikněte v nabídce **vytvořit** na **Sestavit řešení** zatím potvrďte přesnost vašem čísle do zaměstnání.

### <a name="example"></a>Příklad

Následující kód ukazuje základní rozhraní, který definuje služby Bus smlouvy.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Krok 3: Implementace smlouvy služeb na základě ZBÝVAJÍCÍ WCF používat službu Bus

Vytvoření služby Bus služba REST styl vyžaduje nejprve vytvořit smlouvy, který je definován pomocí nějakého rozhraní. Dalším krokem je implementovat rozhraní. Tento postup představuje vytvoření třídy s názvem **ImageService** vytvářející **IImageContract** rozhraní definované uživatelem. Po provedení smlouvy poté nastavíte rozhraní pomocí konfigurační soubor. Konfigurační soubor obsahuje potřebných informací pro aplikací, jako je název služby název smlouvy a zadejte protokol, který slouží ke komunikaci s Bus služby. Kód použité u těchto úkolů je k dispozici v příkladu postupu.

Pomocí předchozích kroků, je velmi málo rozdíl mezi provádění smlouvy ZBÝVAJÍCÍ styl a základní smlouvy Bus služby.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Implementace smlouvy Bus služba REST stylu

1. Vytvoření nové třídy s názvem **ImageService** přímo za definici rozhraní **IImageContract** . Třídy **ImageService** používá rozhraní **IImageContract** .

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Podobně jako jiné implementace rozhraní využijete definici v jiný soubor. Však pro účely tohoto návodu provedení se zobrazí ve stejném souboru jako definici rozhraní a `Main()` metody.

2. Použijte [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribut třídy **IImageService** označíte, že je třída implementaci WCF smlouvy.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Jak jsme zmínili dříve, tento obor názvů je tradiční názvů. Místo toho je součástí architektura WCF, který identifikuje smlouvy. Další informace najdete v tématu [Názvy smlouvy dat](https://msdn.microsoft.com/library/ms731045.aspx) v dokumentaci WCF.

3. Přidání obrázku .jpg do projektu.  

    Toto je obrázek, který službu zobrazí přijímání prohlížeč. Klikněte pravým tlačítkem myši projektu a potom klikněte na **Přidat**. Potom klikněte na **Existující položky**. Pomocí dialogového okna **Přidat existující položku** umožňuje přecházet na příslušné .jpg a klikněte na tlačítko **Přidat**.

    Až přidáte soubor, zkontrolujte, že **Všechny soubory** vybraný v rozevíracím seznamu vedle **název souboru:** pole. Zbytku tohoto kurzu se předpokládá, že je v poli Název obrázku "image.jpg". Pokud máte jiný soubor, budete muset přejmenovat obrázek nebo změnit kódu pro vyrovnání.

4. Abyste měli jistotu, že služba pracovního najdou souboru s obrázkem, v **Okně Průzkumník** klepněte pravým tlačítkem na obrázek, a pak klikněte na **Vlastnosti**. V podokně **Vlastnosti** nastavte **Kopírovat do adresáře výstup** **kopie, pokud novější**.

5. Přidání odkazu do sestavení **System.Drawing.dll** projektu a také přidat následující přidružené `using` příkazy.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Ve třídě **ImageService** přidejte následující konstruktor, který načítá rastrový obrázek a připraví odešlete prohlížeče klienta.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Přímo po předchozí kód přidáte metodu **GetImage** ve třídě **ImageService** vrátíte HTTP zprávu, která obsahuje obrázek.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Tato implementace používá **MemoryStream** k načtení obrázku a jeho Příprava pro datové proudy do prohlížeče. Spustí pozice toku od nuly, deklaruje proudů ve formátu jpeg a přenáší datové proudy informace.

8. V nabídce **sestavení** klikněte na tlačítko **Sestavit řešení**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Stanovit konfigurační výpočetnímu webová služba Bus služby

1. V **Okně Průzkumník**poklikejte na **App.config** a otevřete ji v editoru Visual Studio.

    **Konfiguračního** souboru podobá konfiguračního souboru WCF a obsahuje název služby, koncového bodu (to znamená umístění služby Bus zpřístupňuje pro zařízení a hosts vzájemně komunikovat) a vazby (typ protokol, který slouží ke komunikaci). Hlavní rozdíl je, že koncový bod nakonfigurované služby odkazuje [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) vazba, která není součástí .NET Framework.

2. `<system.serviceModel>` Elementů XML je element WCF, který definuje jeden nebo víc služeb. Zde se používá k definovat název a služby koncového bodu. V dolní části `<system.serviceModel>` prvek (, ale pořád v rámci `<system.serviceModel>`), přidejte `<bindings>` prvek, který obsahuje následující obsah. Tato možnost definuje vazby používaných v aplikaci. Můžete definovat několik vazby, ale na tento kurz definujete pouze jeden.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Tento krok definuje služby Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) vazbu s **relayClientAuthenticationType** nastavený na **žádný**. Toto nastavení označuje koncový bod pomocí této vazby nevyžaduje pověření klienta.

3. Po `<bindings>` prvek, přidejte `<services>` prvek. Podobně jako vazby více služeb můžete definovat v souboru jednotné. Však pro účely tohoto návodu definujete pouze jeden.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Tento krok nakonfiguruje služby, které používá dříve definované výchozí **webHttpRelayBinding**. Používá se také výchozí **sbTokenProvider**, která je definovaná v dalším kroku.

4. Po `<services>` prvek, vytvořit `<behaviors>` elementem následující obsah nahrazení "SAS_KEY" dříve získané [Azure portál][]klíč *Sdílené podpis přístup* (přidružení zabezpečení).

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Pořád v App.config, v `<appSettings>` prvek, nahradit celý připojovací řetězec hodnoty pomocí připojovacího řetězce dříve získat z portálu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. V nabídce **sestavení** klikněte na tlačítko **Sestavit řešení** k vytvoření celé řešení.

### <a name="example"></a>Příklad

Následující kód ukazuje implementaci smlouvy a přihlašovacích údajů služby na základě REST, na kterém běží na služby Bus pomocí vazby **WebHttpRelayBinding** .

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Následující příklad ukazuje konfiguračního souboru přidružený ke službě.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Krok 4: Hostitelem služby WCF ZBÝVAJÍCÍ založené na používání služby Bus

Tento krok popisuje, jak provádět webové služby pomocí aplikace konzoly na Bus služby. Kompletní seznam kód napsaný v tomto kroku je k dispozici v příkladu postupu.

### <a name="to-create-a-base-address-for-the-service"></a>Chcete-li vytvořit základní adresu pro službu

1. V `Main()` fungovat deklarace, vytvořte proměnnou pro uložení názvů služby Bus projektu. Ujistěte se, pokud chcete nahradit `yourNamespace` s názvem obor názvů služby jste dříve vytvořili.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Služby Bus používá název vaší názvů vytvořit jedinečný identifikátor URI.

2. Vytvoření `Uri` instanci pro základní adresy služby, který je založený na obor.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Vytvoření a konfigurace hostitele webové služby

- Vytvoření hostitele služby webové adresy URI vytvořený v této části.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Hostitele služby je WCF objekt, který vytvoří hostitelská aplikace. V tomto příkladu předává typu host, které chcete vytvořit (v **ImageService**) a adresu pro niž chcete vystavit hostitelská aplikace.

### <a name="to-run-the-web-service-host"></a>Spuštění hostitele webové služby

1. Otevřete službu.

    ```
    host.Open();
    ```
    Službou nyní.

2. Zobrazí se zpráva oznamující, že běží služba a jak zastavit službu.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Po dokončení zavřete hostitele služby.

    ```
    host.Close();
    ```

## <a name="example"></a>Příklad

V následujícím příkladu obsahuje smlouvu a implementaci z předchozích kroků v tomto kurzu a hostitelem služby do konzoly aplikací. Následující kód kompilaci do spustitelný soubor s názvem ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Kompilace kódu

Po vytvoření řešení, postupujte podle následujících pokynů spustit aplikaci:

1. Stisknutím klávesy **F5**nebo přejděte do umístění, spustitelný soubor (ImageListener\bin\Debug\ImageListener.exe), spusťte službu. Zůstal aplikaci systém, musí provést další krok.

2. Zkopírujte a vložte adresu z příkazového řádku do prohlížeče zobrazíte obrázku.

3. Po dokončení stiskněte klávesu **Enter** v okně příkazového řádku pro aplikaci ukončíte.

## <a name="next-steps"></a>Další kroky

Teď jste integrované aplikace, která používá služby pro přenos dat Bus služby, najdete v následujících článcích Další informace o stavu zasílání zpráv:

- [Přehled Azure Bus služby](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Použití služby pro přenos dat Bus služby](service-bus-dotnet-how-to-use-relay.md)

[Azure portálu]: https://portal.azure.com