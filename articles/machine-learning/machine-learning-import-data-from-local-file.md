<properties
    pageTitle="Import dat do počítače výukové Studio ze souboru místní | Microsoft Azure"
    description="Návod k importu dat školení Azure počítače výukové Studio z místního souboru."
    keywords="Importujte dat, formát dat ve sloupcích, datové typy, zdrojů dat, školení dat"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Import dat školení do Azure počítače výukové Studio z místního souboru

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Použít vlastní data ve počítače výukové studiu, můžete odeslat datový soubor předem z pevného disku vytvořit modulu datovou sadu v pracovním prostoru. 


## <a name="import-data-from-a-local-file"></a>Import dat z místní soubor

Import dat z pevného disku následujícím způsobem:

1. Klikněte na **+ Nový** v dolní části okna Studio výukové počítače.
2. Vyberte **datovou sadu** a **od místní soubor**.
3. V dialogovém okně **Odeslat novou datovou sadu** vyhledejte soubor, který chcete nahrát
4. Zadejte název, vystihují typ dat a volitelně zadejte popis. Doporučuje se popis – umožňuje provádět jakékoli vlastnosti o data, která budete chtít mějte na paměti při používání dat v budoucnu.
5. Zaškrtávací políčko **Toto je nová verze existující datovou sadu** umožňuje aktualizovat existující datovou sadu novými daty. Stačí kliknout toto políčko a zadejte název existující datovou sadu.

Při nahrávání zobrazí se zpráva, že je uložení souboru. Nahrání doba závisí na velikosti dat a rychlosti připojení ke službě.
Pokud víte, že soubor bude trvat dlouho, můžete dělat další věci do počítače výukové Studio popředí. Však zavírání prohlížeče způsobí selhání odeslání dat.

Po nahrání data uložená v modulu datovou sadu a je dostupná pokusu v pracovním prostoru.
Když upravujete pokus, můžete najít, který jste vytvořili v seznamu **Moje datové sady** v seznamu **Uložili datové sady** v modulu palety datové sady. Můžete přetažení datovou sadu na plátno experiment když budete chtít použít uvedenou množinu dat pro další analýzu a Výuka počítače.



