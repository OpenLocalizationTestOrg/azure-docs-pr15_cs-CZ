<properties
   pageTitle="Publikování WebApplicationVM | Microsoft Azure"
   description="Naučte se nasadit webové aplikace do virtuálního počítače. Jestliže neexistují tento skript vytvoří požadované zdroje předplatné Azure."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publikování-WebApplicationVM (skriptu prostředí Windows PowerShell)

Nasadí webové aplikace do virtuálního počítače. Jestliže neexistují vytvoří skript požadované zdroje v Azure předplatné.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfigurace

Cesta k souboru konfigurace JSON, který popisuje podrobnosti o nasazení.

|Aliasy|žádná|
|---|---|
|Povinné?|PRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

### <a name="subscriptionname"></a>SubscriptionName

Název Azure předplatné, ve kterém chcete vytvořit virtuální počítač.

|Aliasy|žádná|
|---|---|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|Použije první předplatné v souboru předplatného|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

### <a name="webdeploypackage"></a>WebDeployPackage

Cesta k nasazení balíčku webového publikovat do virtuálního počítače. Pomocí Průvodce Publikovat Web ve Visual Studiu můžete vytvořit balíček. V tématu [jak: vytvořit balíček pro nasazení Web ve Visual Studiu](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliasy|žádná|
|---|---|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

### <a name="allowuntrusted"></a>AllowUntrusted

Pokud je hodnota true, povoluje použití certifikáty, které nejsou podepsáno důvěryhodná kořenová autorita.

|Aliasy|žádná|
|---|---|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|NEPRAVDA|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

### <a name="vmpassword"></a>VMPassword

Přihlašovací údaje účtu virtuálního počítače. Příklad: - VMPassword @{Name = "správce"; Heslo = "heslo"}

|Aliasy|žádná|
|---|---|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Přihlašovací údaje pro databázi SQL Azure. Příklad: - DatabaseServerPassword @{Name = "správce"; Heslo = "heslo"}

|Aliasy|žádná|
|---|---|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Pokud je hodnota true, Tisk zprávy, skript výstupního datového proudu.

|Aliasy|žádná|
|---|---|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|NEPRAVDA|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

## <a name="remarks"></a>Poznámky:

Podrobné informace o použití skript k vytvoření vývojáře a testovací prostředí, najdete v článku [Pomocí skriptů Windows Powershellu publikovat pro vývojáře a Test prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfigurační soubor JSON Určuje podrobnosti co má být nasazené. Obsahuje informace, které jste zadali při vytváření projektu, například název, spřažení skupiny, obrázek virtuálního pevného disku a velikost virtuálního počítače. Je taky obsahuje koncové body v počítači virtuální databází zřízení a webové parametry nasazení. Následující kód ukazuje příklad JSON konfiguračního souboru:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Můžete upravit tento soubor konfigurace JSON chcete změnit, co máte k dispozici. Jsou vyžadovány virtuálního počítače a do cloudové služby, ale části databáze je nepovinný krok.
