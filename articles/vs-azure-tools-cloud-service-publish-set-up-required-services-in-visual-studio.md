<properties
   pageTitle="Příprava přechodu na publikovat nebo nasadit Azure aplikace Visual Studio | Microsoft Azure"
   description="Další postupy se dají vytvořit cloudu a úložiště služby účtů a konfigurace aplikace Azure."
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

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Příprava přechodu na publikovat nebo nasadit Azure aplikace Visual Studio

## <a name="overview"></a>Základní informace

Musíte před publikováním projekt cloudové služby, musíte nastavit následující služby:

- **Cloudová služba** spustit vaše role v Azure prostředí

- **Účet úložiště** , která zajišťuje přístup ke službám objektů Blob fronty a tabulky.

Pomocí tohoto nastavení těchto služeb a konfigurace aplikace


## <a name="create-a-cloud-service"></a>Vytvoření do cloudové služby

Publikovat do cloudové služby Azure, musíte nejdřív vytvořit do cloudové služby, který spouští vaše role v Azure prostředí. Můžete vytvořit do cloudové služby [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)podle popisu v části **Vytvoření do cloudové služby tak, že na portálu Azure klasické**, dál v tomto tématu. Můžete taky vytvořit Cloudová služba ve Visual Studiu pomocí Průvodce publikování.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Vytvoření do cloudové služby pomocí aplikace Visual Studio

1. Otevření místní nabídky pro Azure projektu a vyberte **Publikovat**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Pokud ještě nejste přihlášení, přihlaste se pomocí uživatelského jména a hesla pro účet Microsoft nebo účet organizace, který máte přidružený k předplatnému Azure.

