<properties
   pageTitle="Vytváření webových front-end pro aplikace pomocí technologie ASP.NET základní | Microsoft Azure"
   description="Zveřejnění služby struktury aplikace na webu pomocí rozhraní API webových Core ASP.NET projektu a mezi service komunikace přes ServiceProxy."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Vytvoření webové služby front-end pro aplikace pomocí základní technologie ASP.NET

Ve výchozím nastavení služby Azure služby struktury nenabízejí veřejné rozhraní na webu. Ke zpřístupnění funkcí aplikace klientům HTTP, bude potřeba vytvořit projekt web sloužit jako vstupní bod a potom komunikovat odtud ke službám od jednotlivé.

V tomto kurzu budeme vystopovat kde jsme přestali kurz [vytvoření první aplikace ve Visual Studiu](service-fabric-create-your-first-application-in-visual-studio.md) a přidejte webové služby před službu stavové čítačů. Pokud jste to ještě neudělali, doporučujeme vrátit zpátky a přecházet mezi tento kurz nejdřív.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Přidání služby základní ASP.NET do aplikace

Základní ASP.NET je lehké, různé platformy webové vývojové rozhraní, které můžete vytvořit web moderní uživatelské rozhraní a webového rozhraní API. Pojďme přidat projekt rozhraní API webových ASP.NET naše existující aplikace.

>[AZURE.NOTE] Tento kurz budete muset [nainstalovat .NET Core 1.0][dotnetcore-install].

1. V okně Průzkumník řešení **služby** v rámci projektu aplikace pravým tlačítkem vyberte **Přidat > Nová služba struktury**.

    ![Přidání nové služby do existující aplikace][vs-add-new-service]

2. Na stránce **vytvořit službu** zvolte **ASP.NET Core** a pojmenujte ho.

    ![Výběr webové služby ASP.NET v dialogovém okně Nový služby][vs-new-service-dialog]

3. Další stránka k dispozici sadu ASP.NET základní šablony projektů. Poznámka: Toto jsou stejné šablony, které by se zobrazí, pokud jste vytvořili projekt ASP.NET Core mimo aplikaci služby struktury. Pro účely tohoto návodu jsme zvolte **Rozhraní API webových**. Koncepty však můžete použít k vytváření úplnou webovou aplikaci.

    ![Výběr typu projektu ASP.NET][vs-new-aspnet-project-dialog]

    Po vytvoření projektu rozhraní API webových budete mít dvě služby v aplikaci. Při dalším vytvářet aplikace přidá další služby stejným způsobem. Každý může mít nezávisle na sobě verzí a upgradovaný.

