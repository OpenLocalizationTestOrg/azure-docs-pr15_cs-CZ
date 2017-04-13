<properties
    pageTitle=" Vytvořte účet Azure Media Services pomocí portálu Azure | Microsoft Azure"
    description="Tento kurz vás provede kroky pro vytvoření účtu Azure Media Services pomocí portálu Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Vytvořte účet Azure Media Services pomocí portálu Azure

> [AZURE.SELECTOR]
- [Portál](media-services-portal-create-account.md)
- [Prostředí PowerShell](media-services-manage-with-powershell.md)
- [ZBÝVAJÍCÍ](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

Portál Azure umožňuje rychle vytvořit účet Azure Media Services (AMS). Váš účet umožňuje přístup k, které umožňují uložení šifrování, kódování a spravovat vysílat mediální obsah v Azure Media Services. Při vytvoření účtu Media Services, můžete taky vytvořit účet přidružené úložiště (nebo použít existující) ve stejné zeměpisná oblast účtem Media Services.

Tento článek popisuje některé běžné koncepty a ukazuje, jak vytvořit účet Media Services pomocí portálu Azure.

## <a name="concepts"></a>Koncepty

Přístup k Media Services vyžaduje dva přidružené účty:

- Účet Media Services. Váš účet umožňuje přístup k sadu cloudové Media Services, které jsou k dispozici v Azure. Účet Media Services neukládá skutečné mediální obsah. Místo toho ukládá metadata obsah média a média zpracování úlohy ve vašem účtu. Při vytvoření účtu vyberte dostupné oblasti Media Services. Oblast, kterou vyberete je datové centrum, které jsou uloženy záznamy metadat pro váš účet.

    K dispozici Media Services (AMS) oblastí, patří: Severní Europe, západní Europe, západ USA, východoasijských USA, jihovýchodní Asie, východní Asie Japonsko Západ, Japonsko východ. Media Services nepoužívá spřažení skupiny.
    
    AMS má nyní také k dispozici v následujících datacentrech: Brazílie jih Indie západní, Indie jih a Indie centrální. Teď můžete portálu Azure vytvoření účtů služeb média a dělat nejrůznější věci zde popsané. Však Live kódování není povolený tyto datacentrech. Kromě toho ne všechny typy zboží kódování rezervovaná jsou dostupné v těchto datacentrech.
    
    - Brazílie jih: Standardních a základní kódování rezervovaná jednotky jsou k dispozici pouze.
    - Indie Západ, že hodnota jih Indie: 

- Účet Azure úložiště. Úložiště účty musí být umístěné v stejné zeměpisná oblast účtem Media Services. Při vytváření účtu Media Services můžete buď zvolte existujícího účtu úložiště ve stejné oblasti nebo můžete vytvořit nový účet úložiště ve stejné oblasti. Pokud byste odstranili Media Services účtu, se neodstraní objekty BLOB ve vašem účtu související úložiště.

## <a name="create-an-ams-account"></a>Vytvořte účet AMS

Postup v této části ukazují, jak vytvořit účet AMS.

1. Přihlaste se na [portál Azure](https://portal.azure.com/).
2. Klikněte na **+ Nový** > **Web + Mobile** > **Media Services**.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Vytvoření** účtu služby médií zadejte požadované hodnoty.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Do pole **Název účtu**zadejte název nového účtu AMS. Název účtu Media Services je všechna malá písmena číslic nebo písmen bez mezer a 3 až 24 znaků.
    2. V předplatného vyberte mezi různých Azure předplatné, které máte přístup.
    
    2. **Pole Skupina zdroje**vyberte zdroj nové nebo existující.  Skupina zdroje je kolekce zdroje, které mají životního cyklu, oprávnění a zásady. Další informace [v tomto poli](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Do pole **umístění**vyberte zeměpisnou oblast, která se použije k ukládání média a metadata záznamů pro váš účet Media Services. Tato oblast se použijí k obrázku a vysílání datových proudů. Do pole rozevíracího seznamu se zobrazí jenom k dispozici oblasti Media Services. 
    
    3. **Úložiště účtu**vyberte účet úložiště k poskytování úložiště objektů blob obsahu ze svého účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisná oblast účtem Media Services nebo můžete vytvořit účet úložiště. Vytvoření nového účtu úložiště ve stejné oblasti. Pravidla pro názvy účtů úložiště jsou stejné jako u účtů Media Services.

        Další informace o ukládání [tady](storage-introduction.md).

    4. Vyberte **Připnout do řídicího panelu** zobrazíte průběh nasazení účtu.
    
7. Klikněte na **vytvořit** v dolní části formuláře.

    Po úspěšném vytvoření účtu stav se změní na **systém**. 

    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Správa účtu AMS (například nahrát videa, kódovat prostředky, sledovat pokrok projektu) pomocí okna **Nastavení** .

## <a name="manage-keys"></a>Správa klíče

Potřebujete název účtu a primární klíče informace programově přístup k účtu Media Services.

1. Na portálu Azure vyberte svůj účet. 

    **Nastavení** okno na pravé straně. 

2. V okně **Nastavení** vyberte **klíče**. 

    **Správa klíčů** windows se zobrazí název účtu a zobrazí se primární a sekundární klíče. 
3. Stiskněte tlačítko Kopírovat a zkopírujte tyto hodnoty.
    
    ![Mediální služby klíče](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Další kroky

Teď můžete nahrávat soubory do účtu AMS. Další informace najdete v tématu [nahrání souborů](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


