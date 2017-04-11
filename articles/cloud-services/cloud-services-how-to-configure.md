<properties 
    pageTitle="Postup při konfiguraci do cloudové služby (klasické portál) | Microsoft Azure" 
    description="Zjistěte, jak nakonfigurovat cloudovým službám v Azure. Zjistěte, jak aktualizovat konfiguraci služby cloudu a konfigurace vzdálený přístup k roli instance." 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-configure-cloud-services"></a>Postup při konfiguraci Cloudovým službám

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-configure-portal.md)
- [Azure klasické portálu](cloud-services-how-to-configure.md)

Konfigurace nejčastěji používaná nastavení do cloudové služby na portálu Azure klasické. Nebo pokud se vám aktualizace souborů konfigurace přímo, stáhněte si soubor konfigurace služby aktualizovat a pak uložit ten doplněný soubor a aktualizovat nejnovějšími změnami konfigurace cloudové služby. V obou případech konfigurace aktualizace jsou použiti na všechny instance role.

Portál Azure klasické také umožňuje [Povolit připojení ke vzdálené ploše pro roli v Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)

Azure pouze zajistit dostupnost služby 99.95 procent během aktualizací konfigurace Pokud máte alespoň dvě instance role pro každý roli. V jednom počítači virtuální zpracovat žádosti klienta o době, kdy je aktualizován druhý, který umožňuje. Další informace najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Změna do cloudové služby

1. [Azure klasické portál](http://manage.windowsazure.com/)klikněte na **Cloudové služby**, klikněte na název cloudovou službu a potom klikněte na **Konfigurovat**.

    ![Konfigurace stránky](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Na stránce **Konfigurovat** můžete konfigurovat sledování, aktualizovat nastavení role a zvolte Host operačního systému a rodina instancí role. 

2. **Sledování**nastavte úroveň sledování podrobné nebo minimálními a nakonfigurujte diagnostických nástrojů, které jsou zapotřebí pro podrobný sledování.

3. Pro službu role (seskupené podle rolí) můžete aktualizovat následující nastavení:
    
    >**Nastavení**  
    >Změňte hodnoty různé nastavení, která byla vyplněná prvků *ConfigurationSettings* souboru konfigurace (.cscfg) služby.
    >
    >**Certifikáty**  
    >Změna miniatury certifikát používaný v šifrování SSL pro roli. Pokud chcete změnit certifikát, musíte nejdřív nahrajete nový certifikát (na stránce **certifikáty** ). Aktualizujte Miniatura v řetězci certifikát zobrazit v nastavení role.

4. V **operačním systému**můžete změnit operační systém řady nebo verze pro instance role nebo zvolte **Automatické** povolit automatické aktualizace verze operačního systému. Operační systém nastavení pro web rolí a pracovní vhodná, ale nemají vliv na virtuálních počítačích.

    Při nasazení je na všechny instance role nainstalovali nejnovější verze operačního systému a operačních systémech se automaticky aktualizují ve výchozím nastavení. 
    
    Pokud potřebujete cloudové službě dělat různé operační systém verze z důvodu požadavky na kompatibilitu v kódu, můžete řady operačního systému a verze. Když zvolíte konkrétní operační systém verze, se pozastaví automatické operační systém zavedli cloudovou službu. Bude nutné ověřit, že v operačních systémech přijímat aktualizace.
    
    Není-li odstranit všechny problémy s kompatibilitou s aplikací s nejnovější verzí operačního systému umožňuje nastavením verze operačního systému na **Automatické**aktualizace automatické operační systém. 
    
    ![Nastavení OS](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Uložení nastavení konfigurace a nabízená na instance roli, klikněte na **Uložit**. (Klikněte **Zahodit** ji zrušíte změny). **Uložit** a **Zrušit** se přidají do panelu s příkazy po změně nastavení.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualizace souboru konfigurace služby cloudu

1. Stáhněte si cloudové služby konfiguračního souboru (.cscfg) s aktuální konfigurace. Na stránce **Konfigurovat** cloudové služby klikněte na **Stáhnout**. Klikněte na tlačítko **Uložit**nebo klikněte na **Uložit jako** a soubor uložte.

2. Po aktualizaci service konfigurační soubor nahrát a nainstalovat aktualizace konfigurace:

    1. Na stránce **Konfigurovat** klikněte na **Odeslat**.
    
        ![Nahrání konfigurace](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. **Konfigurační soubor**použijte **Procházet** a vyberte soubor aktualizované .cscfg.
    
    3. Obsahuje-li cloudové služby role, které mají jenom jedna instance, **konfiguraci použít i v případě jedné nebo více rolí obsahují jedna instance** zaškrtněte políčko Povolit aktualizace konfigurace z role pokračovat.
    
        Pokud definovat nejméně dvě instance jednotlivých rolích Azure nezaručuje alespoň 99.95 procent dostupnost cloudové služby během aktualizací konfigurace služeb. Další informace najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
    
    4. Klikněte na **OK** (zaškrtnuté). 


## <a name="next-steps"></a>Další kroky

* Zjistěte, jak [nasadit do cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Spravovat Cloudová služba](cloud-services-how-to-manage.md).
* [Povolit připojení ke vzdálené ploše pro roli v Azure Cloudovým službám](cloud-services-role-enable-remote-desktop.md)
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).
