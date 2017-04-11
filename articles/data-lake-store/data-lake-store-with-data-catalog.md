<properties
   pageTitle="Registrace data z úložiště jezera dat v katalogu dat Azure | Microsoft Azure"
   description="Registrace data z úložiště jezera dat v katalogu dat Azure"
   services="data-lake-store,data-catalog" 
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registrace data z úložiště jezera dat v katalogu dat Azure

V tomto článku se dozvíte, jak integrovat úložiště jezera dat Azure s katalogem dat Azure tak, aby vaše data zjistitelný v rámci organizace integrací s katalogem dat. Další informace o vytváření katalogu urychlen dat najdete v článku [Katalog dat Azure](../data-catalog/data-catalog-what-is-data-catalog.md). Scénáře, které můžete v katalogu dat, najdete v tématu [katalog dat Azure obvyklé scénáře](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Povolení předplatného Azure** dat jezera úložiště veřejného (verze Preview). V tématu [pokyny](data-lake-store-get-started-portal.md#signup).

- **Úložiště jezera dat azure účtu**. Postupujte podle pokynů na [Začínáme s úložiště jezera Azure dat na portálu Azure](data-lake-store-get-started-portal.md). Pro účely tohoto návodu dejte nám vytvořte účet úložiště jezera dat s názvem **datacatalogstore**. 

    Po vytvoření účtu, nahrajte ukázkové datové sady. Tento kurz dejte nám nahrajte soubor .csv ve složce **AmbulanceData** v [Azure dat jezera libovolná úložiště](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Odeslání dat do kontejneru objektů blob můžete různých klientů, například [Průzkumníka úložišť Azure](http://storageexplorer.com/).

- **Katalog dat azure**. Vaše organizace může mít už katalog dat Azure vytvořené pro vaši organizaci. Pouze jeden katalog jsou povolené pro každou organizaci.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register úložišti jezera jako zdroj pro katalog dat

>[AZURE.VIDEO adcwithadl] 

1. Přejděte na `https://azure.microsoft.com/services/data-catalog`a klikněte na **Začínáme**.

2. Přihlaste se k portálu katalog dat Azure a klikněte na **publikovat data**.

    ![Registrace zdroje dat] (./media/data-lake-store-with-data-catalog/register-data-source.png "Registrace zdroje dat")

3. Na další stránce klikněte na **Spustit aplikaci**. To stáhne seznamu souboru aplikace ve vašem počítači. Poklikejte na položku seznamu souboru ke spuštění aplikace.

4. Na stránce Vítejte klikněte na tlačítko **přihlásit**a zadejte svoje přihlašovací údaje.

    ![Úvodní obrazovka] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Úvodní obrazovka")

5. Na stránce Vybrat zdroj dat vyberte **Jezera dat Azure**a klikněte na tlačítko **Další**.

    ![Vyberte zdroj dat] (./media/data-lake-store-with-data-catalog/select-source.png "Vyberte zdroj dat")

6. Na další stránce zadejte název účtu úložiště jezera dat, který chcete zaregistrovat v katalogu dat. Nechte další možnosti jako výchozí a pak klikněte na **Připojit**.

    ![Připojení ke zdroji dat] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Připojení ke zdroji dat")

7. Na další stránku možné rozdělit následujícím segmentů.

    na. Do pole **Server hierarchie** představuje struktura složek účtu jezera úložiště. **$Root** představuje účtu kořenové úložiště jezera dat a **AmbulanceData** představuje složek vytvořených v kořenové úložiště jezera dat účtu.

    b. Pole **dostupné objekty** seznam souborů a složek ve složce **AmbulanceData** .

    c. **Objektů registrovaných pole** zobrazuje souborů a složek, které chcete zaregistrovat v katalogu dat Azure.

    ![Datová struktura zobrazení] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Datová struktura zobrazení")

8. Pro účely tohoto návodu byste měli zaregistrovat všechny soubory v adresáři. K těmto tlačítko (![přesunout objekty](./media/data-lake-store-with-data-catalog/move-objects.png "přesunout objekty")) do pole **objekty zaregistrovat** přesunete všechny soubory. 

    Protože v katalogu dat celou organizaci bude registrovaná data, je doporučeno přístup k přidání některé metadat, která později můžete rychle najít data. Můžete například přidat e-mailovou adresu pro data vlastníka (například kontaktem, který se odesílá data) nebo přidat značky k identifikaci data. Snímek obrazovky pod zobrazí značku přičteme k datům.

    ![Datová struktura zobrazení] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Datová struktura zobrazení")

    Klikněte na **zaregistrovat**.

8. Následující snímek obrazovky označuje, že v katalogu dat se úspěšně zaznamenává data.

    ![Registrace je dokončena.] (./media/data-lake-store-with-data-catalog/registration-complete.png "Datová struktura zobrazení")

9. Klikněte na **Zobrazení portál** a vraťte se do portálu katalog dat a ověřte, že můžete teď vyvolat registrovaných dat na portálu. Pokud chcete hledat data, můžete na značku, kterou jste použili při registraci data.

    ![Hledání dat v katalogu] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Hledání dat v katalogu")

10. Nyní můžete provádět operací, jako je přidání poznámky a si přečtěte následující dokumentaci k datům. Další informace najdete v následujících tématech.
    * [Pořizovat poznámky zdroje dat v katalogu dat](../data-catalog/data-catalog-how-to-annotate.md)
    * [Zdroje dat dokumentu v katalogu dat](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Viz taky

* [Pořizovat poznámky zdroje dat v katalogu dat](../data-catalog/data-catalog-how-to-annotate.md)
* [Zdroje dat dokumentu v katalogu dat](../data-catalog/data-catalog-how-to-documentation.md)
* [Integrace úložiště jezera dat s jinými službami Azure](data-lake-store-integrate-with-other-services.md)
