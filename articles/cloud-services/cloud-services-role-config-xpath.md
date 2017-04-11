<properties 
pageTitle="Cloud Services Role config XPath pomocný | Microsoft Azure" 
description="Různá nastavení XPath můžete v konfiguraci rolí služby cloudu vystavit nastavení jako proměnná prostředí." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Konfigurace nastavení role vystavit jako datový proměnná prostředí s výrazu XPath.

V cloudové služby pracovních nebo soubor definice služby role web můžete hodnoty konfigurace runtime vystavit jako proměnné. Následující výraz XPath hodnoty jsou podporovány (které hodnotám rozhraní API).

Tyto hodnoty XPath jsou k dispozici prostřednictvím [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) knihovny. 

## <a name="app-running-in-emulator"></a>Aplikace spuštěné v emulátoru

Označuje, že aplikace běží v emulátor.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Kód  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>ID nasazení

Načte ID nasazení instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Kód  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Role ID 

Načte ID role aktuální instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kód  | var id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Aktualizace domény

Načte domény aktualizace instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kód  | var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Poruchy domény

Načte domény poruch instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kód  | var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Název role

Načte role název instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kód  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Konfigurace nastavení

Vyhledá hodnotu zadanou konfiguraci nastavení.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Kód  | nastavení var = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Místní uložení cesta

Načte cestu místní úložiště u instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Kód  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Místní uložení velikost

Načte velikost místní úložiště u instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Kód  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Koncový bod Protocol (protokol) 

Načte protokol koncový bod pro instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kód  | var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protocol (protokol); |

## <a name="endpoint-ip"></a>Koncový bod IP

Získá IP adresu určený koncový bod.

| Typ | Příklad |
| ----- | ---- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kód  | var adresu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Port koncového bodu 

Načte port koncového bodu instance.

| Typ  | Příklad |
| ----- | ------- |
| Výraz XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kód  | var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Příklad

Tady je příklad roli pracovníka, která vytvoří úkol při spuštění proměnné s názvem `TestIsEmulated` nastavena [ @emulated xpath hodnotu](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Další kroky

Další informace o souboru [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Vytvoření balíčku [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) .

Povolte [připojení ke vzdálené ploše](cloud-services-role-enable-remote-desktop.md) pro roli.
