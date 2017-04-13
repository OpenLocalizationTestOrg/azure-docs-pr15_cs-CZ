<properties
   pageTitle="Jak migrovat a publikovat webové aplikace služby Azure cloudu z aplikace Visual Studio | Microsoft Azure"
   description="Naučte se migrovat a publikovat webové aplikace do služby Azure cloudu pomocí aplikace Visual Studio."
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

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Postup: migrace a publikovat webové aplikace služby Azure cloudu z aplikace Visual Studio

Umožní využít výhod hostitelské služby a škálovatelnost Azure můžete migrovat a publikovat webové aplikace Azure cloudové služby. Můžete použít webovou aplikaci v Azure s minimálními změny existující aplikaci.

>[AZURE.NOTE] Toto téma je o nasazení ke cloudovým službám, nikoli na weby. Informace o instalaci na weby najdete v tématu [nasadit do webových aplikací v aplikaci služby Azure](./app-service-web/web-sites-deploy.md).

Seznam specifické šablony, které jsou podporovány pro Visual Basic i Visual Basic najdete v části **Podporované šablony projektů** dál v tomto tématu.

Je třeba nejprve povolit webové aplikace pro Azure z aplikace Visual Studio. Následující obrázek znázorňuje základní kroky pro publikování stávající webové aplikace přidáním Azure projektu pro účely nasazení. Tento proces přidá Azure projekt s rolí požadovaný web na řešení. Na základě typu webu projektu, který máte, vlastnosti projektu pro sestavení také aktualizují Pokud balíčku služby vyžaduje další sestavení nasazení.

