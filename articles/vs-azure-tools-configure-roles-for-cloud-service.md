<properties
   pageTitle="Konfigurace role pro službu Azure cloudu pomocí aplikace Visual Studio | Microsoft Azure"
   description="Zjistěte, jak se dají vytvořit a nakonfigurovat rolí služby Azure cloudu pomocí aplikace Visual Studio."
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

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Konfigurace role pro službu Azure cloudu pomocí aplikace Visual Studio

Azure cloudové služby může mít pracovních nebo web role. Pro každou roli musíte definovat, jak nastavit tuto roli a taky nakonfigurovat spuštění určitou roli. Další informace o rolích ve službě cloud services najdete v tématu video [Úvod ke Cloudovým službám Azure](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). Informace o cloudové služby je uložena v následující soubory:

- **ServiceDefinition.csdef**

    Soubor definice služby definuje nastavení runtime pro vaše cloudové služby, včetně rolích jsou potřeba koncové body a velikost virtuálního počítače. Žádná z dat uložených v tomto souboru je možné změnit při spuštění vaše role.

- **ServiceConfiguration.cscfg**

    Konfigurační soubor služby nakonfiguruje kolik instancí role pracují a hodnoty nastavení definované pro roli. Data uložená v tomto souboru je možné změnit je spuštěná vaše role.

Pokud chcete mít možnost uložit různé hodnoty u tohoto nastavení pro spuštění vaše role, může mít více konfigurace služeb. Konfigurace jiné službě můžete použít pro každé prostředí nasazení. Můžete například vytvořit úložiště připojovací řetězec účtu pomocí emulátoru místní Azure úložiště v konfiguraci místního služby a vytvořte další konfiguraci služby používat Azure úložiště v cloudu.

Při vytváření nové Azure Cloudová služba ve Visual Studiu dvě konfigurace služby vzniká ve výchozím nastavení. Tyto konfigurace se přidají do Azure projektu. Konfigurace jsou s názvem:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Konfigurace služby Azure cloudu

Ve Visual Studiu, můžete nakonfigurovat Azure cloudové služby z Průzkumníka řešení, jak je znázorněno na následujícím obrázku.

