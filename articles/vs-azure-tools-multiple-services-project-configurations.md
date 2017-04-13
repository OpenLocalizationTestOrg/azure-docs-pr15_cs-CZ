<properties
   pageTitle="Konfigurace Azure projektu přes více konfigurace služeb | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat projekt služby Azure cloudu změnou ServiceDefinition.csdef a ServiceConfiguration.cscfg soubory."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurace Azure projektu přes více konfigurace služeb

Projekt služby Azure cloudu obsahuje dva konfigurační soubory: ServiceDefinition.csdef a ServiceConfiguration.cscfg. Tyto soubory jsou součástí aplikace služby Azure cloudu a používaný Azure.

- Soubor **ServiceDefinition.csdef** obsahuje metadata, která je potřebná Azure prostředím požadavky aplikace vaše cloudové služby, včetně rolích obsahuje. Tento soubor obsahuje také nastavení, která platí pro všechny instance. Nastavení konfigurace může číst za běhu s použitím API pro Runtime hostitelské služby Azure. Tento soubor nelze aktualizovat, když běží služba Azure.

- Soubor **ServiceConfiguration.cscfg** nastaví hodnoty pro nastavení definovaný v souboru definice služby a určuje, kolik instancí spustit pro každou roli. Tento soubor je možné aktualizovat v Azure je spuštěná cloudové služby.

