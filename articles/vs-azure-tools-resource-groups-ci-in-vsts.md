<properties
    pageTitle="Nepřetržitý integrace služby týmu a používání Azure zdroje skupinových projektech | Microsoft Azure"
    description="Popisuje, jak nastavit nepřetržitý integrace ve Visual Studiu týmovou pomocí pole Skupina zdroje Azure nasazení projekty ve Visual Studiu."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Nepřetržitý integrace služby Visual Studio týmu pomocí projekty nasazení Azure pole Skupina zdroje

Abyste mohli nasadit Azure šablony, budete muset úkoly projít různých fází: sestavení Test, kopírovat do Azure (neboli "Pracovní") a nasazení šablon.  Akce služby Visual Studio týmu (a týmovou) dvěma různými způsoby. Obě metody poskytují stejných výsledků, takže klikněte na ten, který nejlépe odpovídá pracovní postup.

-   Přidání jednoduchého definici vaší Tvůrce dotazů, které se spouští skript Powershellu, který je součástí projektu Azure pole Skupina zdroje nasazení (nasazení AzureResourceGroup.ps1). Skript zkopíruje artefakty a potom nasadí šablonu.
-   Přidáte že více služeb týmu a vytvářet kroky, každý z nich provedení úkolu fáze.

Tento článek ukazuje, jak používat volbu první (použijte definici sestavení spustit skript Powershellu). Jednou z výhod tato možnost je skript používají vývojáři ve Visual Studiu stejné skript, který používá a týmovou. Tento postup předpokládá, že už máte projekt nasazení aplikace Visual Studio změnami do a týmovou.

## <a name="copy-artifacts-to-azure"></a>Zkopírujte artefakty Azure 

Bez ohledu na scénáře Pokud máte artefakty, které jsou potřebné pro nasazení šablony, musíte se k nim přístup správce prostředků Azure. Tyto artefakty lze zahrnout jako soubory:

-   Vnořené šablony
-   Konfigurace a DSC skriptů
-   Binární soubory aplikace

