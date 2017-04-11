<properties 
   pageTitle="Začínáme s úložiště jezera dat | Azure" 
   description="Umožňuje vytvořit účet úložiště jezera dat a provádění základních operací v úložišti jezera dat na portálu" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Začínáme s úložiště jezera Azure dat na portálu Azure

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat portál Azure a vytvořit účet Azure úložišti jezera provádění základních operací, například při vytvoření složek, nahrávání a stahování souborů dat, odstraňte klienta, atd. Další informace o úložiště jezera dat najdete v článku [Základní informace o Azure jezera úložiště](data-lake-store-overview.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Zjistěte jak rychle se videa?

Podívejte se na následující videa začít pracovat s jezera úložiště.

* [Vytvoření účtu úložiště jezera dat](https://mix.office.com/watch/1k1cycy4l4gen)
* [Správa dat v úložišti jezera dat pomocí Průzkumníka dat](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu úložiště jezera dat Azure

1. Přihlaste se na novém [Portálu Azure](https://portal.azure.com).

2. Klikněte na **Nový**, klikněte na **Data + úložiště**a potom klikněte na **Úložiště jezera dat Azure**. Přečtěte si informace v **Úložišti jezera dat Azure** zásuvné a potom na tlačítko **vytvořit** v levém dolním rohu zásuvné.

3. V **Nové datové jezera úložiště** zásuvné obsahují hodnoty, jak je znázorněno níže snímek obrazovky:

    ![Vytvoření nového účtu úložiště jezera dat Azure] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Vytvořit nový účet jezera dat Azure")

    - **Předplatné**. Vyberte předplatné, pod kterým chcete vytvořit nový účet jezera úložiště.
    - **Pole Skupina zdroje**. Vyberte existující skupiny zdrojů, nebo klikněte na **vytvořit skupinu zdrojů** a vytvořte si ho. Skupina zdroje je kontejner obsahující související materiály pro aplikaci. Další informace najdete v tématu [Skupiny zdrojů v Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Umístění**: Vyberte umístění, ve které chcete vytvořit účet jezera úložiště.

4. Vyberte **Připnout k Startboard** , pokud mají účet úložiště jezera dat aby byly přístupné z Startboard.

5. Klikněte na **vytvořit**. Pokud jste se rozhodli Připnutí účet, který chcete startboard, přejdete zpět startboard a zobrazí se průběh zřízení účtu úložiště jezera dat. Po zřízení účtu úložiště jezera dat zásuvné účtu se zobrazí.

6. Rozbalení **Essentials** rozevíracího seznamu zobrazíte že informace o účtu úložiště jezera dat jako zdroj seskupte související informace jsou součástí umístění, atd. Klikněte na ikonu zobrazíte odkazy na další prostředky vztahující se k úložišti jezera **Rychlý Start** .

    ![Vaše Azure jezera úložišti účet] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Vaše jezera dat Azure účtu")

## <a name="createfolder"></a>Vytvoření složek v úložišti jezera dat Azure účtu

V části účtu úložiště jezera dat pro správu a ukládání dat můžete vytvořit složky.

1. Potřebujete založit účet úložiště jezera dat, který jste právě vytvořili. V levém podokně klikněte na tlačítko **Procházet**a **Jezera úložiště**na zásuvné úložiště jezera dat klikněte na název účtu, ve kterém chcete vytvořit složky. Pokud připnuté účet, který chcete startboard klikněte na tuto dlaždici účtu.

2. V svůj účet zásuvné úložiště jezera dat klikněte na položku **Průzkumník Data**.

    ![Vytvoření složek v úložišti jezera účtu] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Vytvoření složek v úložišti jezera účet")

3. Ve vaší zásuvné účtu úložiště jezera dat klikněte na **Nová složka**zadejte název nové složky a klikněte na tlačítko **OK**.
    
    ![Vytvoření složek v úložišti jezera účtu] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Vytvoření složek v úložišti jezera účtu")
    
    Nově vytvořené složky se zobrazí v zásuvné **Průzkumník dat** . Až vnořené složky můžete vytvořit všechny úrovně.

    ![Vytvoření složky v účtu jezera dat] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Vytvoření složky v účtu jezera dat")


## <a name="uploaddata"></a>Odeslání dat do účtu úložiště jezera dat Azure

Data můžete nahrávat s klientem úložiště jezera dat Azure přímo na kořenové úrovni nebo do složky, který jste vytvořili v rámci účet. Ve snímku obrazovky pod postupujte podle pokynů k nahrání souboru do podřízené složky z **Průzkumníka dat** zásuvné. V tomto snímku obrazovky soubor odeslat na podsložku zobrazené v popisu cesty (označené červeným ohraničením).

Pokud hledáte ukázkových dat chcete odeslat, můžete získat složce **Ambulance dat** z [Azure dat jezera libovolná úložiště](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Odeslání dat] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Odeslání dat")


## <a name="properties"></a>Vlastnosti a akce dostupné na uložená data

Klikněte na nově přidaný soubor otevřete zásuvné **Vlastnosti** . Vlastnostech přidružených k souboru a akce, které můžete dělat v souboru jsou dostupné v tomto zásuvné. Můžete taky zkopírovat úplnou cestu k souboru ve vašem účtu úložiště jezera dat Azure červeným ohraničením následující snímek obrazovky se zvýrazněnými.

![Vlastnosti na kartě data] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Vlastnosti na kartě data")

* Klikněte na **Náhled** zobrazíte náhled souboru přímo z prohlížeče. Můžete zadat formát taky verze preview. Klikněte na tlačítko **Náhled**, klikněte na **Formát** v **Souboru náhledu** zásuvné a v **Náhledu formátu** zásuvné zadejte požadované možnosti například počet řádků zobrazíte kódování, oddělovač použít, atd.

  ![Formát souboru náhledu] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Formát souboru náhledu")

* Klikněte na **Stáhnout** soubor stáhnout do počítače.

* Klikněte na **přejmenujte soubor** na přejmenujte soubor.

* Klikněte na **soubor odstranit** pro odstranění souboru.


## <a name="secure-your-data"></a>Zabezpečení dat

Je možné zabezpečit data uložená ve vašem účtu úložiště jezera dat Azure pomocí služby Azure Active Directory a řízení přístupu (ACL). Další informace o tom, jak to udělat, najdete v článku [zabezpečení dat v úložišti jezera dat Azure](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Úložiště jezera dat Azure odstranění účtu

Odstranění účtu úložiště jezera dat Azure z zásuvné úložiště jezera dat, klikněte na **Odstranit**. Potvrďte, budete vyzváni k zadání názvu účtu, který chcete odstranit. Zadejte název účtu a potom klikněte na **Odstranit**.

![Odstranění dat jezera účtu] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Odstranění dat jezera účtu")


## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Protokoly pro diagnostiku přístup pro jezera úložiště dat](data-lake-store-diagnostic-logs.md)