1. Klikněte na tlačítko **Další** přejděte na stránku **Nastavení** .

    ![Nastavení běžných publikování průvodce](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. V seznamu **Cloudovým službám** zvolte **Vytvořit nový**. Zobrazí se dialogové okno **Vytvořit služby Azure** .

1. Zadejte název cloudové služby. Název formuláře část adresy URL pro službu a proto musí být jedinečné. Název není malá a velká písmena.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Vytvoření do cloudové služby na portálu Azure klasické

1. Přihlaste se k [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft.

1. (volitelné) Chcete-li zobrazit seznam cloudové služby, které jste už vytvořili, zvolte odkaz Cloud Services na levé straně stránky.

1. Klikněte **+** ikonu v levém dolním rohu a v zobrazené nabídce zvolte **Cloudové služby** . Zobrazí se jiné obrazovky s možností **Vytvořit** a **Vytvořit vlastní**. Pokud se rozhodnete, **Vytvořit**, můžete vytvořit do cloudové služby jenom tím, že zadáte jeho adresu URL a oblasti, kde bude mít fyzicky hostovaný. Pokud se rozhodnete, **Vytvořte vlastní**, můžete okamžitě publikovat do cloudové služby zadáním balíčku (.cspkg soubor), souboru konfigurace (.cscfg) a certifikát. Vytvořit vlastní není povinný, pokud chcete publikovat cloudové služby pomocí příkazu **Publikovat** v Azure projektu. Příkaz **Publikovat** je k dispozici v místní nabídce Azure projektu.

1. Zvolte **Vytvořit** pomocí aplikace Visual Studio později publikovat cloudové služby.

1. Zadejte název pro vaše cloudové služby. Úplnou adresu URL se zobrazí vedle názvu.

1. V seznamu vyberte oblast, kde jsou umístěny většina uživatelů.

1. V dolní části okna vyberte odkaz **Vytvořit cloudové služby** .

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

Účet úložiště poskytuje přístup ke službám objektů Blob, fronty a tabulek. Vytvořit účet úložiště pomocí aplikace Visual Studio nebo [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Vytvoření účtu úložiště pomocí aplikace Visual Studio

1. V **Okně Průzkumník**otevřete místní nabídky pro uzel **úložiště** a klikněte na **Vytvořit účet úložiště**.

    ![Vytvořte nový účet Azure úložiště](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Vyberte nebo zadejte následující informace pro nový účet úložiště v dialogovém okně **Vytvořit účet úložiště** .
    - Azure předplatné, ke kterému chcete přidat účet úložiště.
    - Název, který chcete použít pro nový účet úložiště.
    - Oblast nebo spřažení skupiny (například západní USA nebo jihovýchodní Asie).
    - Typ replikace, který chcete použít účtu úložiště, jako jsou například Geo nadbytečné.

1. Až budete hotovi, vyberte **vytvořit**. V seznamu **úložiště** v **Průzkumníku serveru**se zobrazí nový účet úložiště.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Vytvoření účtu úložiště tak, že na portálu Azure klasické

1. Přihlaste se k [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkId=253103) na webu společnosti Microsoft.

1. (Volitelné) Účty úložiště zobrazíte zvolte **úložiště** odkaz v panelu na levé straně stránky.

1. V levém dolním rohu stránky vyberte **+** ikonu.

1. V zobrazené nabídce zvolte **úložiště**a klikněte na **Vytvořit**.

1. Název účtu úložiště být, bude výsledkem jedinečnou adresu url.

1. Cloudová služba pojmenujte. Úplnou adresu URL se zobrazí vedle názvu.

1. V seznamu oblastí vyberte oblast, kde jsou umístěny většina uživatelů.

1. Určete, jestli chcete povolit geo replikace. Pokud povolíte geo replikace, vaše data budou uložena na několika místech fyzické snížit šanci ztráty. Tato funkce umožňuje větší úložiště, ale můžete snížit náklady povolením geo umístění při vytváření účtu úložiště namísto přidávání funkci později. Další informace najdete v tématu [Geo replikace](http://go.microsoft.com/fwlink/?LinkId=253108).

1. V dolní části okna vyberte odkaz **Vytvořit účet úložiště** .

Po vytvoření účtu úložiště, zobrazí se adresy URL, které můžete použít k přístupu k prostředkům ve všech služby Azure úložiště a primárních a sekundárních přístupových kláves pro váš účet. Pomocí klávesy ověření žádosti proti úložiště služby.

>[AZURE.NOTE] Sekundární přístupová klávesa poskytuje stejný přístup ke svému účtu úložiště jako primární přístupové klávesy a vygeneruje se jako zálohu ohroženo primární přístupové klávesy. Kromě toho je vhodné obnovit přístupových kláves v pravidelných intervalech. Řetězec nastavení připojení pro použití sekundárního klíče při obnovit primární klíč, můžete ho pro použití regenerované primárního klíče při obnovit sekundárního klíče upravit a pak můžete změnit.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Konfigurace aplikace využití služeb účtu úložiště

Musíte nakonfigurovat role, který má přístup k úložiště služby použití služby Azure úložiště, které jste vytvořili. K tomuto účelu můžete více konfigurací služby Azure projektu. Ve výchozím nastavení dvě vytvořené v Azure projektu. Pomocí několika konfigurace služeb můžete používat stejný připojovací řetězec v kódu, ale mají jinou hodnotu pro připojovacího řetězce v každé konfiguraci služby. Můžete třeba jeden konfiguraci služby ke spouštění a ladění aplikace místně pomocí emulátoru Azure úložiště a jinou službu publikovat aplikaci Azure. Další informace o konfiguraci služby najdete v článku [Konfigurace Your Azure pomocí několika služby konfigurace projektu](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Konfigurace aplikace pomocí služby, které poskytuje účtu úložiště

1. Ve Visual Studiu otevřete Azure řešení. V Průzkumníku otevření místní nabídky pro každou roli v Azure projektu, který má přístup ke službě úložiště a zvolte **Vlastnosti**. Zobrazí se stránka s názvem roli v editoru Visual Studio. Na stránce se zobrazí pole na kartě **Konfigurace** .

1. Na stránkách vlastností pro roli zvolte **Nastavení**.

1. V seznamu **Konfigurace služby** vyberte název konfiguraci služby, kterou chcete upravit. Pokud chcete provést změny u všech konfigurace služeb pro tuto roli, můžete **Všechny konfigurace**.  Další informace o tom, jak aktualizovat konfigurace služeb naleznete v části **Správa připojovací řetězec pro účty úložiště** v tématu [Konfigurace role pro službu Azure cloudu pomocí aplikace Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Chcete-li změnit nastavení řetězec připojení, zvolte **...** tlačítko vedle požadované nastavení. Zobrazí se dialogové okno **Vytvořit úložiště připojovací řetězec** .

1. V části **Připojit pomocí**vyberte možnost **předplatného** .

1. V seznamu **předplatné** vyberte předplatné. Pokud seznam předplatných nezahrnuje ten, který se má, zvolte odkaz **Nastavení stahování publikovat** .

1. V seznamu **název účtu** vyberte název účtu úložiště. Přihlašovací údaje účtu úložiště Azure nástroje automaticky získá pomocí souboru .publishsettings. Chcete-li zadat svoje přihlašovací údaje účtu úložiště ručně, vyberte možnost **ručně zadané přihlašovací údaje** a pokračujte tento postup. Název účtu úložiště a primární klíč můžete získat z [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885). Pokud nechcete, aby k určení úložišti nastavení účtu ručně, klikněte na tlačítko **OK** zavřete dialogová okna.

1. Zvolte odkaz přihlašovací údaje **účtu úložiště Enter** .

1. Do pole **název účtu** zadejte název účtu úložiště.

    >[AZURE.NOTE] Přihlaste se k [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)a potom klikněte na tlačítko **úložiště** . Na portálu zobrazuje seznam účtů úložiště. Pokud se rozhodnete účet, zobrazí se stránka pro něj. Název účtu úložiště můžete zkopírovat z této stránky. Pokud používáte dřívější verzi portálu klasické název účtu úložiště zobrazí v zobrazení **Úložiště účty** . Zkopírujte tento název, zvýrazněte tuto v okně **vlastností** tohoto zobrazení a potom zvolte klávesy Ctrl + C. Chcete-li vložit název do aplikace Visual Studio, vyberte textové pole **název účtu** a klikněte klávesy Ctrl + V.

1. Do pole **klíč účtu** primární klíč, zadejte nebo zkopírujte a vložte z [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).
    Zkopírujte tento klíč:

    1. V dolní části na stránce účtu odpovídající úložiště klikněte na tlačítko **Spravovat klíče** .

    1. Na stránce **Spravovat přístup klíče** vyberte text primární přístupové klávesy a pak zvolte klávesy Ctrl + C.

    1. Na kartě nástroje Azure vložte do pole **klíč účtu** klávesu.

    1. Je třeba vybrat jednu z následujících možností a zjistit, jak službu bude získat přístup k účtu úložiště:
        - **Použití protokolu HTTP**. Toto je standardní možnost. Například `http://<account name>.blob.core.windows.net`.
        - **Použití HTTPS** pro zabezpečené připojení. Například `https://<accountname>.blob.core.windows.net`.
        - **Zadejte vlastní koncové body** pro každou z těchto tří služeb. Tyto koncové body můžete zadejte do pole pro konkrétní službu.

        >[AZURE.NOTE] Pokud jste vytvořili vlastní koncové body, můžete vytvořit složitější připojovací řetězec. Když použijete tento formát řetězce, můžete určit službu koncové body úložiště, které obsahují vlastní název domény, které máte zaregistrované účtu úložiště objektů Blob službou. Taky můžete udělit přístup jenom pro zdroje objektů blob v kontejneru jednoho prostřednictvím podpis sdílený přístup. Další informace o tom, jak vytvořit vlastní koncové body najdete v článku [Konfigurace Azure úložiště připojovací řetězec](storage-configure-connection-string.md).

1. Uložte tyto změny řetězec připojení, klikněte na tlačítko **OK** a potom klikněte na tlačítko **Uložit** na panelu nástrojů. Po uložení tyto změny se může vstoupit hodnotu tento připojovací řetězec kódu pomocí [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Pokud publikujete aplikaci Azure, zvolte konfiguraci služby, který obsahuje účet Azure úložiště pro připojovací řetězec. Po publikování aplikace ověřte, zda aplikace funguje očekávaným proti služby Azure úložiště

## <a name="next-steps"></a>Další kroky

Další informace o publikování aplikací na Azure z aplikace Visual Studio, najdete v článku [publikování do cloudové služby používat nástroje Azure](vs-azure-tools-publishing-a-cloud-service.md).
