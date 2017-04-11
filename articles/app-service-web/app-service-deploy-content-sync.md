<properties
    pageTitle="Synchronizace obsahu ve složce cloudu pro aplikaci služby Azure"
    description="Naučte se nasadit aplikaci služby Azure aplikace prostřednictvím synchronizace obsahu ve složce cloudu."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronizace obsahu ve složce cloudu pro aplikaci služby Azure

Tento kurz se dozvíte, jak nasazení služby [Azure aplikaci](http://go.microsoft.com/fwlink/?LinkId=529714) synchronizace obsahu z oblíbených cloudové úložiště služby, jako třeba Dropbox a OneDrive. 

## <a name="overview"></a>Základní informace o nasazení synchronizace obsahu

Nasazení synchronizace obsahu na vyžádání používá technologii [Kudu nasazení engine](https://github.com/projectkudu/kudu/wiki) integrovaný s aplikaci služby. Na [Portálu Azure](https://portal.azure.com)můžete určit složku v cloudového úložiště, pracovat s kódem aplikace a obsah do této složky a synchronizovat s aplikací služby kliknutím tlačítko. Synchronizace obsahu využívá Kudu proces vytvoření a nasazení. 
    
## <a name="contentsync"></a>Jak povolit nasazení synchronizace obsahu
Povolení obsahu synchronizace z [Portálu Azure](https://portal.azure.com), postupujte takto:

1. Ve vaší aplikaci zásuvné na portálu Azure, klikněte na **Nastavení** > **Zdroj zavedení**. Klikněte na **Zvolit zdroj**a potom vyberte **OneDrive** nebo **Dropbox** jako zdroj pro nasazení. 

    ![Synchronizace obsahu](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Z důvodu základní rozdíly v rozhraní API není v současné době podporované **Onedrivu pro firmy** . 

2. Dokončení pracovního postupu se tak mohli ověřovat povolit aplikaci služby pro přístup k cestu konkrétní předdefinovaných určené pro OneDrive nebo Dropbox veškerého obsahu aplikaci služby uložení.  
    Po povolení služby aplikace platformy vám nabídne možnost vytvoření obsahu složky určené obsahu cestě nebo zvolte existující obsah složky v části cesta určené obsahu. Určené cesty obsahu v části účty cloudové úložiště pro synchronizační aplikaci služby jsou následující:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Po počáteční obsahu synchronizace obsahu synchronizace můžete být, které iniciuje jako službu z portálu Microsoft Azure. Historie nasazení je dostupný u zásuvné **nasazení** .

    ![Historie nasazení](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Další informace o nasazení Dropbox je k dispozici v části [instalovat z Dropboxu](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