![Publikování webové aplikace Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] **Převést**, příkaz **převést na projekt cloudové služby Azure** se zobrazí pouze pro web projektu v řešení. Příkaz například není k dispozici pro projekt Silverlight ve vašem řešení.
Při vytvoření balíčku služby nebo publikovat aplikaci Azure, může dojít k upozornění nebo chyby. Tyto upozornění a chyby vám můžou pomoct řešení problémů s před nasazením Azure. Například nebudete dostávat upozornění na chybějící sestavení. Další informace o tom, jak zacházet s všechna upozornění jako chybné, přečtěte si téma [Konfigurace projekt Azure cloudové služby aplikace Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Pokud vytvářet aplikace, spustit místně pomocí emulátoru výpočetním nebo publikujte projekt do Azure, může se zobrazit chybová zpráva v okně **Seznam chyb** : **je příliš dlouhá cesta, název souboru nebo obě**. K této chybě dochází, protože je příliš dlouhá délka názvu plně kvalifikovaný Azure projektu. Délka názvu projektu, včetně úplnou cestu nemůže být více než 146 znaky. Například to je název úplné projektu včetně cesta k souboru pro Azure projekt, který se vytvoří aplikace Silverlight: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Bude pravděpodobně nutné přesunout řešení různých adresář, který má kratší cestu k Zkraťte název plně kvalifikovaný projektu.

Migrace a publikovat webové aplikace Azure z aplikace Visual Studio, postupujte takto.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Povolit webových aplikací pro nasazení Azure

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Chcete-li povolit webovou aplikaci pro nasazení Azure

1. Povolit webové aplikace pro nasazení Azure, otevřete místní nabídky pro web projektu v řešení a zvolte Přidat projekt nasazení Azure.

    Provést následující akce:

    - Projekt aplikace Azure nazývá `<name of the web project>.Azure` je přidán do řešení pro aplikaci.

    - Web role pro web project se přidá do tohoto Azure projektu.

    - Vlastnost **Místní kopie** je nastavena na hodnotu true pro sestavení, které jsou potřeba pro MVC 2, MVC 3 MVC 4 a Silverlight podnikových aplikací. Přidá tyto sestavení balíčku služby, který se používá pro nasazení.

  >[AZURE.IMPORTANT] Pokud máte jiné sestavení nebo soubory, které jsou potřeba pro tuto webovou aplikaci, je nutné ručně nastavit vlastnosti pro tyto soubory. Informace o tom, jak nastavit tyto vlastnosti naleznete v části **Zahrnout soubory do balíčku služby** dál v tomto článku.

  >[AZURE.NOTE] Pokud web role pro konkrétní web projektu již existuje v Azure projektu v řešení **Převést**, **převést na projekt cloudové služby Azure** nezobrazuje v místní nabídce pro tento web projektu.

  Pokud máte více projektů web ve webové aplikaci a chcete vytvořit web role pro každý web projekt, musíte udělat kroky tohoto postupu pro každý web projektu. Tím vytvoříte samostatný Azure projektů pro každou roli web. Každý web projektu můžete publikovat samostatně. Můžete taky můžete ručně přidat další roli web k existujícímu Azure projektu ve webové aplikaci. K tomuto účelu otevřete místní nabídky pro složku **role** v Azure projektu, zvolte **Přidat**a pak na **Web projektu Role v řešení**, zvolte projektu přidat roli web a potom klikněte na tlačítko **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Použití databáze Azure SQL aplikace

Pokud máte připojovací řetězec pro webovou aplikaci používající databáze SQL serveru, který je na místě, musíte změnit tento připojovací řetězec používat instanci databáze SQL Azure, která hostuje místo.

>[AZURE.IMPORTANT] Vaše předplatné musí umožňují používat databázi SQL. Pokud přejdete z [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)předplatného, můžete určit, na jaké služby obsahuje vaše předplatné. Následující pokyny se vztahují k vydané [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885). Pokud používáte [Azure portál](http://portal.microsoft.com), přejděte k dalšímu postupu.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Pro účely instance SQL databáze v roli web připojovacího řetězce

1. K vytvoření instance databáze SQL [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885), postupujte podle kroků v následujícím článku: [Vytvoření databázi serveru SQL](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Když nastavíte pravidla brány firewall pro instanci aplikace databáze SQL, musí zaškrtněte políčko **Povolit jiných Azure služeb pro přístup k tomuto serveru** .

1. Pokud chcete vytvořit instance databáze SQL pro účely připojovací řetězec, postupujte podle pokynů v následující části v následujícím článku: [vytvoření databáze SQL](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Zkopírovat připojovací řetězec ADO.NET pro účely připojovací řetězec, proveďte následující kroky [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Zvolte tlačítko **databáze** a poté rozbalte uzel pro předplatné, které jste použili pro vytvoření instance SQL databáze.

  1. Zobrazit dostupné instance databáze SQL, vyberte uzel **SQL databáze** .

  1. Chcete-li zobrazit vlastnosti pro databázi, vyberte databázi. Zobrazení **vlastností** se zobrazí.

      >[AZURE.NOTE] Pokud zobrazení **vlastností** nezobrazí, je potřeba otevřít pomocí oddělovač.

  1. Zobrazení řetězců připojení, zvolte tlačítko se třemi tečkami (...) vedle tlačítka zobrazit.

    Zobrazí se dialogové okno **Připojovací řetězec** .

  1. Zkopírovat připojovací řetězec ADO.NET, zvýrazněte vybraný text a zvolte klávesy Ctrl + C.

  1. Zavřete dialogové okno, klikněte na tlačítko **Zavřít** .

1. Pokud chcete nahradit připojovací řetězec do nastavení(Web.config)) používat tuto instanci databáze SQL, otevřete nastavení(Web.config)), zvýraznění existující připojovací řetězec a pak zvolte klávesy Ctrl + V. ADO.NET připojovací řetězec pro instanci systému SQL databáze nahradí existující připojovací řetězec.

1. Je třeba přidat také parametr `MultipleActiveResultSets=True` připojovací řetězec. Připojovací řetězec měli v tomto formátu:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Volitelné) Alternativní metody po změnu připojovací řetězec přímo v souboru web.config je přidání oddílu do jedné ze souborů transformace web.config, v závislosti na konfiguraci sestavení, můžete použít k vytvoření balíčku služby. Otevřete soubor Web.Debug.Config nebo soubor Web. Přidejte následující část do tohoto souboru:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Uložte soubor, který jste upravili a znovu publikujte aplikaci.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Použití instance databáze SQL pomocí portálu Azure klasické

1. [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)vyberte uzel SQL databáze.

  - Pokud se zobrazí instanci systému SQL databáze, kterou chcete použít, vyberte a otevřete ho.

  - Pokud jste nevytvořili všechny instance, vyberte příslušný odkaz a pak vytvořte instance.

1. Když otevřete nebo vytvořte instance databázového, zvolte odkaz **Připojovací řetězec** .

1. V dolní části stránky zvolte odkaz Konfigurovat nastavení brány firewall a nechejte výchozí hodnoty nebo konfigurace hodnoty, které potřebujete.

1. Zkopírujte připojovací řetězec ADO.NET, vložte do souboru Web.Configuration přes staré připojovací řetězec pro místní databázi a přidejte `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Publikování webové aplikace Azure

### <a name="to-publish-a-web-application-to-azure"></a>Chcete-li publikovat webové aplikace Azure

1. Testování aplikace v místním vývojové prostředí pomocí Azure výpočet emulátoru otevření místní nabídky pro Azure project web rolí a zvolte **nastavit jako projekt při spuštění**. Klikněte na **ladění**, **Spustit ladění** (klávesnice: **F5**).

    Otevře dialogové okno **spustit prostředí ladění Azure** a spuštění aplikace v prohlížeči. Podrobnosti o tom, jak začít jednotlivých typů webové aplikace ve výpočetním emulátoru najdete v tabulce v této části.

1. Pro nastavení služeb pro aplikaci publikujete Azure, musíte mít účet Microsoft a Azure předplatného. Postupujte podle kroků v tématu nastavení vašich služeb: [Příprava na publikovat nebo nasadit Azure aplikace Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Publikování webové aplikace Azure, otevřete místní nabídky pro web project a zvolte **Publikovat Azure**.

    Otevře se dialogové okno **Publikovat aplikace Azure** a Visual Studio spustí proces nasazení. Další informace o tom, jak publikovat aplikaci naleznete v části **Publikovat Azure aplikace Visual Studio** v [publikování do cloudové služby používat nástroje Azure](vs-azure-tools-publishing-a-cloud-service.md).

    >[AZURE.NOTE] Můžete taky publikovat webové aplikace z Azure projektu. Otevření místní nabídky pro Azure projektu a vyberte **Publikovat**.

1. Průběh nasazení se zobrazí, můžete zobrazit okna **Protokolu činnosti Azure** . Tento protokol se automaticky zobrazí při spuštění procesu nasazení. Řádkové položky protokol aktivity zobrazíte podrobné informace najdete v můžete rozbalit, jak je znázorněno na následujícím obrázku:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Volitelné) Zrušit procesu nasazení, otevřete místní nabídky pro položky v protokolu aktivity a zvolte **zrušit a odebrat**. Ukončení procesu nasazení a odstraní prostředí nasazení z Azure.

    >[AZURE.NOTE] Po byly nasazeny odebrat toto prostředí nasazení, je nutné použít [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Volitelné) Po role instance jste zahájili, Visual Studio automaticky se zobrazuje nasazení prostředí v **Azure výpočet** uzel v **Průzkumníkovi cloudu** nebo **Průzkumník serveru**. Tady můžete zobrazit stav instancí jednotlivé role.

    Následující obrázek znázorňuje instancí role v **Průzkumníku serveru** době, kdy je stále ve stavu Probíhá inicializace:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Přístup k aplikaci po nasazení, kliknutím na šipku vedle nasazení začalo stav **Dokončeno** v **protokolu Azure činnosti**. Adresa URL webové aplikace se zobrazí v Azure. Podívejte se na následující tabulku Podrobnosti o tom, jak začít určitý typ webové aplikace z Azure.

    V následující tabulce jsou uvedeny informace o tom, od které začnete konkrétních webových aplikací Azure nebo ke spouštění nebo ladění webové aplikace místně pomocí emulátoru výpočet Azure:

  	|Typ webové aplikace|Spustit/ladění místně pomocí emulátoru výpočetním|Spuštění v Azure|
  	|---|---|---|
  	|Webové aplikace|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Vyberte adresu URL hypertextového odkazu zobrazené na kartě **nasazení** **protokolu Azure činnosti** načíst úvodní stránku v prohlížeči.|
  	|Webová aplikace ASP.NET MVC 2|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Vyberte adresu URL hypertextového odkazu zobrazené na kartě **nasazení** **protokolu Azure činnosti** načíst úvodní stránku v prohlížeči.|
  	|Webová aplikace ASP.NET MVC 3|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Vyberte adresu URL hypertextového odkazu zobrazené na kartě **nasazení** **protokolu Azure činnosti** načíst úvodní stránku v prohlížeči.|
  	|Webová aplikace ASP.NET MVC 4|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Vyberte adresu URL hypertextového odkazu zobrazené na kartě **nasazení** **protokolu Azure činnosti** načíst úvodní stránku v prohlížeči.|
  	|Prázdná webová aplikace ASP.NET|Musíte přidat stránku .aspx v aplikaci nastavit jako úvodní stránku pro web projektu. Na řádku nabídek klikněte na **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Pokud máte výchozí aspx stránky v aplikaci, vyberte adresu URL hypertextového odkazu zobrazené na kartě **nasazení** **protokolu Azure aktivity** a načtení této stránky v prohlížeči. Pokud máte jiný aspx stránky, musíte přejít do této konkrétní stránce pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of page>.aspx`|
  	|Aplikace Silverlight|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Budete muset přejít na konkrétní stránku aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight podnikové aplikaci|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Budete muset přejít na konkrétní stránku aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of page>.aspx`|
  	|Aplikace Silverlight navigace|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Budete muset přejít na konkrétní stránku aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of page>.aspx`|
  	|Aplikace služby WCF|Soubor SVC musíte nastavit jako úvodní stránku služby WCF projektu. Na řádku nabídek klikněte na **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Je potřeba přejít k souboru svc aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of service file>.svc`|
  	|Aplikace služby WCF pracovního postupu|Soubor SVC musíte nastavit jako úvodní stránku služby WCF projektu. Na řádku nabídek klikněte na **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Je potřeba přejít k souboru svc aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of service file>.svc`|
  	|Entity dynamické ASP.NET|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Musíte aktualizovat připojovací řetězec (viz následující části). Je taky potřeba přejít na konkrétní stránku aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of page>.aspx`|
  	|Linq dynamických dat ASP.NET SQL|Na řádku nabídek vyberte **ladění**, **Spustit ladění** (klávesnice: Zvolte klávesu **F5** .).|Musíte postupujte podle pokynů v tomto postupu: použití databáze SQL Azure aplikace (viz výše uvedené části v tomto tématu). Je taky potřeba přejít na konkrétní stránku aplikace pomocí následujícího formátu pro svoji adresu url:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualizace připojovací řetězec pro entity dynamické ASP.NET

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualizovat připojovací řetězec pro entity dynamické ASP.NET

1. Pokud chcete vytvořit databázi SQL Azure, která se dá použít pro webovou aplikaci dynamické entity technologie ASP.NET, postupujte podle postup **použít k databázi SQL Azure aplikace** dříve v tomto tématu.

1. Přidání tabulek a polí, které potřebujete pro tuto databázi z [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Připojovací řetězec pro tento typ aplikace má tento formát souboru web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Aktualizujte hodnoty *connectionString* ADO.NET připojovací řetězec pro databázi SQL Azure následujícím způsobem:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Uložte nastavení(Web.config)) se změnami, které jste provedli připojovací řetězec na řádku nabídek vyberte **soubor**, **uložte web.config**.

## <a name="supported-project-templates"></a>Šablony podporované projektů

Pokud chcete publikovat webové aplikace Azure, aplikace musí používat jednu ze šablon projektu pro C# nebo Visual Basic, který je uvedený v následující tabulce.

|Skupina šablony projektu|Šablony projektu|
|---|---|
|Web|Webové aplikace|
|Web|Webová aplikace ASP.NET MVC 2|
|Web|Webová aplikace ASP.NET MVC 3|
|Web|ASP.NET MVC4 webové aplikace|
|Web|Prázdná webová aplikace ASP.NET|
|Web|Prázdná webová aplikace ASP.NET MVC 2|
|Web|Webová aplikace entit ASP.NET dynamických dat|
|Web|Linq dynamických dat ASP.NET SQL webovou aplikaci|
|Silverlight|Aplikace Silverlight|
|Silverlight|Silverlight podnikové aplikaci|
|Silverlight|Aplikace Silverlight navigace|
|WCF|Aplikace služby WCF|
|WCF|Aplikace služby WCF pracovního postupu|
|Pracovní postup|Aplikace služby WCF pracovního postupu|

## <a name="next-steps"></a>Další kroky
Další informace o publikování najdete v článku [Příprava publikovat nebo nasadit Azure aplikace Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Se taky podívejte [Nastavení nahoru s názvem přihlašovacími údaji](vs-azure-tools-setting-up-named-authentication-credentials.md).