![Konfigurace služby cloudu](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Konfigurace služby Azure cloudu

1. Konfigurace jednotlivým rolím v Azure projektu z **Průzkumníka řešení**, otevřete v místní nabídce rolí v Azure projektu a klikněte na **Vlastnosti**.

    Zobrazí se stránka s názvem roli v editoru Visual Studio. Na stránce se zobrazí pole na kartě **Konfigurace** .

1. V seznamu **Konfigurace služby** vyberte název konfiguraci služby, kterou chcete upravit.

    Pokud chcete provést změny u všech konfigurace služeb pro tuto roli, můžete **Všechny konfigurace**.

    >[AZURE.IMPORTANT] Pokud se rozhodnete konfigurace konkrétní službu, některé vlastnosti jsou neaktivní, protože lze nastavit jenom pro všechny konfigurace. Úprava těchto vlastností, musí vyberete všechny konfigurace.

    Teď můžete na kartě aktualizovat povolené vlastnosti toto zobrazení.

## <a name="change-the-number-of-role-instances"></a>Změnit počet instancí rolí

Aby se zvýšil výkon cloudové služby, můžete změnit počet role spuštěné instance, na základě počtu uživatelů nebo k načtení očekávání pro konkrétní rolí. Samostatné virtuálního počítače se vytvoří každý výskyt role při spuštění cloudovou službu v Azure. Bude to mít vliv fakturace nasazení cloudové služby. Další informace o fakturaci najdete v tématu [Vysvětlení informací na faktuře pro Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Chcete-li změnit počet instancí pro roli

1. Vyberte kartu **Konfigurace** .

1. V seznamu **Konfigurace služby** zvolte konfiguraci služby, kterou chcete aktualizovat.

    >[AZURE.NOTE] Můžete nastavit počet instancí pro konkrétní službu konfiguraci nebo pro všechny služby konfigurace.

1. Do textového pole **Instance počet** zadejte počet instance, které chcete začít tuto roli.

    >[AZURE.NOTE] Jednotlivé instance se spustí na samostatném virtuálního počítače při publikování cloudové služby Azure.

1. Klikněte na tlačítko **Uložit** na panelu nástrojů na tyto změny uložit do služby konfiguračního souboru.

## <a name="manage-connection-strings-for-storage-accounts"></a>Správa připojovací řetězec pro účty úložiště

Můžete přidat, odebrat nebo změnit připojovací řetězec pro konfiguraci služby. Můžete různých připojovací řetězec pro jiné službě konfigurace. Například, budete chtít místní připojovací řetězec pro konfiguraci místní služba, která obsahuje hodnotu `UseDevelopmentStorage=true`. Můžete taky konfiguraci s cloudové služby, který používá účet úložiště v Azure.

>[AZURE.WARNING] Když zadáte Azure úložiště klíčové informace o účtu připojovací řetězec účtu úložiště, tyto informace uložena v souboru konfigurace služby. Tyto informace nejsou aktuálně uložena jako zašifrované text.

Pomocí jinou hodnotu pro každou službu konfiguraci není nutné použít jiný připojovací řetězec do cloudové služby nebo upravit kód při publikování cloudové služby Azure. Můžete použít stejný název pro připojovací řetězec v kódu a hodnotu budou lišit podle konfigurace služby, kterou jste vybrali při vytváření cloudové služby, nebo při jeho publikování.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Ke správě připojovací řetězec pro účty úložiště

1. Klikněte na kartu **Nastavení** .

1. V seznamu **Konfigurace služby** zvolte konfiguraci služby, kterou chcete aktualizovat.

    >[AZURE.NOTE] Je možné aktualizovat připojovací řetězec pro konkrétní službu konfiguraci, ale v případě potřeby přidat nebo odstranit připojovací řetězec je nutné vybrat všechny konfigurace.

1. Pokud chcete přidat připojovací řetězec, klikněte na tlačítko **Přidat nastavení** . Nová položka se přidá do seznamu.

1. Do textového pole **název** zadejte název, který chcete použít pro připojovací řetězec.

1. V rozevíracím seznamu **Typ** vyberte **Připojovací řetězec**.

1. Pokud chcete změnit hodnotu pro připojovací řetězec, zvolte tlačítko se třemi tečkami (...). Zobrazí se dialogové okno **Vytvořit úložiště připojovací řetězec** .

1. Použití účtu emulátoru místní úložiště, vyberte přepínač **emulátoru úložiště Microsoft Azure** a klikněte na tlačítko **OK** .

1. Postup použití účtu úložiště v Azure, zvolte přepínač **předplatného** a vyberte účet požadované úložiště.

1. Pokud chcete použít vlastní přihlašovací údaje, klikněte na tlačítko Možnosti **ručně zadané přihlašovací údaje** . Zadejte název účtu úložiště a primární nebo druhý klíč. Informace o tom, jak vytvořit účet úložiště a zadejte podrobnosti o účtu úložiště v dialogovém okně **Vytvořit úložiště připojovací řetězec** najdete v článku [Příprava publikovat nebo nasadit Azure aplikace Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Odstranit připojovací řetězec, vyberte připojovací řetězec a pak klikněte na tlačítko **Nastavení odebrat** .

1. Zvolte ikonu **Uložit** na panelu nástrojů tyto změny uložit do služby konfiguračního souboru.

1. Přístup k připojovací řetězec v souboru konfigurace služby, musíte nejprve získat přínosu nastavení konfigurace. Následující kód ukazuje příklad tam, kde je vytvořili úložiště objektů blob a data nahráli pomocí připojovacího řetězce `MyConnectionString` ze služby konfiguračního souboru, když uživatel vybere **Button1** na stránce Default.aspx v roli webové služby Azure cloudu. Přidejte následující pomocí příkazů na Default.aspx.cs:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Otevřete Default.aspx.cs v návrhovém zobrazení a přidání tlačítka z panelu nástrojů. Přidat následující kód, který `Button1_Click` metody. Tento kód používá `GetConfigurationSettingValue` abyste získali hodnotu ze souboru konfigurace služby pro připojovací řetězec. Klepněte objektů blob vytvořené v účtu úložiště, který se odkazuje v připojovacím řetězci `MyConnectionString` a nakonec programu se přidá text objektů blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Přidání vlastního nastavení můžete v Azure cloudové službě

Vlastní nastavení v souboru konfigurace služby umožňuje přidat název a hodnotu řetězce pro konkrétní službu konfiguraci. Je možné toto nastavení slouží k nakonfigurovat funkci do cloudové služby čtení hodnotu nastavení a používání tuto hodnotu k řízení logickou v kódu. Změna tyto hodnoty konfigurace služby aniž by bylo nutné znovu vytvořit balíček služeb nebo pokud je spuštěná cloudové služby. Kód můžete vyhledat oznámení o při změně nastavení. V tématu [RoleEnvironment.Changing události](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Můžete přidat, odebrat nebo změnit vlastní nastavení pro konfiguraci služby. Můžete různé hodnoty pro tyto řetězce pro jiné službě konfigurace.

Pomocí jinou hodnotu pro každou službu konfiguraci není nutné použít různé řetězce v cloudové službě nebo upravit kód při publikování cloudové služby Azure. Můžete použít stejný název pro tento řetězec v kódu a hodnotu budou lišit podle konfigurace služby, kterou jste vybrali při vytváření cloudové služby, nebo při jeho publikování.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Chcete-li přidat vlastní nastavení pro použití v Azure cloudové službě

1. Klikněte na kartu **Nastavení** .

1. V seznamu **Konfigurace služby** zvolte konfiguraci služby, kterou chcete aktualizovat.

    >[AZURE.NOTE] Aktualizovat řetězce pro konkrétní službu konfiguraci, ale pokud potřebujete přidat nebo odstranit řetězce, je nutné vybrat **Všechny konfigurace**.

1. Chcete-li přidat řetězec, klikněte na tlačítko **Přidat nastavení** . Nová položka se přidá do seznamu.

1. Do textového pole **název** zadejte název, který chcete použít pro řetězec.

1. V rozevíracím seznamu **Typ** položku **řetězec**.

1. Chcete-li přidat nebo změnit hodnotu řetězce, do pole **hodnota** zadejte novou hodnotu.

1. Odstranit řetězec, vyberte řetězec a pak klikněte na tlačítko **Nastavení odebrat** .

1. Klikněte na tlačítko **Uložit** na panelu nástrojů na tyto změny uložit do služby konfiguračního souboru.

1. Přístup k řetězec v souboru konfigurace služby, musíte nejprve získat přínosu nastavení konfigurace.

    Potřebujete Ujistěte se, že následující pomocí příkazů už přidají Default.aspx.cs stejně jako v předchozím postupu.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Přidat následující kód, který `Button1_Click` způsob, jak získat přístup k tento řetězec stejným způsobem, že máte přístup k připojovací řetězec. Kód pak můžete provádět několik konkrétní kód založený na hodnotu řetězce nastavení služby konfiguračního souboru, který se používá.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Správa místních úložiště pro každou roli instanci

Můžete přidat místního systému úložiště souborů pro každý výskyt roli. Místní data tady, která nemusí být používána v ostatních rolí mohou být uloženy. Všechna data, která se nemusí uložit do tabulky, objektů blob nebo databáze SQL úložiště mohou být uloženy zde. Můžete třeba použít tento místní úložiště na data v mezipaměti, možná budete muset znovu použít. Tento uložená data nemají přístup k další instance roli. 

Místní uložení nastavení platí pro všechny služby konfigurace. Je možné přidávat jen, odebrání nebo změna místní úložiště pro všechny služby konfigurace.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Ke správě místní úložiště pro každou roli instanci

1. Klikněte na kartu **Místní úložiště** .

1. V seznamu **Konfigurace služby** vyberte **Všechny konfigurace**.

1. Přidání položky místní úložiště, klikněte na tlačítko **Přidat místní úložiště** . Nová položka se přidá do seznamu.

1. Do textového pole **název** zadejte název, který chcete použít pro místní úložiště.

1. Do pole **velikost** zadejte velikost v MB, které potřebujete pro místní úložiště.

1. Po z koše virtuálního počítače pro tuto roli: Odeberte data v místním úložišti, zaškrtněte políčko **Odpadkového čištění na roli** .

1. Úpravy existující položky místní úložiště, vyberte řádek, budete muset aktualizovat. Můžete upravovat pole, podle popisu v předchozích krocích.

1. Odstranění položky místní úložiště, vyberte položku úložiště v seznamu a klikněte na tlačítko **Odebrat místní úložiště** .

1. Tyto změny uložit do služby konfigurační soubory, vyberte ikonu **Uložit** na panelu nástrojů.

1. Přístup k místní úložiště, který jste přidali v souboru konfigurace služby, musíte nejprve získat přínosu nastavení konfigurace místních zdrojů. Následující řádky kódu umožňuje přístup k tuto hodnotu a vytvořte soubor s názvem **MyStorageTest.txt** a napište řádek testovací data do tohoto souboru. Můžete přidat tento kód do `Button_Click` metodu, která jste použili v předchozích kroků:

1. Potřebujete Ujistěte se, že následující pomocí příkazů se přidají do Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Přidat následující kód, který `Button1_Click` metody. Vytvoří soubor v místní úložiště a zapisuje testovací data do tohoto souboru.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Volitelné) Tento soubor, který jste vytvořili při spuštění cloudové služby místně zobrazíte pomocí následujících kroků:

  1. Spusťte roli web a vyberte **Button1** , abyste měli jistotu, že kód uvnitř `Button1_Click` získá s názvem.

  1. V oznamovací oblasti otevřete místní nabídky pro ikonu Azure a zvolte **Zobrazit výpočet emulátoru uživatelského rozhraní**. Zobrazí se dialogové okno **Emulátoru výpočet Azure** .

  1. Vyberte roli, web.

  1. Na řádku nabídek vyberte **Nástroje**, **otevření místním úložišti**. Zobrazí se okno Průzkumníka Windows.

  1. Na řádku nabídek **MyStorageTest.txt** zadejte do textového pole **Hledat** a pak zvolte **Enter** spusťte hledání.

    Soubor se zobrazí v seznamu výsledků hledání.

  1. Pokud chcete zobrazit obsah souboru, otevřete v místní nabídce souboru a zvolte **Otevřít**.

## <a name="collect-cloud-service-diagnostics"></a>Shromáždit diagnostiky služby cloudu

Shromažďovat data o diagnostických nástrojů pro službu Azure cloudu. Tato data se přidá k účtu úložiště. Můžete různých připojovací řetězec pro jiné službě konfigurace. Například, budete chtít účet místní úložiště pro konfiguraci místní služba, která obsahuje hodnotu UseDevelopmentStorage = true. Můžete taky konfiguraci s cloudové služby, který používá účet úložiště v Azure. Další informace o Azure diagnostiky najdete v článku shromáždit protokolování údajů pomocí diagnostických nástrojů Azure.

>[AZURE.NOTE] Konfigurace službu nakonfigurovaný už použít místní zdroje. Pokud používáte konfiguraci služby cloudu publikovat Azure cloudové služby, připojovací řetězec, který zadáte při publikování se používá taky pro diagnostiku připojovací řetězec Pokud jste zadali připojovacího řetězce. Pokud balení služby cloudu pomocí aplikace Visual Studio připojovací řetězec v konfiguraci služby nezmění.

### <a name="to-collect-cloud-service-diagnostics"></a>Získat informace o cloudové služby diagnostiky

1. Vyberte kartu **Konfigurace** .

1. V seznamu **Konfigurace služby** zvolte konfiguraci služby, kterou chcete aktualizovat nebo vyberte **Všechny konfigurace**.

    >[AZURE.NOTE] Aktualizujete účtu úložiště pro konkrétní službu konfiguraci, ale pokud chcete povolit nebo zakázat diagnostiky musíte vybrat všechny konfigurace.

1. Povolit diagnostiky, zaškrtněte políčko **Povolit Diagnostika** .

1. Pokud chcete změnit hodnotu účtu úložiště, zvolte tlačítko se třemi tečkami (...).

    Zobrazí se dialogové okno **Vytvořit úložiště připojovací řetězec** .

1. Použít místní připojovací řetězec, vyberte možnost emulátoru Azure úložiště a klikněte na tlačítko **OK** .

1. Postup použití účtu úložiště přidružený k předplatnému Azure, vyberte možnost **předplatného** .

1. Postup použití účtu úložiště pro místní připojovací řetězec, vyberte možnost **ručně zadané přihlašovací údaje** .

    Další informace o tom, jak vytvořit účet úložiště a zadejte podrobnosti o účtu úložiště v dialogovém okně **Vytvořit úložiště připojovací řetězec** najdete v článku [Příprava publikovat nebo nasadit Azure aplikace Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Zvolte úložiště účet, který chcete použít do pole **název účtu**.

    Pokud jsou pro vás ruční zadávání svoje přihlašovací údaje účtu úložiště zkopírujte nebo zadejte primární klíč **klíč účtu**. Tento klíč můžete zkopírovat z [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885). Zkopírujte tento klíč tohoto návodu v zobrazení **Úložiště účty** [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885):
    
  1. Vyberte účet úložiště, který chcete použít ke cloudové službě.

  1. Klikněte na tlačítko **Spravovat přístupových kláves** nachází v dolní části obrazovky. Zobrazí se dialogové okno **Spravovat přístupové klávesy** .

  1. Pokud chcete zkopírovat přístupové klávesy, klikněte na tlačítko **Kopírovat do schránky** . Tento klíč můžete vložit do pole **klíč účtu** .

1. Použití úložiště účet, který zadáte, jako připojovací řetězec pro diagnostiky (a ukládání do mezipaměti) při publikování cloudové služby Azure, zaškrtněte políčko **aktualizace vývoj úložiště připojovací řetězec pro diagnostiku a ukládání do mezipaměti s Azure úložiště zadání přihlašovacích údajů při publikování na Azure účet** .

1. Klikněte na tlačítko **Uložit** na panelu nástrojů na tyto změny uložit do služby konfiguračního souboru.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Změna velikosti virtuální počítač používá pro každou roli

Můžete nastavit velikost virtuálního počítače pro každou roli. Můžete nastavit jenom velikost pro všechny konfigurace služby. Pokud vyberete zmenšené počítače, pak méně procesoru jádra paměti a místní disk úložiště je přidělené. Přidělené šířky pásma je také menší. Další informace o těchto velikosti a zdroje přidělené najdete v článku [velikosti Cloudovým službám](cloud-services/cloud-services-sizes-specs.md).

Materiály potřebné pro každý počítač virtuální v Azure ovlivňuje náklady aplikaci cloudové služby Azure. Další informace o fakturaci Azure najdete v článku [Vysvětlení informací na faktuře pro Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Pokud chcete změnit velikost virtuálního počítače

1. Vyberte kartu **Konfigurace** .

1. V seznamu **Konfigurace služby** vyberte **Všechny konfigurace**.

1. Vyberte požadovanou velikost virtuálního počítače pro tuto roli, zvolte ze seznamu **velikost OM** správné velikosti.

1. Klikněte na tlačítko **Uložit** na panelu nástrojů na tyto změny uložit do služby konfiguračního souboru.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Správa koncové body a certifikáty pro vaše role

Konfigurace sítě koncové body zadáním protokol číslo portu a HTTPS, informace o certifikátu SSL. Uvolnění před června 2012 podporu protokolu HTTP, HTTPS a TCP. Uvolnění. června 2012 podporuje tyto protokoly a UDP. UDP nemůžete používat pro zadávání koncové body ve výpočetním emulátoru. Protokolu můžete použít jenom pro interní koncové body.

Abychom vylepšili zabezpečení Azure cloudové služby, můžete vytvořit koncové body, které používají protokol HTTPS. Například pokud jste ještě do cloudové služby, používaný zákazníků na nákupní objednávky, chcete mít jistotu, že informace o jejich zabezpečené pomocí SSL.

Můžete taky přidat koncové body použitelná interně nebo externě. Externí koncové body se označují jako vstupní koncové body. Zadávání koncový bod umožňuje jiné přístup, přejděte na uživatelé ke cloudové službě. Pokud máte službě WCF, můžete zobrazit vnitřní koncový bod pro roli web používat pro přístup ke tuto službu.

>[AZURE.IMPORTANT] Aktualizovat lze pouze koncové body pro všechny konfigurace služby.

Pokud chcete přidat koncové body HTTPS, musíte použít certifikát SSL. K tomuto můžete přidružit certifikáty vaše role pro všechny konfigurace služeb a použít pro svůj koncové body.

>[AZURE.IMPORTANT] Tato poukázky nejsou balení s vašimi službami. Certifikáty musí samostatně nahrávat do Azure přes [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

Všech Správa certifikátů, které přidružit konfigurace služeb použít pouze v případě, že vaše cloudové služby běží v Azure. Při spuštění Cloudová služba ve vývojovém místního prostředí se používá standardní certifikát, který se spravuje emulátorem Azure výpočetním.

### <a name="to-add-a-certificate-to-a-role"></a>Chcete-li přidat k roli certifikát

1. Zvolte kartu **certifikáty** .

1. V seznamu **Konfigurace služby** vyberte **Všechny konfigurace**.

    >[AZURE.NOTE] Přidání nebo odebrání certifikáty, je nutné vybrat všechny konfigurace. V případě potřeby můžete aktualizovat název a miniatura pro konkrétní službu konfiguraci.

1. Pokud chcete přidat certifikát pro tuto roli, klikněte na tlačítko **Přidat certifikát** . Nová položka se přidá do seznamu.

1. Do pole **název** zadejte název pro certifikát.

1. V seznamu **Úložiště umístění** vyberte umístění pro certifikát, který chcete přidat.

1. V seznamu **Ukládat název** zvolte úložiště, který chcete použít na vyberte certifikát.

1. Pokud chcete přidat certifikát, zvolte tlačítko se třemi tečkami (...). Zobrazí se dialogové okno **Zabezpečení systému Windows** .

1. Vyberte certifikát, který chcete použít v seznamu a potom klikněte na tlačítko **OK** .

    >[AZURE.NOTE] Po přidání certifikát z úložiště certifikátů intermediate certifikáty se automaticky přidají nastavení konfigurace za vás. Přechodné certifikáty musí taky ho nahrát Azure k správně nakonfigurovat službu pro SSL.

1. Odstranění certifikátu, vyberte certifikát a klikněte na tlačítko **Odebrat certifikát** .

1. Vyberte ikonu **Uložit** na panelu nástrojů uložte tyto změny ke svým souborům konfigurace služby.

### <a name="to-manage-endpoints-for-a-role"></a>Ke správě koncové body pro roli

1. Zvolte kartu **koncové body** .

1. V seznamu **Konfigurace služby** vyberte **Všechny konfigurace**.

1. Přidání koncový bod, klikněte na tlačítko **Přidat koncový bod** . Nová položka se přidá do seznamu.

1. Do textového pole **název** zadejte název, který chcete použít pro tento koncový bod.

1. V seznamu **Typ** vyberte typ koncový bod, který budete potřebovat.

1. Ze seznamu **protokol** zvolte protokol koncového bodu, které potřebujete.

1. Při zadávání koncový bod, zadejte do textového pole **Port veřejné** veřejné portu, který chcete použít.

1. Do textového pole **Port soukromé** zadejte soukromé portu, který chcete použít.

1. Pokud koncový bod vyžaduje protokol https, vyberte v seznamu **Název certifikátu SSL** certifikát používat.

    >[AZURE.NOTE] Tento seznam uvádí certifikáty, které jste přidali pro tuto roli na kartě **certifikáty** .

1. Klikněte na tlačítko **Uložit** na panelu nástrojů uložte tyto změny ke svým souborům konfigurace služby.

## <a name="next-steps"></a>Další kroky
Další informace o Azure projekty ve Visual Studiu [Konfigurace Azure projektu](vs-azure-tools-configuring-an-azure-project.md). Další informace o schématu služby cloudu [Přehled schématu](https://msdn.microsoft.com/library/azure/dd179398).
