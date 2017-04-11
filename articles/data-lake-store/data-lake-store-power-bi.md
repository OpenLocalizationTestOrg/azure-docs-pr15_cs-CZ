<properties
   pageTitle="Analýza dat v úložišti jezera dat pomocí Power BI | Microsoft Azure"
   description="Analýza dat uložených v úložišti jezera dat Azure pomocí Power BI"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analýza dat v úložišti jezera dat pomocí Power BI

V tomto článku se dozvíte, jak používat Power BI Desktop analýza a vizualizace dat uložených v úložišti jezera dat Azure.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Úložiště jezera dat azure účtu**. Postupujte podle pokynů na [Začínáme s úložiště jezera Azure dat na portálu Azure](data-lake-store-get-started-portal.md). Tento článek předpokládá, že máte už vytvořený účet služby úložiště jezera dat s názvem **mybidatalakestore**a nahráli Ukázka datového souboru (**Drivers.txt**). Tento ukázkový soubor je k dispozici ke stažení z [Azure dat jezera libovolná úložiště](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).

- **Power BI Desktop**. Je možné stáhnout z [Webu služby Stažení softwaru](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 


## <a name="create-a-report-in-power-bi-desktop"></a>Vytvoření sestavy v Power BI Desktop

1. Spusťte Power BI Desktop ve vašem počítači.

2. Z **Domů** pásu karet klikněte na **Načíst Data**a potom klikněte na další. V dialogovém okně **Načíst Data** klikněte na **Azure**, klikněte na **Úložiště jezera dat Azure**a pak klikněte na **Připojit**.

    ![Připojení k úložišti jezera dat.] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Připojení k úložišti jezera dat.")

3. Pokud se zobrazí dialogové okno informace o spojnice je ve fázi vývoj, určovat, jestli Pokud chcete pokračovat.

4. V dialogovém okně **Microsoft Azure jezera úložiště** zadejte adresu URL ke svému účtu úložiště jezera dat a klikněte na **OK**.

    ![Adresa URL jezera úložiště dat] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Adresa URL jezera úložiště dat")

5. V dialogovém okně Další klikněte na **přihlásit** a přihlaste se k úložišti jezera účtu. Budete přesměrováni na přihlašovací stránce vaší organizace. Postupujte podle pokynů a přihlaste se do účtu.

    ![Přihlaste se do úložiště jezera dat] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Přihlaste se do úložiště jezera dat")

6. Po úspěšném jste přihlášení, klikněte na **Připojit**.

    ![Připojení k úložišti jezera dat.] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Připojení k úložišti jezera dat.")

7. Dialogové okno Další zobrazuje soubor, který jste nahráli ke svému účtu jezera úložiště. Zkontrolujte informace a potom klikněte na **načíst**.

    ![Načtení dat z úložiště jezera dat] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Načtení dat z úložiště jezera dat")

8. Po do Power BI byly úspěšně načtení dat, zobrazí se následující pole na kartě **pole** .

    ![Importovaných polí] (./media/data-lake-store-power-bi/imported-fields.png "Importovaných polí")

    Však vizualizace a analyzovat data, jsme raději používáte data, která mají být k dispozici pro tato pole

    ![Požadovaná pole] (./media/data-lake-store-power-bi/desired-fields.png "Požadovaná pole")

    V dalším krokům budeme aktualizovat dotaz převést importovaná data na požadovaný formát.

9. **Home** pásu karet klikněte na **Upravit dotazů**.

    ![Úprava dotazů] (./media/data-lake-store-power-bi/edit-queries.png "Úprava dotazů")

10. V editoru dotazů, ve sloupci **obsahu** klikněte na **binární**.

    ![Úprava dotazů] (./media/data-lake-store-power-bi/convert-query1.png "Úprava dotazů")

11. Zobrazí se ikona souboru, představovaná **Drivers.txt** souborů, které jste nahráli. Klikněte pravým tlačítkem myši na soubor a klikněte na **CSV**.  

    ![Úprava dotazů] (./media/data-lake-store-power-bi/convert-query2.png "Úprava dotazů")

12. Výstup byste měli vidět, jak je ukázáno v následujícím příkladu. Data jsou nyní k dispozici ve formátu, který slouží k vytváření vizualizací.

    ![Úprava dotazů] (./media/data-lake-store-power-bi/convert-query3.png "Úprava dotazů")

13. Z **Domů** pásu karet klikněte na **Zavřít a použít**a potom klikněte na **Zavřít a použít**.

    ![Úprava dotazů] (./media/data-lake-store-power-bi/load-edited-query.png "Úprava dotazů")

14. Po aktualizaci dotazu na kartě **pole** se zobrazí nový dostupná pole pro vizualizaci.

    ![Aktualizované pole] (./media/data-lake-store-power-bi/updated-query-fields.png "Aktualizované pole")

15. Dejte nám vytvoření výsečového grafu představující ovladače v jednotlivých městech pro dané zemi. K tomu, zkontrolujte následující možnosti.

    1. Na kartě vizualizace klikněte na požadovaný symbol ve výsečovém grafu.

        ![Vytvoření výsečového grafu] (./media/data-lake-store-power-bi/create-pie-chart.png "Vytvoření výsečového grafu")

    2. Sloupce, které budeme používat se **sloupec 4** (název města) **7 sloupec** (název země). Jak je ukázáno v následujícím příkladu přetáhněte tyto sloupce z karty **pole** **vizualizace** karta.

        ![Vytvoření vizualizace] (./media/data-lake-store-power-bi/create-visualizations.png "Vytvoření vizualizace")

    3. Výsečový graf nyní by měl vypadat jako takové, jaké vidíte níže.

        ![Výsečový graf] (./media/data-lake-store-power-bi/pie-chart.png "Vytvoření vizualizace")

16. Výběrem určité země ze stránky úrovně filtrů teď uvidíte počet ovladače v jednotlivých městech vybrané země. Na kartě **vizualizací** v části **filtry úrovni stránky**, vyberte **Brazílie**.

    ![Vyberte zemi] (./media/data-lake-store-power-bi/select-country.png "Vyberte zemi")

17. Výsečový graf se automaticky aktualizuje a zobrazí ovladače ve městech Brazílie.

    ![Ovladače země] (./media/data-lake-store-power-bi/driver-per-country.png "Ovladače podle země")

18. V nabídce **soubor** klikněte na **Uložit** uložte jako soubor Power BI Desktop vizualizace.

## <a name="publish-report-to-power-bi-service"></a>Publikování sestavy do služby Power BI

Po vytvoření vizualizace v Power BI Desktop, můžete ho sdílet s ostatními jeho publikováním na službu Power BI. Další informace o tom, jak to udělat najdete v článku [Publikovat na Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Viz taky

* [Analýza dat v úložišti jezera dat pomocí analýzy jezera dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
