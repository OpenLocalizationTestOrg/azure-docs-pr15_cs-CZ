<properties 
   pageTitle="Další Data jezera technologie pro analýzu a U SQL pomocí Azure portál interaktivní výukové programy pro | Azure" 
   description="Rychlé zahájení výukové dat jezera technologie pro analýzu a U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Použití interaktivní výukové programy pro analýzy jezera dat Azure

Na portálu Azure obsahuje interaktivní kurz můžete začít pracovat s analýzy dat jezera. Tento článek ukazuje, jak absolvovat kurz pro analýzu protokoly webu.


>[AZURE.NOTE]Pokud budete chtít absolvovat stejné kurz používání aplikace Visual Studio, najdete v článku [protokoly webu analyzovat pomocí analýzy dat jezera](data-lake-analytics-analyze-weblogs.md).
>Další interaktivní výukové programy pro které budou přidány do portálu.


Další kurzy najdete tady:

- [Začínáme s jezera analýzy dat pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s jezera analýzy dat pomocí prostředí PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Začínáme s jezera analýzy dat pomocí .NET SDK](data-lake-analytics-get-started-net-sdk.md)
- [Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Zjistit předpoklady pro**

Před zahájením tohoto kurzu, musíte mít takto:

- **Jezera analýzy dat A účtu**.  V tématu [Začínáme s Azure dat jezera technologie pro analýzu portálu Azure](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Vytvoření účtu jezera analýzy dat 

Před spuštěním všechny úlohy musíte mít účet analýzy dat jezera.

Každý účet analýzy dat jezera má závislost účtu [Úložiště jezera dat Azure](../data-lake-store/data-lake-store-overview.md) .  Tento účet se označuje jako výchozí úložiště jezera dat účet.  Vytvořit účet úložiště jezera dat předem nebo při vytváření účtu dat jezera analýzy. V tomto kurzu vytvoříte účtu úložiště jezera dat pomocí analýzy účtu

**Vytvoření účtu jezera analýzy dat**

1. Přihlaste se k [portálu Azure](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Klikněte na **Microsoft Azure** v levém horním rohu otevřete StartBoard.
3. Klikněte na dlaždici **Marketplace** .  
3. **Jezera analýzy dat Azure** zadejte do pole Hledat na zásuvné **vše** a potom stiskněte klávesu **ENTER**. Zobrazí se **Jezera analýzy dat Azure** v seznamu.
4. Klikněte na **Analýza jezera Azure dat** ze seznamu.
5. Klikněte na **vytvořit** v dolní části zásuvné.
6. Zadejte nebo vyberte takto:

    ![Portálu zásuvné technologie pro analýzu dat jezera Azure](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Název**: název účtu analýzy.
    - **Úložiště jezera dat**: účtu každý jezera analýzy dat má závislá účet jezera úložiště. Účet jezera analýzy dat a závislá účet úložiště jezera dat musí být umístěné v centru stejné Azure data. Postupujte podle pokynů k vytvoření nového účtu úložiště jezera dat, nebo vyberte stávající.
    - **Předplatné**: Zvolte Azure předplatné pro účet analýzy.
    - **Pole Skupina zdroje**. Vyberte existující skupinu zdroje Azure nebo vytvořte nový účet. Aplikace se obvykle vytvářejí součástí mnoho, třeba do webových aplikací databáze, databázový server, úložiště a 3 stran služby. Správce Azure zdroje (ARM) umožňuje práce se zdroji v aplikaci jako skupinu, označovaný taky jako skupina zdroje Azure. Můžete nasadit, aktualizace, sledovat nebo odstranit všechny zdroje pro aplikaci v operaci jediné, koordinovaný. Použití šablony pro nasazení a této šablony můžete pracovat na jiném prostředí například testování pracovní a výroby. Fakturace můžete vysvětlit pro vaši organizaci, zobrazením nákladů úrovní pro celou skupinu. Další informace najdete v tématu [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md). 
    - **Umístění**. Vyberte Azure datacentrem analýzy dat jezera účtu. 
7. Vyberte **Připnout k Startboard**. Toto je nutný pro po tomto kurzu.
8. Klikněte na **vytvořit**. Přenese vás do portálu StartBoard. Nová dlaždice se přidá na domovskou stránku s popiskem "Nasazení Azure dat jezera analýzy". Na krátkou chvíli vytvořit účet analýzy dat jezera trvá. Po vytvoření účtu na portálu otevře účtu na nové zásuvné.

    ![Portálu zásuvné technologie pro analýzu dat jezera Azure](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Spuštění analýzy protokolu webu Interaktivní kurz

**Chcete-li otevřít kurz interaktivní Analytics protokolu webu**

1. Z portálu Microsoft klikněte na **Microsoft Azure** v nabídce nalevo StartBoard otevřete.
2. Klikněte na dlaždici spojeným s vaším účtem dat jezera analýzy.
3. Na panelu **Essentials** klikněte na **Prozkoumat interaktivní kurzy** .

    ![Kurzy interaktivní Analytics jezera dat](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Pokud se zobrazí oranžová upozornění oznámením "vzorky není nastaven, klikněte na...", klikněte na **Kopírovat ukázková Data** zkopírujte ukázková data do výchozí úložiště jezera dat účet. Interaktivní kurz vyžaduje data, která chcete spustit.
5. **Interaktivní výukové programy pro** zásuvné klikněte na **Web Analytics protokolu**. Na portálu otevře kurzu v nové portálu zásuvné.
5. Klikněte na **1 Úvod** a postupujte podle pokynů

##<a name="see-also"></a>Viz taky

- [Přehled analýzy dat jezera Microsoft Azure](data-lake-analytics-overview.md)
- [Začínáme s jezera analýzy dat pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s jezera analýzy dat pomocí prostředí PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Můžete vyvíjet U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Analýza protokoly webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md)
