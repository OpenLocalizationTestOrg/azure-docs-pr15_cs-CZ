<properties 
   pageTitle="Poznámky k verzi Azure SDK pro .NET 2.9" 
   description="Poznámky k verzi Azure SDK pro .NET 2.9" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>Poznámky k verzi Azure SDK pro .NET 2.9

##<a name="overview"></a>Základní informace

Tento dokument obsahuje poznámky k verzi pro SDK Azure .NET 2.9 verzi. 

Podrobné informace o aktualizacích v této verzi najdete v článku [Azure SDK 2.9 oznámení publikovat](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Zobrazení náhledu Azure SDK 2.9 for Visual Studio 2015 aktualizace 2 a Visual Studio "15"
 
Tato aktualizace zahrnuje tyto opravy chyb:

- Problém týkající se UMÍSTĚTE generování klientského rozhraní API v ve kterém se zobrazí řetězec "Neznámý typ" jako název složky gen kód nebo název oboru nezobrazí do generovaného kódu.
- Problém týkající se naplánované WebJobs, ve kterém byl ověřovací údaje selhání předávat Plánovač zřizování obrázku.

Tato aktualizace obsahuje tyto nové funkce:

- Podpora služby sekundární aplikace na kartě "Služby" obrazovky s dialogem zřizovací aplikaci služby. 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure datové jezera nástroje pro aktualizaci 2015 Visual Studio 2
 
Tato aktualizace zahrnuje následující:

- **Azure Data jezera Tools** for Visual Studio je teď sloučit SDK Azure .NET verzi. Nástroj je automaticky instalován při instalaci Azure SDK. 

    Nástroj se aktualizuje často, přejděte [sem](http://aka.ms/datalaketool) získáte aktualizace.

- **Průzkumník serveru** teď můžete zobrazit všechny a vytvářet některé entity metadat U SQL. Další informace najdete v tématu [tomto](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogu.


##<a name="hdinsight-tools"></a>HDInsight nástroje 

**HDInsight nástroje** for Visual Studio nyní podporuje HDInsight verze 3.3, včetně zobrazující Tez grafy a opravy jiný jazyk.


##<a name="azure-resource-manager"></a>Azure správce prostředků 

Tato verze přidá [KeyVault](../resource-manager-keyvault-parameter.md) podporu pro ARM šablony.

##<a name="see-also"></a>Viz taky

[Publikovat oznámení Azure SDK 2.9](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