### <a name="nested-templates-and-configuration-scripts"></a>Konfigurace skripty a vnořené šablon
Při použití šablon, které aplikace Visual Studio (nebo vytvořené pomocí aplikace Visual Studio fragmenty) skript Powershellu nejen fáze artefaktům, taky parameterizes URI v tématech různé nasazení. Skript pak zkopíruje artefaktům zabezpečeného kontejneru v Azure, vytvoří token přidružení zabezpečení, které obsahují a předá tyto informace k nasazení šablony. V tématu [Vytvoření šablony nasazení](https://msdn.microsoft.com/library/azure/dn790564.aspx) Další informace o vnořené šablony.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Nastavení nepřetržitý nasazení služby týmu a

Volání a týmovou skript Powershellu, budete muset aktualizovat definice sestavení. Stručně řečeno se: 

1.  Úprava definice Tvůrce dotazů.
1.  Nastavte si Azure povolení služby a týmu.
1.  Přidání kroku sestavení Azure Powershellu, který odkazuje skript Powershellu v projektu nasazení Azure pole Skupina zdroje.
1.  Nastavte hodnotu parametru *- ArtifactsStagingDirectory* pro práci s projektem integrované a týmovou.

### <a name="detailed-walkthrough"></a>Podrobné informace

Podle těchto kroků vás provede jednotlivými kroky potřebné k konfigurace nepřetržitý nasazení služby týmu a 

1.  Úprava definice Tvůrce dotazů a týmovou a přidání kroku sestavení Azure Powershellu. Vyberte definice sestavení ve skupinovém rámečku kategorie **Sestavit definice** a klikněte odkaz pro **Úpravy** .

    ![][0]

1.  Přidejte nový krok sestavení **Azure PowerShell** definici Tvůrce dotazů a pak zvolte **Přidat sestavit krok...** tlačítko.

    ![][1]

1.  Klikněte na kategorie **úloh nasazení** , vyberte úkol **Azure PowerShell** a pak zvolte jeho tlačítko **Přidat** .

    ![][2]

1.  Zvolte krok sestavení **Azure PowerShell** a poté vyplnit hodnoty.

    1.  Pokud už máte koncový bod služby Azure přidali ke službám týmu a zvolte předplatné v **Azure předplatné** rozevírací seznam a potom přejděte k následující části. 

        Pokud nemáte koncový bod služby Azure služby a týmu, musíte ho vytvořit. Tento pododdílu vás provede procesu. Pokud váš účet Azure používá svým účtem Microsoft (třeba Hotmail), musíte pomocí následujících kroků získat jistinu Služba ověřování.

    1.  Klikněte na odkaz **Spravovat** vedle **Azure předplatné** rozevírací seznam.

        ![][3]

    1. Vyberte v rozevíracím seznamu **Nové koncový bod služby** **Azure** .

        ![][4]

    1.  V dialogovém okně **Přidat Azure předplatného** vyberte požadovanou možnost **Jistinu služby** .

        ![][5]

    1.  Přidání informací o vašem předplatném Azure s dialogovým oknem **Přidat Azure předplatného** . Budete muset zadat následující položky:
        -   Id předplatného
        -   Název předplatného
        -   Id jistinu služby
        -   Služba jistinu klíč
        -   Id klienta

    1.  Přidejte název podle svého výběru do pole název **předplatného** . Tato hodnota se zobrazí dál v **Azure předplatné** rozevíracího seznamu v a týmovou. 

    1.  Pokud si nejste jisti ID Azure předplatného, můžete některou z následujících příkazů ho.
        
        U skriptů Powershellu můžete pomocí:

        `Get-AzureRmSubscription`

        U Azure rozhraní příkazového řádku můžete pomocí:

        `azure account show`
    

    1.  ID služeb jistinu získáte jistinu služby spolu s ID klienta, postupujte podle [vytvořit Active Directory aplikace a služby jistinu portálu](resource-group-create-service-principal-portal.md) nebo [ověřování služby jistinu pomocí Správce prostředků Azure](resource-group-authenticate-service-principal.md).

    1.  Přidejte hodnoty služby jistinu ID, klíče jistinu služby a ID klienta do dialogového okna **Přidat Azure předplatného** a potom klikněte na tlačítko **OK** .

        Nyní máte platný jistinu služby používat ke spuštění skript Powershellu Azure.

1.  Úprava definice Tvůrce dotazů a zvolte krok sestavení **Azure Powershellu** . V rozevíracím seznamu **Azure předplatné** vyberte předplatné. (Pokud předplatné nezobrazí, zvolte tlačítko **Aktualizovat** další odkaz **Spravovat** .) 

    ![][8]

1.  Zadejte cestu k skript Powershellu AzureResourceGroup.ps1 nasazení. K tomuto účelu zvolte tlačítko se třemi tečkami (...) vedle pole **Cesta skriptu** vyhledejte skript Powershellu nasazení AzureResourceGroup.ps1 ve složce **skripty** projektu, vyberte ji a potom klikněte na tlačítko **OK** . 

    ![][9]

1. Když vyberete skript, aktualizujte cestu k skript tak, aby se spouští z Build.StagingDirectory (adresář, která je nastavená jako *ArtifactsLocation* ). Můžete to uděláte přidáním "$(Build.StagingDirectory)/"na začátku cesty skriptu.

    ![][10]

1.  V dialogovém okně **Argumenty skriptu** zadejte následujících parametrů (v jednom řádku). Když spustíte skript ve Visual Studiu, najdete v článku použití parametry a v okně **výstupu** . Můžete to jako výchozí bod pro nastavení hodnoty parametrů v kroku Tvůrce dotazů.

  	| Parametr | Popis|
  	|---|---|
  	| -ResourceGroupLocation           | Hodnotu geo umístění, kde skupiny zdrojů nachází, například **eastus** nebo **"východoasijských USA"**. (Pokud je mezera v názvu, přidejte apostrofy.) Další informace najdete v tématu [Azure oblastí](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Název Skupina zdroje pro toto nasazení.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Tento parametr, pokud jsou k dispozici, určuje, že artefakty muset nahrát Azure z místního počítače. Potřebujete jenom tento přepínač pokud nasazení šablony vyžaduje navíc artefakty, které chcete fáze pomocí skriptů Powershellu (například konfigurace skripty nebo vnořené šablony).                                                                                                                                                                 |
  	| -StorageAccountName              | Název účtu úložiště slouží k fázi artefakty pro toto nasazení. Tento parametr se pouze když kopírujete artefakty Azure. Tento účet úložiště nebude vytvářeny automaticky službami nasazení, již musí existovat.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Název přidružené k účtu úložiště skupina zdroje. Tento parametr se pouze když kopírujete artefakty Azure.|                                                                                                                                                                                                                                                                                                                               |
  	| -TemplateFile                    | Cesta k souboru šablony v projektu nasazení Azure pole Skupina zdroje. K vylepšení flexibilitu, použijte cesty pro tento parametr, který závisí na umístění skript Powershellu místo absolutní.|
  	| -TemplateParametersFile          | Cesta k souboru parametrů v projektu nasazení Azure pole Skupina zdroje. K vylepšení flexibilitu, použijte cesty pro tento parametr, který závisí na umístění skript Powershellu místo absolutní.|
  	| -ArtifactStagingDirectory        | Tento parametr umožňuje Powershellu skript vědět složce na místo, kam by měla zkopírovala binární soubory projektu. Tato hodnota přepíše výchozí hodnota použitá skript Powershellu. Pro týmovou a používat, nastavte hodnotu: - ArtifactStagingDirectory $(Build.StagingDirectory)                                                                                                                                                                                              |

    Tady je ukázka skriptu argumenty (řádek porušený pro snazší čitelnost):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Po dokončení pole **Skript argumenty** by měl vypadat takto.

    ![][11]

1.  Až přidáte lidi, že všechny požadované položky k Powershellu Azure sestavit krok, zvolte **fronty** sestavení s cílem sestavení projektu. **Vytvoření** obrazovka ukazuje výstup skript Powershellu.

## <a name="next-steps"></a>Další kroky

Přečtěte si [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md) zobrazíte další informace o správce prostředků Azure a skupin Azure zdrojů.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