>[AZURE.TIP] Další informace o vytváření ASP.NET Core services, najdete v [Dokumentaci základní ASP.NET](https://docs.asp.net).

## <a name="run-the-application"></a>Spusťte aplikaci

Abyste získali přehled o co jsme máte hotové, Pojďme nasaďte novou aplikaci a podívejte se na výchozí chování poskytujícím rozhraní API webových ASP.NET základní šablonu.

1. Stisknutím klávesy F5 ve Visual Studiu ladění aplikace.

2. Po dokončení nasazení aplikace Visual Studio spusťte prohlížeč kořenovém ASP.NET rozhraní API webových služeb – něco jako http://localhost:33003. Číslo portu je přiřazovat náhodně a se může lišit v počítači. Šablona rozhraní API webových Core ASP.NET nenabízí možnost výchozí chování ke kořenovému, tak, abyste se dostali k chybě v prohlížeči.

3. Přidání `/api/values` umístění v prohlížeči. Tím se vyvolá `Get` metoda na ValuesController v šabloně rozhraní API webových. Vrátí výchozí odpověď, která poskytuje společnost šabloně – JSON pole, které obsahuje dva řetězce:

    ![Výchozí hodnoty vrácené ze šablony rozhraní API webových základní technologie ASP.NET][browser-aspnet-template-values]

    Na konci kurzu budeme bude nahrazení tyto výchozí hodnoty poslední hodnota čítače z našich stavové služby.


## <a name="connect-the-services"></a>Připojení služeb

Služby struktury nabízí kompletní pružnost při komunikaci s spolehlivé služby. V rámci jedné aplikace bude pravděpodobně nutné služby, které jsou dostupné prostřednictvím protokolu TCP jiných služeb, které jsou dostupné prostřednictvím rozhraní REST API HTTP a pořád jiných služeb, které jsou dostupné prostřednictvím webu sockets. Dostupné možnosti a souvisejících nevýhody, najdete v [Communicating se službami](service-fabric-connect-and-communicate-with-services.md). V tomto kurzu budeme proveďte jeden z jednodušší postupů a používat `ServiceProxy` / `ServiceRemotingListener` třídy, které jsou k dispozici v sadě SDK.

V `ServiceProxy` přístup (modelovat na vzdálených volání procedur nebo RPC), definujete rozhraní má fungovat jako veřejné smlouva pro službu. Potom pomocí rozhraní generovat třídy proxy pro komunikaci s službu.


### <a name="create-the-interface"></a>Vytvoření rozhraní

Bude začneme vytvořením rozhraní má fungovat jako smlouvy mezi stavová služba a klientů včetně ASP.NET Core projektu.

1. V Průzkumníku řešení klikněte pravým tlačítkem myši a zvolte **Přidat** > **Nový projekt**.

2. V levém navigačním podokně zvolte položku **Visual Basic** a vyberte šablonu **Knihovna tříd** . Ujistěte se, že verze .NET Framework je nastavený na **4.5.2**.

    ![Vytvoření projektu rozhraní stavové službě][vs-add-class-library-project]

3. Aby rozhraní možné používat tak, že `ServiceProxy`, musí pocházet ze rozhraní IService. Rozhraní nachází v jednom z balíčků NuGet struktury služby. Přidat balíček, klikněte pravým tlačítkem Nový projekt knihovny tříd a zvolte **Spravovat balíčků NuGet**.

4. Vyhledejte balíček **Microsoft.ServiceFabric.Services** a nainstalovat ho.

    ![Přidání služeb NuGet balíčku][vs-services-nuget-package]

5. V knihovně tříd vytvoření rozhraní s jedinou metodu `GetCountAsync`, a rozšíření rozhraní z IService.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Implementace uživatelského rozhraní stavová služba

Teď můžeme definovali rozhraní, potřebujeme implementace ve službě stavové.

1. Ve stavové služby přidejte odkaz na projektu třídy knihovny obsahující rozhraní.

    ![Přidání odkazu na projektu třídy knihovny v stavová služba][vs-add-class-library-reference]

2. Vyhledejte předmětu, které dědí z `StatefulService`, jako například `MyStatefulService`, a rozšíření ho provádět `ICounter` rozhraní.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Teď implementujte jednu metodu, která je definovaná v `ICounter` rozhraní `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Vystavení stavové služby pomocí posluchače vzdálené služby

S `ICounter` rozhraní implementovaná posledním krokem s povolením stavová služba je možné volat z jiných služeb, je otevřete komunikační kanál. Stavová státní, struktury služby poskytuje metodu overridable názvem `CreateServiceReplicaListeners`. Tímto způsobem můžete určit jeden nebo více komunikace posluchače, v závislosti na typu komunikace, které chcete povolit služby.

>[AZURE.NOTE] Odpovídající metodu pro otevírání komunikační kanál příslušnosti služeb se nazývá `CreateServiceInstanceListeners`.

V tomto případě jsme nahradí stávající `CreateServiceReplicaListeners` metoda a poskytovat instance `ServiceRemotingListener`, které vytvoří RPC koncový bod, který je možné volat ze klientů prostřednictvím `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Slouží k interakci se službou ServiceProxy třídy

Naše stavová služba je nyní připravena k přijímat přenosy z jiných služeb. Takže zůstane je přidání kódu pro komunikaci s nimi z webové služby ASP.NET.

1. V ASP.NET projektu, přidejte odkaz na knihovna tříd, který obsahuje `ICounter` rozhraní.

2. V nabídce **vytvořit** spusťte **Správce konfigurace systému**. Měli byste vidět přibližně takto:

    ![Knihovna tříd zobrazující Správce konfigurace jako AnyCPU][vs-configuration-manager]

    Všimněte si, že projekt knihovna tříd **MyStatefulService.Interface**nakonfigurovaný tak, aby sestavit pro libovolný procesor. Správně zprovoznit struktury služby, je nutné explicitně poskytované x64. Klikněte na rozevírací seznam platformy a zvolte **Nový**a pak vytvořit x64 konfiguraci platformy.

    ![Vytvoření nového platformu pro knihovna tříd][vs-create-platform]

3. Přidáte do Microsoft.ServiceFabric.Services ASP.NET projektu, stejně jako pro projekt knihovna tříd dříve. Bude k dispozici `ServiceProxy` předmětu.

4. Ve složce **řadiče** otevřete `ValuesController` předmětu. Všimněte si, že `Get` metoda aktuálně jenom vrátí matici pevně řetězec "hodnota1" a "hodnota2" – což odpovídá co jsme viděli dříve v prohlížeči. Tato implementace nahraďte následující kód:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    První řádek kódu je klíče. Vytvoření proxy ICounter stavové služby, musíte zadat dvěma datovými informací: ID oddílu a název služby.

    Můžete použít rozdělení stavové službám měřítko tak, že rozdělení stavu do různých blocích podle klíč, který určíte, jako je ID zákazníka nebo PSČ. V našem důvodu aplikace má stavová služba pouze jeden oddíl tak klávesu nezáleží. Klíč, který zadáte povede do stejného oddílu. Další informace o rozdělení služeb, najdete v článku [jak oddíl struktury spolehlivé služby](service-fabric-concepts-partitioning.md).

    Identifikátor URI struktury formuláře je název služby: /&lt;název_aplikace&gt;/&lt;service_name&gt;.

    S těmito dvěma datovými informace struktury služby jednoznačně identifikovat počítač, který by měl být odešle žádost o. `ServiceProxy` Třídy také bezproblémově pracuje s případ, kdy se nezdaří počítače hostujícího oddílu stavová služba a jiného počítače musí úroveň se zvýší jeho proběhnout. Tento odběru zajišťuje psaní kód klienta pro práci s jinými službami výrazně zjednodušuje.

    Když máme proxy server, můžeme jednoduše vyvolat `GetCountAsync` metoda a jeho výsledek.

5. Stisknutím klávesy F5 znova spusťte aplikaci změněné. Jako dřív, Visual Studio automaticky spustí prohlížeč kořenový web projektu. Přidejte si cestu "rozhraní api/hodnoty" a byste měli vidět aktuální hodnota čítač.

    ![Stavová čítač hodnota zobrazená v prohlížeči][browser-aspnet-counter-value]

    Aktualizujte prohlížeč pravidelně zobrazíte hodnotu čítač aktualizovat.


>[AZURE.WARNING] Webový server ASP.NET Core uvedenou v šabloně jmenoval Kestrel, je [aktuálně podporované pro zpracování přímé internetový provoz](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Výrobní scénáře, zvažte možnost umístění svého ASP.NET Core koncové body za [Správu rozhraní API] [ api-management-landing-page] nebo jiné internetového brány. Všimněte si, že služba struktury nepodporuje nasazení služby IIS.


## <a name="what-about-actors"></a>Co dávat pozor účastníky?

Tento kurz se zaměřuje na přidání web přední komunikovali stavová služba. Však můžete sledovat velmi podobné modelu ke komunikaci s účastníky. Ve skutečnosti je poněkud jednodušší.

Při vytváření projekt actor Visual Studio automaticky vygeneruje projekt rozhraní za vás. Rozhraní umožňuje generovat actor proxy v project web komunikovat s actor. Komunikační kanál je k dispozici automaticky. Takže nemusíte udělat nic, která je ekvivalentní k vytvoření `ServiceRemotingListener` jako jste provedli pro službu stavové v tomto kurzu.

## <a name="how-web-services-work-on-your-local-cluster"></a>Jak fungují webové služby na svůj místní cluster

Můžete obecně nasazení přesně stejné služby struktury aplikace do více počítačů clusteru, které nasazené na svůj místní cluster a mít vysoce jistotu, že to funguje očekávaným. Důvodem je místní clusteru je jednoduše pět uzel konfiguraci, která je sbalený na jednom počítači.

Když přijde k webovým službám, je však jeden klíčové nuance. Když svůj cluster nachází za při vyrovnávání zatížení, stejně jako v Azure, musíte se ujistit, že vaše webové služby od Vyrovnávání zatížení se jednoduše kruhového přenosy přes strojů zavedení na každém počítači. Můžete to udělat pomocí nastavení `InstanceCount` pro službu zvláštní hodnotu "-1".

Naopak když spustíte webové služby místně, je nutné zajistit dané jenom jedna instance běží služba. V ostatních případech bude narazíte na konflikty z více procesů, které naslouchají na stejnou cesty a portu. Počet instanci služby web jako výsledek, je třeba nastavit na "1" místní nasazení.

Naučíte se konfigurovat odlišné hodnoty pro různé prostředí, najdete v článku [Správa parametry aplikace na více prostředí](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Další kroky

- [Vytvoření clusteru v Azure pro nasazení aplikace v cloudu](service-fabric-cluster-creation-via-portal.md)
- [Další informace o komunikaci se službami](service-fabric-connect-and-communicate-with-services.md)
- [Další informace o vytváření oddílů stavová služba](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