Azure nástroje pro Microsoft Visual Studio nabízejí stránky vlastností, které můžete použít k nastavení konfigurace uložené v těchto souborech. Přístup ke stránkám vlastnost, poklikejte na odkaz na roli pod projekt služby Azure cloudu v Průzkumníku nebo klikněte pravým tlačítkem na odkaz na roli a zvolte **Vlastnosti**, jak je znázorněno na následujícím obrázku.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informace o základní schémata definice služby a služby konfigurace souborů najdete v tématu [Přehled schématu](https://msdn.microsoft.com/library/azure/dd179398.aspx). Další informace o konfiguraci služby najdete v článku [jak konfigurovat Cloudovým službám](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurace vlastností rolí

Vlastnost stránky pro web roli a roli pracovní podobají, sice několik rozdílů, zdůraznit v následujících částech.

Na stránce **mezipaměť** můžete nakonfigurovat ukládání do mezipaměti služby Azure.

### <a name="configuration-page"></a>Konfigurace stránky

Na stránce **Konfigurace** můžete nastavit následující vlastnosti:

**Případy**

Nastavte vlastnost počet **Instance** počty instance, které služba by měla běžet pro tuto roli.

Nastavte vlastnost **velikost OM** **Navíc malá**, **malá**, **Střední**, **velká**nebo **Velký**.  Další informace najdete v tématu [velikosti Cloudovým službám](./cloud-services/cloud-services-sizes-specs.md).

**Spuštění akce** (Pouze web Role)

Vlastnost můžete určit, že by měl Visual Studio při spuštění ladění spuštění webového prohlížeče pro koncové body HTTP nebo koncové body HTTPS nebo obojí.

Protokol HTTPS v koncový bod možnost je dostupná jenom v případě, že už jste definovali HTTPS koncový bod pro vaše role. Na stránce **koncové body** vlastnost můžete definovat koncový bod HTTPS.

Pokud jste už přidali koncového HTTPS, možnost koncový bod HTTPS aktivované ve výchozím nastavení a Visual Studio spustí prohlížeče pro tento koncový bod se při spuštění ladění kromě prohlížeči koncového bodu HTTP. To se předpokládá, že jsou povoleny obě možnosti při spuštění.

**Diagnostika**

Ve výchozím nastavení diagnostických nástrojů aktivované řešení roli Web. Účet aplikace project a úložiště služby Azure cloudu se nastavit, aby používal emulátoru místní úložiště. Až budete chtít nasazení Azure, můžete vybrat tlačítko nástroje pro sestavení (****...) aktualizoval úložiště používat Azure úložiště v cloudu. Na vyžádání nebo automaticky naplánované intervalech, můžete přenášet data diagnostiky k tomuto účtu úložiště. Další informace o Azure diagnostiky najdete v článku [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Nastavení stránky

Na stránce **Nastavení** můžete přidat nastavení konfigurace službě. Konfigurace nastavení jsou dvojice název hodnota. Kód spuštěný v roli může číst hodnoty nastavení konfigurace za běhu pomocí poskytovanou [Azure Řízená knihovna](http://go.microsoft.com/fwlink?LinkID=171026)tříd. Konkrétně metodu [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) vrátí hodnotu pojmenované nastavení za běhu.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfigurace připojovací řetězec k účtu úložiště

Připojovací řetězec je nastavení, které obsahuje informace o připojení a ověřování pro emulátoru úložiště nebo účet Azure úložiště. Pokaždé, když kódu musí přístup k datům služby Azure úložiště – tedy objektů blob fronty nebo tabulková data – z kód spuštěný v roli, budete muset definovat připojovací řetězec pro jeho účet úložiště.

Připojovací řetězec, který odkazuje na účet Azure úložiště musíte použít definovaný formát. Další informace o tom, jak vytvořit připojení řetězců najdete v článku [Konfigurace Azure úložiště připojovací řetězec](./storage/storage-configure-connection-string.md).

Jakmile budete připraveni službu proti služby Azure úložiště testovat nebo když budete chtít nasazení cloudové služby Azure, můžete změnit hodnotu jakékoli řetězce připojení tak, aby ukazovaly na váš účet Azure úložiště. Vyberte (****...), vyberte **přihlašovací údaje účtu úložiště Enter**. Zadání informací o účtu, který obsahuje název účtu a klíč účtu. V dialogu **Úložiště účtu připojovací řetězec** můžete určit, zda chcete použít výchozí HTTPS koncové body (výchozí možnost), koncové body HTTP výchozí nebo vlastní koncové body. Může se rozhodnete používat vlastní koncové body, pokud jste si zaregistrovali vlastního názvu domény pro službu, jak je uvedeno v [článku Konfigurace vlastního názvu domény pro data objektů blob Azure úložiště účtu](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Je třeba změnit připojení řetězců tak, aby ukazovaly na účet Azure úložiště před nasazením služby. V opačném případě to může způsobit, že vaše role spustit nebo pokud chcete cyklicky procházet stavy inicializace, zaneprázdněn a ukončení.

## <a name="endpoints-page"></a>Stránka Koncové body

Pracovní role může mít libovolný počet HTTP, HTTPS nebo TCP koncové body. Koncové body může být zadávání koncové body, které jsou k dispozici pro externí klienti, nebo interní koncové body, které jsou k dispozici ostatních rolí, ve kterých se nepoužívá v rámci služby.

- Koncový bod HTTP zpřístupnit externí klientům a webových prohlížečích, změňte typ koncového bodu zadat a zadejte název a číslo portu veřejné.

- Koncový bod HTTPS zpřístupnit externí klientům a webových prohlížečích, změňte typ koncového bodu na **vstupní**a zadejte název, čísla veřejné port a název certifikátu správy.

    Všimněte si, že před určením správy certifikát musí definovat certifikát na stránce vlastností **certifikáty** .

- Koncový bod zpřístupnit pro interní přístup tak, že ostatní role ve službě cloud, změňte typ koncového bodu k interním a zadejte název a možné soukromé porty pro tento koncový bod.

## <a name="local-storage-page"></a>Místní uložení stránky

Na stránce vlastností **Místní úložiště** umožňuje rezervovat jeden nebo více zdrojů místní úložiště pro roli. Místní uložení zdroje je rezervovaná adresář v systému souborů Azure virtuálního počítače, ve kterém je spuštěný instanci roli.

## <a name="certificates-page"></a>Certifikáty stránky

Na stránce **certifikáty** můžete certifikáty přidružit vaše role. Certifikáty, které přidáte mohou sloužit k konfigurace koncové body HTTPS na stránce vlastností **koncové body** .

Na stránce vlastností **certifikáty** přidá informace o vlastních certifikátů konfigurace služby. Všimněte si, že nejsou s vašimi službami; balení vlastních certifikátů certifikáty musí samostatně nahrávat do Azure přes [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

Pokud chcete přidružit certifikát vaše role, zadejte název pro certifikát. Použijte tento název při konfiguraci koncového HTTPS na stránce **koncové body** vlastnost odkázat na certifikát. Potom určete, jestli je úložiště certifikátů **Místního počítače** nebo **Aktuálního uživatele** a název úložiště. Nakonec zadejte Miniatura certifikátu. Pokud certifikát v aktuálním User\Personal (My) úložiště, můžete zadat certifikátu Miniatura tak, že vyberete certifikát vyplněné seznamu. Pokud se nachází v jiném umístění, zadejte hodnotu Miniatura ručně.

Po přidání certifikát z úložiště certifikátů intermediate certifikáty se automaticky přidají do nastavení konfigurace za vás. Přechodné certifikáty musí taky ho nahrát Azure k správně nakonfigurovat službu pro SSL.

Všech Správa certifikátů, které se spojit s vašimi službami použít ke službě pouze v případě, že je spuštěná v cloudu. Když místní vývojové prostředí běží služba, používá standardní certifikát, který se spravuje emulátorem výpočetním.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfigurace projekt služby Azure mraků

Pokud chcete konfigurovat nastavení, které se vztahují na projekt celý Azure cloudové služby, musíte nejdřív otevřít místní nabídky pro uzel projektu a vyberte vlastnosti zobrazíte její vlastnosti stránky. Následující tabulka zobrazuje tyto vlastnosti stránky.

|Na stránce vlastností|Popis|
|---|---|
|Aplikace|Na této stránce můžete zobrazit informace o verzi Azure nástrojů, které používá cloudové služby projektu a na aktuální verzi nástroje můžete upgradovat.|
|Vytvoření události|Na této stránce můžete nastavit před a po sestavení události.|
|Vývojové|Na této stránce můžete určit pokyny ke konfiguraci Tvůrce dotazů a podmínky uvedené v části, které se mají spustit po sestavení události.|
|Web|Na této stránce můžete konfigurovat nastavení, které se vztahují k webovému serveru.|
