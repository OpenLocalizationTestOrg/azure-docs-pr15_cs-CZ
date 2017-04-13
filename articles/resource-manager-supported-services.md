<properties
   pageTitle="Správce prostředků podporované služby | Microsoft Azure"
   description="Popisuje poskytovatelů zdroje, které podporují správce prostředků, jejich schémata a dostupných verzí rozhraní API a oblasti, které můžete hostovat zdroje."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Správce prostředků poskytovatelů, oblasti, verze rozhraní API a schémata

Azure správce prostředků poskytuje nový způsob nasazením a správou služeb, které tvoří aplikace. Většina, ale ne všech služeb podpory správce prostředků a některých služeb jen částečně podporují správce prostředků. Toto téma obsahuje seznam poskytovatelů podporované zdroje pro správce prostředků Azure.

Když nasadíte zdrojů, budete potřebovat vědět, oblasti, které tyto prostředky podpory a které rozhraní API jsou k dispozici pro zdroje. V části [podporované oblastí](#supported-regions) ukazuje, jak zjistit, která pracovní oblastí pro svoje předplatné a prostředky. V části [podporované rozhraní API verze](#supported-api-versions) se dozvíte, jak zjistit, jakou verzi rozhraní API můžete použít.

Služby, které podporují služby Azure portálem a klasické portál najdete v tématu [Azure portálu dostupnost grafu](https://azure.microsoft.com/features/azure-portal/availability/). Přesunutí prostředky podpory služby, které najdete v tématu [přesunutí zdrojů do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).

Následující tabulka uvádí, které nasazení služby podpory společnosti Microsoft a správy pomocí Správce prostředků a které ne. Odkaz ve sloupci **Rychlý úvod šablony** odešle dotaz do úložiště Azure rychlý úvod šablony pro daný zdroj poskytovatele. Rychlý úvod šablony se přidá a často aktualizovány. Informace o stavu odkaz pro konkrétní službu neznamená, že dotaz vrací šablony z úložiště. Jsou taky spoustu poskytovatelů jiného zdroje, které podporují správce prostředků. Informace o najdete v článku všech poskytovatelů zdroje v části [poskytovatelů zdroje a typy](#resource-providers-and-types) .


## <a name="compute"></a>Výpočet

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------------------------ |-------- | ------ | ------ |
| Dávkové   | Ano     | [Dávkové REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015-12: 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Kontejner | Ano | [Kontejner služba REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016 – 03 – 30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics životního cyklu služby | Ano  |      |        |  |
| Sady měřítko | Ano | [Nastavte měřítko REST](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Služba struktury | Ano  | [Služba Rest struktury](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtuálních počítačích | Ano | [ZBÝVAJÍCÍ OM](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuálních počítačích (klasický) | Omezené | - | - | - |
| Vzdálené aplikace | Ne      | -  | - | - |
| Cloud Services (klasický) | Omezené (viz níže) | - | - | - |

Virtuálních počítačích (klasický) odkazuje na zdroje, které byly nasazeny prostřednictvím klasické nasazení modelu, ne prostřednictvím nasazení modelu správce prostředků. Obecně tyto materiály nepodporují operace správce prostředků, ale některé operace, kteří mají povolenou. Další informace o těchto modelech nasazení najdete v článku [Správce prostředků Principy nasazení a klasické nasazení](resource-manager-deployment-model.md). 

Cloud Services (klasické) se dá používat se dalších klasické zdrojů; klasický zdroje však není využít funkce všechny správce prostředků a ostatní vás vhodný pro budoucí řešení. Místo toho zvažte možnost infrastrukturu aplikace používat zdroje z obory názvů Microsoft.Compute Microsoft.Storage a Microsoft.Network.


## <a name="networking"></a>Sítě

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | -------  | -------- | ------ | ------ |
| Aplikace brány | Ano | [Aplikace brány REST](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Ano | [ZBÝVAJÍCÍ DNS](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016 04 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Ano  | [ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Vyrovnávání zatížení | Ano  | [Vyrovnávání zatížení REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Přenosy správce | Ano | [ZBÝVAJÍCÍ přenosy správce](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-11 – 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuální sítě | Ano| [Virtuální sítě REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| Brána VPN | Ano | [Brána sítě REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[připojení](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Úložiště

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Úložiště | Ano  | [Úložiště REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Úložiště účtu](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Ano  |         |        |  |

## <a name="databases"></a>Databáze

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Ano  | [DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015 04 08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis mezipaměti | Ano |   | [2016 04 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| Databáze SQL | Ano | [Databáze SQL REST](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014 04 01 náhledu](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| Datový sklad SQL | Ano |   |      |


## <a name="web--mobile"></a>Web a Mobile

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| Rozhraní API aplikace | Ano |   | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Rozhraní API aplikace](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| Rozhraní API správy | Ano  | [Rozhraní API správy REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016 07 07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Obsahu moderátorovi | Ano |   |   |   |
| Funkce aplikace | Ano |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Použití logických operátorů aplikace | Ano   | [Pracovní postup služby REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016 06 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobilní aplikace | Ano |  | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobilní závazky | Ano  |  [Mobilní zapojení REST](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Hledání | Ano  | [Hledání REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web Apps | Ano |          | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Technologie pro analýzu

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | -------  | -------- | ------ | ------ |
| Katalog dat | Ano  | [Katalog dat REST](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016 – 03 – 30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Data Factory | Ano | [Data Factory REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Jezera analýzy dat | Ano |   |   [2015 10 01 preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Úložiště jezera dat | Ano  | [ZBÝVAJÍCÍ jezera úložiště dat](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015 10 01 preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Ano | [HDInsights REST](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Výukové počítače | Ano | [Počítač výukové REST](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016 05 01 preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Technologie pro analýzu toku | Ano  | [Technologie pro analýzu páry REST](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Ano | [Power BI vložené REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016 – 01 – 29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Analytických nástrojů

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| Kognitivní služby | Ano |  | [2016 02 01 preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internet věci

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| Centrální události | Ano  | [Událost centrální REST](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Ano | [Rozbočovač IoT REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016 02 03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Oznámení o rozbočovače | Ano | [Oznámení o rozbočovači REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015 04 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Média a CDN

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Ano | [ZBÝVAJÍCÍ CDN](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016 04 02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Služba médií | Ano | [Mediální služby REST](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015 10 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Hybridní integrace

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk služby | Ano |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Obnovení služby | Ano | [Obnovení webu REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016 06 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Služba Bus | Ano | [Bus služba REST](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015. 08 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identita a správu přístupu 

Azure Active Directory funguje s správce prostředků povolit řízení přístupu na základě rolí pro vaše předplatné. Další informace o pomocí řízení přístupu na základě rolí a služby Active Directory najdete v tématu [Řízení přístupu na základě rolí Azure](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Karta Vývojář služby 

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| Přehledy aplikace | Ano  | [Přehledy REST](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Mapy Bing | Ano  |          |        |  |
| DevTest Labs | Ano |   | [2016 05 15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Účet aplikace Visual Studio | Ano   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Správa a zabezpečení

| Služba | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------- | ------ | ------ |
| Automatizace | Ano | [Automatizace REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10 – 31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Klíčové trezoru | Ano | [Klíčové trezoru REST](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Klíčové trezoru](resource-manager-template-keyvault.md)<br />[Tajná klíčové trezoru](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Provozní přehledy | Ano |          |        |  |
| Plánovač | Ano  | [Plánovač REST](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016 03 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Zabezpečení (verze preview) | Ano | [Zabezpečení REST](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Správce prostředků

| Funkce | Správce prostředků povolena | ROZHRANÍ REST API | Schéma | Rychlý úvod šablony |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Povolení | Ano | [Správa zámků webů](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Řízení přístupu na základě rolí](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Zámek prostředků](resource-manager-template-lock.md)<br />[Přiřazení rolí](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Zdroje informací | Ano | [Propojené prostředky](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Propojení zdroje](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Typy a poskytovatelů zdroje

Při nasazení prostředků, často potřebujete získat informace o poskytovatelích zdroje a typy. Tyto informace pomocí rozhraní REST API, Azure PowerShell nebo rozhraní příkazového řádku Azure můžete načíst.

Pracovat s poskytovatelem zdroje, musí být zprostředkovatele zdroje registrován u svého účtu. Ve výchozím nastavení jsou automaticky registrována mnohé poskytovatele zdroje. potřebujete však ručně zaregistrovat někteří poskytovatelé zdroje. Následující příklady ukazují, jak získat stav registrace zprostředkovatele zdroje a zaregistrovat zprostředkovatele prostředků v případě potřeby.

### <a name="portal"></a>Portál

Můžete snadno zobrazit seznam podporovaných zdrojů poskytovatelů pomocí následujícího postupu:

1. Vyberte **Moje oprávnění**.

    ![Moje oprávnění](./media/resource-manager-supported-services/my-permissions.png)

2. Vyberte **poskytovatele stav zdroje**

    ![Stav zprostředkovatele zdroje](./media/resource-manager-supported-services/resource-provider-status.png)

3. Zobrazí úplný seznam zprostředkovatelů zdroje pro vaše předplatné.

    ![Seznam zdrojů poskytovatelů](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>ROZHRANÍ REST API

Všechny dostupné zdroje poskytovatelů, včetně jejich typy umístění, verze rozhraní API a stav registrace, použijte operaci [seznam všech poskytovatelů zdroje](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Pokud se musíte zaregistrovat zprostředkovatele zdroje, přečtěte si článek [zaregistrovat předplatné s poskytovatelem zdroje](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>Prostředí PowerShell

Následující příklad ukazuje, jak dostat všechny poskytovatelů dostupné zdroje.

    Get-AzureRmResourceProvider -ListAvailable
    

Následující příklad ukazuje, jak zjistit, jaké typy zdrojů pro daný zdroj poskytovatele.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Výstup je podobný:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Zaregistrujte zprostředkovatele prostředků, zadejte obor názvů:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Následující příklad ukazuje, jak dostat všechny poskytovatelů dostupné zdroje.

    azure provider list
    
Výstup je podobný:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Informace o zprostředkovatele konkrétního zdroje můžete uložit do souboru pomocí následujícího příkazu.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Zaregistrujte zprostředkovatele prostředků, zadejte obor názvů:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Podporovaných oblastí

Při nasazení prostředků, obvykle potřebujete zadejte oblast pro zdroje. Správce prostředků je podporované ve všech oblastech, ale prostředky, které nasadíte nemusí být podporovány ve všech oblastech. Kromě toho může být omezení vašeho předplatného, které bránit v používání některých oblastech, které podporují zdroje. Tato omezení související se dani problémy týkající se výchozí země nebo v důsledku zásad umístěná správcem předplatné používat jenom některých oblastech. 

Kompletní seznam všech podporovaných oblastí pro všechny služby Azure najdete v článku [služby podle regionů](https://azure.microsoft.com/regions/#services); Tento seznam však může zahrnovat oblasti, které předplatné nepodporuje. Můžete zjistit oblasti konkrétního zdroje typ, který podporuje předplatného pomocí portálu, rozhraní REST API, prostředí PowerShell nebo Azure rozhraní příkazového řádku.

### <a name="portal"></a>Portál

Zobrazí se podporovaných oblastí pro typ zdroje následujícími kroky:

1. Vyberte **Další služby** > **Explorer zdroje**.

    ![Průzkumník zdroje](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Rozbalte uzel **poskytovatelů** .

    ![Vyberte poskytovatele](./media/resource-manager-supported-services/select-providers.png)

3. Vyberte poskytovatel zdroje a zobrazení podporovaných oblastí a rozhraní API verze.

    ![zprostředkovatele zobrazení.](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>ROZHRANÍ REST API

Zjistit oblasti, které jsou dostupné pro konkrétní zdroje typ ve vašem předplatném pomocí operace [seznam všech poskytovatelů zdroje](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>Prostředí PowerShell

Následující příklad ukazuje, jak získat podporovaných oblastí pro weby.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Výstup je podobný:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Následující příklad vrátí všechna umístění podporovaných u jednotlivých typů zdrojů.

    azure location list

Můžete taky filtrovat výsledky umístění s nástrojem JSON jako [jq](https://stedolan.github.io/jq/).

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Vracející:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Podporované rozhraní API verze

Při nasazení šablony, je nutné zadat verze rozhraní API pro použití při vytváření jednotlivé zdroje. Verze rozhraní API odpovídá verzi rozhraní REST API operace vydané zprostředkovatelem zdroje. Jako poskytovatele zdroje umožňuje nových funkcí, vydání novou verzi rozhraní REST API. Verze rozhraní API určíte v šabloně ovlivňuje vlastnosti, které můžete použít v šabloně. Obecně který chcete vybrat nejnovější verze rozhraní API při vytváření šablon. Pro existující šablony můžete se rozhodnout, jestli chcete pokračovat ve formátu starší verze rozhraní API aplikace nebo aktualizovat šablony pro nejnovější verzi umožní využít výhod nových funkcí.

### <a name="portal"></a>Portál

Určení podporovaných verzí rozhraní API stejným způsobem jste určili podporovaných oblastí (viz dříve).

### <a name="rest-api"></a>ROZHRANÍ REST API

Chcete-li zjistit, jaký rozhraní API jsou k dispozici pro typy prostředků, pomocí operace [seznam všech poskytovatelů zdroje](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>Prostředí PowerShell

Následující příklad ukazuje, jak získat dostupných verzí rozhraní API pro typ určitého zdroje.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Výstup je podobný:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Zprostředkovatele prostředků do souboru pomocí následujícího příkazu lze uložit informace (včetně dostupných verzí rozhraní API).

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Otevřete soubor a najděte prvek **apiVersions**

## <a name="next-steps"></a>Další kroky

- Další informace o vytváření správce prostředků šablon najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
- Další informace o nasazení materiály, najdete v článku [nasadit aplikaci Správce prostředků Azure šablony](resource-group-template-deploy.md).
