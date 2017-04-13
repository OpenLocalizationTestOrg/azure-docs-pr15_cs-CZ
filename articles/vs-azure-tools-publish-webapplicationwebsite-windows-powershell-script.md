<properties
   pageTitle="Publikování-WebApplicationWebSite (skriptu prostředí Windows PowerShell) | Microsoft Azure"
   description="Zjistěte, jak publikovat web projektu na Azure Web. Jestliže neexistují tento skript vytvoří požadované zdroje předplatné Azure."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publikování-WebApplicationWebSite (skriptu prostředí Windows PowerShell)

##<a name="syntax"></a>Syntaxe

Web projektu publikuje Azure Web. Jestliže neexistují vytvoří skript požadované zdroje v Azure předplatné.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfigurace

Cesta k souboru konfigurace JSON, který popisuje podrobnosti o nasazení.

|Parametr|Výchozí hodnota|
|---|---|
|Aliasy|žádná|
|Povinné?|PRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

## <a name="subscriptionname"></a>SubscriptionName

Název Azure předplatné, které chcete vytvořit web v.

|Parametr|Výchozí hodnota|
|---|---|
|Aliasy|žádná|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

## <a name="webdeploypackage"></a>WebDeployPackage

Cesta k balíček pro nasazení web publikování na web. Pomocí Průvodce Publikovat Web ve Visual Studiu můžete vytvořit balíček. Další informace najdete v tématu [Začínáme s Azure cloudovými službami a ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parametr|Výchozí hodnota|
|---|---|
|Aliasy|žádná|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Uživatelské jméno a heslo pro databázi SQL Azure.

|Parametr|Výchozí hodnota|
|---|---|
|Aliasy|žádná|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|žádná|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Pokud je hodnota true, Tisk zprávy, skript výstupního datového proudu.

|Parametr|Výchozí hodnota|
|---|---|
|Aliasy|žádná|
|Povinné?|NEPRAVDA|
|Umístění|s názvem|
|Výchozí hodnota|NEPRAVDA|
|Zadávají kanálem k odesílání zpráv?|NEPRAVDA|
|Použít zástupné znaky?|NEPRAVDA|

## <a name="remarks"></a>Poznámky:

Podrobné informace o použití skript k vytvoření vývojáře a testovací prostředí, najdete v článku [Pomocí skriptů Windows Powershellu publikovat pro vývojáře a Test prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfigurační soubor JSON Určuje podrobnosti co má být nasazené. Obsahuje informace, které jste zadali při vytváření projektu, třeba název a uživatelské jméno pro web. Také databázi zřízení, pokud existuje. Následující kód ukazuje příklad JSON konfiguračního souboru:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Můžete upravit tento soubor konfigurace JSON změnit co nasazení. Požaduje oddíl webu, ale části databáze je nepovinný krok.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Publikování WebApplicationVM (skriptu prostředí Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)
