<properties 
pageTitle="Povolit připojení ke vzdálené ploše pro roli v Azure Cloudovým službám" 
description="Jak nakonfigurovat aplikaci služby azure cloudu povolit připojení ke vzdálené ploše" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Povolit připojení ke vzdálené ploše pro roli v Azure Cloudovým službám

>[AZURE.SELECTOR]
- [Azure klasický portálu](cloud-services-role-enable-remote-desktop.md)
- [Prostředí PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Vzdálené plochy umožňuje přístup k plochy spuštěné v Azure role. Připojení ke vzdálené ploše slouží k řešení potíží s a Diagnostika problémů s aplikací je spuštěná. 

Můžete povolit připojení ke vzdálené ploše vaše role průběhu vývoje zahrnutím moduly Vzdálená plocha definice služby nebo můžete zvolit povolit vzdálená plocha prostřednictvím vzdálené plochy rozšíření. Preferovaný přístup je používat rozšíření Vzdálená plocha, protože i po nasazení aplikace aniž byste museli přeinstalujte aplikaci můžete povolit vzdálená plocha. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurace připojení ke vzdálené ploše z portálu Microsoft Azure klasické
Azure klasické portálu používá vzdálené plochy rozšíření přístup, můžete povolit vzdálené plochy i po nasazení aplikace. Stránka **Konfigurovat** cloudové službě umožňuje povolit vzdálená plocha, změňte místního účtu správce připojuje k virtuálních počítačích, certifikát použitá při ověřování a nastavte datum vypršení platnosti. 


1. Klikněte na **Cloudové služby**, klikněte na název cloudovou službu a potom klikněte na **Konfigurovat**.

2. Klepněte na **Remote**.
    
    ![Cloudovým službám remote](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Všechny instance role bude nutné restartovat při prvním povolit vzdálená plocha a klikněte na tlačítko OK (zaškrtnuté). Nechcete, aby restartování počítače, musí být certifikát používaný k šifrování heslo na roli nainstalovaný. Chcete-li zabránit restartování počítače, [nahrajte certifikát cloudovou službu](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) a vraťte se do tohoto dialogového okna.
    

3. V části **role**vyberte roli, kterou chcete aktualizovat nebo vyberte **všechny** pro všechny role.

4. Proveďte některou z těchto změn:
    
    - Povolit vzdálená plocha, zaškrtněte políčko **Povolit vzdálená plocha** . Pokud chcete zakázat vzdálená plocha, zrušte zaškrtnutí políčka.
    
    - Vytvořte účet, který chcete použít v připojení ke vzdálené ploše na instance role.
    
    - Aktualizujte heslo stávajícímu účtu.
    
    - Vyberte certifikát nahraný použít pro ověřování (nahrát certifikátu pomocí **Nahrát** na stránce **certifikáty** ) nebo vytvořte nový certifikát. 
    
    - Změňte datum vypršení platnosti pro konfiguraci Vzdálená plocha.

5. Po dokončení konfigurace aktualizace, klikněte na **OK** (zaškrtnuté).


## <a name="remote-into-role-instances"></a>Vzdálené do instance rolí
Po připojení ke vzdálené ploše je zapnuta role můžete vzdáleného do instanci rolí pomocí různých nástrojů.

Připojení k instanci rolí z portálu Microsoft Azure klasické:
    
  1.   Klikněte na **případy** a otevře se stránka **instance** .
  2.   Vyberte roli instanci obsahující Vzdálená plocha nakonfigurované.
  3.   Klikněte na **Připojit**a postupujte podle pokynů otevřete na plochu. 
  4.   Klikněte na **Otevřít** a potom **Připojit** ke spuštění připojení ke vzdálené ploše. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Pomocí aplikace Visual Studio vzdálené do instanci rolí

Ve Visual Studiu Průzkumník serveru:

1. Rozbalení **Azure\\Cloudovým službám\\[název služby cloudu]** uzlů.
2. Rozbalte položku **pracovní** nebo **výroby**.
3. Rozbalte jednotlivé role.
4. Klikněte pravým tlačítkem na jednu z instancí roli, klikněte na **Připojit pomocí vzdálené plochy …**a zadejte uživatelské jméno a heslo. 

![Server explorer Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Použití Powershellu ke stažení souboru RDP
Můžete použít rutinu [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) k načtení souboru RDP. Pak můžete soubor RDP připojení ke vzdálené ploše pro přístup ke cloudové službě.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programově RDP budou moct soubor stáhnete prostřednictvím služby správy REST API
Můžete [Stáhnout RDP](https://msdn.microsoft.com/library/jj157183.aspx) ZBÝVAJÍCÍ operace ke stažení souboru RDP. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Konfigurace vzdálené plochy v souboru definice služby

Tento způsob umožňuje povolit připojení ke vzdálené ploše pro aplikaci průběhu vývoje. Tento postup vyžaduje šifrované hesla být uložena v konfiguraci služby souborů a všechny aktualizace vzdálené plochy konfigurace vyžadují nového nasazení aplikace. Pokud chcete blokovat tyto downsides postup vzdálené plochy rozšíření na základě jsme je popsali výše byste měli použít.  

Visual Studio můžete použít k [Povolení připojení ke vzdálené ploše](../vs-azure-tools-remote-desktop-roles.md) pomocí přístup soubor definice služby.  
Následující kroky popisují změny umožňující ke svým souborům modelu služby připojení ke vzdálené ploše. Visual Studio se tyto změny automaticky provede při publikování.

### <a name="set-up-the-connection-in-the-service-model"></a>Nastavení připojení v modelu služby 
Umožňuje importovat modulu **RemoteForwarder** a modul **vzdálený přístup** k souboru [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) element **importy** .

Soubor definice služby by měl být stejný jako v následujícím příkladu s `<Imports>` prvek přidaný.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Soubor [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) by měl být podobně jako v následujícím příkladu, nezapomeňte `<ConfigurationSettings>` a `<Certificates>` prvky. Certifikát určený musí být [odeslány do cloudové služby](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Další zdroje informací

[Postup při konfiguraci Cloudovým službám](cloud-services-how-to-configure.md)