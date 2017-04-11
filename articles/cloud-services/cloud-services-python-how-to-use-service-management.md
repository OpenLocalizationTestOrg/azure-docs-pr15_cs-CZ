<properties
    pageTitle="Použití rozhraní API (Python) – funkce Příručka pro správu služby"
    description="Zjistěte, jak programově provádět běžné úlohy správy služby z Python."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Jak pomocí služby správy z Python

> [AZURE.NOTE] Rozhraní API služby správy je je někdo jiný nahradil, pomocí nového zdroje správy rozhraní API, aktuálně dostupné ve verzi preview.  Přečtěte si článek [si přečtěte následující dokumentaci řízení zdrojů Azure](http://azure-sdk-for-python.readthedocs.org/) podrobnosti na pomocí nového rozhraní API správy zdrojů z Python.

Tato příručka, uvidíte, jak programově provádět běžné úlohy správy služby z Python. Třídy **ServiceManagementService** v [Azure SDK Python](https://github.com/Azure/azure-sdk-for-python) podporuje programový přístup k hodně služby týkajících se správy funkci, která je dostupná v [Azure klasické portál] [ management-portal] (například **Vytvoření, aktualizace a odstranění cloudové služby, nasazení, služby správy dat a virtuálních počítačích**). Tato funkce může být užitečné při vytváření aplikací, které potřebují programový přístup pro správu služby.

## <a name="WhatIs"> </a>Co je Správa služby
Rozhraní API služby správy poskytuje programový přístup k hodně funkci Správa služeb dostupné prostřednictvím [Azure klasické portál][management-portal]. Azure SDK Python umožňuje spravovat cloudovými službami a úložiště účty.

Použití rozhraní API služby správy, je potřeba [vytvořit účet Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Koncepty
Azure SDK Python zalamuje [Rozhraní API správy služby Azure][svc-mgmt-rest-api], což je rozhraní REST API. Všechny operace rozhraní API provádí přes připojení SSL a vzájemně ověřovány certifikáty x.509v3. Službu pro správu může být dostupný v služba spuštěná v Azure nebo přímo přes Internet z libovolné aplikace, který můžete poslat žádost o konverzaci HTTPS a přijímat odpověď HTTPS.

## <a name="Installation"> </a>Instalace

Všechny funkce popisované v tomto článku jsou dostupné v `azure-servicemanagement-legacy` balíčku, které si můžete nainstalovat pomocí pip. Pro další podrobnosti o instalaci (například pokud začínáte Python), naleznete v tomto článku: [instalace Python a Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Jak: připojení k Správa služby
Připojení k koncového bodu služby správy, potřebujete ID Azure předplatného a certifikát platné správy. ID předplatného prostřednictvím [Azure klasické portál]můžete získat[management-portal].

> [AZURE.NOTE] Od Azure SDK Python v0.8.0 je možné použít certifikáty vytvořené pomocí OpenSSL spuštěná v systému Windows.  Vyžaduje Python 2.7.4 nebo novější. Doporučujeme uživatelé můžou používat OpenSSL místo .pfx, od podpory pro .pfx, které certifikáty pravděpodobně odeberou v budoucnu.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Správa certifikátů v systému Windows nebo Mac a Linux (OpenSSL)
[OpenSSL](http://www.openssl.org/) slouží k vytvoření správy certifikátu.  Potřebujete skutečně vytvořit dva certifikáty, jednu serveru ( `.cer` soubor) a jedno pro klienta ( `.pem` soubor). Vytvoření `.pem` soubor, provést:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Vytvoření `.cer` certifikát a provést:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Podrobnosti o Azure certifikáty najdete v článku [Přehled certifikátů pro službu Azure Cloud Services](./cloud-services-certs-create.md). Detailní popis OpenSSL parametry najdete v dokumentaci na [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Po vytvoření těchto souborů, budete muset nahrát `.cer` soubor, který chcete Azure prostřednictvím "Nahrát" akce kartě "Nastavení" [Azure klasické portál][management-portal], a poznamenejte si místo, kam jste si uložili `.pem` soubor.

Po získání ID předplatného vytvořili certifikát a nahrát `.cer` soubor na Azure, můžete připojit k koncový bod Azure správy předáním id předplatného a cestu k `.pem` soubor, který chcete **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

V předchozím příkladu `sms` je objekt **ServiceManagementService** . Třídy **ServiceManagementService** je třída primární sloužící ke správě Azure služby.

### <a name="management-certificates-on-windows-makecert"></a>Správa certifikátů v systému Windows (MakeCert)

Na počítači pomocí můžete vytvořit certifikát podepsaný svým držitelem správy `makecert.exe`.  Otevřete **Příkazový řádek Visual Studia** jako **Správce** a zadejte následující příkaz nahrazením *AzureCertificate* názvem certifikát, který chcete použít.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Příkaz vytvoří `.cer` souborů a nainstaluje v úložišti **osobních** certifikátů. Další informace najdete v tématu [Přehled certifikátů pro službu Azure Cloud Services](./cloud-services-certs-create.md).

Po vytvoření certifikátu, budete muset nahrát `.cer` soubor, který chcete Azure prostřednictvím "Nahrát" akce kartě "Nastavení" [Azure klasické portál][management-portal].

Po získání ID předplatného vytvořili certifikát a nahrát `.cer` soubor na Azure, můžete připojit k koncový bod Azure správy předáním id předplatného a umístění certifikátu v úložišti **osobních** certifikátů **ServiceManagementService** (znovu nahradit *AzureCertificate* název vašeho certifikátu):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

V předchozím příkladu `sms` je **ServiceManagementService** objekt. Třídy **ServiceManagementService** je třída primární sloužící ke správě Azure služby.

## <a name="ListAvailableLocations"> </a>Jak: seznamu dostupná umístění

Seznam umístění, které jsou k dispozici pro hostitelské služby, můžete **seznamu\_umístění** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Když vytvoříte cloudové služby nebo služba úložiště potřebujete zadání platného umístění. **Seznamu\_umístění** metoda vždy vrátí aktuální seznam aktuálně dostupná umístění. K zápisu, jsou dostupná umístění:

- Západní Evropě
- Severní Evropě
- Jihovýchodní Asie
- Východní Asie
- Centrální USA
- Severní centrální USA
- Jižní centrální USA
- Západ USA
- Východní USA
- Japonsko východ
- Japonsko západní
- Brazílie Jižní
- Austrálie východ
- Austrálie jihovýchodní

## <a name="CreateCloudService"> </a>Postup: vytvoření do cloudové služby

Při vytváření aplikace a spuštění v Azure, kód a konfigurace společně se označují jako Azure [cloudové služby][] (označované jako *hostitelem služby* v dřívějších verzích Azure). **Vytvořit\_hostované\_služby** metoda umožňuje vytvářet novou hostovanou službu zadáním názvu hostovanou službu (které musí být jedinečný v Azure), popisku (automaticky zakódovaný pro ve formátu Base 64), popis a umístění.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Seznam všech hostovanou službu pro vaše předplatné s **seznamu\_hostované\_služby** metodu:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Pokud chcete získat informace o konkrétní hostovanou službu, lze provést předáním názvu hostovanou službu **získat\_hostované\_služby\_vlastnosti** metodu:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Po vytvoření do cloudové služby, nasazením kódu ke službě s **vytvořit\_nasazení** metody.

## <a name="DeleteCloudService"> </a>Jak: odstranění do cloudové služby

Odstraníte do cloudové služby předáním název služby **Odstranit\_hostované\_služby** metodu:

    sms.delete_hosted_service('myhostedservice')

Chcete-li odstranit služby, je třeba odstranit všechna nasazení služby. (Viz [jak: odstranění nasazení](#DeleteDeployment) podrobnosti.)

## <a name="DeleteDeployment"> </a>Postup: nasazení odstranit

Můžete odstranit nasazení **Odstranit\_nasazení** metody. Následující příklad ukazuje, jak odstranit nasazení s názvem `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Postup: vytvoření služba úložiště

[Služba úložiště](../storage/storage-create-storage-account.md) vám umožňuje přístup k [objektů BLOB](../storage/storage-python-how-to-use-blob-storage.md)Azure, [tabulek](../storage/storage-python-how-to-use-table-storage.md)a [dotazů](../storage/storage-python-how-to-use-queue-storage.md). Vytvoření úložiště služby, musíte název službu (mezi 3 a 24 malá písmena a jedinečný v rámci Azure), popis, popisku (až 100 znaků, automaticky zakódovaný ve formátu Base 64) a umístění. Následující příklad ukazuje, jak vytvořit služba úložiště zadáním umístění.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Poznámky v předchozím příkladu, stav **vytvořit\_úložiště\_účtu** operace lze získat předáním vrácený výsledek **vytvořit\_úložiště\_účtu** k **získat\_operace\_stav** metody.  

Můžete vytvořit seznam účtů úložiště a jejich vlastnosti **seznamu\_úložiště\_účty** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Jak: odstranění služba úložiště

Služba úložiště můžete odstranit předáním názvu služba úložiště **Odstranit\_úložiště\_účtu** metody. Odstranění služba úložiště odstraní všech dat uložených ve službě (objektů BLOB, tabulky a fronty).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Jak: seznam dostupných operačních systémů

Seznam operačních systémů, které jsou k dispozici pro hostitelské služby, můžete **seznamu\_provozní\_systémy** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Můžete taky můžete použít **seznamu\_provozní\_systém\_rodiny** metodu, která seskupuje operačních systémech kontrolou rodinné:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Postup: Vytvoření obrázku operační systém

Přidat obrázek operační systém obrázek úložiště, můžete **Přidat\_os\_obrázek** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Operační systém obrázky, které jsou k dispozici, použijte **seznamu\_os\_obrázky** metody. Zahrnuje všechny platformy obrázky a obrázky na uživateli:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Jak: odstranit obrázek operační systém

Můžete odstranit obrázek uživatele **Odstranit\_os\_obrázek** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Postup: vytvoření virtuálního počítače

Pokud chcete vytvořit virtuální počítač, musíte nejdřív vytvořit [cloudové služby](#CreateCloudService).  Vytvořte nasazení virtuálního počítače pomocí **vytvořit\_virtuální\_počítače\_nasazení** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Jak: odstranění virtuálního počítače

Pokud chcete odstranit virtuálního počítače, nejdřív odstraníte nasazení pomocí **Odstranit\_nasazení** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Cloudové služby můžete odstranit pomocí **Odstranit\_hostované\_služby** metodu:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Postup: Vytvoření virtuálního počítače z obrázku zachycený virtuálního počítače

Zachycení obrázku OM, nejdřív voláte **zachytit\_OM\_obrázek** metodu:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Další, abyste měli jistotu, že jste úspěšně zaznamenali obrázku, použít **seznamu\_OM\_obrázky** rozhraní api a ujistěte se, obrázek se zobrazí v seznamu výsledků:

    images = sms.list_vm_images()

Nakonec vytvořit virtuální počítač pomocí zachycený obrázek, můžete **vytvořit\_virtuální\_počítače\_nasazení** metodou podle před, ale tentokrát předat vm_image_name místo

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Další informace o tom, jak zachytit Linux virtuálního počítače najdete v tématu [způsob zachycení virtuálního počítače Linux.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Další informace o tom, jak zachytit virtuálního počítače Windows najdete v tématu [způsob zachycení Windows virtuální počítač.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Další kroky

Teď, když jste se naučili základy Správa služeb, můžete zpřístupnit [odkaz dokončení rozhraní API si přečtěte následující dokumentaci pro Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) a složité úkoly snadno ke správě aplikace python.

Další informace najdete v tématu [Středisko pro vývojáře Python](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[Cloudová služba]:/services/cloud-services/

