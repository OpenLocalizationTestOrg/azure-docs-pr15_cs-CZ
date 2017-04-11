<properties
    pageTitle="Co je Cloudová služba modelu a balíček | Microsoft Azure"
    description="Popisuje cloudové služby modelu (.csdef, .cscfg) a balíčku (.cspkg) v Azure"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Co je Cloudová služba modelu a jak ho balíček?
Do cloudové služby je vytvořen tři součásti, definice služby _(.csdef)_, config služby _(.cscfg)_a balíčku služby _(.cspkg)_. Soubory **ServiceDefinition.csdef** a **ServiceConfig.cscfg** založeném na jazyce XML a popisu struktury Cloudová služba a jak je nakonfigurovaný. nazývají modelu. **ServicePackage.cspkg** je zip soubor, který je generováno z **ServiceDefinition.csdef** a mimo jiné, že obsahuje všechny požadované binární závislosti. Azure vytvoří do cloudové služby z **ServicePackage.cspkg** a **ServiceConfig.cscfg**.

Jakmile cloudovou službu běží v Azure, je lze překonfigurovat prostřednictvím souboru **ServiceConfig.cscfg** , ale nemůžete změnit definici.

## <a name="what-would-you-like-to-know-more-about"></a>Co chcete vědět víc?

* Budu chtít vědět víc o [ServiceDefinition.csdef](#csdef) a [ServiceConfig.cscfg](#cscfg) soubory.
* Už vím, o, získávat [pár příkladů](#next-steps) , co můžete nakonfigurovat.
* Chci vytvořit [ServicePackage.cspkg](#cspkg).
* Používám Visual Studiu a co chcete udělat...
    * [Vytvoření nového cloudové služby][vs_create]
    * [Změna konfigurace existující cloudové služby][vs_reconfigure]
    * [Nasazení projektu cloudové služby][vs_deploy]
    * [Vzdálená plocha na instance služby cloudu][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Soubor **ServiceDefinition.csdef** určuje nastavení, které využívají Azure konfigurace do cloudové služby. [Definice schématu služby Azure (.csdef soubor)](https://msdn.microsoft.com/library/azure/ee758711.aspx) poskytuje povolenou formát souboru definice služby. Následující příklad ukazuje nastavení, která je to možné definovat pro Web a pracovní role:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Můžete vytvořit odkaz [[služby definice schématu]] půjde vám ještě snadněji pochopit schéma XML použité v tomto poli, ale tady je stručný popis některé prvky:

**Weby**  
Definice pro weby webové aplikace, které jsou hostované v IIS7 obsahuje.

**InputEndpoints**  
Obsahuje definice pro koncové body, které se používají kontaktovat cloudovou službu.

**InternalEndpoints**  
Obsahuje definice pro koncové body, které využívají instance role pro vzájemně komunikovat.

**ConfigurationSettings**  
Obsahuje nastavení definice týkající se funkcí konkrétní rolí.

**Certifikáty**  
Obsahuje definice certifikátů, které jsou potřebné pro roli. Předchozí příklad ukazuje certifikát, který se používá pro konfiguraci Azure připojení.

**LocalResources**  
Obsahuje definice pro místní úložiště zdroje. Místní uložení zdroje je rezervovaná adresář systému souborů virtuálního počítače, ve kterém je spuštěný instanci roli.

**Importy**  
Obsahuje definice pro importované moduly. Předchozí příklad ukazuje moduly pro připojení ke vzdálené ploše a připojte Azure.

**Při spuštění**  
Obsahuje úkoly, které se spouští při spuštění roli. Úkoly se definují cmd nebo spustitelný soubor.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Konfigurace nastavení pro vaše cloudové služby, je určený podle hodnot v poli **ServiceConfiguration.cscfg** soubor. Zadejte počet instance, které chcete nasadit pro každou roli v tomto souboru. Hodnoty pro nastavení konfigurace, které jste definovali v souboru definice služby se přidají do souboru konfigurace služby. Thumbprints všech Správa certifikátů, které jsou přidružené k cloudové služby se přidají také do souboru. [Schéma konfigurace služby Azure (.cscfg soubor)](https://msdn.microsoft.com/library/azure/ee758710.aspx) poskytuje povolenou formát souboru konfigurace služby.

Konfigurační soubor služby není součástí aplikací, ale je nahráli Azure jako samostatný soubor a se používá ke konfiguraci cloudovou službu. Můžete nahrát nový soubor konfigurace služby znovu nasazení cloudové služby. Konfigurace hodnoty cloudovou službu bude možné měnit je spuštěná cloudovou službu. Následující příklad ukazuje nastavení definované pro Web a pracovní role:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Můžete odkázat [Schématu konfigurace služby](https://msdn.microsoft.com/library/azure/ee758710.aspx) pro lepší Principy XML schématu tady, ale tady je rychlý vysvětlení prvků:

**Případy**  
Konfiguruje počet spuštěné instance rolí. Zabránit potenciálně nedostupnost během upgrady cloudové služby, je doporučená nasazení více než jedna instance web vystavený role. Tímto způsobem, jsou splněny pokyny v [Azure výpočet služby úroveň dohodu (SLA)](http://azure.microsoft.com/support/legal/sla/), která zaručuje 99.95 % externí připojení pro internetové role nasazené dvě nebo více role instancí služby.

**ConfigurationSettings**  
Konfiguruje nastavení spuštěné instance pro roli. Název `<Setting>` prvky se musí shodovat nastavení definice v souboru definice služby.

**Certifikáty**  
Konfiguruje certifikáty, které využívají službu. Předchozí příklad ukazuje, jak definovat certifikát modulu vzdáleného přístupu. Hodnota atributu *Miniatura* musíte nastavit Miniatura certifikátu používat.

<p/>

 >[AZURE.NOTE] Miniatura certifikátu lze přidat do konfiguračního souboru pomocí textového editoru nebo hodnoty můžete přidat na kartě **certifikáty** na stránce **Vlastnosti** role ve Visual Studiu.



## <a name="defining-ports-for-role-instances"></a>Definování porty instancí rolí
Azure umožňuje pouze jedna položka přejděte na web roli. To znamená, že všechny přenosy dojde prostřednictvím IP adres. Můžete nakonfigurovat vaše weby sdílet port nakonfigurováním záhlaví hostitele směrování požadavku na správné místo. Můžete taky nakonfigurovat aplikace poslouchat známých portů na IP adresu.

Následující příklad ukazuje konfiguraci role web s webu a webové aplikace. Na webu nakonfigurování jako výchozí umístění položky na portu 80 a webových aplikací není nakonfigurován přijímat požadavky od hostitele alternativní záhlaví, která je označená jako "mail.mysite.cloudapp.net".

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Změna konfigurace role
Konfigurace cloudové služby můžete aktualizovat ho je spuštěná v Azure, bez nastavování služby do offline režimu. Chcete-li změnit informace o konfiguraci, můžete nahrát nový soubor konfigurace nebo upravte konfiguračního souboru na místě a použít pracovního služby. Konfigurace služby lze následující změny:

- **Změna hodnoty nastavení**  
Při konfiguraci nastavení změn, můžete zvolit instanci rolí změnu potvrďte při instance je online nebo odpadkového instance řádně a použít změny při instance je offline.

- **Změna topologie služby instancí rolí**  
Topologie změny se neprojeví spuštěné instance, s výjimkou případů, kde má být odebrán instance. Všechny zbývající instance obecně nemusí být z koše; Můžete však odpadkového role instance topologie změny.

- **Změna miniaturu certifikátu**  
Certifikát lze aktualizovat pouze pokud instanci rolí je offline. Pokud certifikát je přidat, odstranit nebo změnit, dokud instanci rolí je online, Azure řádně má instance offline aktualizovat certifikát a znovu režimu online po dokončení změny.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Zpracování konfigurace změn pomocí události Runtime služby
[Knihovna Runtime Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) obsahuje obor názvů [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) , který obsahuje třídy pro komunikaci s Azure prostředí z kód spuštěný v instanci roli. Třídy [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definuje následující události, které jsou aktivovány před a po změně konfigurace:

- **[Změna](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) události**  
K tomu dojde, před použitím změna konfigurace zadaný instanci role s možností umožní dolů instance role případné potíže.
- **[Změněné](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) události**  
Nastane poté konfigurace změna se projeví zadaný instanci roli.

> [AZURE.NOTE] Protože certifikát změny vždy jako offline výskyty roli, vyvolají není RoleEnvironment.Changing nebo RoleEnvironment.Changed události.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Nasazení aplikace jako do cloudové služby Azure, musíte nejdřív balíček aplikace v odpovídajícím formátu. Vytvořit soubor balíčku jako alternativu k Visual Studio můžete nástroj **CSPack** příkazového řádku (nainstalované s [Azure SDK](https://azure.microsoft.com/downloads/)).

**CSPack** používá obsah souboru definice služby a služby konfigurační soubor k definování obsah balíčku. **CSPack** vygeneruje soubor balíčku aplikace (.cspkg), můžete na nahrát Azure pomocí [Azure portálu](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Ve výchozím nastavení se nazývá balíček `[ServiceDefinitionFileName].cspkg`, ale zadejte jiný název pomocí `/out` možnost **CSPack**.

**CSPack** obecně nachází na  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (ve windows) je k dispozici spuštěním, které máte nainstalované s SDK zástupce **Microsoft Azure příkazový řádek** .  
>  
>Automatické spuštění programu CSPack.exe zobrazíte si přečtěte následující dokumentaci o všech možných přepínače a příkazy.

<p />

>[AZURE.TIP]
Spustit cloudové služby místně v **Microsoft Azure výpočet emulátoru**, použijte možnost **/copyonly** tato možnost slouží ke kopírování binární soubory aplikace do adresáře rozložení odkud jejich spuštěním ve výpočetním emulátoru.

### <a name="example-command-to-package-a-cloud-service"></a>Příklad příkazu zabalit do cloudové služby
Následující příklad vytvoří s informacemi o role webového balíčku aplikace. Příkaz určuje soubor definice služby používat, v adresáři, kde najdete binární soubory, a název souboru balíčku.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Obsahuje-li aplikaci roli web a pracovní role, je použita následující příkaz:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Kde jsou proměnné definována následujícím způsobem:

| Proměnná                  | Hodnota |
| ------------------------- | ----- |
| \[DirectoryName\]         | Podadresáře v kořenovém adresáři projektu, který obsahuje požadovaný soubor .csdef Azure projektu.|
| \[ServiceDefinition\]     | Název souboru definice služby. Ve výchozím nastavení je ServiceDefinition.csdef název tohoto souboru.  |
| \[Název_výstupního_souboru\]        | Název souboru vygenerovaný balíčku. Obvykle to nastavenou název aplikace. Pokud je zadán žádný název souboru, vytvoří se balíček aplikace jako \[ApplicationName\].cspkg.|
| \[RoleName\]              | Název rolí definované v souboru definice služby.|
| \[RoleBinariesDirectory] | Umístění binární soubory rolí.|
| \[Virtuální_cesta\]           | Pole fyzicky adresářů pro každou virtuální cestu podle části webů definice služby.|
| \[Fyzická_cesta\]          | Pole fyzicky adresáře obsah pro každou virtuální cestu podle uzel webu definice služby.|
| \[RoleAssemblyName\]      | Název souboru binární rolí.| 


## <a name="next-steps"></a>Další kroky

Mám vytvoření balíčku služby cloudu a co chcete udělat...

* [Nastavení ke vzdálené ploše pro instanci služby cloudu][remotedesktop]
* [Nasazení projektu cloudové služby][deploy]

Používám Visual Studiu a co chcete udělat...

* [Vytvoření nového cloudové služby][vs_create]
* [Změna konfigurace existující cloudové služby][vs_reconfigure]
* [Nasazení projektu cloudové služby][vs_deploy]
* [Nastavení ke vzdálené ploše pro instanci služby cloudu][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
